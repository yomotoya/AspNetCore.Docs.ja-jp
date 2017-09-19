---
title: "ASP.NET Core で Google 外部ログインのセットアップ"
author: rick-anderson
description: "ASP.NET Core で Google 外部ログインのセットアップ"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 7e37a8af4ae5a957483fa5f4a89ea4e8999a3d1d
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a>ASP.NET Core で Google の認証を構成します。

<a name=security-authentication-google-logins></a>

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、Google + アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](index.md)です。 まず、次の[公式手順](https://developers.google.com/identity/sign-in/web/devconsole-project)Google API コンソールで新しいアプリを作成します。

## <a name="create-the-app-in-google-api-console"></a>Google API コンソールでのアプリを作成します。

* 移動[https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library)してサインインします。 Google アカウントがない場合を使用して**より多くのオプション** > **[アカウントを作成する](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**リンクを作成するのには。

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
   * **Google + API**
   * **Web サーバー (例: node.js、Tomcat)**、および
   * **ユーザー データ**:

![API のマネージャーの資格情報 ページ: どのような種類の資格情報を調べるには、パネルが必要があります](index/_static/GoogleConsoleChooseCred.png)

* タップ**資格情報が必要ですか?**アプリの構成の 2 番目の手順を行います**OAuth 2.0 クライアント ID の作成**:

![API のマネージャーの資格情報 ページ: OAuth 2.0 クライアント ID の作成](index/_static/GoogleConsoleCreateClient.png)

* 1 つの機能 (サインイン) を入力するには同じで Google + プロジェクトを作成するため**名前**OAuth 2.0 クライアント ID のプロジェクトを使用したものとします。

* 開発 URI を入力と*/signin-google*に追加されます、**承認されているリダイレクト Uri**フィールド (例: `https://localhost:44320/signin-google`)。 このチュートリアルで後で構成されている Google の認証はで、要求を自動的に処理*/signin-google* OAuth フローを実装するルート。

* Tab キーを追加する、**承認されているリダイレクト Uri**エントリです。

* タップ**クライアント ID を作成する**、3 番目の手順を行います**OAuth 2.0 同意画面設定**:

![API のマネージャーの資格情報 ページ: OAuth 2.0 同意画面の設定](index/_static/GoogleConsoleAddCred.png)

* 入力、公開されて**電子メール アドレス**と**製品名**Google + にサインインするユーザーを要求するときに、アプリに表示します。 追加のオプションを 利用可能な**より多くのカスタマイズ オプション**です。

* タップ**続行**最後の手順を続行する**資格情報をダウンロード**:

![API のマネージャーの資格情報 ページ: 資格情報をダウンロード](index/_static/GoogleConsoleFinish.png)

* タップ**ダウンロード**アプリケーション シークレットの JSON ファイルを保存して**完了**新しいアプリの作成を完了します。

* サイトを展開するときにを再表示する必要があります、 **Google コンソール**し、新しいパブリック url を登録します。

## <a name="store-google-clientid-and-clientsecret"></a>ストア Google ClientID および ClientSecret

Google のように機密設定をリンク`Client ID`と`Client Secret`、アプリケーションを使用して構成する、[シークレット Manager](../../app-secrets.md)です。 このチュートリアルの目的で、名前トークン`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`です。

前の手順でダウンロードした JSON ファイルでこれらのトークンの値が見つかります`web.client_id`と`web.client_secret`です。

## <a name="configure-google-authentication"></a>Google 認証を構成します。

このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)パッケージがインストールされています。

 * Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。
 * .NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Google のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

`AddAuthentication`メソッドのみ呼び出されます一度に複数の認証プロバイダーを追加する場合にします。 後続の呼び出しのいずれかの以前に構成をオーバーライドする可能性のある[AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)プロパティです。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

内で Google ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

参照してください、 [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) Google 認証でサポートされる構成オプションの詳細についての API リファレンスです。 ユーザーに関するさまざまな情報を要求するために使用できます。

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
* **ASP.NET Core 2.x のみ:**場合の Id が呼び出すことによって構成されていない`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。 このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。
* 最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。 タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。

## <a name="next-steps"></a>次のステップ

* この記事では、Google との認証方法を示しました。 記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](index.md)です。

* リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ClientSecret` Google API コンソールでします。

* 設定、`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`として Azure ポータルでのアプリケーション設定。 構成システムは、環境変数からキーを読み取れませんを設定します。
