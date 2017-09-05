---
title: "Twitter 外部ログインのセットアップ"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 11/1/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 800f98285859a54198b76411aea000384de05cd3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-twitter-authentication"></a>Twitter 認証を構成します。

<a name=security-authentication-twitter-logins></a>

によって[Valeriy Novytskyy](https://github.com/01binary)と[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルをユーザーに許可する方法を示します[の Twitter アカウントでサインイン](https://dev.twitter.com/web/sign-in/desktop-browser)で作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、[前のページ](index.md)です。

## <a name="create-the-app-in-twitter"></a>Twitter でのアプリを作成します。

* 移動[https://apps.twitter.com/](https://apps.twitter.com/)してサインインします。 Twitter アカウントがない場合を使用して、 **[今すぐサインアップ](https://twitter.com/signup)**リンクを作成します。 サインインした後、**アプリケーション管理**ページが表示。

![Twitter のアプリケーション管理の Microsoft Edge で開く](index/_static/TwitterAppManage.png)

* タップ**Create New App**し、アプリケーションに記入**名前**、**説明**とパブリック**web サイト**(このする一時的なまで URIドメイン名の登録)。

![アプリケーション ページを作成します。](index/_static/TwitterCreate.png)

* 開発 URI を入力と*/signin-twitter*に追加されます、**有効な OAuth リダイレクト Uri**フィールド (例: `https://localhost:44320/signin-twitter`)。 このチュートリアルで後で構成されている Twitter 認証スキームはで、要求を自動的に処理*/signin-twitter* OAuth フローを実装するルート。

* フォームの残りの部分を入力し、タップ**、Twitter アプリケーションを作成**です。 新しいアプリケーションの詳細が表示されます。

![[アプリケーション] ページの [詳細] タブ](index/_static/TwitterAppDetails.png)

* サイトを展開するときにを再表示する必要があります、**アプリケーション管理**ページおよび新しいパブリック URI を登録します。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Twitter ConsumerKey と ConsumerSecret を格納します。

Twitter などの機密設定をリンク`Consumer Key`と`Consumer Secret`、アプリケーションを使用して構成する、[シークレット Manager](../../app-secrets.md)です。 このチュートリアルの目的で、名前トークン`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`です。

これらのトークンにあります、**キーとアクセス トークン**新しい Twitter アプリケーションを作成した後タブ。

![キーとアクセス トークン タブ](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter 認証を構成します。

このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)パッケージが既にインストールされています。

* Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。
* .NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

Twitter のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。

```csharp
services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

`AddAuthentication`メソッドのみ呼び出されます一度に複数の認証プロバイダーを追加する場合にします。 後続の呼び出しのいずれかの以前に構成をオーバーライドする可能性のある[AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)プロパティです。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Twitter ミドルウェア内の追加、`Configure`メソッド*Startup.cs*ファイル。

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

参照してください、 [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 認証でサポートされる構成オプションの詳細についての API リファレンスです。 ユーザーに関するさまざまな情報を要求するために使用できます。

## <a name="sign-in-with-twitter"></a>Twitter でサインイン

アプリケーションを実行し、をクリックして**ログイン**です。 Twitter でサインインするオプションが表示されます。

![Web アプリケーション: ユーザーが認証されません](index/_static/DoneTwitter.png)

クリックすると**Twitter** Twitter 認証にリダイレクトします。

![Twitter 認証 ページ](index/_static/TwitterLogin.png)

Twitter の資格情報を入力した後は、電子メールを設定する web サイトにリダイレクトされます。

これで、Twitter の資格情報を使用してをログインしています。

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a>トラブルシューティング

* **ASP.NET Core 2.x のみ:**場合の Id が呼び出すことによって構成されていない`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。 このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。
* 最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。 タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。

## <a name="next-steps"></a>次のステップ

* この記事では、Twitter で認証する方法を示しました。 記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](index.md)です。

* リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ConsumerSecret` Twitter 開発者ポータルにします。

* 設定、`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`として Azure ポータルでのアプリケーション設定。 構成システムは、環境変数からキーを読み取れませんを設定します。
