---
title: ASP.NET Core での twitter 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリに Twitter アカウントのユーザー認証の統合について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708427"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>ASP.NET Core での twitter 外部ログインのセットアップ

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ユーザーに有効にする方法[、Twitter アカウントでサインイン](https://dev.twitter.com/web/sign-in/desktop-browser)で作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、[前のページ](xref:security/authentication/social/index)します。

## <a name="create-the-app-in-twitter"></a>Twitter でアプリケーションを作成します。

* 移動します[ https://apps.twitter.com/ ](https://apps.twitter.com/)してサインインします。 Twitter アカウントがない場合は、使用、 **[今すぐサインアップ](https://twitter.com/signup)** リンクを作成します。 サインインした後、**アプリケーション管理**ページが表示されます。

  ![Microsoft Edge で開いている twitter アプリケーションの管理](index/_static/TwitterAppManage.png)

* タップ**Create New App**アプリケーション記入と**名前**、**説明**とパブリック**web サイト**URI (このことは一時的なものまで、ドメイン名を登録)。

  ![アプリケーション ページを作成します。](index/_static/TwitterCreate.png)

* 開発 URI を入力と`/signin-twitter`に追加されます、**有効な OAuth リダイレクト Uri**フィールド (例: `https://localhost:44320/signin-twitter`)。 このチュートリアルの後半で構成されている Twitter 認証の構成は自動的に要求を処理`/signin-twitter`OAuth フローを実装するルート。

  > [!NOTE]
  > URI セグメント`/signin-twitter`Twitter の認証プロバイダーの既定のコールバックとして設定されます。 既定のコールバック URI を変更するには、継承を使用して、Twitter 認証ミドルウェアを構成するときに[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)クラス。

* フォームの残りの部分を入力し、タップ**Twitter アプリケーションを作成**です。 新しいアプリケーションの詳細が表示されます。

  ![[アプリケーション] ページの [詳細] タブ](index/_static/TwitterAppDetails.png)

* サイトをデプロイするときに、再アクセスする必要があります、**アプリケーション管理**ページし、新しいパブリック URI を登録します。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Twitter ConsumerKey ConsumerSecret を格納します。

Twitter などの機密設定をリンク`Consumer Key`と`Consumer Secret`アプリケーションの構成を使用して、 [Secret Manager](xref:security/app-secrets)します。 このチュートリアルの目的で、名前トークン`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`します。

これらのトークンで確認できます、 **Keys and Access Tokens**新しい Twitter アプリケーションを作成した後タブ。

![キーとアクセス トークン タブ](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter 認証を構成します。

このチュートリアルで使用するプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)パッケージが既にインストールされています。

* Visual Studio 2017 では、このパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**します。
* で .NET Core CLI をインストールするには、プロジェクト ディレクトリで、次を実行します。

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Twitter サービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Twitter ミドルウェア内の追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

参照してください、 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 認証でサポートされる構成オプションの詳細について、API リファレンス。 これは、ユーザーに関するさまざまな情報を要求を使用できます。

## <a name="sign-in-with-twitter"></a>Sign in with Twitter

アプリケーションを実行し、をクリックして**ログイン**します。 Twitter でサインインするためのオプションが表示されます。

![Web アプリケーション: ユーザーが認証されていません。](index/_static/DoneTwitter.png)

クリックすると**Twitter**認証の Twitter にリダイレクトします。

![Twitter 認証ページ](index/_static/TwitterLogin.png)

Twitter の資格情報を入力した後、電子メールを設定する web サイトにリダイレクトされます。

これで、Twitter の資格情報を使用してをログインしています。

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>トラブルシューティング

* **ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*します。 このチュートリアルで使用するプロジェクト テンプレートによりこれが行われるようになります。
* 最初の移行を適用することで、サイト データベースが作成されていない場合になります*要求の処理中にデータベース操作が失敗しました*エラー。 タップ**適用移行**データベースを作成し、エラーを引き続き更新します。

## <a name="next-steps"></a>次の手順

* この記事では、Twitter に認証する方法を示しました。 記載されているその他のプロバイダーで認証する同様のアプローチを利用できる、[前のページ](xref:security/authentication/social/index)します。

* リセットする必要があります、web サイトを Azure web アプリを発行すると、 `ConsumerSecret` Twitter の開発者ポータルでします。

* 設定、`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`として、Azure portal でアプリケーションの設定。 構成システムは、環境変数からキーの読み取りを設定します。
