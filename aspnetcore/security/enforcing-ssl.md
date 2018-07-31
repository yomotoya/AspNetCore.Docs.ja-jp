---
title: ASP.NET Core での HTTPS を適用します。
author: rick-anderson
description: Web アプリに ASP.NET Core では、HTTPS や TLS を必要とする方法を示します。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a4ab91ef23a798c919a23a44f5a050bd3c09d56a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356689"
---
# <a name="enforce-https-in-aspnet-core"></a>ASP.NET Core での HTTPS を適用します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントでは次の方法について説明します:

* すべての要求に HTTPS を必要とさせる。
* すべての HTTP 要求を HTTPS にリダイレクトさせる。

> [!WARNING]
> **いない**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)機密情報を受け取る Web Api で。 `RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。 API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。 このようなクライアントは、HTTP 経由で情報を送信することがあります。 Web API は次のいずれかの対策を講じるべきです:
>
> * HTTP をリッスンしない。
> * ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。


<a name="require"></a>
## <a name="require-https"></a>HTTPS が必要です。

::: moniker range=">= aspnetcore-2.1"

すべての ASP.NET Core web アプリは、HTTPS のリダイレクトのミドルウェアを呼び出すことをお勧めします。 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) すべての HTTP 要求を HTTPS にリダイレクトします。

次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

上記の強調表示されたコード。

* 既定値を使用して[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`)。 実稼働アプリを呼び出す必要があります[UseHsts](#hsts)します。
* 既定値を使用して[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443)。

次のコード呼び出し[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)ミドルウェアのオプションを構成します。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

上記の強調表示されたコード。

* セット[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)に`Status307TemporaryRedirect`、これは、既定値。 実稼働アプリを呼び出す必要があります[UseHsts](#hsts)します。
* HTTPS ポートを 5001 に設定します。 既定値には 443 です。

次のメカニズムでは、自動的にポートを設定します。

* ミドルウェアを使用したポートを検出できる[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)次の条件が適用されます。
  - Kestrel または HTTP.sys を使用して HTTPS エンドポイントを直接持つ (Visual Studio Code のデバッガーで、アプリを実行中にも適用されます)。
  - のみ**1 つの HTTPS ポート**アプリによって使用されます。
* Visual Studio が使用されます。
  - IIS Express には、HTTPS を有効になっているがあります。
  - *launchSettings.json*設定、 `sslPort` IIS Express 用です。

> [!NOTE]
> アプリを実行したリバース プロキシ (たとえば、IIS、IIS Express) の背後にあるときに`IServerAddressesFeature`は使用できません。 ポートを手動で構成する必要があります。 ポートが設定されていないときに要求をリダイレクトされません。

設定して、ポートを構成することができます、 [https_port Web ホストの構成設定](xref:fundamentals/host/web-host#https-port):

**キー**: https_port**型**:*文字列*
**既定**: 既定値が設定されていません。
**使用して設定**: `UseSetting` 
**環境変数**: `<PREFIX_>HTTPS_PORT` (プレフィックスは`ASPNETCORE_`Web ホストを使用する場合)。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> ポート構成できます直接の URL を設定して、`ASPNETCORE_URLS`環境変数。 環境変数は、サーバーを構成し、ミドルウェア直接を検出しない経由で HTTPS ポート`IServerAddressesFeature`します。

ポートが設定されていない: 場合

* 要求はリダイレクトされません。
* ミドルウェアは、警告を記録します。

> [!NOTE]
> HTTPS のリダイレクトのミドルウェアを使用する代わりに (`UseHttpsRedirection`) は、URL リライト ミドルウェアを使用する (`AddRedirectToHttps`)。 `AddRedirectToHttps` 設定こともできます、ステータス コードとポートのリダイレクトを実行するとします。 さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。
>
> HTTPS リダイレクト ミドルウェアの使用をお勧めときに、追加のリダイレクト ルールを必要としないを HTTPS にリダイレクトする、(`UseHttpsRedirection`) このトピックで説明します。

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) は HTTPS を必須とさせるために使用します。 `[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。 属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

上記の強調表示されたコードでは、すべての要求を使用して、必要があります`HTTPS`。 したがって、HTTP 要求は無視されます。 次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。 ミドルウェアは、アプリのリダイレクトを実行すると、状態コードまたは状態コードと、ポートを設定することもできます。

グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。 `[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。 新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport Security プロトコル (HSTS)

あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)は特別な応答ヘッダーを使用して web アプリケーションによって指定されるオプトイン セキュリティ拡張機能です。 サポートされているブラウザーがこのヘッダーを受け取るとは、代わりにすべての通信を HTTPS 経由で送信がもとそのブラウザーは指定されたドメインに HTTP 経由で送信される通信ができなくなります。 HTTPS クリックスルー ブラウザーでのプロンプトも回避されます。

ASP.NET Core 2.1 以降で HSTS を実装する、`UseHsts`拡張メソッド。 次のコード呼び出し`UseHsts`でアプリができないときに[開発モード](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` お勧めしません開発 HSTS ヘッダーがキャッシュ可能であるためのブラウザーでします。 既定では、`UseHsts`ローカル ループバック アドレスを除外します。

コード例を次に示します。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Strict-トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。 プリロードされていないの一部、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)、HSTS サイトは新規インストールを事前に web ブラウザーでがサポートされています。 詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。
* により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)サブドメインのホストに HSTS ポリシーを適用します。 
* 明示的に 60 日間にする高レベルのトランスポート セキュリティ ヘッダーの最長有効期間パラメーターを設定します。 既定値は 30 日間を設定しない場合。 参照してください、[最長ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。
* 追加`example.com`を除外するホストの一覧にします。

`UseHsts` 次のループバック ホストを除外するには。

* `localhost` IPv4 ループバック アドレス。
* `127.0.0.1` IPv4 ループバック アドレス。
* `[::1]` IPv6 ループバック アドレス。

前の例では、ホストを追加する方法を示します。
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>オプトアウト HTTPS のプロジェクトを作成

(Visual Studio または dotnet コマンド ライン) から ASP.NET Core 2.1 以降の web アプリケーション テンプレートを使用する[HTTPS へのリダイレクト](#require)と[HSTS](#hsts)します。 HTTPS を必要としない展開ではオプトアウトする HTTPS の。 たとえば、一部のバックエンド サービスで HTTPS が処理される外部でエッジに HTTPS を使用して、各ノードでは必要ありません。

オプトアウトの HTTPS。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

オフにして、 **HTTPS の構成**チェック ボックスをオンします。

![エンティティ図](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

`--no-https` オプションを使用します。 次に例を示します。

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Docker の開発者の証明書をセットアップする方法

参照してください[この GitHub の問題](https://github.com/aspnet/Docs/issues/6199)します。

::: moniker-end
