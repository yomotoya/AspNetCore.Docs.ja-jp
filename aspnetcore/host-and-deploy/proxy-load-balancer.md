---
title: プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する
author: guardrex
description: プロキシ サーバーとロード バランサーの背後にホストされているアプリの構成について説明します。このような構成では、要求の重要な情報がわからなくなることがよくあります。
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
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740947"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する

著者: [Luke Latham](https://github.com/guardrex)、[Chris Ross](https://github.com/Tratcher)

ASP.NET Core の推奨される構成では、アプリは IIS/ASP.NET Core モジュール、Nginx、または Apache を使ってホストされます。 プロキシ サーバー、ロード バランサー、および他のネットワーク アプライアンスにより、要求に関する情報が、アプリに到達する前にわからなくなることがよくあります。

* HTTPS 要求が HTTP によってプロキシされると、元のスキーム (HTTPS) は失われ、ヘッダーで転送される必要があります。
* アプリは、インターネット上または社内ネットワーク上の本来の送信元ではなく、プロキシから要求を受信するため、送信元クライアントの IP アドレスもヘッダーで転送される必要があります。

この情報は、リダイレクト、認証、リンクの生成、ポリシーの評価、クライアントの位置情報など、要求の処理で重要になる可能性があります。

## <a name="forwarded-headers"></a>転送されるヘッダー

慣例によりは、プロキシは HTTP ヘッダーで情報を転送します。

| Header | 説明 |
| ------ | ----------- |
| X-Forwarded-For | 要求を開始したクライアントと、プロキシ チェーン内の後続のプロキシに関する情報を保持します。 このパラメーターは、IP アドレス (および、必要に応じてポート番号) を含むことがあります。 プロキシ サーバーのチェーンにおいては、最初のパラメーターが、要求を最初に行ったクライアントを示します。 その後に、後続のプロキシの識別子が指定されています。 チェーン内の最後のプロキシは、パラメーターのリストには含まれません。 最後のプロキシの IP アドレスとオプションのポート番号は、トランスポート層のリモート IP アドレスとして入手できます。 |
| X-Forwarded-Proto | 開始スキーム (HTTP/HTTPS) の値です。 要求が複数のプロキシを通過している場合、値はスキームのリストである場合もあります。 |
| X-Forwarded-Host | ホスト ヘッダー フィールドの元の値です。 通常、プロキシはホスト ヘッダーを変更しません。 プロキシにおいてホスト ヘッダーが既知の適切な値であることが検証されない、または既知の適切な値に制限されないシステムに影響を与える、特権の昇格脆弱性については、[マイクロソフト セキュリティ アドバイザリ CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) をご覧ください。 |

[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) パッケージの Forwarded Headers Middleware は、これらのヘッダーを読み取り、[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext) の関連するフィールドに設定します。 

ミドルウェアの更新:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; `X-Forwarded-For` ヘッダーの値を使って設定します。 追加の設定は、ミドルウェアが `RemoteIpAddress` を設定する方法に影響を与えます。 詳しくは、「[Forwarded Headers Middleware のオプション](#forwarded-headers-middleware-options)」をご覧ください。
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; `X-Forwarded-Proto` ヘッダーの値を使って設定します。
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; `X-Forwarded-Host` ヘッダーの値を使って設定します。

すべてのネットワーク アプライアンスが追加構成なしで `X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを追加するわけではないことに注意してください。 プロキシされた要求がアプリに届いたときにこれらのヘッダーが含まれていない場合は、アプライアンスの製造元のガイダンスを参照してください。

Forwarded Headers Middleware の[既定の設定](#forwarded-headers-middleware-options)は構成できます。 既定の設定は次のとおりです。

* アプリと要求のソースの間には、"*1 つのプロキシ*" だけが存在します。
* 既知のプロキシと既知のネットワークに対しては、ループバック アドレスのみが構成されています。

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express と ASP.NET Core モジュール

アプリが IIS と ASP.NET Core モジュールの背後で実行されると、IIS Integration Middleware は Forwarded Headers Middleware を既定で有効にします。 転送されるヘッダーの信頼に関する問題のため (たとえば、[IP スプーフィング](https://www.iplocation.net/ip-spoofing))、Forwarded Headers Middleware はアクティブ化されると最初に、ASP.NET Core モジュールに固有の制限された構成を使って、ミドルウェア パイプライン内で実行します。 ミドルウェアは、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送するように構成され、単一の localhost プロキシに制限されます。 追加の構成が必要な場合は、「[Forwarded Headers Middleware のオプション](#forwarded-headers-middleware-options)」をご覧ください。

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーの他のシナリオ

IIS Integration Middleware が使われていない場合は、Forwarded Headers Middleware は既定では有効になりません。 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) を含む転送されたヘッダーをアプリで処理するためには、Forwarded Headers Middleware を有効にする必要があります。 ミドルウェアを有効にした後、ミドルウェアに対して [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が指定されていない場合の既定の [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) は [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) です。

`Startup.ConfigureServices` において `X-Forwarded-For` ヘッダーと `X-Forwarded-Proto` ヘッダーを転送するには、[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) を使ってミドルウェアを構成します。 他のミドルウェアを呼び出す前に、`Startup.Configure` 内で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。

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
> [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が `Startup.ConfigureServices` において指定されていない場合、または [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_) を使って拡張メソッドに直接渡されない場合、転送される既定のヘッダーは [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) です。 転送するヘッダーで [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) プロパティが構成されている必要があります。

## <a name="forwarded-headers-middleware-options"></a>Forwarded Headers Middleware のオプション

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) は、Forwarded Headers Middleware の動作を制御します。

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
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-For` です。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | 処理する必要があるフォワーダーを識別します。 適用されるフィールドの一覧については、「[ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)」(ForwardedHeaders 列挙型) をご覧ください。 このプロパティに割り当てられる標準値は、<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code> です。<br><br>既定値は [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) です。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-Host` です。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-Proto` です。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 処理されるヘッダー内のエントリの数を制限します。 制限を無効にするには `null` に設定しますが、これは `KnownProxies` または `KnownNetworks` が構成されている場合にのみ行う必要があります。<br><br>既定値は 1 です。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | 受け付けるヘッダーの転送元である既知のプロキシのアドレス範囲です。 クラスレス ドメイン間ルーティング (CIDR) の表記を使って、IP の範囲を指定します。<br><br>既定値は、`IPAddress.Loopback` に対する単一のエントリを含む [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> です。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 受け付けるヘッダーの転送元である既知のプロキシのアドレスです。 IP アドレスの正確な一致を指定するには、`KnownProxies` を使います。<br><br>既定値は、`IPAddress.IPv6Loopback` に対する単一のエントリを含む [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> です。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-For` です。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-Host` です。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-Proto` です。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 処理対象の [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) の間でヘッダー値の数が同期していることを要求します。<br><br>ASP.NET Core 1.x での既定値は `true` です。 ASP.NET Core 2.0 以降での既定値は `false` です。 |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| オプション | 説明 |
| ------ | ----------- |
| AllowedHosts | `X-Forwarded-Host` ヘッダーによるホストを指定した値に制限します。<ul><li>値は、大文字と小文字を無視して序数で比較されます。</li><li>ポート番号は除外されている必要があります。</li><li>リストが空の場合、すべてのホストが許可されます。</li><li>最上位のワイルドカード `*` は、すべての空でないホストを許可します。</li><li>サブドメインのワイルドカードは許可されますが、ルート ドメインとは一致しません。 たとえば、`*.contoso.com` はサブドメイン `foo.contoso.com` と一致しますが、ルート ドメイン `contoso.com` とは一致しません。</li><li>Unicode のホスト名は許可されますが、一致のために [Punycode](https://tools.ietf.org/html/rfc3492) に変換されます。</li><li>[IPv6 アドレス](https://tools.ietf.org/html/rfc4291)は境界の角かっこを含む必要があり、[従来の形式](https://tools.ietf.org/html/rfc4291#section-2.2) (例: `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`) になっている必要があります。 IPv6 アドレスは、異なる形式間で論理的な等価性を調べるために特別な大文字小文字にはされず、正規化は行われません。</li><li>許可されるホストの制限に失敗すると、サービスによって生成されたリンクを攻撃者が偽装する可能性があります。</li></ul>既定の値は、空の [IList\<string>](/dotnet/api/system.collections.generic.ilist-1) です。 |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-For` です。 |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | 処理する必要があるフォワーダーを識別します。 適用されるフィールドの一覧については、「[ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)」(ForwardedHeaders 列挙型) をご覧ください。 このプロパティに割り当てられる標準値は、<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code> です。<br><br>既定値は [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) です。 |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-Host` です。 |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Forwarded-Proto` です。 |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | 処理されるヘッダー内のエントリの数を制限します。 制限を無効にするには `null` に設定しますが、これは `KnownProxies` または `KnownNetworks` が構成されている場合にのみ行う必要があります。<br><br>既定値は 1 です。 |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | 受け付けるヘッダーの転送元である既知のプロキシのアドレス範囲です。 クラスレス ドメイン間ルーティング (CIDR) の表記を使って、IP の範囲を指定します。<br><br>既定値は、`IPAddress.Loopback` に対する単一のエントリを含む [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> です。 |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | 受け付けるヘッダーの転送元である既知のプロキシのアドレスです。 IP アドレスの正確な一致を指定するには、`KnownProxies` を使います。<br><br>既定値は、`IPAddress.IPv6Loopback` に対する単一のエントリを含む [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> です。 |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-For` です。 |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-Host` です。 |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername) によって指定されているヘッダーではなく、このプロパティによって指定されているヘッダーを使います。<br><br>既定値は、`X-Original-Proto` です。 |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | 処理対象の [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) の間でヘッダー値の数が同期していることを要求します。<br><br>ASP.NET Core 1.x での既定値は `true` です。 ASP.NET Core 2.0 以降での既定値は `false` です。 |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>シナリオとユース ケース

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>転送されるヘッダーを追加することができず、すべての要求が安全な場合

アプリにプロキシされる要求に、転送されるヘッダーを追加できない場合があります。 すべてのパブリック外部要求が HTTPS を使うことをプロキシが強制している場合、すべての種類のミドルウェアを使う前に、`Startup.Configure` でスキームを手動で設定できます。

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

このコードは、開発環境またはステージング環境の環境変数または他の構成設定によって、無効にすることができます。

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>要求のパスを変更するパス ベースとプロキシを処理する

一部のプロキシでは、パスはそのまま渡されますが、ルーティングが正しく機能するためには削除する必要のあるアプリ ベース パスが含まれます。 [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) ミドルウェアは、パスを [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) と [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) へのアプリ ベース パスに分割します。

`/foo` が `/foo/api/1` として渡されるプロキシ パスのアプリ ベース パスである場合、ミドルウェアは次のコマンドで `Request.PathBase` を `/foo`に、`Request.Path` を `/api/1` に設定します。

```csharp
app.UsePathBase("/foo");
```

ミドルウェアが逆方向にもう一度呼び出されると、元のパスとパス ベースが再度適用されます。 ミドルウェアの処理の順序について詳しくは、「[ASP.NET Core のミドルウェア](xref:fundamentals/middleware/index)」をご覧ください。

プロキシがパスをトリミングする場合は (たとえば、`/foo/api/1` を `/api/1` に転送)、要求の [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) プロパティを設定することによって、リダイレクトとリンクを修正します。

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

プロキシがパス データを追加している場合は、[StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) を使用して [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) プロパティに割り当てることで、パスの一部を破棄してリダイレクトとリンクを修正します。

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

ヘッダーが意図したとおりに転送されない場合は、[ログ](xref:fundamentals/logging/index) を有効にします。 ログで問題のトラブルシューティングに十分な情報が提供されない場合は、サーバーが受信した要求ヘッダーを列挙します。 ヘッダーは、インライン ミドルウェアを使ってアプリの応答に書き込むことができます。

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

サーバーが予期される値を持つ X-Forwarded-* ヘッダーを受け取ることを確認します。 特定のヘッダーに複数の値がある場合、Forwarded Headers Middleware は右から左の逆の順序でヘッダーを処理することに注意してください。

X-Forwarded-For が処理される前に、要求の元のリモート IP アドレスが `KnownProxies` リストまたは `KnownNetworks` リストのエントリと一致する必要があります。 これにより、信頼されないプロキシからの転送を受け付けないことで、ヘッダーのスプーフィングを制限します。

## <a name="additional-resources"></a>その他の技術情報

* [Microsoft セキュリティ アドバイザリ CVE-2018-0787: ASP.NET Core の特権の昇格脆弱性](https://github.com/aspnet/Announcements/issues/295)
