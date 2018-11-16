---
title: ASP.NET Core での Google 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリに Google アカウントのユーザー認証の統合について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: dfda83e1d7cf3c5ff8e31de20c15d468de5d15c0
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708453"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core での Google 外部ログインのセットアップ

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、サンプルの ASP.NET Core 2.0 プロジェクトが作成を使用して、Google + アカウントでサインインするユーザーを有効にする方法、[前のページ](xref:security/authentication/social/index)します。 まず、次の[公式手順](https://developers.google.com/identity/sign-in/web/devconsole-project)Google API コンソールで新しいアプリを作成します。

## <a name="create-the-app-in-google-api-console"></a>Google API コンソールで、アプリを作成します。

* 移動します[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)してサインインします。 Google アカウントがない場合は、使用**より多くのオプション** > **[アカウントを作成する](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** 作成へのリンク。

![Google API コンソール](index/_static/GoogleConsoleLogin.png)

* リダイレクトされます**API Manager ライブラリ**ページ。

![API Manager ライブラリのページ](index/_static/GoogleConsoleSwitchboard.png)

* タップ**作成**を入力し、**プロジェクト名**:

![[新しいプロジェクト] ダイアログ](index/_static/GoogleConsoleNewProj.png)

* ダイアログを受け入れた場合、新しいアプリの機能を選択できるようにライブラリ ページにリダイレクトされます。 検索**Google + API**リストを API 機能を追加するには、そのリンクをクリックします。

![API Manager ライブラリのページ](index/_static/GoogleConsoleChooseApi.png)

* 新しく追加された API のページが表示されます。 タップ**を有効にする**機能で、アプリに Google + 記号を追加します。

![Google + API の API マネージャー ページ](index/_static/GoogleConsoleEnableApi.png)

* API を有効にした後は、タップ**資格情報を作成**シークレットを構成します。

![Google + API の API マネージャー ページ](index/_static/GoogleConsoleGoCredentials.png)

* 選択するオプション
  * **Google+ API**
  * **Web サーバー (例: node.js、Tomcat)** と
  * **ユーザー データ**:

![API マネージャーの資格情報 ページ: 必要なパネルをどのような種類の資格情報をご確認ください](index/_static/GoogleConsoleChooseCred.png)

* タップ**どのような資格情報が必要ですか?** アプリの構成の 2 番目の手順に移動する**OAuth 2.0 クライアント ID を作成**:

![API マネージャーの資格情報 ページ: OAuth 2.0 クライアント ID の作成](index/_static/GoogleConsoleCreateClient.png)

* 1 つの機能 (サインイン)、同じ入力で Google + プロジェクトを作成するため**名前**としてプロジェクトを使用した OAuth 2.0 クライアントの id。

* 開発 URI を入力と`/signin-google`に追加されます、**承認済みのリダイレクト Uri**フィールド (例: `https://localhost:44320/signin-google`)。 このチュートリアルの後半で構成されている Google 認証の要求では自動的に処理`/signin-google`OAuth フローを実装するルート。

> [!NOTE]
> URI セグメント`/signin-google`Google の認証プロバイダーの既定のコールバックとして設定されます。 既定のコールバック URI を変更するには、継承を使用して、Google の認証ミドルウェアを構成するときに[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)クラス。

* Tab キーを追加する、**承認済みのリダイレクト Uri**エントリ。

* タップ**クライアント ID の作成**、3 番目の手順に移動する **、OAuth 2.0 同意画面設定**:

![API マネージャーの資格情報 ページ: OAuth 2.0 同意画面設定](index/_static/GoogleConsoleAddCred.png)

* 入力、パブリックを対象に**電子メール アドレス**と**製品名**Google + にサインインするユーザーを要求するときに、アプリに表示します。 追加のオプションが 利用可能な**その他のカスタマイズ オプション**します。

* タップ**続行**最後の手順を続行する**資格情報をダウンロード**:

![API マネージャーの資格情報 ページ: 資格情報のダウンロード](index/_static/GoogleConsoleFinish.png)

* タップ**ダウンロード**アプリケーションのシークレットの JSON ファイルを保存して**完了**新しいアプリの作成を完了します。

* サイトをデプロイするときに、再アクセスする必要があります、 **Google コンソール**し、新しいパブリック url を登録します。

## <a name="store-google-clientid-and-clientsecret"></a>ストア Google ClientID と ClientSecret

Google のような機密性の高い設定リンク`Client ID`と`Client Secret`アプリケーションの構成を使用して、 [Secret Manager](xref:security/app-secrets)します。 このチュートリアルの目的で、名前トークン`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`します。

前の手順でダウンロードした JSON ファイルでこれらのトークンの値が見つかります`web.client_id`と`web.client_secret`します。

## <a name="configure-google-authentication"></a>Google 認証を構成します。

::: moniker range=">= aspnetcore-2.0"

Google のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

このチュートリアルで使用するプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)パッケージがインストールされています。

* Visual Studio 2017 では、このパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**します。
* で .NET Core CLI をインストールするには、プロジェクト ディレクトリで、次を実行します。

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Google のミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

参照してください、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google 認証でサポートされる構成オプションの詳細について、API リファレンス。 これは、ユーザーに関するさまざまな情報を要求を使用できます。

## <a name="sign-in-with-google"></a>Google のサインイン

アプリケーションを実行し、をクリックして**ログイン**します。 Google でサインインするためのオプションが表示されます。

![Microsoft Edge で実行されている web アプリケーション: ユーザーが認証されていません。](index/_static/DoneGoogle.png)

Google をクリックすると認証に Google にリダイレクトされます。

![Google 認証のダイアログ ボックス](index/_static/GoogleLogin.png)

Google の資格情報を入力すると、し、リダイレクトされます、電子メールを設定する web サイトにフェールバックします。

Google の資格情報を使用してログインしました。

![Microsoft Edge で実行されている web アプリケーション: 認証されたユーザー](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>トラブルシューティング

* 表示された場合、`403 (Forbidden)`開発モード (または同じエラーでデバッガーを中断) で実行していることを確認すると、独自のアプリからのエラー ページ**Google + API**が有効になって、 **API Manager ライブラリ**記載されている手順に従って[このページで以前](#create-the-app-in-google-api-console)します。 サインインが機能しないすべてのエラー通知が届かない場合は、問題をデバッグしやすくする開発モードに切り替えます。
* **ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*します。 このチュートリアルで使用するプロジェクト テンプレートによりこれが行われるようになります。
* 最初の移行を適用することで、サイト データベースが作成されていない場合になります*要求の処理中にデータベース操作が失敗しました*エラー。 タップ**適用移行**データベースを作成し、エラーを引き続き更新します。

## <a name="next-steps"></a>次の手順

* この記事では、Google で認証する方法を示しました。 記載されているその他のプロバイダーで認証する同様のアプローチを利用できる、[前のページ](xref:security/authentication/social/index)します。

* リセットする必要があります、web サイトを Azure web アプリを発行すると、 `ClientSecret` Google API コンソールでします。

* 設定、`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`として、Azure portal でアプリケーションの設定。 構成システムは、環境変数からキーの読み取りを設定します。
