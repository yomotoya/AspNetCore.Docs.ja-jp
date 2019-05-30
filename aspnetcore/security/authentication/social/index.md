---
title: Facebook、Google、ASP.NET Core での外部プロバイダーの認証
author: rick-anderson
description: このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451047"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="885ce-103">Facebook、Google、ASP.NET Core での外部プロバイダーの認証</span><span class="sxs-lookup"><span data-stu-id="885ce-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="885ce-104">作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="885ce-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="885ce-105">このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してサインインできる、ASP.NET Core 2.2 アプリケーションを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="885ce-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="885ce-106">ここでは、[Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins)、および [Microsoft](xref:security/authentication/microsoft-logins) の各プロバイダーを対象に説明します。</span><span class="sxs-lookup"><span data-stu-id="885ce-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="885ce-107">他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="885ce-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google+、Windows のソーシャル メディア アイコン](index/_static/social.png)

<span data-ttu-id="885ce-109">既存の資格情報でユーザーがサインインできるようになると: </span><span class="sxs-lookup"><span data-stu-id="885ce-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="885ce-110">ユーザーにとって便利です。</span><span class="sxs-lookup"><span data-stu-id="885ce-110">Is convenient for the users.</span></span>
* <span data-ttu-id="885ce-111">サインイン プロセスの複雑な管理の多くが、サード パーティに移ります。</span><span class="sxs-lookup"><span data-stu-id="885ce-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="885ce-112">ソーシャル ログインによってトラフィックとユーザーの変換を促進する方法の例については、[Facebook](https://www.facebook.com/unsupportedbrowser) と [Twitter](https://dev.twitter.com/resources/case-studies) によるケース スタディを参照してください。</span><span class="sxs-lookup"><span data-stu-id="885ce-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="885ce-113">新しい .NET Core プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="885ce-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="885ce-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="885ce-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="885ce-115">Visual Studio の **[ファイル]** メニューから、 **[新規作成]**  >  **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="885ce-116">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="885ce-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="885ce-117">ドロップダウン リストで **[ASP.NET Core 2.2]** を選択してから、 **[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="885ce-118">**[認証の変更]** を選択し、認証を **[個人のユーザー アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="885ce-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="885ce-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="885ce-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="885ce-120">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="885ce-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="885ce-121">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="885ce-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="885ce-122">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="885ce-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="885ce-123">`dotnet new` コマンドでは、*WebApp1* フォルダーに新しい Razor Pages プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="885ce-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="885ce-124">`-uld` では、SQLite ではなく LocalDB を使用します。</span><span class="sxs-lookup"><span data-stu-id="885ce-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="885ce-125">`-uld` を省略して SQLite を使用します。</span><span class="sxs-lookup"><span data-stu-id="885ce-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="885ce-126">`-au Individual` によって、個々の認証に対するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="885ce-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="885ce-127">`code` コマンドでは、Visual Studio Code の新しいインスタンス内に *WebApp1* フォルダーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="885ce-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="885ce-128">"**ビルドとデバッグに必要な資産が 'WebApp1' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="885ce-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="885ce-129">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="885ce-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="885ce-130">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="885ce-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="885ce-131">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="885ce-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="885ce-132">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="885ce-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="885ce-133">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="885ce-133">Open the project</span></span>

<span data-ttu-id="885ce-134">Visual Studio から、 **[ファイル]、[開く]** の順に選択し、*WebApp1.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="885ce-135">移行を適用する</span><span class="sxs-lookup"><span data-stu-id="885ce-135">Apply migrations</span></span>

* <span data-ttu-id="885ce-136">アプリを実行し、 **[登録]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="885ce-137">新しいアカウントの電子メール アドレスとパスワードを入力し、 **[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="885ce-138">指示に従って移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="885ce-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="885ce-139">SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する</span><span class="sxs-lookup"><span data-stu-id="885ce-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="885ce-140">ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="885ce-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="885ce-141">完全なトークン名はプロバイダーにより異なります。</span><span class="sxs-lookup"><span data-stu-id="885ce-141">The exact token names vary by provider.</span></span> <span data-ttu-id="885ce-142">これらのトークンは、アプリが API にアクセスするために使用する資格情報を示します。</span><span class="sxs-lookup"><span data-stu-id="885ce-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="885ce-143">トークンは、[Secret Manager](xref:security/app-secrets#secret-manager) のヘルプにより、アプリの構成にリンクすることが可能な "シークレット" になります。</span><span class="sxs-lookup"><span data-stu-id="885ce-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="885ce-144">Secret Manager は、*appsettings.json* などの構成ファイルのトークンに格納される、セキュリティがさらに向上した方法です。</span><span class="sxs-lookup"><span data-stu-id="885ce-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="885ce-145">Secret Manager は、開発目的のみのためのものです。</span><span class="sxs-lookup"><span data-stu-id="885ce-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="885ce-146">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)により、Azure テストと運用のシークレットを格納し、保護することが可能です。</span><span class="sxs-lookup"><span data-stu-id="885ce-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="885ce-147">「[Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。</span><span class="sxs-lookup"><span data-stu-id="885ce-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="885ce-148">アプリケーションに必要なログイン プロバイダーをセットアップする</span><span class="sxs-lookup"><span data-stu-id="885ce-148">Setup login providers required by your application</span></span>

<span data-ttu-id="885ce-149">各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="885ce-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="885ce-150">[Facebook](xref:security/authentication/facebook-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="885ce-151">[Twitter](xref:security/authentication/twitter-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="885ce-152">[Google](xref:security/authentication/google-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="885ce-153">[Microsoft](xref:security/authentication/microsoft-logins) の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="885ce-154">[その他のプロバイダー](xref:security/authentication/otherlogins)の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="885ce-155">必要に応じてパスワードを設定する</span><span class="sxs-lookup"><span data-stu-id="885ce-155">Optionally set password</span></span>

<span data-ttu-id="885ce-156">外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。</span><span class="sxs-lookup"><span data-stu-id="885ce-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="885ce-157">そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。</span><span class="sxs-lookup"><span data-stu-id="885ce-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="885ce-158">外部ログイン プロバイダーを使用できない場合、Web サイトにサインインすることができません。</span><span class="sxs-lookup"><span data-stu-id="885ce-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="885ce-159">外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="885ce-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="885ce-160">右上にある **[Hello &lt; 電子メール エイリアス&gt;]** リンクを選択して**管理**ビューに移動します。</span><span class="sxs-lookup"><span data-stu-id="885ce-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* <span data-ttu-id="885ce-162">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="885ce-162">Select **Create**</span></span>

![パスワード ページを設定する](index/_static/pass2a.png)

* <span data-ttu-id="885ce-164">有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。</span><span class="sxs-lookup"><span data-stu-id="885ce-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="885ce-165">次の手順</span><span class="sxs-lookup"><span data-stu-id="885ce-165">Next steps</span></span>

* <span data-ttu-id="885ce-166">このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。</span><span class="sxs-lookup"><span data-stu-id="885ce-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="885ce-167">アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="885ce-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="885ce-168">ユーザーとそのアクセス トークン許可および更新トークンに関する追加のデータを保持することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="885ce-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="885ce-169">詳細については、「<xref:security/authentication/social/additional-claims>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="885ce-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
