---
title: "Facebook、Google、および他の外部プロバイダーを使用する認証の有効化"
author: rick-anderson
description: "このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。"
keywords: "ASP.NET Core, 認証, ソーシャル, 認証プロバイダー, google, facebook, twitter, microsoft アカウント"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9cc637f469dcb7097ee1b3996fde8a4ebac8d7ff
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="6a629-104">Facebook、Google、および他の外部プロバイダーを使用する認証の有効化</span><span class="sxs-lookup"><span data-stu-id="6a629-104">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="6a629-105">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a629-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6a629-106">このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してログインできる、ASP.NET Core 2.x アプリケーションを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6a629-106">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="6a629-107">ここでは、[Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md)、および [Microsoft](microsoft-logins.md) の各プロバイダーを対象に説明します。</span><span class="sxs-lookup"><span data-stu-id="6a629-107">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="6a629-108">他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="6a629-108">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+、Windows のソーシャル メディア アイコン](index/_static/social.png)

<span data-ttu-id="6a629-110">ユーザーが既存の資格情報でサインインできるようにすると、ユーザーにとって便利なだけでなく、サインイン プロセスを管理する複雑な処理の多くをサードパーティに移行できます。</span><span class="sxs-lookup"><span data-stu-id="6a629-110">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="6a629-111">ソーシャル ログインによってトラフィックとユーザーの変換を促進する方法の例については、[Facebook](https://www.facebook.com/unsupportedbrowser) と [Twitter](https://dev.twitter.com/resources/case-studies) によるケース スタディを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6a629-111">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="6a629-112">注: ここで紹介するパッケージでは、OAuth 認証フローの複雑な処理の多くを抽象化していますが、トラブルシューティングの際には詳細を理解することが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="6a629-112">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="6a629-113">利用できる資料は多数あります。たとえば、「[Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)」(OAuth 2 の紹介) や「[Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/)」(OAuth 2 の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6a629-113">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="6a629-114">「[ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src)」(プロバイダー パッケージ用の ASP.NET Core ソース コード) を参照すると、いくつかの問題を解決できます。</span><span class="sxs-lookup"><span data-stu-id="6a629-114">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6a629-115">新しい .NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="6a629-115">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="6a629-116">Visual Studio 2017 のスタート ページから、または **[ファイル]、[新規作成]、[プロジェクト]** の順に選択して、新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6a629-116">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="6a629-117">**[Visual C#] > [.NET Core]** カテゴリにある **[ASP.NET Core Web アプリケーション]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="6a629-117">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![[新しいプロジェクト] ダイアログ](index/_static/new-project.png)

* <span data-ttu-id="6a629-119">**[Web アプリケーション]** をタップし、**[認証]** が **[個人のユーザー アカウント]** に設定されていることを確認します</span><span class="sxs-lookup"><span data-stu-id="6a629-119">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![[新しい Web アプリケーションの作成] ダイアログ](index/_static/select-project.png)

<span data-ttu-id="6a629-121">注: このチュートリアルは、ASP.NET Core 2.0 SDK バージョンに適用されます。バージョンはウィザードの上部で選択できます。</span><span class="sxs-lookup"><span data-stu-id="6a629-121">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="6a629-122">移行を適用する</span><span class="sxs-lookup"><span data-stu-id="6a629-122">Apply migrations</span></span>

* <span data-ttu-id="6a629-123">アプリを実行して **[ログイン]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="6a629-123">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="6a629-124">**[新規ユーザーとして登録する]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="6a629-124">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="6a629-125">新しいアカウントの電子メール アドレスとパスワードを入力し、**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6a629-125">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="6a629-126">指示に従って移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="6a629-126">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6a629-127">SSL を必須にする</span><span class="sxs-lookup"><span data-stu-id="6a629-127">Require SSL</span></span>

<span data-ttu-id="6a629-128">OAuth 2.0 では、HTTPS プロトコル経由での認証に SSL を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6a629-128">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="6a629-129">注: 上の図のようにプロジェクト ウィザードの **[認証の変更]** ダイアログで **[個人のユーザー アカウント]** オプションを選択している場合、ASP.NET Core 2.x 用の **[Web アプリケーション]** または **[Web API]** プロジェクト テンプレートを使用して作成されたプロジェクトは、SSL を有効にし、https URL を使用して起動するように自動的に構成されます。</span><span class="sxs-lookup"><span data-stu-id="6a629-129">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="6a629-130">[「ASP.NET Core アプリケーションで SSL を適用します」](xref:security/enforcing-ssl)トピックの手順に従ってサイトで SSL を必須にします。</span><span class="sxs-lookup"><span data-stu-id="6a629-130">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="6a629-131">SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する</span><span class="sxs-lookup"><span data-stu-id="6a629-131">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="6a629-132">ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます (正確な名前はプロバイダーによって異なります)。</span><span class="sxs-lookup"><span data-stu-id="6a629-132">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="6a629-133">これらの値は、実質的に、アプリケーションが API へのアクセスに使用する*ユーザー名*と*パスワード*です。また、構成ファイルに直接格納したり、ハードコーディングしたりする代わりに、**シークレット マネージャー**のサポートによりアプリケーションの構成にリンクできる "シークレット" を構成します。</span><span class="sxs-lookup"><span data-stu-id="6a629-133">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="6a629-134">「[Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリケーションのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。</span><span class="sxs-lookup"><span data-stu-id="6a629-134">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="6a629-135">アプリケーションに必要なログイン プロバイダーをセットアップする</span><span class="sxs-lookup"><span data-stu-id="6a629-135">Setup login providers required by your application</span></span>

<span data-ttu-id="6a629-136">各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6a629-136">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="6a629-137">[Facebook](facebook-logins.md) の手順</span><span class="sxs-lookup"><span data-stu-id="6a629-137">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="6a629-138">[Twitter](twitter-logins.md) の手順</span><span class="sxs-lookup"><span data-stu-id="6a629-138">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="6a629-139">[Google](google-logins.md) の手順</span><span class="sxs-lookup"><span data-stu-id="6a629-139">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="6a629-140">[Microsoft](microsoft-logins.md) の手順</span><span class="sxs-lookup"><span data-stu-id="6a629-140">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="6a629-141">[その他のプロバイダー](other-logins.md)の手順</span><span class="sxs-lookup"><span data-stu-id="6a629-141">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="6a629-142">必要に応じてパスワードを設定する</span><span class="sxs-lookup"><span data-stu-id="6a629-142">Optionally set password</span></span>

<span data-ttu-id="6a629-143">外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。</span><span class="sxs-lookup"><span data-stu-id="6a629-143">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="6a629-144">そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。</span><span class="sxs-lookup"><span data-stu-id="6a629-144">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="6a629-145">外部ログイン プロバイダーを使用できない場合、Web サイトにログインすることができません。</span><span class="sxs-lookup"><span data-stu-id="6a629-145">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="6a629-146">外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="6a629-146">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="6a629-147">右上にある **[Hello]<email alias>** リンクをタップして**管理**ビューに移動します。</span><span class="sxs-lookup"><span data-stu-id="6a629-147">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* <span data-ttu-id="6a629-149">**[作成]** をタップします</span><span class="sxs-lookup"><span data-stu-id="6a629-149">Tap **Create**</span></span>

![パスワード ページを設定する](index/_static/pass2a.png)

* <span data-ttu-id="6a629-151">有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。</span><span class="sxs-lookup"><span data-stu-id="6a629-151">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a629-152">次のステップ</span><span class="sxs-lookup"><span data-stu-id="6a629-152">Next steps</span></span>

* <span data-ttu-id="6a629-153">このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。</span><span class="sxs-lookup"><span data-stu-id="6a629-153">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="6a629-154">アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6a629-154">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
