---
title: ASP.NET Core で HTTPS を適用します。
author: rick-anderson
description: Web アプリで ASP.NET Core HTTPS/TLS を必要とする方法を示します。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6a16bb2253fcb6e81a294f1c484db1a3e80796e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277271"
---
# <a name="enforce-https-in-aspnet-core"></a>ASP.NET Core で HTTPS を適用します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントでは次の方法について説明します:

* すべての要求に HTTPS を必要とさせる。
* すべての HTTP 要求を HTTPS にリダイレクトさせる。

> [!WARNING]
> 操作を行います**いない**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)機密情報を受信する Web Api にします。 `RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。 API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。 このようなクライアントは、HTTP 経由で情報を送信することがあります。 Web API は次のいずれかの対策を講じるべきです:
>
> * HTTP をリッスンしない。
> * ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。


<a name="require"></a>
## <a name="require-https"></a>HTTPS が必要

::: moniker range=">= aspnetcore-2.1"

すべての ASP.NET Core web アプリケーションが HTTPS のリダイレクトのミドルウェアを呼び出すことをお勧め ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) すべての HTTP 要求を HTTPS にリダイレクトします。

次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

次のコード呼び出し[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)ミドルウェアのオプションを構成します。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

上記の強調表示されたコード。

* セット[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)に`Status307TemporaryRedirect`、これは、既定値です。 実稼働アプリケーションで呼び出す必要があります[UseHsts](#hsts)です。
* 5001 を HTTPS ポートを設定します。 既定値は 443 です。

次のメカニズムでは、ポートが自動的に設定します。

* ミドルウェアを使用してポートを検出できる[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)次の条件を適用する場合。
  - HTTPS エンドポイントを直接 kestrel または HTTP.sys を使用 (Visual Studio のコードのデバッガーでは、アプリの実行にも適用されます)。
  - のみ**1 つの HTTPS ポート**アプリで使用します。
* Visual Studio を使用します。
  - IIS Express、HTTPS 対応があります。
  - *launchSettings.json*設定、 `sslPort` IIS Express 用です。

> [!NOTE]
> (たとえば、IIS、IIS Express) は、リバース プロキシの背後にあるアプリの実行時に`IServerAddressesFeature`は使用できません。 ポートを手動で構成する必要があります。 ポートが設定されていない、ときに要求をリダイレクトされません。

設定して、ポートを構成することができます、します。

* `ASPNETCORE_HTTPS_PORT` 環境変数。
* `http_port` ホストの構成のキー (などを介して*hostsettings.json*またはコマンドライン引数)。
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)です。 5001 にポートを設定する方法を示しています。 前の例を参照してください。

> [!NOTE]
> ポートを設定することが直接使用して URL を設定して、`ASPNETCORE_URLS`環境変数。 環境変数は、サーバーを構成し、ミドルウェア直接、検出されません経由の HTTPS ポート`IServerAddressesFeature`です。

ポートが設定されていない: 場合

* 要求がリダイレクトされません。
* ミドルウェアは、警告を記録します。

> [!NOTE]
> HTTPS のリダイレクトのミドルウェアを使用する代わりに (`UseHttpsRedirection`) は、URL 書き換えミドルウェアを使用する (`AddRedirectToHttps`)。 `AddRedirectToHttps` 設定できます、ステータス コードとポートのリダイレクトを実行するとします。 さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。
>
> HTTPS のリダイレクトのミドルウェアの使用をお勧めを HTTPS にリダイレクトする、追加のリダイレクト ルールを必要としない、ときに (`UseHttpsRedirection`) このトピックで説明します。

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) は HTTPS を必須とさせるために使用します。 `[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。 属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。 次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。 ミドルウェアは、アプリのリダイレクトを実行すると、ステータス コードまたはステータス コードと、ポートを設定することもできます。

グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。 `[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。 新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP 厳密なトランスポート セキュリティ プロトコル (HSTS)

あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP 厳密なトランスポート セキュリティ (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)オプトイン セキュリティ拡張機能を利用、特別な応答ヘッダーを使用して web アプリケーションによって指定されています。 サポートされているブラウザーがこのヘッダーを受け取るし、そのブラウザーから、指定したドメインに HTTP 経由で送信されるすべての通信を防ぐ HTTPS 経由ですべての通信を代わりに送信されます。 ブラウザーでのプロンプトの HTTPS クリックスルーも回避されます。

ASP.NET Core 2.1 以降と HSTS を実装して、`UseHsts`拡張メソッド。 次のコード呼び出し`UseHsts`にアプリがないとき[開発モード](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` お勧めできません開発における HSTS ヘッダーが高いキャッシュ可能なためのブラウザーでします。 既定では、`UseHsts`ローカル ループバック アドレスを除外します。

コード例を次に示します。

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Strict トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。 プリロードされていないの一部、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)が新規インストールで HSTS サイトをプリロードする web ブラウザーでサポートされています。 詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。
* により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)、HSTS ポリシー サブドメインをホストに適用されます。 
* 明示的に 60 日間にする高レベルのトランスポートのセキュリティ ヘッダーの最大継続期間パラメーターを設定します。 設定されていない場合、既定値は 30 日間です。 参照してください、[最大継続期間ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。
* 追加`example.com`を除外するホストの一覧にします。

`UseHsts` 次のループバックのホストを除外します。

* `localhost` : IPv4 ループバック アドレス。
* `127.0.0.1` : IPv4 ループバック アドレス。
* `[::1]` : IPv6 ループバック アドレス。

前の例では、他のホストを追加する方法を示します。
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>オプトアウト HTTPS でプロジェクトの作成

(Visual Studio または dotnet コマンド ライン) から ASP.NET Core 2.1 以降の web アプリケーション テンプレートを使用する[HTTPS リダイレクト](#require)と[HSTS](#hsts)です。 HTTPS は必要ありません、展開にすることができますオプトアウト HTTPS です。 たとえば、ここで HTTPS を処理している外部で、エッジに各ノードで HTTPS を使用して一部のバックエンド サービスは必要ありません。

無効にするは、HTTPS:

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

参照してください[この GitHub 問題](https://github.com/aspnet/Docs/issues/6199)です。

::: moniker-end
