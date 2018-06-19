---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトの ASP.NET Web 内の外部のサイトを使用してログイン |Microsoft ドキュメント
author: tfitzmac
description: この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して、ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サポートする方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530171"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ee692-103">ASP.NET Web Pages (Razor) サイトの外部のサイトを使用してログイン</span><span class="sxs-lookup"><span data-stu-id="ee692-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ee692-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ee692-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ee692-105">この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して、ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サイトで OAuth および OpenID をサポートする方法です。</span><span class="sxs-lookup"><span data-stu-id="ee692-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="ee692-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="ee692-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ee692-107">WebMatrix のスターター サイト テンプレートを使用するときに、他のサイトからのログインを有効にする方法です。</span><span class="sxs-lookup"><span data-stu-id="ee692-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="ee692-108">これは、アーティクルで導入された ASP.NET の機能です。</span><span class="sxs-lookup"><span data-stu-id="ee692-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="ee692-109">`OAuthWebSecurity`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ee692-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee692-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="ee692-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ee692-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ee692-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ee692-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="ee692-112">WebMatrix 3</span></span>

<span data-ttu-id="ee692-113">ASP.NET Web Pages にはサポートが含まれています[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="ee692-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="ee692-114">これらのプロバイダーを使用して、Facebook、Twitter、Microsoft と Google からの既存の資格情報を使用して、サイトにユーザーがログインをさせることができます。</span><span class="sxs-lookup"><span data-stu-id="ee692-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="ee692-115">たとえば、Facebook アカウントを使用してログイン、ユーザーだけを選択できます Facebook アイコンには、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ee692-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="ee692-116">Facebook ログインをサイトで各自のアカウント、関連付けることができますし、します。</span><span class="sxs-lookup"><span data-stu-id="ee692-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="ee692-117">Web ページのメンバーシップ機能に関連する拡張機能です。 ユーザーが (ソーシャル ネットワー キング サイトからのログインを含む) 複数のログインに関連付けることができる web サイトで 1 つのアカウント。</span><span class="sxs-lookup"><span data-stu-id="ee692-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="ee692-118">この図では、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、Google、または Microsoft のアイコンを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ee692-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![外部プロバイダー](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="ee692-120">数行のコードのコメントを解除して OAuth および OpenID のメンバーシップを有効にすることができます、**スターター サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="ee692-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="ee692-121">メソッドとプロパティを使用する作業で、OAuth および OpenID プロバイダーでは、`WebMatrix.Security.OAuthWebSecurity`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee692-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="ee692-122">**スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースのすべてのコードと完了する必要がローカルの資格情報または別のサイトからこれらを使用してサイトにユーザーがログインを使用できます.</span><span class="sxs-lookup"><span data-stu-id="ee692-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="ee692-123">このセクションでは、外部サイトから基になっているサイトにログインできるようにする方法の例を示します、**スターター サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="ee692-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="ee692-124">スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。</span><span class="sxs-lookup"><span data-stu-id="ee692-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="ee692-125">OAuth プロバイダー (Facebook、Twitter、および Microsoft) を使用しているサイトでは、外部のサイトにアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee692-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="ee692-126">これにより、それらのサイトのログインの機能を実行するために必要となるアプリケーション キー。</span><span class="sxs-lookup"><span data-stu-id="ee692-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="ee692-127">OpenID プロバイダー (Google) を使用するサイトでは、アプリケーションを作成するはありません。</span><span class="sxs-lookup"><span data-stu-id="ee692-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="ee692-128">ログインするために、開発者向けアプリケーションを作成して、これらのサイトのすべてのアカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="ee692-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee692-129">Microsoft アプリケーションでは、作業用の web サイトの URL をライブのみ承認し、ログインをテストするため、ローカルの web サイトの URL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ee692-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="ee692-130">適切な認証プロバイダーを指定するために、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。</span><span class="sxs-lookup"><span data-stu-id="ee692-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="ee692-131">この記事では、次のタスクを個別の手順では。</span><span class="sxs-lookup"><span data-stu-id="ee692-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="ee692-132">Google ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="ee692-133">Facebook ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="ee692-134">Twitter ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="ee692-135">Google ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="ee692-136">作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee692-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ee692-137">開く、  *\_AppStart.cshtml*ページし、次のコード行のコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="ee692-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="ee692-138">Google ログインのテスト</span><span class="sxs-lookup"><span data-stu-id="ee692-138">Testing Google login</span></span>

1. <span data-ttu-id="ee692-139">実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="ee692-140">*ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタン。</span><span class="sxs-lookup"><span data-stu-id="ee692-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="ee692-141">この例では、Google ログインを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee692-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="ee692-142">Web ページは、Google ログイン ページに、要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ee692-142">The web page redirects the request to the Google login page.</span></span>

    ![Google のサインイン](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="ee692-144">既存の Google アカウントの資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee692-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="ee692-145">Google は、許可するかどうかを求める場合*Localhost*情報を使用して、アカウントから、をクリックして**許可**です。</span><span class="sxs-lookup"><span data-stu-id="ee692-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="ee692-146">コードでは、Google トークンを使用して、ユーザーを認証し、web サイトでこのページに返します。</span><span class="sxs-lookup"><span data-stu-id="ee692-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="ee692-147">このページでは、Google ログインを web サイト上の既存のアカウントに関連付けるユーザーまたは新しいアカウントに外部ログインとを関連付けるには、サイトに登録します。</span><span class="sxs-lookup"><span data-stu-id="ee692-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="ee692-149">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-149">Choose the **Associate** button.</span></span> <span data-ttu-id="ee692-150">ブラウザーは、アプリケーションのホーム ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="ee692-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="ee692-151">Facebook ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="ee692-152">移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(ログインされていないログインしている場合)。</span><span class="sxs-lookup"><span data-stu-id="ee692-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="ee692-153">選択、 **Create New App**ボタンをクリックし、指示に従って名前を指定し、新しいアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee692-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="ee692-154">セクションで**アプリを Facebook と統合する方法を選択**、選択、 **web サイト**セクションです。</span><span class="sxs-lookup"><span data-stu-id="ee692-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="ee692-155">入力、**サイトの URL**フィールドをサイトの URL (たとえば、 `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="ee692-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="ee692-156">**ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。</span><span class="sxs-lookup"><span data-stu-id="ee692-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ee692-157">URL を使用して、ローカル コンピューターにサイトを実行している場合と同様に`http://localhost:12345`(数は、ローカル ポート番号)、するには、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="ee692-158">ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイト URL**アプリケーションのフィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="ee692-159">選択、 **Save Changes**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="ee692-160">選択、**アプリ**もう一度、タブし、アプリケーションのスタート ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="ee692-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="ee692-161">コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ee692-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ee692-162">これらの値は、web サイトのコードで Facebook プロバイダーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ee692-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="ee692-163">Facebook の開発者向けサイトを終了します。</span><span class="sxs-lookup"><span data-stu-id="ee692-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="ee692-164">今すぐを変更する 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用して、サイトにログインすることです。</span><span class="sxs-lookup"><span data-stu-id="ee692-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="ee692-165">作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee692-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ee692-166">開く、  *\_AppStart.cshtml*ページおよび Facebook OAuth プロバイダーのコードのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="ee692-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="ee692-167">コメント解除されたコード ブロックは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee692-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="ee692-168">コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、 `appId` (引用符を除く) のパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ee692-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ee692-169">コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`appSecret`パラメーターの値。</span><span class="sxs-lookup"><span data-stu-id="ee692-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="ee692-170">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="ee692-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="ee692-171">Facebook ログインをテストします。</span><span class="sxs-lookup"><span data-stu-id="ee692-171">Testing Facebook login</span></span>

1. <span data-ttu-id="ee692-172">サイトの実行*default.cshtml*ページし、選択、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="ee692-173">*ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Facebook**アイコン。</span><span class="sxs-lookup"><span data-stu-id="ee692-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="ee692-174">Web ページは、Facebook のログイン ページに、要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ee692-174">The web page redirects the request to the Facebook login page.</span></span>

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="ee692-176">Facebook アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="ee692-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="ee692-177">コードでは、Facebook トークンを使用してユーザーを認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますページに返します。</span><span class="sxs-lookup"><span data-stu-id="ee692-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="ee692-178">ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="ee692-180">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ee692-181">ブラウザーのホーム ページに返しに記録されます。</span><span class="sxs-lookup"><span data-stu-id="ee692-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="ee692-182">Twitter ログインを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ee692-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="ee692-183">参照、 [Twitter 開発者サイト](https://dev.twitter.com/)です。</span><span class="sxs-lookup"><span data-stu-id="ee692-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="ee692-184">選択、**アプリの作成**リンクし、サイトにログインします。</span><span class="sxs-lookup"><span data-stu-id="ee692-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="ee692-185">**アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="ee692-186">**Web サイト**フィールドに、サイトの URL を入力してください (たとえば、 `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="ee692-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ee692-187">ローカル サイトをテストする場合は (のように URL を使用して`http://localhost:12345`)、Twitter、URL を受け付けないことがあります。</span><span class="sxs-lookup"><span data-stu-id="ee692-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="ee692-188">ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。</span><span class="sxs-lookup"><span data-stu-id="ee692-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="ee692-189">これには、アプリケーションをローカルでのテストのプロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="ee692-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="ee692-190">ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新、 **web サイト**アプリケーションのフィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="ee692-191">**コールバック URL**フィールドに、ユーザーが Twitter にログインした後に返される web サイト内のページの URL を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee692-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="ee692-192">たとえば、ユーザーを (それらのログインの状態が認識されます) をスターター サイトのホーム ページに送信 を同じ URL を入力に入力した、 **web サイト**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="ee692-193">条項に同意し、選択、 **、Twitter アプリケーションを作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="ee692-194">**マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="ee692-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="ee692-195">**詳細** タブで、一番下までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="ee692-196">**詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ee692-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="ee692-197">これらの値は、web サイトのコードで Twitter プロバイダーに渡すします。</span><span class="sxs-lookup"><span data-stu-id="ee692-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="ee692-198">Twitter のサイトを終了します。</span><span class="sxs-lookup"><span data-stu-id="ee692-198">Exit the Twitter site.</span></span>

<span data-ttu-id="ee692-199">今すぐを変更する 2 つのページ、web サイトのユーザーが自分の Twitter アカウントを使用して、サイトにログインできるようにします。</span><span class="sxs-lookup"><span data-stu-id="ee692-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="ee692-200">作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="ee692-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="ee692-201">開く、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="ee692-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="ee692-202">コメント解除されたコード ブロックは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee692-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="ee692-203">コピー、**コンシューマー キー**の値として Twitter アプリケーションからの値、 `consumerKey` (引用符を除く) のパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="ee692-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="ee692-204">コピー、**コンシューマー シークレット**の値として Twitter アプリケーションからの値、`consumerSecret`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ee692-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="ee692-205">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="ee692-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="ee692-206">Twitter ログインのテスト</span><span class="sxs-lookup"><span data-stu-id="ee692-206">Testing Twitter login</span></span>

1. <span data-ttu-id="ee692-207">実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="ee692-208">*ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Twitter**アイコン。</span><span class="sxs-lookup"><span data-stu-id="ee692-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="ee692-209">Web ページは、作成したアプリケーションの Twitter ログイン ページに、要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ee692-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![oauth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="ee692-211">Twitter アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="ee692-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="ee692-212">コードは、Twitter のトークンを使用してユーザーを認証し、返すページに関連付けることができます、web サイトのアカウントでログイン。</span><span class="sxs-lookup"><span data-stu-id="ee692-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="ee692-213">名前や電子メール アドレスを格納、**電子メール**フォームのフィールドです。</span><span class="sxs-lookup"><span data-stu-id="ee692-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="ee692-215">選択、**関連付ける**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee692-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="ee692-216">ブラウザーのホーム ページに返しに記録されます。</span><span class="sxs-lookup"><span data-stu-id="ee692-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ee692-217">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ee692-217">Additional Resources</span></span>


- [<span data-ttu-id="ee692-218">サイト全体の動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ee692-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="ee692-219">ASP.NET Web Pages サイトへのセキュリティとメンバーシップの追加</span><span class="sxs-lookup"><span data-stu-id="ee692-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
