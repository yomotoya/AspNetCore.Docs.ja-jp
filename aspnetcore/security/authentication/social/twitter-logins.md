---
title: Twitter の ASP.NET Core を使用して外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、Twitter アカウント ユーザーの認証方式を既存の ASP.NET Core アプリケーションの統合について説明します。
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f59f7d1bf0280cef8f7757e8cd57d4872769b3d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688997"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="17ea7-103">Twitter の ASP.NET Core を使用して外部ログインのセットアップ</span><span class="sxs-lookup"><span data-stu-id="17ea7-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="17ea7-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17ea7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17ea7-105">このチュートリアルをユーザーに許可する方法を示します[の Twitter アカウントでサインイン](https://dev.twitter.com/web/sign-in/desktop-browser)で作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="17ea7-106">Twitter でのアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-106">Create the app in Twitter</span></span>

* <span data-ttu-id="17ea7-107">移動[ https://apps.twitter.com/ ](https://apps.twitter.com/)してサインインします。</span><span class="sxs-lookup"><span data-stu-id="17ea7-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="17ea7-108">Twitter アカウントがない場合を使用して、 **[今すぐサインアップ](https://twitter.com/signup)** リンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="17ea7-109">サインインした後、**アプリケーション管理**ページが表示。</span><span class="sxs-lookup"><span data-stu-id="17ea7-109">After signing in, the **Application Management** page is shown:</span></span>

![Twitter のアプリケーション管理の Microsoft Edge で開く](index/_static/TwitterAppManage.png)

* <span data-ttu-id="17ea7-111">タップ**Create New App**し、アプリケーションに記入**名前**、**説明**とパブリック**web サイト**(このする一時的なまで URIドメイン名の登録)。</span><span class="sxs-lookup"><span data-stu-id="17ea7-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![アプリケーション ページを作成します。](index/_static/TwitterCreate.png)

* <span data-ttu-id="17ea7-113">開発 URI を入力と */signin-twitter*に追加されます、**有効な OAuth リダイレクト Uri**フィールド (例: `https://localhost:44320/signin-twitter`)。</span><span class="sxs-lookup"><span data-stu-id="17ea7-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="17ea7-114">このチュートリアルで後で構成されている Twitter 認証スキームはで、要求を自動的に処理 */signin-twitter* OAuth フローを実装するルート。</span><span class="sxs-lookup"><span data-stu-id="17ea7-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="17ea7-115">フォームの残りの部分を入力し、タップ **、Twitter アプリケーションを作成**です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="17ea7-116">新しいアプリケーションの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="17ea7-116">New application details are displayed:</span></span>

![[アプリケーション] ページの [詳細] タブ](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="17ea7-118">サイトを展開するときにを再表示する必要があります、**アプリケーション管理**ページおよび新しいパブリック URI を登録します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="17ea7-119">Twitter ConsumerKey と ConsumerSecret を格納します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="17ea7-120">Twitter などの機密設定をリンク`Consumer Key`と`Consumer Secret`、アプリケーションを使用して構成する、[シークレット Manager](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="17ea7-121">このチュートリアルの目的で、名前トークン`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="17ea7-122">これらのトークンにあります、**キーとアクセス トークン**新しい Twitter アプリケーションを作成した後タブ。</span><span class="sxs-lookup"><span data-stu-id="17ea7-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![キーとアクセス トークン タブ](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="17ea7-124">Twitter 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="17ea7-125">このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)パッケージが既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="17ea7-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="17ea7-126">Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="17ea7-127">.NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="17ea7-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="17ea7-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="17ea7-129">Twitter のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="17ea7-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="17ea7-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="17ea7-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="17ea7-131">Twitter ミドルウェア内の追加、`Configure`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="17ea7-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="17ea7-132">参照してください、 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 認証でサポートされる構成オプションの詳細についての API リファレンスです。</span><span class="sxs-lookup"><span data-stu-id="17ea7-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="17ea7-133">ユーザーに関するさまざまな情報を要求するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="17ea7-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="17ea7-134">Twitter でサインイン</span><span class="sxs-lookup"><span data-stu-id="17ea7-134">Sign in with Twitter</span></span>

<span data-ttu-id="17ea7-135">アプリケーションを実行し、をクリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="17ea7-136">Twitter でサインインするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="17ea7-136">An option to sign in with Twitter appears:</span></span>

![Web アプリケーション: ユーザーが認証されません](index/_static/DoneTwitter.png)

<span data-ttu-id="17ea7-138">クリックすると**Twitter** Twitter 認証にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="17ea7-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter 認証 ページ](index/_static/TwitterLogin.png)

<span data-ttu-id="17ea7-140">Twitter の資格情報を入力した後は、電子メールを設定する web サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="17ea7-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="17ea7-141">これで、Twitter の資格情報を使用してをログインしています。</span><span class="sxs-lookup"><span data-stu-id="17ea7-141">You are now logged in using your Twitter credentials:</span></span>

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="17ea7-143">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="17ea7-143">Troubleshooting</span></span>

* <span data-ttu-id="17ea7-144">**ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試行することになります*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="17ea7-145">このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。</span><span class="sxs-lookup"><span data-stu-id="17ea7-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="17ea7-146">最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。</span><span class="sxs-lookup"><span data-stu-id="17ea7-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="17ea7-147">タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17ea7-148">次の手順</span><span class="sxs-lookup"><span data-stu-id="17ea7-148">Next steps</span></span>

* <span data-ttu-id="17ea7-149">この記事では、Twitter で認証する方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="17ea7-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="17ea7-150">記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="17ea7-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="17ea7-151">リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ConsumerSecret` Twitter 開発者ポータルにします。</span><span class="sxs-lookup"><span data-stu-id="17ea7-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="17ea7-152">設定、`Authentication:Twitter:ConsumerKey`と`Authentication:Twitter:ConsumerSecret`として Azure ポータルでのアプリケーション設定。</span><span class="sxs-lookup"><span data-stu-id="17ea7-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="17ea7-153">構成システムは、環境変数からキーを読み取れませんを設定します。</span><span class="sxs-lookup"><span data-stu-id="17ea7-153">The configuration system is set up to read keys from environment variables.</span></span>
