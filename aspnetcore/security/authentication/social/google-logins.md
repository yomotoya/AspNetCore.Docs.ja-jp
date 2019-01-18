---
title: ASP.NET Core での Google 外部ログインのセットアップ
author: rick-anderson
description: このチュートリアルでは、既存の ASP.NET Core アプリに Google アカウントのユーザー認証の統合について説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 98857a84238124e75d695242c8d421b9a29f02e7
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396098"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="38bf5-103">ASP.NET Core での Google 外部ログインのセットアップ</span><span class="sxs-lookup"><span data-stu-id="38bf5-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="38bf5-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="38bf5-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="38bf5-105">2019 年 1 月で Google の開始を[シャット ダウン](https://developers.google.com/+/api-shutdown)Google + にサインインして、年 3 月でシステム内の開発者の新しい Google サインインを移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38bf5-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="38bf5-106">ASP.NET Core 2.1 と Google 認証用の 2.2 のパッケージは、変化に対応する年 2 月に更新されます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="38bf5-107">詳細と ASP.NET Core 用の一時的な軽減策は、次を参照してください。[この GitHub の問題](https://github.com/aspnet/AspNetCore/issues/6486)します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="38bf5-108">このチュートリアルは、新しいセットアップ プロセスで更新されました。</span><span class="sxs-lookup"><span data-stu-id="38bf5-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="38bf5-109">このチュートリアルでは、ASP.NET Core 2.2 プロジェクトが作成を使用して、Google アカウントでサインインするユーザーを有効にする方法、[前のページ](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="38bf5-110">Google API コンソール プロジェクトとクライアント ID を作成します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="38bf5-111">移動します[統合で Google サインインを web アプリに](https://developers.google.com/identity/sign-in/web/devconsole-project)選択**A プロジェクトの構成**します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="38bf5-112">**OAuth クライアントを構成する**ダイアログ ボックスで、 **Web server**します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="38bf5-113">**承認済みのリダイレクト Uri**テキスト入力ボックス、リダイレクト URI を設定します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="38bf5-114">たとえば、`https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="38bf5-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="38bf5-115">保存、**クライアント ID**と**クライアント シークレット**します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="38bf5-116">新しいパブリック url の登録、サイトをデプロイするとき、 **Google コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="38bf5-117">ストア Google ClientID と ClientSecret</span><span class="sxs-lookup"><span data-stu-id="38bf5-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="38bf5-118">Google などの機密設定を保存`Client ID`と`Client Secret`で、 [Secret Manager](xref:security/app-secrets)します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="38bf5-119">このチュートリアルの目的で、名前トークン`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="38bf5-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="38bf5-120">API の資格情報と使用量を管理することができます、 [API コンソール](https://console.developers.google.com/apis/dashboard)します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="38bf5-121">Google 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-121">Configure Google authentication</span></span>

<span data-ttu-id="38bf5-122">Google サービスを追加`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="38bf5-123">Google のサインイン</span><span class="sxs-lookup"><span data-stu-id="38bf5-123">Sign in with Google</span></span>

* <span data-ttu-id="38bf5-124">アプリを実行し、をクリックして**ログイン**します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="38bf5-125">Google でサインインするためのオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="38bf5-126">をクリックして、 **Google**ボタンで、認証の Google にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="38bf5-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="38bf5-127">Google の資格情報を入力した後は、web サイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="38bf5-128">参照してください、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google 認証でサポートされる構成オプションの詳細について、API リファレンス。</span><span class="sxs-lookup"><span data-stu-id="38bf5-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="38bf5-129">これは、ユーザーに関するさまざまな情報を要求を使用できます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="38bf5-130">既定のコールバック URI を変更します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-130">Change the default callback URI</span></span>

<span data-ttu-id="38bf5-131">URI セグメント`/signin-google`Google の認証プロバイダーの既定のコールバックとして設定されます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="38bf5-132">既定のコールバック URI を変更するには、継承を使用して、Google の認証ミドルウェアを構成するときに[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)クラス。</span><span class="sxs-lookup"><span data-stu-id="38bf5-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="38bf5-133">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="38bf5-133">Troubleshooting</span></span>

* <span data-ttu-id="38bf5-134">サインインでは機能しません、エラー通知が届かない場合は、問題をデバッグしやすくする開発モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="38bf5-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="38bf5-135">ユーザーが呼び出すことによって構成されていない場合`services.AddIdentity`で`ConfigureServices`で結果を認証しようとすると、 *ArgumentException:'SignInScheme' オプションを指定する必要があります*します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="38bf5-136">このチュートリアルで使用するプロジェクト テンプレートによりこれが行われるようになります。</span><span class="sxs-lookup"><span data-stu-id="38bf5-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="38bf5-137">取得する場合は、初期移行を適用することで、サイト データベースが作成されていない*要求の処理中にデータベース操作が失敗しました*エラー。</span><span class="sxs-lookup"><span data-stu-id="38bf5-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="38bf5-138">タップ**適用移行**データベースを作成し、エラーを引き続き更新します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38bf5-139">次の手順</span><span class="sxs-lookup"><span data-stu-id="38bf5-139">Next steps</span></span>

* <span data-ttu-id="38bf5-140">この記事では、Google で認証する方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="38bf5-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="38bf5-141">記載されているその他のプロバイダーで認証する同様のアプローチを利用できる、[前のページ](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="38bf5-142">アプリを Azure に発行すると、リセット、 `ClientSecret` Google API コンソールでします。</span><span class="sxs-lookup"><span data-stu-id="38bf5-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="38bf5-143">設定、`Authentication:Google:ClientId`と`Authentication:Google:ClientSecret`として、Azure portal でアプリケーションの設定。</span><span class="sxs-lookup"><span data-stu-id="38bf5-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="38bf5-144">構成システムは、環境変数からキーの読み取りを設定します。</span><span class="sxs-lookup"><span data-stu-id="38bf5-144">The configuration system is set up to read keys from environment variables.</span></span>
