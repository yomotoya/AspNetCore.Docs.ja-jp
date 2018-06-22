---
title: Twitter の ASP.NET Core を使用して外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、Twitter アカウント ユーザーの認証方式を既存の ASP.NET Core アプリケーションの統合について説明します。
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/twitter-logins
ms.openlocfilehash: 81c19105e4cda932db3302a5df343322fb85abef
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278493"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Twitter の ASP.NET Core を使用して外部ログインのセットアップ

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルをユーザーに許可する方法を示します[の Twitter アカウントでサインイン](https://dev.twitter.com/web/sign-in/desktop-browser)で作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、[前のページ](xref:security/authentication/social/index)です。

## <a name="create-the-app-in-twitter"></a>Twitter でのアプリを作成します。

* 移動[ https://apps.twitter.com/ ](https://apps.twitter.com/)してサインインします。 Twitter アカウントがない場合を使用して、 **[今すぐサインアップ](https://twitter.com/signup)** リンクを作成します。 サインインした後、**アプリケーション管理**ページが表示。

![Twitter のアプリケーション管理の Microsoft Edge で開く](index/_static/TwitterAppManage.png)

* タップ**Create New App**し、アプリケーションに記入**名前**、**説明**とパブリック**web サイト**(このする一時的なまで URIドメイン名の登録)。

![アプリケーション ページを作成します。](index/_static/TwitterCreate.png)

* 開発 URI を入力と`/signin-twitter`に追加されます、**有効な OAuth リダイレクト Uri**フィールド (例: `https://localhost:44320/signin-twitter`)。 このチュートリアルで後で構成されている Twitter 認証スキームはで、要求を自動的に処理`/signin-twitter`OAuth フローを実装するルート。

> [!NOTE]
> URI セグメント`/signin-twitter`Twitter 認証プロバイダーの既定のコールバックとして設定されます。 既定のコールバック URI を変更するには、継承を使用して Twitter 認証ミドルウェアの構成中に[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)クラス。

* フォームの残りの部分を入力し、タップ **、Twitter アプリケーションを作成**です。 新しいアプリケーションの詳細が表示されます。

![[アプリケーション] ページの [詳細] タブ](index/_static/TwitterAppDetails.png)

* サイトを展開するときにを再表示する必要があります、**アプリケーション管理**ページおよび新しいパブリック URI を登録します。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Twitter ConsumerKey と ConsumerSecret を格納します。

Twitter などの機密設定をリンク`Consumer Key`と`Consumer Secret`、アプリケーションを使用して構成する、[シークレット Manager](xref:security/app-secrets)です。 このチュートリアルの目的で、名前トークン`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`です。

これらのトークンにあります、**キーとアクセス トークン**新しい Twitter アプリケーションを作成した後タブ。

![キーとアクセス トークン タブ](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter 認証を構成します。

このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)パッケージが既にインストールされています。

* Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。
* .NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Twitter のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

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

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Twitter ミドルウェア内の追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

参照してください、 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 認証でサポートされる構成オプションの詳細についての API リファレンスです。 ユーザーに関するさまざまな情報を要求するために使用できます。

## <a name="sign-in-with-twitter"></a>Twitter でサインイン

アプリケーションを実行し、をクリックして**ログイン**です。 Twitter でサインインするオプションが表示されます。

![Web アプリケーション: ユーザーが認証されません](index/_static/DoneTwitter.png)

クリックすると**Twitter** Twitter 認証にリダイレクトします。

![Twitter 認証 ページ](index/_static/TwitterLogin.png)

Twitter の資格情報を入力した後は、電子メールを設定する web サイトにリダイレクトされます。

これで、Twitter の資格情報を使用してをログインしています。

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a>トラブルシューティング

* **ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試行することになります*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。 このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。
* 最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。 タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。

## <a name="next-steps"></a>次の手順

* この記事では、Twitter で認証する方法を示しました。 記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](xref:security/authentication/social/index)です。

* リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ConsumerSecret` Twitter 開発者ポータルにします。

* 設定、`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`として Azure ポータルでのアプリケーション設定。 構成システムは、環境変数からキーを読み取れませんを設定します。
