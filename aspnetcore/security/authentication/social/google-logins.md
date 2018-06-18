---
title: ASP.NET Core で Google 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリケーションに Google アカウントのユーザー認証の統合について説明します。
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 878c0b16e24f48a0ee84f93393af67af1728e284
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725966"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="ed17e-103">ASP.NET Core で Google 外部ログインのセットアップ</span><span class="sxs-lookup"><span data-stu-id="ed17e-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="ed17e-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed17e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed17e-105">このチュートリアルで作成されたサンプルの ASP.NET Core 2.0 プロジェクトを使用して、Google + アカウントでサインインするユーザーを有効にする方法を示します、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="ed17e-106">まず、次の[公式手順](https://developers.google.com/identity/sign-in/web/devconsole-project)Google API コンソールで新しいアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="ed17e-107">Google API コンソールでのアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="ed17e-108">移動[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)してサインインします。</span><span class="sxs-lookup"><span data-stu-id="ed17e-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="ed17e-109">Google アカウントがない場合を使用して**より多くのオプション** > **[アカウントを作成する](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** リンクを作成するのには。</span><span class="sxs-lookup"><span data-stu-id="ed17e-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Google API コンソール](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="ed17e-111">リダイレクトされます**API マネージャー ライブラリ**ページ。</span><span class="sxs-lookup"><span data-stu-id="ed17e-111">You are redirected to **API Manager Library** page:</span></span>

![ライブラリの API マネージャー ページ](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="ed17e-113">タップ**作成**を入力し、**プロジェクト名**:</span><span class="sxs-lookup"><span data-stu-id="ed17e-113">Tap **Create** and enter your **Project name**:</span></span>

![[新しいプロジェクト] ダイアログ](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="ed17e-115">受け入れた場合、ダイアログ ボックスが、新しいアプリの機能を選択することができます、ライブラリのページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ed17e-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="ed17e-116">検索**Google + API** API 機能を追加するには、そのリンクをクリックしてリストに。</span><span class="sxs-lookup"><span data-stu-id="ed17e-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![ライブラリの API マネージャー ページ](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="ed17e-118">新しく追加された API のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="ed17e-119">タップ**を有効にする**をアプリの機能で Google + 記号を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![API マネージャー Google + API ページ](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="ed17e-121">API を有効にした後は、タップ**資格情報を作成する**シークレットを構成します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![API マネージャー Google + API ページ](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="ed17e-123">選択するオプション</span><span class="sxs-lookup"><span data-stu-id="ed17e-123">Choose:</span></span>
   * <span data-ttu-id="ed17e-124">**Google+ API**</span><span class="sxs-lookup"><span data-stu-id="ed17e-124">**Google+ API**</span></span>
   * <span data-ttu-id="ed17e-125">**Web サーバー (例: node.js、Tomcat)**、および</span><span class="sxs-lookup"><span data-stu-id="ed17e-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="ed17e-126">**ユーザー データ**:</span><span class="sxs-lookup"><span data-stu-id="ed17e-126">**User data**:</span></span>

![API のマネージャーの資格情報 ページ: どのような種類の資格情報を調べるには、パネルが必要があります](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="ed17e-128">タップ**資格情報が必要ですか?** アプリの構成の 2 番目の手順を行います**OAuth 2.0 クライアント ID の作成**:</span><span class="sxs-lookup"><span data-stu-id="ed17e-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![API のマネージャーの資格情報 ページ: OAuth 2.0 クライアント ID の作成](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="ed17e-130">1 つの機能 (サインイン) を入力するには同じで Google + プロジェクトを作成するため**名前**OAuth 2.0 クライアント ID のプロジェクトを使用したものとします。</span><span class="sxs-lookup"><span data-stu-id="ed17e-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="ed17e-131">開発 URI を入力と`/signin-google`に追加されます、**承認されているリダイレクト Uri**フィールド (例: `https://localhost:44320/signin-google`)。</span><span class="sxs-lookup"><span data-stu-id="ed17e-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="ed17e-132">このチュートリアルで後で構成されている Google の認証はで、要求を自動的に処理`/signin-google`OAuth フローを実装するルート。</span><span class="sxs-lookup"><span data-stu-id="ed17e-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="ed17e-133">URI セグメント`/signin-google`は Google の認証プロバイダーの既定のコールバックとして設定します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="ed17e-134">既定のコールバック URI を変更するには、継承を使用して Google の認証ミドルウェアの構成中に[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)クラスです。</span><span class="sxs-lookup"><span data-stu-id="ed17e-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="ed17e-135">Tab キーを追加する、**承認されているリダイレクト Uri**エントリです。</span><span class="sxs-lookup"><span data-stu-id="ed17e-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="ed17e-136">タップ**クライアント ID を作成する**、3 番目の手順を行います**OAuth 2.0 同意画面設定**:</span><span class="sxs-lookup"><span data-stu-id="ed17e-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![API のマネージャーの資格情報 ページ: OAuth 2.0 同意画面の設定](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="ed17e-138">入力、公開されて**電子メール アドレス**と**製品名**Google + にサインインするユーザーを要求するときに、アプリに表示します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="ed17e-139">追加のオプションを 利用可能な**より多くのカスタマイズ オプション**です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="ed17e-140">タップ**続行**最後の手順を続行する**資格情報をダウンロード**:</span><span class="sxs-lookup"><span data-stu-id="ed17e-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![API のマネージャーの資格情報 ページ: 資格情報をダウンロード](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="ed17e-142">タップ**ダウンロード**アプリケーション シークレットの JSON ファイルを保存して**完了**新しいアプリの作成を完了します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="ed17e-143">サイトを展開するときにを再表示する必要があります、 **Google コンソール**し、新しいパブリック url を登録します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="ed17e-144">ストア Google ClientID および ClientSecret</span><span class="sxs-lookup"><span data-stu-id="ed17e-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="ed17e-145">Google のように機密設定をリンク`Client ID`と`Client Secret`、アプリケーションを使用して構成する、[シークレット Manager](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="ed17e-146">このチュートリアルの目的で、名前トークン`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="ed17e-147">前の手順でダウンロードした JSON ファイルでこれらのトークンの値が見つかります`web.client_id`と`web.client_secret`です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="ed17e-148">Google 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-148">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed17e-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed17e-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ed17e-150">Google のサービスを追加、`ConfigureServices`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ed17e-150">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed17e-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed17e-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ed17e-152">このチュートリアルで使用されるプロジェクト テンプレートにより[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)パッケージがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="ed17e-152">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="ed17e-153">Visual Studio 2017 でこのパッケージをインストールするには、クリックし、プロジェクトを右クリックし**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-153">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="ed17e-154">.NET Core cli をインストールするには、プロジェクト ディレクトリに、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-154">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="ed17e-155">内で Google ミドルウェアを追加、`Configure`メソッド*Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ed17e-155">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="ed17e-156">参照してください、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google 認証でサポートされる構成オプションの詳細についての API リファレンスです。</span><span class="sxs-lookup"><span data-stu-id="ed17e-156">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="ed17e-157">ユーザーに関するさまざまな情報を要求するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-157">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="ed17e-158">Google でサインイン</span><span class="sxs-lookup"><span data-stu-id="ed17e-158">Sign in with Google</span></span>

<span data-ttu-id="ed17e-159">アプリケーションを実行し、をクリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-159">Run your application and click **Log in**.</span></span> <span data-ttu-id="ed17e-160">Google でサインインするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-160">An option to sign in with Google appears:</span></span>

![Microsoft Edge で実行されている web アプリケーション: ユーザーが認証されません](index/_static/DoneGoogle.png)

<span data-ttu-id="ed17e-162">Google をクリックすると、認証用 Google にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-162">When you click on Google, you are redirected to Google for authentication:</span></span>

![Google 認証ダイアログ ボックス](index/_static/GoogleLogin.png)

<span data-ttu-id="ed17e-164">Google の資格情報を入力すると、しにリダイレクトされます戻る、web サイト、メールを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-164">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="ed17e-165">これで Google の資格情報を使用してをログインしています。</span><span class="sxs-lookup"><span data-stu-id="ed17e-165">You are now logged in using your Google credentials:</span></span>

![Microsoft Edge で実行されている web アプリケーション: 認証されたユーザー](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="ed17e-167">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="ed17e-167">Troubleshooting</span></span>

* <span data-ttu-id="ed17e-168">表示された場合、`403 (Forbidden)`開発モード (または同じのエラーのため、デバッガーにブレーク) で実行されていることを確認時に、独自のアプリケーションからのエラー ページ**Google + API**が有効になって、 **API マネージャー ライブラリ**表示されている手順に従って、[このページで以前](#create-the-app-in-google-api-console)です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-168">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="ed17e-169">サインインがそれでもエラーが表示されていない場合は、問題のデバッグの簡略化を行うには開発モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="ed17e-169">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="ed17e-170">**ASP.NET Core 2.x のみ:** 呼び出すことによって構成されていない場合の Identity`services.AddIdentity`で`ConfigureServices`、認証を試行することになります*ArgumentException: 'SignInScheme' オプションを指定する必要があります*です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-170">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="ed17e-171">このチュートリアルで使用されるプロジェクト テンプレートは、これが行われるようにします。</span><span class="sxs-lookup"><span data-stu-id="ed17e-171">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="ed17e-172">最初の移行を適用することで、サイト データベースが作成されていない場合、表示される*要求の処理中にデータベース操作が失敗しました*エラーです。</span><span class="sxs-lookup"><span data-stu-id="ed17e-172">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="ed17e-173">タップ**適用移行**データベースを作成し、エラーを越えて続行を更新します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-173">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed17e-174">次の手順</span><span class="sxs-lookup"><span data-stu-id="ed17e-174">Next steps</span></span>

* <span data-ttu-id="ed17e-175">この記事では、Google との認証方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="ed17e-175">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="ed17e-176">記載されているその他のプロバイダーでの認証に同様のアプローチを行うことができる、[前のページ](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="ed17e-176">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="ed17e-177">リセットする必要があります、web サイトを Azure web アプリに発行した後、 `ClientSecret` Google API コンソールでします。</span><span class="sxs-lookup"><span data-stu-id="ed17e-177">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="ed17e-178">設定、`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`として Azure ポータルでのアプリケーション設定。</span><span class="sxs-lookup"><span data-stu-id="ed17e-178">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="ed17e-179">構成システムは、環境変数からキーを読み取れませんを設定します。</span><span class="sxs-lookup"><span data-stu-id="ed17e-179">The configuration system is set up to read keys from environment variables.</span></span>
