---
title: ASP.NET Core で Google 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリケーションに Google アカウントのユーザー認証の統合について説明します。
ms.author: riande
ms.date: 08/02/2017
uid: security/authentication/google-logins
ms.openlocfilehash: c5b6c992e134a2c4f0314d9d6e0465e6228c54ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274912"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core で Google 外部ログインのセットアップ

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、Google + アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](xref:security/authentication/social/index)です。 まず、次の[公式手順](https://developers.google.com/identity/sign-in/web/devconsole-project)Google API コンソールで新しいアプリを作成します。

## <a name="create-the-app-in-google-api-console"></a>Google API コンソールでのアプリを作成します。

* 移動[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)してサインインします。 Google アカウントがない場合を使用して**より多くのオプション** > **[アカウントを作成する](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** リンクを作成するのには。

![Google API コンソール](index/_static/GoogleConsoleLogin.png)

* リダイレクトされます**API マネージャー ライブラリ**ページ。

![ライブラリの API マネージャー ページ](index/_static/GoogleConsoleSwitchboard.png)

* タップ**作成**を入力し、**プロジェクト名**:

![[新しいプロジェクト] ダイアログ](index/_static/GoogleConsoleNewProj.png)

* 受け入れた場合、ダイアログ ボックスが、新しいアプリの機能を選択することができます、ライブラリのページにリダイレクトします。 検索**Google + API** API 機能を追加するには、そのリンクをクリックしてリストに。

![ライブラリの API マネージャー ページ](index/_static/GoogleConsoleChooseApi.png)

* 新しく追加された API のページが表示されます。 タップ**を有効にする**をアプリの機能で Google + 記号を追加します。

![API マネージャー Google + API ページ](index/_static/GoogleConsoleEnableApi.png)

* API を有効にした後は、タップ**資格情報を作成する**シークレットを構成します。

![API マネージャー Google + API ページ](index/_static/GoogleConsoleGoCredentials.png)

* 選択するオプション
   * **Google+ API**
   * **Web サーバー (例: node.js、Tomcat)**、および
   * **ユーザー データ**:

![API のマネージャーの資格情報 ページ: どのような種類の資格情報を調べるには、パネルが必要があります](index/_static/GoogleConsoleChooseCred.png)

* タップ**資格情報が必要ですか?** アプリの構成の 2 番目の手順を行います**OAuth 2.0 クライアント ID の作成**:

![API のマネージャーの資格情報 ページ: OAuth 2.0 クライアント ID の作成](index/_static/GoogleConsoleCreateClient.png)

* 1 つの機能 (サインイン) を入力するには同じで Google + プロジェクトを作成するため**名前**OAuth 2.0 クライアント ID のプロジェクトを使用したものとします。

* 開発 URI を入力と`/signin-google`に追加されます、**承認されているリダイレクト Uri**フィールド (例: `https://localhost:44320/signin-google`)。 このチュートリアルで後で構成されている Google の認証はで、要求を自動的に処理`/signin-google`OAuth フローを実装するルート。

> [!NOTE]
> URI セグメント`/signin-google`は Google の認証プロバイダーの既定のコールバックとして設定します。 既定のコールバック URI を変更するには、継承を使用して Google の認証ミドルウェアの構成中に[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)クラスです。

* Tab キーを追加する、**承認されているリダイレクト Uri**エントリです。

* タップ**クライアント ID を作成する**、3 番目の手順を行います**OAuth 2.0 同意画面設定**:

![API のマネージャーの資格情報 ページ: OAuth 2.0 同意画面の設定](index/_static/GoogleConsoleAddCred.png)

* 入力、公開されて**電子メール アドレス**と**製品名**Google + にサインインするユーザーを要求するときに、アプリに表示します。 追加のオプションを 利用可能な**より多くのカスタマイズ オプション**です。

* タップ**続行**最後の手順を続行する**資格情報をダウンロード**:

![API のマネージャーの資格情報 ページ: 資格情報をダウンロード](index/_static/GoogleConsoleFinish.png)

* タップ**ダウンロード**アプリケーション シークレットの JSON ファイルを保存して**完了**新しいアプリの作成を完了します。

* サイトを展開するときにを再表示する必要があります、 **Google コンソール**し、新しいパブリック url を登録します。

## <a name="store-google-clientid-and-clientsecret"></a>ストア Google ClientID および ClientSecret

Google のように機密設定をリンク`Client ID`と`Client Secret`、アプリケーションを使用して構成する、[シークレット Manager](xref:security/app-secrets)です。 このチュートリアルの目的で、名前トークン`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`です。

前の手順でダウンロードした JSON ファイルでこれらのトークンの値が見つかります`web.client_id`と`web.client_secret`です。

## <a name="configure-google-authentication"></a>Google 認証を構成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

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

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)パッケージがインストールされています。

* Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。
* .NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

内で Google ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

参照してください、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google 認証でサポートされる構成オプションの詳細についての API リファレンスです。 ユーザーに関するさまざまな情報を要求するために使用できます。

## <a name="sign-in-with-google"></a>Google でサインイン

アプリケーションを実行し、をクリックして**ログイン**です。 Google でサインインするオプションが表示されます。

![Microsoft Edge で実行されている web アプリケーション: ユーザーが認証されません](index/_static/DoneGoogle.png)

Google をクリックすると、認証用 Google にリダイレクトされます。

![Google 認証ダイアログ ボックス](index/_static/GoogleLogin.png)

Google の資格情報を入力すると、しにリダイレクトされます戻る、web サイト、メールを設定することができます。

これで Google の資格情報を使用してをログインしています。

![Microsoft Edge で実行されている web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a>トラブルシューティング

* 表示された場合、`403 (Forbidden)`開発モード (または同じのエラーのため、デバッガーにブレーク) で実行されていることを確認時に、独自のアプリケーションからのエラー ページ**Google + API**が有効になって、 **API マネージャー ライブラリ**表示されている手順に従って、[このページで以前](#create-the-app-in-google-api-console)です。 サインインがそれでもエラーが表示されていない場合は、問題のデバッグの簡略化を行うには開発モードに切り替えます。
* **ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試行することになります*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。 このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。
* 最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。 タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。

## <a name="next-steps"></a>次の手順

* この記事では、Google との認証方法を示しました。 記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](xref:security/authentication/social/index)です。

* リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ClientSecret` Google API コンソールでします。

* 設定、`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`として Azure ポータルでのアプリケーション設定。 構成システムは、環境変数からキーを読み取れませんを設定します。
