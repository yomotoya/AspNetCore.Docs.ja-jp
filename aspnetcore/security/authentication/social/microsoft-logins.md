---
title: "外部ログインの Microsoft アカウントの設定"
author: rick-anderson
description: "このチュートリアルでは、Microsoft アカウント ユーザーの認証方式を既存の ASP.NET Core アプリケーションの統合について説明します。"
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 6e4586eb681bd230413ace67ca9eddc3fe3e9e60
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="14272-103">Microsoft アカウントの認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="14272-103">Configuring Microsoft Account authentication</span></span>

<span data-ttu-id="14272-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14272-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14272-105">このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、Microsoft アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="14272-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="14272-106">Microsoft 開発者ポータルでのアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="14272-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="14272-107">移動[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)して作成するか、Microsoft アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="14272-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![ダイアログをサインインします。](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="14272-109">既に Microsoft アカウントを持っていない場合は、タップ**[を作成します。](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="14272-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="14272-110">サインインした後にリダイレクトされます**マイ アプリケーション**ページ。</span><span class="sxs-lookup"><span data-stu-id="14272-110">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft Developer Portal の Microsoft Edge で開く](index/_static/MicrosoftDev.png)

* <span data-ttu-id="14272-112">タップ**アプリの追加**で右上コーナーを入力し、**アプリケーション名**と**Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="14272-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![新しいアプリケーションの登録 ダイアログ ボックス](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="14272-114">このチュートリアルの目的で、消去、**セットアップのガイド付き**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="14272-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="14272-115">タップ**を作成する**を続行するのには、**登録**ページです。</span><span class="sxs-lookup"><span data-stu-id="14272-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="14272-116">提供、**名**の値をメモし、**アプリケーション Id**、として使用する`ClientId`のチュートリアルの後。</span><span class="sxs-lookup"><span data-stu-id="14272-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![[登録] ページ](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="14272-118">タップ**プラットフォームを追加**で、**プラットフォーム**セクションし、選択、 **Web**プラットフォーム。</span><span class="sxs-lookup"><span data-stu-id="14272-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![追加プラットフォーム ダイアログ ボックス](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="14272-120">新しい**Web**プラットフォーム セクションで、使用、開発の URL を入力*/signin-microsoft*に追加された、**リダイレクト Url**フィールド (例: `https://localhost:44320/signin-microsoft`)。</span><span class="sxs-lookup"><span data-stu-id="14272-120">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="14272-121">このチュートリアルで後で構成されている Microsoft の認証スキームはで、要求を自動的に処理*/signin-microsoft* OAuth フローを実装するルート。</span><span class="sxs-lookup"><span data-stu-id="14272-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Web プラットフォーム セクションの「](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="14272-123">タップ**URL の追加**URL が追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="14272-123">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="14272-124">必要に応じてその他のアプリケーション設定を入力し、タップ**保存**アプリの構成に変更を保存するページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="14272-124">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="14272-125">サイトを展開するときにを再表示する必要があります、**登録**ページおよび新しいパブリック URL を設定します。</span><span class="sxs-lookup"><span data-stu-id="14272-125">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="14272-126">Microsoft のアプリケーション Id とパスワードを保存します。</span><span class="sxs-lookup"><span data-stu-id="14272-126">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="14272-127">注、`Application Id`に表示される、**登録**ページ。</span><span class="sxs-lookup"><span data-stu-id="14272-127">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="14272-128">タップ**新しいパスワードの生成**で、**アプリケーション シークレット**セクションです。</span><span class="sxs-lookup"><span data-stu-id="14272-128">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="14272-129">これには、アプリケーション パスワードをコピーするボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="14272-129">This displays a box where you can copy the application password:</span></span>

![新しいパスワードの生成 ダイアログ ボックス](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="14272-131">Microsoft のような機密設定をリンク`Application ID`と`Password`、アプリケーションを使用して構成する、[シークレット Manager](../../app-secrets.md)です。</span><span class="sxs-lookup"><span data-stu-id="14272-131">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="14272-132">このチュートリアルの目的で、名前トークン`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`です。</span><span class="sxs-lookup"><span data-stu-id="14272-132">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="14272-133">Microsoft アカウントの認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="14272-133">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="14272-134">このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)パッケージが既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="14272-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="14272-135">Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="14272-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="14272-136">.NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="14272-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="14272-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="14272-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="14272-138">Microsoft アカウントのサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="14272-138">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="14272-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="14272-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="14272-140">Microsoft アカウント ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="14272-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="14272-141">Microsoft 開発者ポータルで使用される用語は、これらのトークンを名前が`ApplicationId`と`Password`、として公開される`ClientId`と`ClientSecret`API の構成にします。</span><span class="sxs-lookup"><span data-stu-id="14272-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="14272-142">参照してください、 [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft アカウントの認証でサポートされる構成オプションの詳細についての API リファレンスです。</span><span class="sxs-lookup"><span data-stu-id="14272-142">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="14272-143">ユーザーに関するさまざまな情報を要求するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="14272-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="14272-144">Microsoft アカウントでサインイン</span><span class="sxs-lookup"><span data-stu-id="14272-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="14272-145">アプリケーションを実行し、をクリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="14272-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="14272-146">Microsoft でサインインするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="14272-146">An option to sign in with Microsoft appears:</span></span>

![Web ページで、アプリケーション ログ: ユーザーが認証されません](index/_static/DoneMicrosoft.png)

<span data-ttu-id="14272-148">Microsoft をクリックすると、認証のため Microsoft にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="14272-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="14272-149">(署名されていない) 場合は、Microsoft アカウントによるサインインの後に、アプリがあなたの情報にアクセスするよう求められます。</span><span class="sxs-lookup"><span data-stu-id="14272-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft 認証ダイアログ ボックス](index/_static/MicrosoftLogin.png)

