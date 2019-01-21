---
title: Facebook、Google、ASP.NET Core での外部プロバイダーの認証
author: rick-anderson
description: このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396143"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="3dfa2-103">Facebook、Google、ASP.NET Core での外部プロバイダーの認証</span><span class="sxs-lookup"><span data-stu-id="3dfa2-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="3dfa2-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3dfa2-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3dfa2-105">このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してログインできる、ASP.NET Core 2.2 アプリケーションを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="3dfa2-106">ここでは、[Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins)、および [Microsoft](xref:security/authentication/microsoft-logins) の各プロバイダーを対象に説明します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="3dfa2-107">他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+、Windows のソーシャル メディア アイコン](index/_static/social.png)

<span data-ttu-id="3dfa2-109">ユーザーが既存の資格情報でサインインできるようにすると、ユーザーにとって便利なだけでなく、サインイン プロセスを管理する複雑な処理の多くをサードパーティに移行できます。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="3dfa2-110">ソーシャル ログインによってトラフィックとユーザーの変換を促進する方法の例については、[Facebook](https://www.facebook.com/unsupportedbrowser) と [Twitter](https://dev.twitter.com/resources/case-studies) によるケース スタディを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3dfa2-111">新しい .NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="3dfa2-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="3dfa2-112">Visual Studio 2017 のスタート ページから、または **[ファイル]** > **[新規作成]** > **[プロジェクト]** の順に選択して、新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="3dfa2-113">**[Visual C#]** > **[.NET Core]** カテゴリにある **[ASP.NET Core Web アプリケーション]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="3dfa2-114">**[認証の変更]** を選択し、認証を **[個人のユーザー アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="3dfa2-115">移行を適用する</span><span class="sxs-lookup"><span data-stu-id="3dfa2-115">Apply migrations</span></span>

* <span data-ttu-id="3dfa2-116">アプリを実行し、**[登録]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="3dfa2-117">新しいアカウントの電子メール アドレスとパスワードを入力し、**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="3dfa2-118">指示に従って移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="3dfa2-119">SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する</span><span class="sxs-lookup"><span data-stu-id="3dfa2-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="3dfa2-120">ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="3dfa2-121">完全なトークン名はプロバイダーにより異なります。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-121">The exact token names vary by provider.</span></span> <span data-ttu-id="3dfa2-122">これらのトークンは、アプリが API にアクセスするために使用する資格情報を示します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="3dfa2-123">トークンは、[Secret Manager](xref:security/app-secrets#secret-manager) のヘルプにより、アプリの構成にリンクすることが可能な "シークレット" になります。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="3dfa2-124">Secret Manager は、*appsettings.json* などの構成ファイルのトークンに格納される、セキュリティがさらに向上した方法です。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3dfa2-125">Secret Manager は、開発目的のみのためのものです。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="3dfa2-126">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)により、Azure テストと運用のシークレットを格納し、保護することが可能です。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="3dfa2-127">「[Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="3dfa2-128">アプリケーションに必要なログイン プロバイダーをセットアップする</span><span class="sxs-lookup"><span data-stu-id="3dfa2-128">Setup login providers required by your application</span></span>

<span data-ttu-id="3dfa2-129">各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="3dfa2-130">[Facebook](xref:security/authentication/facebook-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="3dfa2-131">[Twitter](xref:security/authentication/twitter-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="3dfa2-132">[Google](xref:security/authentication/google-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="3dfa2-133">[Microsoft](xref:security/authentication/microsoft-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="3dfa2-134">[その他のプロバイダー](xref:security/authentication/otherlogins)の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="3dfa2-135">必要に応じてパスワードを設定する</span><span class="sxs-lookup"><span data-stu-id="3dfa2-135">Optionally set password</span></span>

<span data-ttu-id="3dfa2-136">外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="3dfa2-137">そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="3dfa2-138">外部ログイン プロバイダーを使用できない場合、Web サイトにログインすることができません。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="3dfa2-139">外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="3dfa2-140">右上にある **[Hello &lt; 電子メール エイリアス&gt;]** リンクを選択して**管理**ビューに移動します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* <span data-ttu-id="3dfa2-142">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-142">Select **Create**</span></span>

![パスワード ページを設定する](index/_static/pass2a.png)

* <span data-ttu-id="3dfa2-144">有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dfa2-145">次の手順</span><span class="sxs-lookup"><span data-stu-id="3dfa2-145">Next steps</span></span>

* <span data-ttu-id="3dfa2-146">このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="3dfa2-147">アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="3dfa2-148">ユーザーとそのアクセス トークン許可および更新トークンに関する追加のデータを保持することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="3dfa2-149">詳細については、「<xref:security/authentication/social/additional-claims>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3dfa2-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
