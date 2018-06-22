---
title: ASP.NET Core で Facebook 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、Facebook アカウント ユーザーの認証方式を既存の ASP.NET Core アプリケーションの統合について説明します。
ms.author: riande
ms.date: 08/01/2017
uid: security/authentication/facebook-logins
ms.openlocfilehash: 53e5fa3ccee44451646c84e58260db23e59d6cbd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273400"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="c7e25-103">ASP.NET Core で Facebook 外部ログインのセットアップ</span><span class="sxs-lookup"><span data-stu-id="c7e25-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="c7e25-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7e25-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7e25-105">このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して自分の Facebook アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="c7e25-106">Facebook の認証が必要です、 [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="c7e25-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="c7e25-107">まず、次の Facebook アプリケーションの ID を作成することで、[公式手順](https://developers.facebook.com)です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="c7e25-108">Facebook でのアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-108">Create the app in Facebook</span></span>

* <span data-ttu-id="c7e25-109">移動し、 [Facebook Developers アプリ](https://developers.facebook.com/apps/)ページし、サインインします。</span><span class="sxs-lookup"><span data-stu-id="c7e25-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="c7e25-110">既に Facebook アカウントを持っていない場合は使用して、 **Facebook にサインアップする**作成するのにはログイン ページにリンクします。</span><span class="sxs-lookup"><span data-stu-id="c7e25-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="c7e25-111">タップして、**アプリを追加する新しい**新しいアプリ ID を作成する右上隅のボタン</span><span class="sxs-lookup"><span data-stu-id="c7e25-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Microsoft Edge で Facebook 開発者ポータルを開く](index/_static/FBMyApps.png)

* <span data-ttu-id="c7e25-113">フォームに入力し、タップ、**アプリ ID の作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c7e25-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![アプリ ID を新しいフォームを作成します。](index/_static/FBNewAppId.png)

* <span data-ttu-id="c7e25-115">**製品を選択して** ページで、をクリックして**設定**上、 **Facebook ログイン**カードです。</span><span class="sxs-lookup"><span data-stu-id="c7e25-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![製品のセットアップ ページ](index/_static/FBProductSetup.png)

* <span data-ttu-id="c7e25-117">**クイック スタート**とウィザードが起動**プラットフォームを選択して**最初のページとして。</span><span class="sxs-lookup"><span data-stu-id="c7e25-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="c7e25-118">クリックして、ここでは、ウィザードをバイパス、**設定**左側のメニュー内のリンク。</span><span class="sxs-lookup"><span data-stu-id="c7e25-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Skip のクイック スタート](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="c7e25-120">表示され、**クライアント OAuth 設定**ページ。</span><span class="sxs-lookup"><span data-stu-id="c7e25-120">You are presented with the **Client OAuth Settings** page:</span></span>

![クライアントの OAuth 設定 ページ](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="c7e25-122">開発 URI を入力と */signin-facebook*に追加されます、**有効な OAuth リダイレクト Uri**フィールド (例: `https://localhost:44320/signin-facebook`)。</span><span class="sxs-lookup"><span data-stu-id="c7e25-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="c7e25-123">このチュートリアルで後で構成されている Facebook 認証はで、要求を自動的に処理 */signin-facebook* OAuth フローを実装するルート。</span><span class="sxs-lookup"><span data-stu-id="c7e25-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="c7e25-124">URI */signin-facebook* Facebook の認証プロバイダーの既定のコールバックとして設定されます。</span><span class="sxs-lookup"><span data-stu-id="c7e25-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="c7e25-125">既定のコールバック URI を変更するには、継承を使用して Facebook の認証ミドルウェアの構成中に[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)クラス。</span><span class="sxs-lookup"><span data-stu-id="c7e25-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="c7e25-126">をクリックして**変更を保存**です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="c7e25-127">クリックして、**ダッシュ ボード**左側のナビゲーション リンク。</span><span class="sxs-lookup"><span data-stu-id="c7e25-127">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="c7e25-128">このページで、書き留めて、`App ID`と`App Secret`です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="c7e25-129">次のセクションでは、ASP.NET Core アプリケーションの両方を追加します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-129">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Facebook 開発者向けダッシュ ボード](index/_static/FBDashboard.png)

* <span data-ttu-id="c7e25-131">サイトを展開するときに再表示する必要があります、 **Facebook ログイン**セットアップ ページと、新しいパブリック URI を登録します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-131">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="c7e25-132">Facebook アプリケーションの ID とアプリのシークレットを格納します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-132">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="c7e25-133">Facebook などの機密設定をリンク`App ID`と`App Secret`、アプリケーションを使用して構成する、[シークレット Manager](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-133">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="c7e25-134">このチュートリアルの目的で、名前トークン`Authentication:Facebook:AppId`と`Authentication:Facebook:AppSecret`です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-134">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="c7e25-135">安全に保管する次のコマンドを実行する`App ID`と`App Secret`シークレット マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-135">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="c7e25-136">Facebook 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-136">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7e25-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c7e25-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c7e25-138">Facebook のサービスを追加、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c7e25-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7e25-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c7e25-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c7e25-140">インストール、 [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="c7e25-140">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="c7e25-141">Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-141">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="c7e25-142">.NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-142">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="c7e25-143">Facebook ミドルウェア内の追加、`Configure`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c7e25-143">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="c7e25-144">参照してください、 [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook 認証でサポートされる構成オプションの詳細についての API リファレンスです。</span><span class="sxs-lookup"><span data-stu-id="c7e25-144">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="c7e25-145">構成オプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="c7e25-145">Configuration options can be used to:</span></span>

* <span data-ttu-id="c7e25-146">ユーザーに関するさまざまな情報を要求します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-146">Request different information about the user.</span></span>
* <span data-ttu-id="c7e25-147">ログイン エクスペリエンスをカスタマイズするクエリ文字列引数を追加します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-147">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="c7e25-148">Facebook でサインイン</span><span class="sxs-lookup"><span data-stu-id="c7e25-148">Sign in with Facebook</span></span>

<span data-ttu-id="c7e25-149">アプリケーションを実行し、をクリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-149">Run your application and click **Log in**.</span></span> <span data-ttu-id="c7e25-150">Facebook でサインインするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c7e25-150">You see an option to sign in with Facebook.</span></span>

![Web アプリケーション: ユーザーが認証されません](index/_static/DoneFacebook.png)

<span data-ttu-id="c7e25-152">クリックすると**Facebook**認証に Facebook にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c7e25-152">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook の認証 ページ](index/_static/FBLogin.png)

<span data-ttu-id="c7e25-154">Facebook の認証は、既定でパブリック プロファイルと電子メール アドレスを要求します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-154">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook の認証 ページ](index/_static/FBLoginDone.png)

<span data-ttu-id="c7e25-156">Facebook 資格情報を入力すると、電子メールを設定することができますのサイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c7e25-156">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="c7e25-157">これで、Facebook の資格情報を使用してをログインしています。</span><span class="sxs-lookup"><span data-stu-id="c7e25-157">You are now logged in using your Facebook credentials:</span></span>

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="c7e25-159">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="c7e25-159">Troubleshooting</span></span>

* <span data-ttu-id="c7e25-160">**ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試行することになります*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-160">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c7e25-161">このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。</span><span class="sxs-lookup"><span data-stu-id="c7e25-161">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="c7e25-162">取得する場合は、初期の移行を適用することで、サイト データベースが作成されていない、*要求の処理中にデータベース操作が失敗しました*エラーです。</span><span class="sxs-lookup"><span data-stu-id="c7e25-162">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c7e25-163">タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-163">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7e25-164">次の手順</span><span class="sxs-lookup"><span data-stu-id="c7e25-164">Next steps</span></span>

* <span data-ttu-id="c7e25-165">この記事では、facebook の認証方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="c7e25-165">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="c7e25-166">記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="c7e25-166">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="c7e25-167">リセットする必要があります、web サイトを Azure web アプリに発行した後、 `AppSecret` Facebook 開発者ポータルにします。</span><span class="sxs-lookup"><span data-stu-id="c7e25-167">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="c7e25-168">設定、`Authentication:Facebook:AppId`と`Authentication:Facebook:AppSecret`として Azure ポータルでのアプリケーション設定。</span><span class="sxs-lookup"><span data-stu-id="c7e25-168">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="c7e25-169">構成システムは、環境変数からキーを読み取れませんを設定します。</span><span class="sxs-lookup"><span data-stu-id="c7e25-169">The configuration system is set up to read keys from environment variables.</span></span>
