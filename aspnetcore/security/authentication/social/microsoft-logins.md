---
title: ASP.NET Core での Microsoft アカウントの外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリケーションを Microsoft アカウント ユーザーの認証の統合について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 89969370cea66b7b6632f1b0be59e135767c831e
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708401"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="c4727-103">ASP.NET Core での Microsoft アカウントの外部ログインのセットアップ</span><span class="sxs-lookup"><span data-stu-id="c4727-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="c4727-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4727-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4727-105">このチュートリアルでは、サンプルの ASP.NET Core 2.0 プロジェクトが作成を使用して、Microsoft アカウントでサインインするユーザーを有効にする方法、[前のページ](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="c4727-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="c4727-106">Microsoft 開発者ポータルで、アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4727-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="c4727-107">移動します[ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com)して作成するか、Microsoft アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="c4727-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![[サインイン] ダイアログ](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="c4727-109">Microsoft アカウントがない場合は、タップ**[を作成します。](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="c4727-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="c4727-110">サインインした後にリダイレクトされます **アプリケーション** ページ。</span><span class="sxs-lookup"><span data-stu-id="c4727-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft 開発者ポータルの Microsoft Edge で開く](index/_static/MicrosoftDev.png)

* <span data-ttu-id="c4727-112">タップ**アプリの追加**、画面右上隅にあるし、入力、**アプリケーション名**と**Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="c4727-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![新しいアプリケーションの登録 ダイアログ ボックス](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="c4727-114">このチュートリアルの目的で、クリア、**ガイド付きセットアップ**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="c4727-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="c4727-115">タップ**作成**を続行する、**登録**ページ。</span><span class="sxs-lookup"><span data-stu-id="c4727-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="c4727-116">提供、**名前**の値を確認し、**アプリケーション Id**、として使用する`ClientId`チュートリアルの後半で。</span><span class="sxs-lookup"><span data-stu-id="c4727-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![登録ページ](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="c4727-118">タップ**プラットフォームの追加**で、**プラットフォーム**セクションし、選択、 **Web**プラットフォーム。</span><span class="sxs-lookup"><span data-stu-id="c4727-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![追加のプラットフォーム ダイアログ ボックス](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="c4727-120">新しい**Web**プラットフォーム セクションで、使用、開発の URL を入力`/signin-microsoft`に追加されます、**リダイレクト Url**フィールド (例: `https://localhost:44320/signin-microsoft`)。</span><span class="sxs-lookup"><span data-stu-id="c4727-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="c4727-121">このチュートリアルの後半で構成されている Microsoft の認証構成はで、要求を自動的に処理`/signin-microsoft`OAuth フローを実装するためにルート。</span><span class="sxs-lookup"><span data-stu-id="c4727-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Web プラットフォーム セクションの「](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="c4727-123">URI セグメント`/signin-microsoft`Microsoft 認証プロバイダーの既定のコールバックとして設定されます。</span><span class="sxs-lookup"><span data-stu-id="c4727-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="c4727-124">既定のコールバック URI を変更するには、継承を使用して、Microsoft の認証ミドルウェアを構成するときに[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)クラス。</span><span class="sxs-lookup"><span data-stu-id="c4727-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="c4727-125">タップ**URL の追加**URL が追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c4727-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="c4727-126">必要に応じて、その他のアプリケーション設定を記入し、タップ**保存**アプリの構成に変更を保存するページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="c4727-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="c4727-127">サイトをデプロイするときに、再アクセスする必要があります、**登録**ページし、新しいパブリック URL を設定します。</span><span class="sxs-lookup"><span data-stu-id="c4727-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="c4727-128">Microsoft のアプリケーション Id とパスワードを保存します。</span><span class="sxs-lookup"><span data-stu-id="c4727-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="c4727-129">注、`Application Id`に表示される、**登録**ページ。</span><span class="sxs-lookup"><span data-stu-id="c4727-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="c4727-130">タップ**新しいパスワードを生成**で、**アプリケーション シークレット**セクション。</span><span class="sxs-lookup"><span data-stu-id="c4727-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="c4727-131">これには、アプリケーション パスワードをコピーするボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c4727-131">This displays a box where you can copy the application password:</span></span>

![新しいパスワードの生成 ダイアログ ボックス](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="c4727-133">Microsoft のような機密性の高い設定リンク`Application ID`と`Password`アプリケーションの構成を使用して、 [Secret Manager](xref:security/app-secrets)します。</span><span class="sxs-lookup"><span data-stu-id="c4727-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="c4727-134">このチュートリアルの目的で、名前トークン`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`します。</span><span class="sxs-lookup"><span data-stu-id="c4727-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="c4727-135">Microsoft アカウント認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="c4727-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="c4727-136">このチュートリアルで使用するプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)パッケージが既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="c4727-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="c4727-137">Visual Studio 2017 では、このパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="c4727-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="c4727-138">で .NET Core CLI をインストールするには、プロジェクト ディレクトリで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="c4727-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c4727-139">Microsoft アカウント サービスの追加、`ConfigureServices`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c4727-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c4727-140">Microsoft アカウント ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c4727-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="c4727-141">Microsoft 開発者ポータルで使用される用語は、これらのトークンを名前が`ApplicationId`と`Password`、として公開`ClientId`と`ClientSecret`構成 API です。</span><span class="sxs-lookup"><span data-stu-id="c4727-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="c4727-142">参照してください、 [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API リファレンスの詳細については、Microsoft アカウント認証でサポートされる構成オプション。</span><span class="sxs-lookup"><span data-stu-id="c4727-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="c4727-143">これは、ユーザーに関するさまざまな情報を要求を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c4727-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="c4727-144">Microsoft アカウントでサインイン</span><span class="sxs-lookup"><span data-stu-id="c4727-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="c4727-145">アプリケーションを実行し、をクリックして**ログイン**します。</span><span class="sxs-lookup"><span data-stu-id="c4727-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="c4727-146">Microsoft アカウントでサインインするためのオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c4727-146">An option to sign in with Microsoft appears:</span></span>

![Web ページで、アプリケーション ログ: ユーザーが認証されていません。](index/_static/DoneMicrosoft.png)

<span data-ttu-id="c4727-148">Microsoft をクリックすると、認証のように、Microsoft にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c4727-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="c4727-149">(サインインされていない) 場合、Microsoft アカウントでサインインしたら、アプリの情報にアクセスできるようにするよう求められます。</span><span class="sxs-lookup"><span data-stu-id="c4727-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft 認証ダイアログ ボックス](index/_static/MicrosoftLogin.png)

<span data-ttu-id="c4727-151">タップ**はい**とメールを設定する web サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c4727-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="c4727-152">Microsoft の資格情報を使用してログインしました。</span><span class="sxs-lookup"><span data-stu-id="c4727-152">You are now logged in using your Microsoft credentials:</span></span>

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="c4727-154">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="c4727-154">Troubleshooting</span></span>

* <span data-ttu-id="c4727-155">Microsoft アカウント プロバイダーでは、サインイン エラー ページにリダイレクトするの場合、エラーのタイトルと説明クエリ文字列パラメーターの直後に注意してください、 `#` (ハッシュタグ) uri。</span><span class="sxs-lookup"><span data-stu-id="c4727-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="c4727-156">エラー メッセージを Microsoft 認証に問題を示すために見えますが、最も一般的な原因は、アプリケーション Uri と一致しない、**リダイレクト Uri**の指定、 **Web**プラットフォーム.</span><span class="sxs-lookup"><span data-stu-id="c4727-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="c4727-157">**ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*します。</span><span class="sxs-lookup"><span data-stu-id="c4727-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c4727-158">このチュートリアルで使用するプロジェクト テンプレートによりこれが行われるようになります。</span><span class="sxs-lookup"><span data-stu-id="c4727-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="c4727-159">最初の移行を適用することで、サイト データベースが作成されていない場合になります*要求の処理中にデータベース操作が失敗しました*エラー。</span><span class="sxs-lookup"><span data-stu-id="c4727-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c4727-160">タップ**適用移行**データベースを作成し、エラーを引き続き更新します。</span><span class="sxs-lookup"><span data-stu-id="c4727-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4727-161">次の手順</span><span class="sxs-lookup"><span data-stu-id="c4727-161">Next steps</span></span>

* <span data-ttu-id="c4727-162">この記事では、Microsoft を認証する方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="c4727-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="c4727-163">記載されているその他のプロバイダーで認証する同様のアプローチを利用できる、[前のページ](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="c4727-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="c4727-164">新規に作成する必要があります、web サイトを Azure web アプリを発行すると`Password`Microsoft 開発者ポータルでします。</span><span class="sxs-lookup"><span data-stu-id="c4727-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="c4727-165">設定、`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`として、Azure portal でアプリケーションの設定。</span><span class="sxs-lookup"><span data-stu-id="c4727-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="c4727-166">構成システムは、環境変数からキーの読み取りを設定します。</span><span class="sxs-lookup"><span data-stu-id="c4727-166">The configuration system is set up to read keys from environment variables.</span></span>
