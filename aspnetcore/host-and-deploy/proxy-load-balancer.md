---
title: プロキシ サーバーを操作して、ロード バランサーに ASP.NET Core の構成します。
author: guardrex
description: プロキシ サーバーと多くの場合、要求の重要な情報がわかりにくくなるは、ロード バランサーの背後にホストされているアプリの構成について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: f18a5c518edc739e0fe667f3aef6ffd38c06366c
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>プロキシ サーバーを操作して、ロード バランサーに ASP.NET Core の構成します。

によって[Luke Latham](https://github.com/guardrex)と[Chris Ross](https://github.com/Tratcher)

ASP.NET Core の推奨構成で、アプリは IIS/ASP.NET コア モジュールや Nginx、Apache を使用してホストされます。 プロキシ サーバー、ロード バランサー、およびその他のネットワーク アプライアンス、アプリケーションに到達する前に多くの場合、要求に関する情報を表面化しません。

* HTTPS 要求は、over HTTP プロキシが、元の設定 (HTTPS) が失われ、ヘッダーで転送する必要があります。
* アプリは、プロキシおよびその場合は true。 ソース インターネットまたは社内ネットワーク上ではなくから要求を受信するため元のクライアント IP アドレスにヘッダーに転送ことも必要があります。

この情報は、要求の処理、たとえばリダイレクト、認証、リンクの生成、ポリシーの評価、およびクライアント geoloation で重要になる可能性があります。

## <a name="forwarded-headers"></a>転送されたヘッダー

慣例によりは、プロキシは、HTTP ヘッダーの情報を転送します。

| Header | 説明 |
| ------ | ----------- |
| X-転送の場合 | 要求とプロキシのチェーン内の後続のプロキシを開始したクライアントに関する情報を保持します。 このパラメーターには、IP アドレス (および、必要に応じて、ポート番号) を含めることがあります。 プロキシ サーバーのチェーンには、最初のパラメーターは、要求が行われた最初のクライアントを示します。 後続のプロキシの識別子に従ってください。 パラメーターの一覧では、チェーン内の最後のプロキシがありません。 最後のプロキシの IP アドレス、および必要に応じて、ポート番号は、トランスポート層でのリモートの IP アドレスとして使用できます。 |
| X-Forwarded-Proto | 発信元スキーム (HTTP または HTTPS) の値。 値場合もありますスキームの一覧要求が複数のプロキシを通過します。 |
| X 転送ホスト | ホスト ヘッダー フィールドの元の値。 通常、プロキシでは、ホスト ヘッダーを変更しないでください。 参照してください[マイクロソフト セキュリティ アドバイザリ CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295)プロキシを検証しませんシステムに影響を与える権限昇格の脆弱性や既知の適切な値に restict ホスト ヘッダーの詳細についてです。 |

ヘッダーの転送ミドルウェアから、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージ化、これらのヘッダーを読み取り、および関連付けられているフィールドに入力[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)です。 

ミドルウェアの更新:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash;設定を使用して、`X-Forwarded-For`ヘッダーの値。 ミドルウェアを設定する方法に影響を与える追加設定`RemoteIpAddress`です。 詳細については、次を参照してください。、[転送ヘッダー ミドルウェアのオプション](#forwarded-headers-middleware-options)です。
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash;設定を使用して、`X-Forwarded-Proto`ヘッダーの値。
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash;設定を使用して、`X-Forwarded-Host`ヘッダーの値。

すべてのネットワーク アプライアンスの追加、`X-Forwarded-For`と`X-Forwarded-Proto`追加構成なしヘッダー。 アプリに到達したときは、プロキシの要求にこれらのヘッダーが含まれていない場合は、アプライアンスの製造元のガイダンスを参照してください。

ヘッダーのミドルウェアを転送[既定の設定](#forwarded-headers-middleware-options)ように構成できます。 既定の設定は次のとおりです。

* のみがある*1 つのプロキシ*アプリケーションと、要求のソースの間です。
* ループバック アドレスのみが既知のプロキシ用に構成し、既知のネットワーク。

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS または IIS Express および ASP.NET Core モジュール

IIS および ASP.NET Core モジュールの背後にあるアプリの実行時に、転送されたヘッダーのミドルウェアを IIS 統合ミドルウェアによって既定で有効です。 転送されたヘッダーのミドルウェアが最初に実行する特定の制限の構成のミドルウェア パイプラインで転送されたヘッダーを含む信頼上の問題により、ASP.NET のコア モジュールにアクティブ化 (たとえば、 [IP スプーフィング](https://www.iplocation.net/ip-spoofing))。 ミドルウェアの転送が構成されて、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダーとが 1 つの localhost プロキシに制限します。 追加の構成が必要な場合を参照してください、[転送ヘッダー ミドルウェアのオプション](#forwarded-headers-middleware-options)です。

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>その他のプロキシ サーバーとロード バランサーのシナリオ

IIS 統合ミドルウェアを使用して、外部ヘッダー ミドルウェアの転送は、既定で有効になっていません。 アプリのヘッダーの転送処理に転送されたヘッダーのミドルウェアを有効にする必要があります[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)です。 ない場合は、ミドルウェアを有効にした後[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)ミドルウェア、既定値に指定された[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)は[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

ミドルウェアを構成する[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)を転送する、`X-Forwarded-For`と`X-Forwarded-Proto`でヘッダー`Startup.ConfigureServices`です。 呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`他のミドルウェアを呼び出す前に。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)で指定された`Startup.ConfigureServices`または使用して拡張メソッドを直接[UseForwardedHeaders (IApplicationBuilder、ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_)、既定値転送するヘッダーが[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)です。 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)を転送するヘッダーのプロパティを構成する必要があります。

## <a name="forwarded-headers-middleware-options"></a>転送されたヘッダーのミドルウェアのオプション

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)ヘッダーの転送ミドルウェアの動作を制御します。

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| オプション | 説明 |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername)です。<br><br>既定値は、`X-Forwarded-For` です。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | どのフォワーダーを処理するかを識別します。 参照してください、 [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)適用されるフィールドの一覧についてはします。 このプロパティに割り当てられている標準値は<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>します。<br><br>既定値は[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)です。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername)です。<br><br>既定値は、`X-Forwarded-Host` です。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername)です。<br><br>既定値は、`X-Forwarded-Proto` です。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 処理されるヘッダー内のエントリの数を制限します。 設定`null`、制限が、これを無効にする必要がある場合にのみ実行`KnownProxies`または`KnownNetworks`が構成されています。<br><br>既定値は 1 です。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | アドレスから転送されたヘッダーをそのまま使用する既知のプロキシの範囲です。 クラスレス ドメイン間ルーティング (CIDR) 表記を使用して IP の範囲を提供します。<br><br>既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[ip ネットワーク](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> の 1 つのエントリを含む`IPAddress.Loopback`です。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 転送されたヘッダーをそのまま使用する既知のプロキシのアドレス。 使用する`KnownProxies`と一致する正確な IP アドレスを指定します。<br><br>既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> の 1 つのエントリを含む`IPAddress.IPv6Loopback`です。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername)です。<br><br>既定値は、`X-Original-For` です。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername)です。<br><br>既定値は、`X-Original-Host` です。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername)です。<br><br>既定値は、`X-Original-Proto` です。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 間に同期するヘッダーの値の数が必要、 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)処理中です。<br><br>既定の ASP.NET Core 1.x は`true`します。 ASP.NET Core 2.0 またはそれ以降の既定値は`false`します。 |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| オプション | 説明 |
| ------ | ----------- |
| AllowedHosts | ホストを制限、`X-Forwarded-Host`ヘッダーに指定された値。<ul><li>序数を無視する case を使用する値が比較されます。</li><li>ポート番号を除外する必要があります。</li><li>リストが空、すべてのホストが許可されます。</li><li>最上位のワイルドカード`*`により、すべての空でないホストします。</li><li>サブドメイン ワイルドカードは許可されますが、ルート ドメインが一致しません。 たとえば、`*.contoso.com`サブドメインに一致`foo.contoso.com`がルート ドメインではなく`contoso.com`です。</li><li>Unicode のホスト名が許可されますに変換されます[Punycode](https://tools.ietf.org/html/rfc3492)照合します。</li><li>[IPv6 アドレス](https://tools.ietf.org/html/rfc4291)角かっこの境界し、である必要があります[従来フォーム](https://tools.ietf.org/html/rfc4291#section-2.2)(たとえば、 `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`)。 IPv6 アドレスは、さまざまな形式の間で論理的に等しいかどうかを確認する特別な大文字と小文字がなく、正規化は行われません。</li><li>許可されているホストの制限に失敗する可能性があります、攻撃者は、サービスによって生成されたリンクを偽装します。</li></ul>既定値は、空[IList\<文字列 >](/dotnet/api/system.collections.generic.ilist-1)です。 |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername)です。<br><br>既定値は、`X-Forwarded-For` です。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | どのフォワーダーを処理するかを識別します。 参照してください、 [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)適用されるフィールドの一覧についてはします。 このプロパティに割り当てられている標準値は<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>します。<br><br>既定値は[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)です。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername)です。<br><br>既定値は、`X-Forwarded-Host` です。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername)です。<br><br>既定値は、`X-Forwarded-Proto` です。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 処理されるヘッダー内のエントリの数を制限します。 設定`null`、制限が、これを無効にする必要がある場合にのみ実行`KnownProxies`または`KnownNetworks`が構成されています。<br><br>既定値は 1 です。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | アドレスから転送されたヘッダーをそのまま使用する既知のプロキシの範囲です。 クラスレス ドメイン間ルーティング (CIDR) 表記を使用して IP の範囲を提供します。<br><br>既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[ip ネットワーク](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> の 1 つのエントリを含む`IPAddress.Loopback`です。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 転送されたヘッダーをそのまま使用する既知のプロキシのアドレス。 使用する`KnownProxies`と一致する正確な IP アドレスを指定します。<br><br>既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> の 1 つのエントリを含む`IPAddress.IPv6Loopback`です。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername)です。<br><br>既定値は、`X-Original-For` です。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername)です。<br><br>既定値は、`X-Original-Host` です。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername)です。<br><br>既定値は、`X-Original-Proto` です。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 間に同期するヘッダーの値の数が必要、 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)処理中です。<br><br>既定の ASP.NET Core 1.x は`true`します。 ASP.NET Core 2.0 またはそれ以降の既定値は`false`します。 |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>シナリオとユース ケース

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>ヘッダーとすべての要求はセキュリティで保護されたときに、転送を追加することはできません。

場合によっては、要求をアプリにプロキシに転送されたヘッダーを追加できない場合があります。 スキームを手動で設定場合は、プロキシを適用するには、すべてのパブリック外部要求が HTTPS である、`Startup.Configure`ミドルウェアの任意の型を使用する前に。

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

環境変数と、開発環境またはステージング環境の他の構成設定は、このコードを無効にすることができます。

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>パスのベースと要求のパスを変更するプロキシを処理します。

一部のプロキシが、パスをそのままの渡しますが、アプリにルーティングされるように削除する必要のある基本パスが正しく機能します。 [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase)ミドルウェアにパスを分割する[HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)とに、アプリの基本パス[HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)です。

場合`/foo`として渡されるプロキシ パスのアプリケーション ベース パスは、 `/foo/api/1`、ミドルウェア セット`Request.PathBase`に`/foo`と`Request.Path`に`/api/1`次のコマンド。

```csharp
app.UsePathBase("/foo");
```

ミドルウェアが逆方向にもう一度呼び出されると、元のパスと基本パスが再適用されます。 ミドルウェアの注文処理の詳細については、次を参照してください。[ミドルウェア](xref:fundamentals/middleware/index)です。

プロキシは、パスを削除します。 場合 (たとえば、転送`/foo/api/1`に`/api/1`)、修正プログラムを選択し、リダイレクトするには、要求のリンク[PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)プロパティ。

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

プロキシは、パスのデータを追加するを使用してリダイレクトとのリンクを解決するパスの一部を破棄して[StartsWithSegments (PathString、PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__)に割り当てると、[パス](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)プロパティ。

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a>トラブルシューティング

ヘッダーは、期待どおりに転送されない、ときに有効に[ログ](xref:fundamentals/logging/index)です。 ログが問題を解決するための十分な情報を提供されない場合は、サーバーが受信した要求ヘッダーを列挙します。 ヘッダーは、インラインのミドルウェアを使用して、アプリの応答に記述できます。

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

転送の X を確認してください * 予期される値を持つサーバー ヘッダーが受信します。 指定されたヘッダーに複数の値がある場合は、逆の順序で右から左へヘッダー ミドルウェアの転送プロセスのヘッダーに注意してください。

要求の元のリモート IP のエントリに一致する必要があります、`KnownProxies`または`KnownNetworks`X-転送の場合は、処理される前に一覧表示します。 これは、信頼されていないプロキシのフォワーダを受け付けないことでヘッダーのスプーフィングを制限します。

## <a name="additional-resources"></a>その他の技術情報

* [マイクロソフト セキュリティ アドバイザリ CVE-2018-0787: ASP.NET Core の昇格の脆弱性](https://github.com/aspnet/Announcements/issues/295)
