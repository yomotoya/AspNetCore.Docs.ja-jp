---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトの外部の ASP.NET web サイトを使用してログ記録 |Microsoft Docs
author: tfitzmac
description: この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サポートする方法.
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a74b13e9d1ddb5bc02f4ea5184108de5e014ead0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825855"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d9039-103">ASP.NET Web ページ (Razor) サイトで外部のサイトを使用したログイン</span><span class="sxs-lookup"><span data-stu-id="d9039-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d9039-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9039-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9039-105">この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サイトの OAuth および OpenID をサポートする方法。</span><span class="sxs-lookup"><span data-stu-id="d9039-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="d9039-106">学習内容。</span><span class="sxs-lookup"><span data-stu-id="d9039-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d9039-107">WebMatrix のスターター サイト テンプレートを使用する場合は、他のサイトからのログインを有効にする方法。</span><span class="sxs-lookup"><span data-stu-id="d9039-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="d9039-108">これは、この記事で導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="d9039-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="d9039-109">`OAuthWebSecurity`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="d9039-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d9039-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="d9039-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d9039-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d9039-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d9039-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="d9039-112">WebMatrix 3</span></span>

<span data-ttu-id="d9039-113">ASP.NET Web Pages にはサポートが含まれています[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="d9039-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="d9039-114">これらのプロバイダーを使用して、Facebook、Twitter、Microsoft、および Google から既存の資格情報を使用して、サイトに、ユーザーがログインをさせることができます。</span><span class="sxs-lookup"><span data-stu-id="d9039-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="d9039-115">たとえば、Facebook アカウントを使用してログイン、ユーザーことができます選択 Facebook アイコンは、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d9039-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="d9039-116">サイトでユーザーのアカウント、Facebook ログインに関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="d9039-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="d9039-117">Web ページのメンバーシップ機能に関連する拡張機能は、web サイトの 1 つのアカウントを持つユーザーが複数回のログイン (ソーシャル ネットワーク サイトからのログインを含む) を関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="d9039-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="d9039-118">この図は、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、Google、Microsoft のアイコンを選択できます。</span><span class="sxs-lookup"><span data-stu-id="d9039-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![外部プロバイダー](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="d9039-120">数行のコードのコメントを解除して OAuth と OpenID のメンバーシップを有効にすることができます、**スターター サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="d9039-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="d9039-121">OAuth を使用するように使用するメソッドとプロパティおよび OpenID プロバイダーはで、`WebMatrix.Security.OAuthWebSecurity`クラス。</span><span class="sxs-lookup"><span data-stu-id="d9039-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="d9039-122">**スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースでは、すべてのコードと完了する必要がありますをローカルの資格情報または別のサイトからこれらのいずれかを使用してサイトに、ユーザーがログインできるようにするには.</span><span class="sxs-lookup"><span data-stu-id="d9039-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="d9039-123">このセクションがユーザーに基づいているサイト外部のサイトからにログインできるようにする方法の例では、**スターター サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="d9039-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="d9039-124">スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。</span><span class="sxs-lookup"><span data-stu-id="d9039-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="d9039-125">OAuth プロバイダー (Facebook、Twitter、および Microsoft) を使用するサイトでは、外部のサイトにアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d9039-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="d9039-126">これにより、それらのサイトのログイン機能を起動するために必要となるアプリケーションのキー。</span><span class="sxs-lookup"><span data-stu-id="d9039-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="d9039-127">OpenID プロバイダー (Google) を使用して、サイトでは、アプリケーションを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d9039-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="d9039-128">これらのサイトのすべてにログインするために、開発者のアプリケーションを作成して、アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="d9039-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d9039-129">Microsoft アプリケーションでは、ログインをテストするため、ローカルの web サイトの URL を使用することはできませんので、作業用の web サイトのライブの URL のみ受け入れます。</span><span class="sxs-lookup"><span data-stu-id="d9039-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="d9039-130">適切な認証プロバイダーを指定するには、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。</span><span class="sxs-lookup"><span data-stu-id="d9039-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="d9039-131">この記事では、次のタスクを個別の手順では。</span><span class="sxs-lookup"><span data-stu-id="d9039-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="d9039-132">Google ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="d9039-133">Facebook ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="d9039-134">Twitter ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="d9039-135">Google ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="d9039-136">作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="d9039-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="d9039-137">開く、  *\_AppStart.cshtml*ページし、次のコード行をコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="d9039-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="d9039-138">Google ログインをテストします。</span><span class="sxs-lookup"><span data-stu-id="d9039-138">Testing Google login</span></span>

1. <span data-ttu-id="d9039-139">実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="d9039-140">*ログイン*] ページの [、**別のサービスを使用してログイン**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="d9039-141">この例では、Google ログインを使用します。</span><span class="sxs-lookup"><span data-stu-id="d9039-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="d9039-142">Web ページは、Google ログイン ページに、要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d9039-142">The web page redirects the request to the Google login page.</span></span>

    ![Google のサインイン](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="d9039-144">既存の Google アカウントの資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="d9039-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="d9039-145">Google は、許可するかどうかを確認する場合*Localhost* 、アカウントから情報を使用する をクリックして**許可**します。</span><span class="sxs-lookup"><span data-stu-id="d9039-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="d9039-146">コードでは、Google トークンを使用して、ユーザーを認証して、web サイトにこのページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="d9039-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="d9039-147">このページでは、Google ログインを web サイト、既存のアカウントに関連付けるユーザーまたはで外部ログインに関連付けるサイトで新しいアカウントを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="d9039-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="d9039-149">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-149">Choose the **Associate** button.</span></span> <span data-ttu-id="d9039-150">ブラウザーは、アプリケーションのホーム ページを返します。</span><span class="sxs-lookup"><span data-stu-id="d9039-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="d9039-151">Facebook ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="d9039-152">移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(まだログインしている場合ログ記録)。</span><span class="sxs-lookup"><span data-stu-id="d9039-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="d9039-153">選択、 **Create New App**ボタンをクリックし、新しいアプリケーションを作成して名前を指示に従います。</span><span class="sxs-lookup"><span data-stu-id="d9039-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="d9039-154">セクションで**アプリを Facebook と統合する方法を選択します。**、選択、 **web サイト**セクション。</span><span class="sxs-lookup"><span data-stu-id="d9039-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="d9039-155">入力、**サイトの URL**フィールドに、サイトの URL (たとえば、 `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="d9039-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="d9039-156">**ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。</span><span class="sxs-lookup"><span data-stu-id="d9039-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="d9039-157">などの URL を使用してローカル コンピューターにサイトを実行している場合`http://localhost:12345`(番号が、ローカル ポート番号)、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="d9039-158">ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイトの URL**アプリケーションのフィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="d9039-159">選択、**変更の保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="d9039-160">選択、**アプリ**タブをもう一度、し、アプリケーションのスタート ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="d9039-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="d9039-161">コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d9039-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="d9039-162">これらの値は、web サイトのコードに Facebook プロバイダーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="d9039-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="d9039-163">Facebook の開発者向けサイトを終了します。</span><span class="sxs-lookup"><span data-stu-id="d9039-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="d9039-164">今すぐに変更を加える 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用してサイトにログインすること。</span><span class="sxs-lookup"><span data-stu-id="d9039-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="d9039-165">作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="d9039-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="d9039-166">開く、  *\_AppStart.cshtml*ページし、Facebook OAuth プロバイダーのコードをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="d9039-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="d9039-167">コメント解除されたコード ブロックは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d9039-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="d9039-168">コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、`appId`パラメーター (引用符を除く)。</span><span class="sxs-lookup"><span data-stu-id="d9039-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="d9039-169">コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`appSecret`パラメーターの値。</span><span class="sxs-lookup"><span data-stu-id="d9039-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="d9039-170">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9039-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="d9039-171">Facebook ログインをテストします。</span><span class="sxs-lookup"><span data-stu-id="d9039-171">Testing Facebook login</span></span>

1. <span data-ttu-id="d9039-172">サイトの実行*default.cshtml*ページ、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="d9039-173">*ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Facebook**アイコン。</span><span class="sxs-lookup"><span data-stu-id="d9039-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="d9039-174">Web ページは、Facebook のログイン ページに、要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d9039-174">The web page redirects the request to the Facebook login page.</span></span>

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="d9039-176">Facebook アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="d9039-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="d9039-177">コードでは、Facebook トークンを使用して、認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますをページに返します。</span><span class="sxs-lookup"><span data-stu-id="d9039-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="d9039-178">ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="d9039-180">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="d9039-181">ブラウザーがホーム ページに戻るしに記録されます。</span><span class="sxs-lookup"><span data-stu-id="d9039-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="d9039-182">Twitter ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d9039-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="d9039-183">参照、 [Twitter 開発者サイト](https://dev.twitter.com/)します。</span><span class="sxs-lookup"><span data-stu-id="d9039-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="d9039-184">選択、**アプリを作成する**リンクし、サイトにログインします。</span><span class="sxs-lookup"><span data-stu-id="d9039-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="d9039-185">**アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="d9039-186">**Web サイト**フィールドに、サイトの URL を入力します (たとえば、 `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="d9039-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="d9039-187">サイトをローカルでテストする場合 (ような URL を使用して`http://localhost:12345`)、Twitter の URL を受け付けないことがあります。</span><span class="sxs-lookup"><span data-stu-id="d9039-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="d9039-188">ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。</span><span class="sxs-lookup"><span data-stu-id="d9039-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="d9039-189">これには、アプリケーションをローカルでのテストのプロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="d9039-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="d9039-190">ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新する、 **web サイト**アプリケーションのフィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="d9039-191">**コールバック URL**フィールドに、ユーザーが Twitter にログインした後に戻り、web サイトのページの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="d9039-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="d9039-192">たとえば、スターター サイト (これは、ログイン ステータスが認識されます) のホーム ページにユーザーを送信するで入力したのと同じ URL を入力、 **web サイト**フィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="d9039-193">条項に同意し、選択、 **Twitter アプリケーションを作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="d9039-194">**マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d9039-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="d9039-195">**詳細** タブで、下部までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="d9039-196">**詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d9039-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="d9039-197">Web サイトのコードで、Twitter プロバイダーにこれらの値を渡すします。</span><span class="sxs-lookup"><span data-stu-id="d9039-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="d9039-198">Twitter サイトを終了します。</span><span class="sxs-lookup"><span data-stu-id="d9039-198">Exit the Twitter site.</span></span>

<span data-ttu-id="d9039-199">今すぐに変更を加える 2 つのページ、web サイトでユーザーが自分の Twitter アカウントを使用してサイトにログインできるようにします。</span><span class="sxs-lookup"><span data-stu-id="d9039-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="d9039-200">作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="d9039-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="d9039-201">開く、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードをコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="d9039-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="d9039-202">コメント解除されたコード ブロックのようになります。</span><span class="sxs-lookup"><span data-stu-id="d9039-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="d9039-203">コピー、**コンシューマー キー**の値としての Twitter アプリケーションからの値、`consumerKey`パラメーター (引用符を除く)。</span><span class="sxs-lookup"><span data-stu-id="d9039-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="d9039-204">コピー、**コンシューマー シークレット**の値としての Twitter アプリケーションからの値、`consumerSecret`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="d9039-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="d9039-205">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="d9039-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="d9039-206">Twitter ログインをテストします。</span><span class="sxs-lookup"><span data-stu-id="d9039-206">Testing Twitter login</span></span>

1. <span data-ttu-id="d9039-207">実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="d9039-208">*ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Twitter**アイコン。</span><span class="sxs-lookup"><span data-stu-id="d9039-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="d9039-209">Web ページは、要求を作成したアプリケーションの Twitter ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d9039-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![oauth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="d9039-211">Twitter アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="d9039-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="d9039-212">コードは、ユーザーの認証に Twitter のトークンを使用し、返すページに関連付けることができます、web サイトのアカウントでログイン。</span><span class="sxs-lookup"><span data-stu-id="d9039-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="d9039-213">名前または電子メール アドレスを格納、**電子メール**フォームのフィールド。</span><span class="sxs-lookup"><span data-stu-id="d9039-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="d9039-215">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9039-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="d9039-216">ブラウザーがホーム ページに戻るしに記録されます。</span><span class="sxs-lookup"><span data-stu-id="d9039-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d9039-217">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="d9039-217">Additional Resources</span></span>


- [<span data-ttu-id="d9039-218">サイト全体の動作をカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="d9039-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="d9039-219">ASP.NET Web ページ サイトへのセキュリティとメンバーシップの追加</span><span class="sxs-lookup"><span data-stu-id="d9039-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