<span data-ttu-id="14272-151">タップ**はい**は、電子メールを設定する web サイトにリダイレクトするとします。</span><span class="sxs-lookup"><span data-stu-id="14272-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="14272-152">今すぐ Microsoft 資格情報を使用してをログインしています。</span><span class="sxs-lookup"><span data-stu-id="14272-152">You are now logged in using your Microsoft credentials:</span></span>

![Web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="14272-154">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="14272-154">Troubleshooting</span></span>

* <span data-ttu-id="14272-155">場合は、Microsoft アカウント プロバイダーにリダイレクトするサインインのエラー ページ、エラー タイトルと説明クエリ文字列パラメーターすぐ後ろに注意してください、 `#` (ハッシュタグ) Uri にします。</span><span class="sxs-lookup"><span data-stu-id="14272-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="14272-156">エラー メッセージでは、マイクロソフトの認証に問題がある、最も一般的な原因は、アプリケーション Uri と一致しない、**のリダイレクト Uri**向けに指定された、 **Web**プラットフォーム.</span><span class="sxs-lookup"><span data-stu-id="14272-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="14272-157">**ASP.NET Core 2.x のみ:**場合の Id が呼び出すことによって構成されていない`services.AddIdentity`で`ConfigureServices`、認証を試みるが*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。</span><span class="sxs-lookup"><span data-stu-id="14272-157">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="14272-158">このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。</span><span class="sxs-lookup"><span data-stu-id="14272-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="14272-159">最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。</span><span class="sxs-lookup"><span data-stu-id="14272-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="14272-160">タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。</span><span class="sxs-lookup"><span data-stu-id="14272-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14272-161">次の手順</span><span class="sxs-lookup"><span data-stu-id="14272-161">Next steps</span></span>

* <span data-ttu-id="14272-162">この記事では、Microsoft との認証方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="14272-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="14272-163">記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="14272-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="14272-164">新規に作成する必要があります、web サイトを Azure web アプリに発行した後`Password`Microsoft 開発者ポータルにします。</span><span class="sxs-lookup"><span data-stu-id="14272-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="14272-165">設定、`Authentication:Microsoft:ApplicationId`と`Authentication:Microsoft:Password`として Azure ポータルでのアプリケーション設定。</span><span class="sxs-lookup"><span data-stu-id="14272-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="14272-166">構成システムは、環境変数からキーを読み取れませんを設定します。</span><span class="sxs-lookup"><span data-stu-id="14272-166">The configuration system is set up to read keys from environment variables.</span></span>
