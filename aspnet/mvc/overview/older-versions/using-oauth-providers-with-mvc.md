---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: MVC 4 で OAuth プロバイダーの使用 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、ユーザー Facebo などの外部プロバイダーからの資格情報でログインできるようにする ASP.NET MVC 4 web アプリケーションを構築する方法を紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: b023d3d78bd703db93deb2e23661c9041fe74694
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372315"
---
<a name="using-oauth-providers-with-mvc-4"></a><span data-ttu-id="5d9c7-103">MVC 4 で OAuth プロバイダーを使用</span><span class="sxs-lookup"><span data-stu-id="5d9c7-103">Using OAuth Providers with MVC 4</span></span>
====================
<span data-ttu-id="5d9c7-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5d9c7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5d9c7-105">このチュートリアルでは、ユーザーが Facebook、Twitter、Microsoft、Google などの外部プロバイダーからの資格情報でログインし、これらのプロバイダーにから機能の一部を統合できる ASP.NET MVC 4 web アプリケーションを構築する方法、web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-105">This tutorial shows you how to build an ASP.NET MVC 4 web application that enables users to log in with credentials from an external provider, such as Facebook, Twitter, Microsoft, or Google, and then integrate some of the functionality from those providers into your web application.</span></span> <span data-ttu-id="5d9c7-106">わかりやすくするため、このチュートリアルは Facebook からの資格情報の操作に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-106">For simplicity, this tutorial focuses on working with credentials from Facebook.</span></span>
> 
> <span data-ttu-id="5d9c7-107">外部資格情報を使用して、ASP.NET MVC 5 web アプリケーションを参照してください。 [Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-107">To use external credentials in an ASP.NET MVC 5 web application, see [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
> 
> <span data-ttu-id="5d9c7-108">数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるため、大きな利点を提供して web サイトでこれらの資格情報を有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-108">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="5d9c7-109">これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-109">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span> <span data-ttu-id="5d9c7-110">また、これらのプロバイダーのいずれかで、ユーザーがログインに後、は、ソーシャル プロバイダーの操作を組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-110">Also, after a user has logged in through one of these providers, you can incorporate social operations from the provider.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="5d9c7-111">構築します</span><span class="sxs-lookup"><span data-stu-id="5d9c7-111">What you'll build</span></span>

<span data-ttu-id="5d9c7-112">このチュートリアルでは、2 つの主な目標があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-112">There are two main goals in this tutorial:</span></span>

1. <span data-ttu-id="5d9c7-113">OAuth プロバイダーからの資格情報でログインするユーザーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-113">Enable a user to log in with credentials from an OAuth provider.</span></span>
2. <span data-ttu-id="5d9c7-114">プロバイダーからのアカウント情報を取得し、サイトのアカウントの登録とその情報を統合します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-114">Retrieve account information from the provider and integrate that information with the account registration for your site.</span></span>

<span data-ttu-id="5d9c7-115">このチュートリアルの例では、認証プロバイダーとして Facebook を使用する焦点として、任意のプロバイダーを使用するコードを変更できます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-115">Although the examples in this tutorial focus on using Facebook as the authentication provider, you can modify the code to use any of the providers.</span></span> <span data-ttu-id="5d9c7-116">任意のプロバイダーを実装する手順は、このチュートリアルに表示される手順とよく似ています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-116">The steps to implement any provider are very similar to the steps you will see in this tutorial.</span></span> <span data-ttu-id="5d9c7-117">大きな違い、プロバイダーの API への直接呼び出しを行うときに設定のみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-117">You will only notice significant differences when making direct calls to the provider's API set.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d9c7-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5d9c7-118">Prerequisites</span></span>

- <span data-ttu-id="5d9c7-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)または[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span><span class="sxs-lookup"><span data-stu-id="5d9c7-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) or [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span></span>

<span data-ttu-id="5d9c7-120">または</span><span class="sxs-lookup"><span data-stu-id="5d9c7-120">Or</span></span>

- <span data-ttu-id="5d9c7-121">Microsoft Visual Studio 2010 SP1 または[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span><span class="sxs-lookup"><span data-stu-id="5d9c7-121">Microsoft Visual Studio 2010 SP1 or [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span></span>
- [<span data-ttu-id="5d9c7-122">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="5d9c7-122">ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)

<span data-ttu-id="5d9c7-123">さらに、このトピックでは、ASP.NET MVC と Visual Studio に関する基本的な知識がある前提としています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-123">Furthermore, this topic assumes you have basic knowledge about ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="5d9c7-124">ASP.NET MVC 4 の概要が必要な場合は、次を参照してください。 [ASP.NET MVC 4 の概要](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-124">If you need an introduction to ASP.NET MVC 4, see [Intro to ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).</span></span>

## <a name="create-the-project"></a><span data-ttu-id="5d9c7-125">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="5d9c7-125">Create the project</span></span>

<span data-ttu-id="5d9c7-126">Visual Studio で、新しい ASP.NET MVC 4 Web アプリケーションを作成し、名前を付けます&quot;OAuthMVC&quot;します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-126">In Visual Studio, create a new ASP.NET MVC 4 Web Application, and name it &quot;OAuthMVC&quot;.</span></span> <span data-ttu-id="5d9c7-127">.NET Framework 4.5 または 4 を対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-127">You can target either .NET Framework 4.5 or 4.</span></span>

![プロジェクトを作成します。](using-oauth-providers-with-mvc/_static/image1.png)

<span data-ttu-id="5d9c7-129">新しい ASP.NET MVC 4 プロジェクト ウィンドウで、次のように選択します。**インターネット アプリケーション**して**Razor**ビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-129">In the New ASP.NET MVC 4 Project window, select **Internet Application** and leave **Razor** as the view engine.</span></span>

![インターネット アプリケーションを選択します。](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a><span data-ttu-id="5d9c7-131">プロバイダーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-131">Enable a provider</span></span>

<span data-ttu-id="5d9c7-132">アプリで AuthConfig.cs という名前のファイル プロジェクトを作成するインターネット アプリケーション テンプレートを使用した MVC 4 web アプリケーションを作成するときに\_開始フォルダー。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-132">When you create an MVC 4 web application with the Internet Application template, the project is created with a file named AuthConfig.cs in the App\_Start folder.</span></span>

![AuthConfig ファイル](using-oauth-providers-with-mvc/_static/image3.png)

<span data-ttu-id="5d9c7-134">AuthConfig ファイルには、クライアントの外部認証プロバイダーを登録するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-134">The AuthConfig file contains code to register clients for external authentication providers.</span></span> <span data-ttu-id="5d9c7-135">既定では、このコードはコメント アウト、ので、外部プロバイダーがない、有効にします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-135">By default, this code is commented out, so none of the external providers are enabled.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

<span data-ttu-id="5d9c7-136">外部認証クライアントを使用するには、このコードのコメントを解除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-136">You must uncomment this code to use the external authentication client.</span></span> <span data-ttu-id="5d9c7-137">サイトに追加するプロバイダーのみのコメントを解除します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-137">You uncomment only the providers you want to include in your site.</span></span> <span data-ttu-id="5d9c7-138">このチュートリアルでは、Facebook の資格情報のみ有効にします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-138">For this tutorial, you will only enable the Facebook credentials.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

<span data-ttu-id="5d9c7-139">メソッドが、登録パラメーター用の空の文字列が含まれている上記の例に注目してください。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-139">Notice in the above example that the method includes empty strings for the registration parameters.</span></span> <span data-ttu-id="5d9c7-140">アプリケーションを実行しようとする場合、アプリケーションは、パラメーターは空の文字列で許可されていないため、引数の例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-140">If you try to run the application now, the application throws an argument exception because empty strings are not allowed for the parameters.</span></span> <span data-ttu-id="5d9c7-141">次のセクションで示すように有効な値を提供するには、外部プロバイダーと、web サイトを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-141">To provide valid values, you must register your web site with the external providers, as shown in the next section.</span></span>

## <a name="registering-with-an-external-provider"></a><span data-ttu-id="5d9c7-142">外部プロバイダーを登録します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-142">Registering with an external provider</span></span>

<span data-ttu-id="5d9c7-143">外部プロバイダーからの資格情報を持つユーザーを認証するには、プロバイダーと、web サイトを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-143">To authenticate users with credentials from an external provider, you must register your web site with the provider.</span></span> <span data-ttu-id="5d9c7-144">サイトを登録するときに、クライアントを登録するときに含める (キーまたは id、シークレットなど) のパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-144">When you register your site, you will receive the parameters (such as key or id, and secret) to include when registering the client.</span></span> <span data-ttu-id="5d9c7-145">使用するプロバイダーを使用したアカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-145">You must have an account with the providers you wish to use.</span></span>

<span data-ttu-id="5d9c7-146">このチュートリアルは、すべての手順でこれらのプロバイダーを登録するために必要にも表示されません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-146">This tutorial does not show all of the steps you must perform to register with these providers.</span></span> <span data-ttu-id="5d9c7-147">手順は、通常難しくありません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-147">The steps are typically not difficult.</span></span> <span data-ttu-id="5d9c7-148">サイトを正常に登録するには、これらのサイト上の指示に従います。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-148">To successfully register your site, follow the instructions provided on those sites.</span></span> <span data-ttu-id="5d9c7-149">サイトの登録を開始するには、開発者向けサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-149">To get started with registering your site, see the developer site for:</span></span>

- [<span data-ttu-id="5d9c7-150">Facebook</span><span class="sxs-lookup"><span data-stu-id="5d9c7-150">Facebook</span></span>](https://developers.facebook.com/)
- [<span data-ttu-id="5d9c7-151">Google</span><span class="sxs-lookup"><span data-stu-id="5d9c7-151">Google</span></span>](https://developers.google.com/)
- [<span data-ttu-id="5d9c7-152">Microsoft</span><span class="sxs-lookup"><span data-stu-id="5d9c7-152">Microsoft</span></span>](http://manage.dev.live.com/)
- [<span data-ttu-id="5d9c7-153">Twitter</span><span class="sxs-lookup"><span data-stu-id="5d9c7-153">Twitter</span></span>](https://dev.twitter.com/)

<span data-ttu-id="5d9c7-154">Facebook を使用してサイトを登録するときに行うことができます&quot;localhost&quot;サイト ドメインと`&quot;http://localhost/&quot;`url、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-154">When registering your site with Facebook, you can provide &quot;localhost&quot; for the site domain and `&quot;http://localhost/&quot;` for the URL, as shown in the image below.</span></span> <span data-ttu-id="5d9c7-155">Localhost を使用してでは、ほとんどのプロバイダーで機能しますが、Microsoft プロバイダーで現在動作しません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-155">Using localhost works with most providers, but currently does not work with the Microsoft provider.</span></span> <span data-ttu-id="5d9c7-156">Microsoft プロバイダーでは、有効な web サイトの URL を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-156">For the Microsoft provider, you must include a valid web site URL.</span></span>

![サイトを登録します。](using-oauth-providers-with-mvc/_static/image4.png)

<span data-ttu-id="5d9c7-158">前の画像では、アプリ id、アプリのシークレット、および連絡先の電子メールの値が削除されました。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-158">In the previous image, the values for the app id, app secret, and contact email have been removed.</span></span> <span data-ttu-id="5d9c7-159">実際には、サイトを登録するときに、これらの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-159">When you actually register your site, those values will be present.</span></span> <span data-ttu-id="5d9c7-160">それらをアプリケーションに追加するために、アプリ id とアプリ シークレットの値をメモしてするされます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-160">You will want to note the values for app id and app secret because you will add them to your application.</span></span>

## <a name="creating-test-users"></a><span data-ttu-id="5d9c7-161">テスト ユーザーの作成</span><span class="sxs-lookup"><span data-stu-id="5d9c7-161">Creating test users</span></span>

<span data-ttu-id="5d9c7-162">既存の Facebook アカウントを使用してサイトをテストしてかまわない場合は、このセクションをスキップできます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-162">If you do not mind using an existing Facebook account to test your site, you can skip this section.</span></span>

<span data-ttu-id="5d9c7-163">Facebook アプリの管理ページ内で、アプリケーションのテスト ユーザーを簡単に作成することができます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-163">You can easily create test users for your application within the Facebook app management page.</span></span> <span data-ttu-id="5d9c7-164">これらを使用して、サイトへのログインにアカウントをテストします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-164">You can use these test accounts to log in to your site.</span></span> <span data-ttu-id="5d9c7-165">クリックしてテスト ユーザーを作成する、**ロール**、クリックすると、左側のナビゲーション ウィンドウのリンク、**作成**リンク。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-165">You create test users by clicking the **Roles** link in the left navigation pane and the clicking the **Create** link.</span></span>

![テスト ユーザーを作成します。](using-oauth-providers-with-mvc/_static/image5.png)

<span data-ttu-id="5d9c7-167">Facebook のサイトには、要求したテスト アカウントの数が自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-167">The Facebook site automatically creates the number of test accounts that you request.</span></span>

## <a name="adding-application-id-and-secret-from-the-provider"></a><span data-ttu-id="5d9c7-168">プロバイダーからのアプリケーション id とシークレットの追加</span><span class="sxs-lookup"><span data-stu-id="5d9c7-168">Adding application id and secret from the provider</span></span>

<span data-ttu-id="5d9c7-169">Facebook から id とシークレットを受信したできた AuthConfig ファイルに戻ってし、パラメーター値として追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-169">Now that you have received the id and secret from Facebook, go back to the AuthConfig file and add them as the parameter values.</span></span> <span data-ttu-id="5d9c7-170">次に示す値は、実際の値ではありません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-170">The values shown below are not real values.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a><span data-ttu-id="5d9c7-171">外部資格情報でログインします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-171">Log in with external credentials</span></span>

<span data-ttu-id="5d9c7-172">サイトの外部資格情報を有効にするために必要です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-172">That is all you have to do to enable external credentials in your site.</span></span> <span data-ttu-id="5d9c7-173">アプリケーションを実行し、右上隅のログイン リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-173">Run your application and click the login link in the upper right corner.</span></span> <span data-ttu-id="5d9c7-174">テンプレートは、プロバイダーとして Facebook を登録して、プロバイダーのボタンが含まれていますに自動的に認識されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-174">The template automatically recognizes that you have registered Facebook as a provider and includes a button for the provider.</span></span> <span data-ttu-id="5d9c7-175">複数のプロバイダーを登録する場合、それぞれのボタンは、自動的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-175">If you register multiple providers, a button for each one is automatically included.</span></span>

![外部ログイン](using-oauth-providers-with-mvc/_static/image6.png)

<span data-ttu-id="5d9c7-177">このチュートリアルでは、外部プロバイダーのボタンでログをカスタマイズする方法は説明しません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-177">This tutorial does not cover how to customize the log in buttons for the external providers.</span></span> <span data-ttu-id="5d9c7-178">詳細については、次を参照してください。 [Oauth/openid を使用する場合は、ログイン UI をカスタマイズする](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-178">For that information, see [Customizing the login UI when using OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).</span></span>

<span data-ttu-id="5d9c7-179">Facebook の資格情報でログインする Facebook ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-179">Click on the Facebook button to log in with Facebook credentials.</span></span> <span data-ttu-id="5d9c7-180">外部プロバイダーのいずれかを選択すると、そのサイトにリダイレクトされ、ログインにそのサービスによってメッセージが表示するは。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-180">When you select one of the external providers, you are redirected to that site and prompted by that service to log in.</span></span>

<span data-ttu-id="5d9c7-181">次の図は、Facebook のログイン画面を示しています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-181">The following image shows the login screen for Facebook.</span></span> <span data-ttu-id="5d9c7-182">Oauthmvcexample という名前のサイトにログインする、Facebook アカウントを使用していることを示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-182">It notes that you are using your Facebook account to log in to a site named oauthmvcexample.</span></span>

![facebook 認証](using-oauth-providers-with-mvc/_static/image7.png)

<span data-ttu-id="5d9c7-184">Facebook の資格情報でログインした後は、ページは、サイトが基本的な情報にアクセスできるようをユーザーに通知します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-184">After logging in with Facebook credentials, a page informs the user that the site will have access to basic information.</span></span>

![要求のアクセス許可](using-oauth-providers-with-mvc/_static/image8.png)

<span data-ttu-id="5d9c7-186">選択した後**アプリに移動して**サイトのユーザーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-186">After selecting **Go to App**, the user must register for the site.</span></span> <span data-ttu-id="5d9c7-187">次の図は、ユーザーが Facebook の資格情報でログインした後に、登録ページを示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-187">The following image shows the registration page after a user has logged in with Facebook credentials.</span></span> <span data-ttu-id="5d9c7-188">ユーザー名は、通常、プロバイダーからの名前が入力されています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-188">The user name is typically pre-filled with a name from the provider.</span></span>

![register](using-oauth-providers-with-mvc/_static/image9.png)

<span data-ttu-id="5d9c7-190">クリックして**登録**登録を完了します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-190">Click **Register** to complete registration.</span></span> <span data-ttu-id="5d9c7-191">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-191">Close the browser.</span></span>

<span data-ttu-id="5d9c7-192">新しいアカウントが、データベースに追加されたことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-192">You can see that the new account has been added to your database.</span></span> <span data-ttu-id="5d9c7-193">サーバー エクスプ ローラーで開く、 **DefaultConnection**開くデータベースにあり、**テーブル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-193">In Server Explorer, open the **DefaultConnection** database and open the **Tables** folder.</span></span>

![データベース テーブル](using-oauth-providers-with-mvc/_static/image10.png)

<span data-ttu-id="5d9c7-195">右クリックし、 **UserProfile**テーブルを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-195">Right-click the **UserProfile** table and select **Show Table Data**.</span></span>

![データを表示します。](using-oauth-providers-with-mvc/_static/image11.png)

<span data-ttu-id="5d9c7-197">追加した新しいアカウントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-197">You will see the new account you added.</span></span> <span data-ttu-id="5d9c7-198">内のデータを見て**web ページ\_OAuthMembership**テーブル。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-198">Look at the data in **webpage\_OAuthMembership** table.</span></span> <span data-ttu-id="5d9c7-199">追加したアカウントの外部プロバイダーに関連する多くのデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-199">You will see more data related to the external provider for the account you just added.</span></span>

<span data-ttu-id="5d9c7-200">外部認証を有効にする場合は、完了です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-200">If you only want to enable external authentication, you are done.</span></span> <span data-ttu-id="5d9c7-201">ただし、次のセクションで示すようには、新しいユーザーの登録プロセスに、プロバイダーからの情報を統合することができますさらにします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-201">However, you can further integrate information from the provider into the new user registration process, as shown in the following sections.</span></span>

## <a name="create-models-for-additional-user-information"></a><span data-ttu-id="5d9c7-202">追加のユーザー情報のモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-202">Create models for additional user information</span></span>

<span data-ttu-id="5d9c7-203">前のセクションでは、通知するように動作する組み込みのアカウントの登録に関する追加情報を取得する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-203">As you noticed in the previous sections, you do not need to retrieve any additional information for the built-in account registration to work.</span></span> <span data-ttu-id="5d9c7-204">ただし、ほとんどの外部プロバイダーは、ユーザーに関する追加情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-204">However, most external providers pass back additional information about the user.</span></span> <span data-ttu-id="5d9c7-205">次のセクションでは、その情報を保持し、データベースに保存する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-205">The following sections show how to retain that information and save it to a database.</span></span> <span data-ttu-id="5d9c7-206">具体的には、ユーザーの完全名、ユーザーの個人の web ページの URI の値と Facebook がアカウントを確認するかどうかを示す値が保持されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-206">Specifically, you will retain values for the user's full name, the URI of the user's personal web page, and a value that indicates whether Facebook has verified the account.</span></span>

<span data-ttu-id="5d9c7-207">使用する[Code First Migrations](https://msdn.microsoft.com/data/jj591621)追加のユーザー情報を格納するためのテーブルを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-207">You will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) to add a table for storing additional user information.</span></span> <span data-ttu-id="5d9c7-208">テーブルは、既存のデータベースのため、現在のデータベースのスナップショットを作成する必要があります追加されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-208">You are adding the table to an existing database, so you will first need to create a snapshot of the current database.</span></span> <span data-ttu-id="5d9c7-209">現在のデータベースのスナップショットを作成すると、後で新しいテーブルのみを含む移行を作成できます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-209">By creating a snapshot of the current database, you can later create a migration that contains only the new table.</span></span> <span data-ttu-id="5d9c7-210">現在のデータベースのスナップショットを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-210">To create a snapshot of the current database:</span></span>

1. <span data-ttu-id="5d9c7-211">開く、**パッケージ マネージャー コンソール**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-211">Open the **Package Manager Console**</span></span>
2. <span data-ttu-id="5d9c7-212">コマンドを実行 **-移行を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-212">Run the command **enable-migrations**</span></span>
3. <span data-ttu-id="5d9c7-213">コマンドを実行**IgnoreChanges 追加移行初期 –**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-213">Run the command **add-migration initial –IgnoreChanges**</span></span>
4. <span data-ttu-id="5d9c7-214">コマンドを実行**データベースの更新**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-214">Run the command **update-database**</span></span>

<span data-ttu-id="5d9c7-215">ここで、新しいプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-215">Now, you will add the new properties.</span></span> <span data-ttu-id="5d9c7-216">Models フォルダーで、AccountModels.cs ファイルを開き RegisterExternalLoginModel クラスを探します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-216">In the Models folder, open the AccountModels.cs file, and find the RegisterExternalLoginModel class.</span></span> <span data-ttu-id="5d9c7-217">RegisterExternalLoginModel クラスは、認証プロバイダーから返される値を保持します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-217">The RegisterExternalLoginModel class holds values that come back from the authentication provider.</span></span> <span data-ttu-id="5d9c7-218">FullName とリンクを以下の強調表示されているという名前のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-218">Add properties named FullName and Link, as highlighted below.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

<span data-ttu-id="5d9c7-219">また、AccountModels.cs では、ExtraUserInformation という新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-219">Also in AccountModels.cs, add a new class called ExtraUserInformation.</span></span> <span data-ttu-id="5d9c7-220">このクラスは、データベースに作成される新しいテーブルを表します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-220">This class represents the new table which will be created in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

<span data-ttu-id="5d9c7-221">UsersContext クラスでは、新しいクラスの DbSet プロパティを作成するのには、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-221">In the UsersContext class, add the highlighted code below to create a DbSet property for the new class.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

<span data-ttu-id="5d9c7-222">新しいテーブルを作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-222">You are now ready to create the new table.</span></span> <span data-ttu-id="5d9c7-223">もう一度と、今度は、パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-223">Open the Package Manager Console again and this time:</span></span>

1. <span data-ttu-id="5d9c7-224">コマンドを実行**追加移行 AddExtraUserInformation**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-224">Run the command **add-migration AddExtraUserInformation**</span></span>
2. <span data-ttu-id="5d9c7-225">コマンドを実行**データベースの更新**</span><span class="sxs-lookup"><span data-stu-id="5d9c7-225">Run the command **update-database**</span></span>

<span data-ttu-id="5d9c7-226">新しいテーブルは、今すぐデータベースに存在します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-226">The new table now exists in the database.</span></span>

## <a name="retrieve-the-additional-data"></a><span data-ttu-id="5d9c7-227">追加のデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-227">Retrieve the additional data</span></span>

<span data-ttu-id="5d9c7-228">追加のユーザー データを取得する 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-228">There are two ways to retrieve additional user data.</span></span> <span data-ttu-id="5d9c7-229">最初の方法は、既定では、認証要求時に、渡されるユーザー データを保持します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-229">The first way is to retain user data that is passed back, by default, during the authentication request.</span></span> <span data-ttu-id="5d9c7-230">2 番目の方法では、具体的にはプロバイダー API を呼び出すし、詳細情報を要求します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-230">The second way is to specifically call the provider API and request more information.</span></span> <span data-ttu-id="5d9c7-231">FullName およびリンクの値は Facebook によって自動的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-231">Values for FullName and Link are automatically passed back by Facebook.</span></span> <span data-ttu-id="5d9c7-232">Facebook がアカウントを確認するかどうかを示す値は、Facebook API を呼び出すことによって取得されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-232">A value that indicates whether Facebook has verified the account is retrieved through a call to the Facebook API.</span></span> <span data-ttu-id="5d9c7-233">最初に、FullName およびリンクの値が設定され、確認済みの値を取得した後で、します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-233">First, you will populate values for FullName and Link, and then later, you will get the verified value.</span></span>

<span data-ttu-id="5d9c7-234">追加のユーザー データを取得するには、開く、 **AccountController.cs**ファイル、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-234">To retrieve the additional user data, open the **AccountController.cs** file in the **Controllers** folder.</span></span>

<span data-ttu-id="5d9c7-235">このファイルには、ログ記録、登録、およびアカウントを管理するためのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-235">This file contains the logic for logging, registering, and managing accounts.</span></span> <span data-ttu-id="5d9c7-236">具体的には、呼び出されたメソッドに注目してください**ExternalLoginCallback**と**ExternalLoginConfirmation**します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-236">In particular, notice the methods called **ExternalLoginCallback** and **ExternalLoginConfirmation**.</span></span> <span data-ttu-id="5d9c7-237">これらのメソッド内では、アプリケーションの外部ログイン操作をカスタマイズするコードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-237">Within these methods, you can add code to customize external login operations for your application.</span></span> <span data-ttu-id="5d9c7-238">最初の行、 **ExternalLoginCallback**メソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-238">The first line of the **ExternalLoginCallback** method contains:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

<span data-ttu-id="5d9c7-239">渡される追加のユーザー データ、 **ExtraData**のプロパティ、 **AuthenticationResult**オブジェクトから返される、 **VerifyAuthentication**メソッド。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-239">Additional user data is passed back in the **ExtraData** property of the **AuthenticationResult** object that is returned from the **VerifyAuthentication** method.</span></span> <span data-ttu-id="5d9c7-240">Facebook クライアントには、次の値が含まれています、 **ExtraData**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-240">The Facebook client contains the following values in the **ExtraData** property:</span></span>

- <span data-ttu-id="5d9c7-241">ID</span><span class="sxs-lookup"><span data-stu-id="5d9c7-241">id</span></span>
- <span data-ttu-id="5d9c7-242">name</span><span class="sxs-lookup"><span data-stu-id="5d9c7-242">name</span></span>
- <span data-ttu-id="5d9c7-243">リンクをクリックする</span><span class="sxs-lookup"><span data-stu-id="5d9c7-243">link</span></span>
- <span data-ttu-id="5d9c7-244">性別</span><span class="sxs-lookup"><span data-stu-id="5d9c7-244">gender</span></span>
- <span data-ttu-id="5d9c7-245">accesstoken</span><span class="sxs-lookup"><span data-stu-id="5d9c7-245">accesstoken</span></span>

<span data-ttu-id="5d9c7-246">その他のプロバイダーは、ExtraData プロパティに似ていますが、若干異なるデータがあります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-246">Other providers will have similar but slightly different data in the ExtraData property.</span></span>

<span data-ttu-id="5d9c7-247">ユーザーがサイトに新しい場合は、いくつか追加のデータを取得し、確認ビューにそのデータを渡すします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-247">If the user is new to your site, you will retrieve some the additional data and pass that data to the confirmation view.</span></span> <span data-ttu-id="5d9c7-248">メソッドのコードの最後のブロックは、ユーザーがサイトに新しい場合にのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-248">The last block of code in the method is run only if the user is new to your site.</span></span> <span data-ttu-id="5d9c7-249">次の行に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-249">Replace the following line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

<span data-ttu-id="5d9c7-250">この行には。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-250">With this line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

<span data-ttu-id="5d9c7-251">この変更には、FullName およびリンクのプロパティの値にはだけが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-251">This change merely includes values for the FullName and Link properties.</span></span>

<span data-ttu-id="5d9c7-252">**ExternalLoginConfirmation**メソッド、強調表示されている追加のユーザー情報を保存するのには、次のコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-252">In the **ExternalLoginConfirmation** method, modify the code as highlighted below to save the additional user information.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a><span data-ttu-id="5d9c7-253">表示を調整します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-253">Adjusting the view</span></span>

<span data-ttu-id="5d9c7-254">登録ページで、プロバイダーから取得された追加のユーザー データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-254">The additional user data that you retrieve from the provider will be displayed in the registration page.</span></span>

<span data-ttu-id="5d9c7-255">**ビュー**/**アカウント**フォルダーを開き、 **ExternalLoginConfirmation.cshtml**します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-255">In the **Views**/**Account** folder, open **ExternalLoginConfirmation.cshtml**.</span></span> <span data-ttu-id="5d9c7-256">ユーザー名の既存のフィールドの下には、FullName、リンク、および PictureLink フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-256">Below the existing field for user name, add fields for FullName, Link, and PictureLink.</span></span>

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

<span data-ttu-id="5d9c7-257">アプリケーションを実行し、保存された追加情報を新しいユーザーを登録する、ほぼ準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-257">You are now almost ready to run the application and register a new user with the additional information saved.</span></span> <span data-ttu-id="5d9c7-258">サイトに既に登録されていないアカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-258">You must have an account that is not already registered with the site.</span></span> <span data-ttu-id="5d9c7-259">別のテスト アカウントを使用か、内の行を削除することができます、 **UserProfile**と**web ページ\_OAuthMembership**で再利用するアカウントのテーブル。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-259">You can either use a different test account, or delete the rows in the **UserProfile** and **webpages\_OAuthMembership** tables for the account you wish to reuse.</span></span> <span data-ttu-id="5d9c7-260">これらの行を削除すると、アカウントが再登録するようになります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-260">By deleting those rows, you will ensure that the account is registered again.</span></span>

<span data-ttu-id="5d9c7-261">アプリケーションを実行し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-261">Run the application and register the new user.</span></span> <span data-ttu-id="5d9c7-262">この時間には確認 ページが含まれているより多くの値に注意してください。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-262">Notice that this time the confirmation page contains more values.</span></span>

![register](using-oauth-providers-with-mvc/_static/image12.png)

<span data-ttu-id="5d9c7-264">登録を完了すると、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-264">After completing registration, close the browser.</span></span> <span data-ttu-id="5d9c7-265">新しい値を確認するデータベース ファイルの場所、 **ExtraUserInformation**テーブル。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-265">Look in the database to notice the new values in the **ExtraUserInformation** table.</span></span>

## <a name="install-nuget-package-for-facebook-api"></a><span data-ttu-id="5d9c7-266">Facebook API の NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-266">Install NuGet package for Facebook API</span></span>

<span data-ttu-id="5d9c7-267">Facebook の提供、 [API](https://developers.facebook.com/docs/reference/apis/)操作を実行するを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-267">Facebook provides an [API](https://developers.facebook.com/docs/reference/apis/) that you can call to perform operations.</span></span> <span data-ttu-id="5d9c7-268">送信側の HTTP 要求を送信するか、それらの要求を送信することを容易にする NuGet パッケージのインストールを使用して、Facebook API を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-268">You can call the Facebook API either by directing sending HTTP requests, or by using installing a NuGet package that facilitates sending those requests.</span></span> <span data-ttu-id="5d9c7-269">このチュートリアルで示すようには NuGet パッケージを使用してが NuGet のインストール パッケージが不可欠です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-269">Using a NuGet package is shown in this tutorial, but installing NuGet package is not essential.</span></span> <span data-ttu-id="5d9c7-270">このチュートリアルでは、Facebook c# SDK パッケージを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-270">This tutorial shows how to use the Facebook C# SDK package.</span></span> <span data-ttu-id="5d9c7-271">Facebook API の呼び出しを支援するその他の NuGet パッケージがあります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-271">There are other NuGet packages that assist with calling the Facebook API.</span></span>

<span data-ttu-id="5d9c7-272">**NuGet パッケージの管理**windows では、Facebook c# SDK パッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-272">From the **Manage NuGet Packages** windows, select the Facebook C# SDK package.</span></span>

![パッケージをインストールします。](using-oauth-providers-with-mvc/_static/image13.png)

<span data-ttu-id="5d9c7-274">Facebook c# SDK を使用して、ユーザーのアクセス トークンを必要とする操作を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-274">You will use the Facebook C# SDK to call an operation that requires the access token for the user.</span></span> <span data-ttu-id="5d9c7-275">次のセクションでは、アクセス トークンを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-275">The next section shows how to get the access token.</span></span>

## <a name="retrieve-access-token"></a><span data-ttu-id="5d9c7-276">アクセス トークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-276">Retrieve access token</span></span>

<span data-ttu-id="5d9c7-277">ほとんどの外部プロバイダーは、ユーザーの資格情報が検証された後、アクセス トークンで渡すバックアップします。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-277">Most external providers pass back an access token after the user's credentials are verified.</span></span> <span data-ttu-id="5d9c7-278">このアクセス トークンは、認証されたユーザーに提供される操作を呼び出すことができるため、非常に重要です。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-278">This access token is very important because it enables you to call operations that are only available to authenticated users.</span></span> <span data-ttu-id="5d9c7-279">そのため、取得して、アクセス トークンを格納するが不可欠ですより多くの機能を提供する場合。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-279">Therefore, retrieving and storing the access token is essential when you want to provide more functionality.</span></span>

<span data-ttu-id="5d9c7-280">外部プロバイダーによってアクセス トークンがある一定期間のみ、有効なする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-280">Depending on the external provider, the access token may be valid for only a limited amount of time.</span></span> <span data-ttu-id="5d9c7-281">有効なアクセス トークンがあることを確認が取得するたびに、ユーザー、ログインし、セッションの値として保存することではなく、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-281">To ensure that you have a valid access token, you will retrieve it each time the user logs in, and store it as a session value rather than save it to a database.</span></span>

<span data-ttu-id="5d9c7-282">**ExternalLoginCallback**メソッドに戻り、アクセス トークンが渡されるも、 **ExtraData**のプロパティ、 **AuthenticationResult**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-282">In the **ExternalLoginCallback** method, the access token is also passed back in the **ExtraData** property of the **AuthenticationResult** object.</span></span> <span data-ttu-id="5d9c7-283">強調表示されたコードを追加**ExternalLoginCallback**でアクセス トークンを保存する、**セッション**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-283">Add the highlighted code to **ExternalLoginCallback** to save the access token in the **Session** object.</span></span> <span data-ttu-id="5d9c7-284">このコードは、Facebook アカウントを使用して、ユーザーがログオンするたびに実行されます。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-284">This code is run every time the user logs in with a Facebook account.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

<span data-ttu-id="5d9c7-285">この例では、Facebook からアクセス トークンを取得、外部プロバイダーからのアクセス トークンを取得という名前のと同じキーを&quot;accesstoken&quot;します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-285">Although this example retrieves an access token from Facebook, you can retrieve the access token from any external provider through the same key named &quot;accesstoken&quot;.</span></span>

## <a name="logging-off"></a><span data-ttu-id="5d9c7-286">ログオフ</span><span class="sxs-lookup"><span data-stu-id="5d9c7-286">Logging off</span></span>

<span data-ttu-id="5d9c7-287">既定の**ログオフ**メソッドは、アプリケーションからユーザーをログ記録は、外部プロバイダーからユーザーをログオンできません。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-287">The default **LogOff** method logs the user out of your application, but does not log the user out of the external provider.</span></span> <span data-ttu-id="5d9c7-288">ログインからユーザーを Facebook、サインアウトし、トークンが、ユーザーがログオフした後に永続化しないようにする、次の強調表示されたコードを追加することができます、**ログオフ**AccountController でメソッド。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-288">To log the user out of Facebook, and prevent the token from persisting after the user has logged off, you can add the following highlighted code to the **LogOff** method in the AccountController.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

<span data-ttu-id="5d9c7-289">指定した値、`next`パラメーターは、Facebook からユーザーがログオンした後に使用する URL。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-289">The value you provide in the `next` parameter is the URL to use after the user has logged out of Facebook.</span></span> <span data-ttu-id="5d9c7-290">をローカル コンピューターにテストする場合、ローカル サイトの正しいポート番号を入力します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-290">When testing on your local computer, you would provide the correct port number for your local site.</span></span> <span data-ttu-id="5d9c7-291">運用環境では、contoso.com などの既定のページを提供します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-291">In production, you would provide a default page, such as contoso.com.</span></span>

## <a name="retrieve-user-information-that-requires-the-access-token"></a><span data-ttu-id="5d9c7-292">アクセス トークンが必要なユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-292">Retrieve user information that requires the access token</span></span>

<span data-ttu-id="5d9c7-293">これで、アクセス トークンを格納して、Facebook c# SDK パッケージをインストールしたことができます一緒に使用するそれらの Facebook から追加のユーザー情報を要求します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-293">Now that you have stored the access token and installed the Facebook C# SDK package, you can use them together to request additional user information from Facebook.</span></span> <span data-ttu-id="5d9c7-294">**ExternalLoginConfirmation**メソッドのインスタンスを作成、 **FacebookClient**アクセス トークンの値を渡すことによってクラス。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-294">In the **ExternalLoginConfirmation** method, create an instance of the **FacebookClient** class by passing the value of the access token.</span></span> <span data-ttu-id="5d9c7-295">値を要求、**検証**の現在の認証済みユーザーのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-295">Request the value of the **verified** property for the current, authenticated user.</span></span> <span data-ttu-id="5d9c7-296">**検証**プロパティは、Facebook が携帯電話にメッセージを送信するなど、他の方法でアカウントを検証するかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-296">The **verified** property indicates whether Facebook has validated the account through some other means, such as sending a message to a cell phone.</span></span> <span data-ttu-id="5d9c7-297">この値は、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-297">Save this value in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

<span data-ttu-id="5d9c7-298">再度、ユーザーのデータベース内のレコードを削除するか、別の Facebook アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-298">You will again need to either delete the records in the database for the user, or use a different Facebook account.</span></span>

<span data-ttu-id="5d9c7-299">アプリケーションを実行し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-299">Run the application, and register the new user.</span></span> <span data-ttu-id="5d9c7-300">見て、 **ExtraUserInformation** Verified プロパティの値を表示するテーブル。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-300">Look at the **ExtraUserInformation** table to see the value for the Verified property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5d9c7-301">まとめ</span><span class="sxs-lookup"><span data-stu-id="5d9c7-301">Conclusion</span></span>

<span data-ttu-id="5d9c7-302">このチュートリアルでは、ユーザーの認証と登録データを Facebook と統合されているサイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-302">In this tutorial, you created a site that is integrated with Facebook for user authentication and registration data.</span></span> <span data-ttu-id="5d9c7-303">MVC 4 web アプリケーション、およびその既定の動作をカスタマイズする方法に設定されている既定の動作について説明しました。</span><span class="sxs-lookup"><span data-stu-id="5d9c7-303">You learned about the default behavior that is set up for MVC 4 web application, and how to customize that default behavior.</span></span>

## <a name="related-topics"></a><span data-ttu-id="5d9c7-304">関連トピック</span><span class="sxs-lookup"><span data-stu-id="5d9c7-304">Related topics</span></span>

- [<span data-ttu-id="5d9c7-305">Auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ</span><span class="sxs-lookup"><span data-stu-id="5d9c7-305">Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service</span></span>](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
