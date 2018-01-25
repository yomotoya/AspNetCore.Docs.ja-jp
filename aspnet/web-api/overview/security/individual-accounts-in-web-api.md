---
uid: web-api/overview/security/individual-accounts-in-web-api
title: "個々 のアカウントと ASP.NET Web API 2.2 でローカルのログインを使用して Web API をセキュリティで保護された |Microsoft ドキュメント"
author: MikeWasson
description: "このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して web API をセキュリティで保護する方法を示します。 ソフトウェアのバージョンが、チュートリアルの Visual Studio 201 で使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a><span data-ttu-id="3d7a1-104">個々 のアカウントと ASP.NET Web API 2.2 でローカルのログインを使用して Web API をセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-104">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="3d7a1-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d7a1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3d7a1-106">サンプル アプリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-106">Download Sample App</span></span>](https://github.com/MikeWasson/LocalAccountsApp)

> <span data-ttu-id="3d7a1-107">このトピックでは、メンバーシップ データベースに対する認証に OAuth2 を使用して web API をセキュリティで保護する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-107">This topic shows how to secure a web API using OAuth2 to authenticate against a membership database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3d7a1-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3d7a1-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="3d7a1-109">Visual Studio 2013 Update 3</span><span class="sxs-lookup"><span data-stu-id="3d7a1-109">Visual Studio 2013 Update 3</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="3d7a1-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="3d7a1-110">Web API 2.2</span></span>](../releases/whats-new-in-aspnet-web-api-22.md)
> - [<span data-ttu-id="3d7a1-111">ASP.NET Identity 2.1</span><span class="sxs-lookup"><span data-stu-id="3d7a1-111">ASP.NET Identity 2.1</span></span>](../../../identity/index.md)


<span data-ttu-id="3d7a1-112">Visual Studio 2013 で、Web API プロジェクト テンプレートを使用して認証用の 3 つのオプション。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-112">In Visual Studio 2013, the Web API project template gives you three options for authentication:</span></span>

- <span data-ttu-id="3d7a1-113">**個々 のアカウントです。**</span><span class="sxs-lookup"><span data-stu-id="3d7a1-113">**Individual accounts.**</span></span> <span data-ttu-id="3d7a1-114">アプリでは、メンバーシップ データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-114">The app uses a membership database.</span></span>
- <span data-ttu-id="3d7a1-115">**組織のアカウントです。**</span><span class="sxs-lookup"><span data-stu-id="3d7a1-115">**Organizational accounts.**</span></span> <span data-ttu-id="3d7a1-116">ユーザーは、その Azure Active Directory、Office 365、または内部設置型 Active Directory の資格情報でサインインします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-116">Users sign in with their Azure Active Directory, Office 365, or on-premise Active Directory credentials.</span></span>
- <span data-ttu-id="3d7a1-117">**Windows 認証です。**</span><span class="sxs-lookup"><span data-stu-id="3d7a1-117">**Windows authentication.**</span></span> <span data-ttu-id="3d7a1-118">このオプションは、イントラネット アプリケーションのためのものでは、し、Windows 認証の IIS モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-118">This option is intended for Intranet applications, and uses the Windows Authentication IIS module.</span></span>

<span data-ttu-id="3d7a1-119">これらのオプションの詳細については、次を参照してください。 [Visual Studio 2013 で ASP.NET Web プロジェクトの作成](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-119">For more details about these options, see [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).</span></span>

<span data-ttu-id="3d7a1-120">個々 のアカウントは、ユーザーがログインするための 2 つの方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-120">Individual accounts provide two ways for a user to log in:</span></span>

- <span data-ttu-id="3d7a1-121">**ローカル ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-121">**Local login**.</span></span> <span data-ttu-id="3d7a1-122">ユーザー登録、サイトでユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-122">The user registers at the site, entering a username and password.</span></span> <span data-ttu-id="3d7a1-123">アプリでは、メンバーシップ データベースのパスワード ハッシュを保存します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-123">The app stores the password hash in the membership database.</span></span> <span data-ttu-id="3d7a1-124">ユーザーがログインすると、ASP.NET Identity システムは、パスワードを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-124">When the user logs in, the ASP.NET Identity system verifies the password.</span></span>
- <span data-ttu-id="3d7a1-125">**ソーシャル ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-125">**Social login**.</span></span> <span data-ttu-id="3d7a1-126">ユーザーは、Facebook、Microsoft、Google などの外部サービスを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-126">The user signs in with an external service, such as Facebook, Microsoft, or Google.</span></span> <span data-ttu-id="3d7a1-127">アプリでは、メンバーシップ データベース内のユーザー エントリを作成できますが、すべての資格情報は格納されません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-127">The app still creates an entry for the user in the membership database, but does not store any credentials.</span></span> <span data-ttu-id="3d7a1-128">ユーザーは、外部のサービスにサインインして認証します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-128">The user authenticates by signing into the external service.</span></span>

<span data-ttu-id="3d7a1-129">この記事では、ローカル ログイン シナリオ、します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-129">This article looks at the local login scenario.</span></span> <span data-ttu-id="3d7a1-130">ローカルおよびソーシャルの両方のログインには、Web API は、要求の認証に OAuth2 を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-130">For both local and social login, Web API uses OAuth2 to authenticate requests.</span></span> <span data-ttu-id="3d7a1-131">ただし、資格情報のフローは、ローカルおよびソーシャル ログインに異なります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-131">However, the credential flows are different for local and social login.</span></span>

<span data-ttu-id="3d7a1-132">この記事で、ユーザーがログインし、web API への認証済みの AJAX 呼び出しを送信できる簡単なアプリを紹介します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-132">In this article, I'll demonstrate a simple app that lets the user log in and send authenticated AJAX calls to a web API.</span></span> <span data-ttu-id="3d7a1-133">サンプル コードをダウンロードする[ここ](https://github.com/MikeWasson/LocalAccountsApp)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-133">You can download the sample code [here](https://github.com/MikeWasson/LocalAccountsApp).</span></span> <span data-ttu-id="3d7a1-134">リリース ノートでは、Visual Studio での最初からサンプルを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-134">The readme describes how to create the sample from scratch in Visual Studio.</span></span>

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

<span data-ttu-id="3d7a1-135">サンプル アプリは、データ バインディング、および jQuery AJAX 要求を送信するための Knockout.js を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-135">The sample app uses Knockout.js for data-binding and jQuery for sending AJAX requests.</span></span> <span data-ttu-id="3d7a1-136">すればを重視して AJAX 呼び出しをこのアーティクルの Knockout.js を知る必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-136">I'll be focusing on the AJAX calls, so you don't need to know Knockout.js for this article.</span></span>

<span data-ttu-id="3d7a1-137">その過程では、説明します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-137">Along the way, I'll describe:</span></span>

- <span data-ttu-id="3d7a1-138">どのようなアプリケーションのクライアント側で実行します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-138">What the app is doing on the client side.</span></span>
- <span data-ttu-id="3d7a1-139">サーバーで処理します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-139">What's happening on the server.</span></span>
- <span data-ttu-id="3d7a1-140">中央の HTTP トラフィック。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-140">The HTTP traffic in the middle.</span></span>

<span data-ttu-id="3d7a1-141">最初に、いくつかの OAuth2 用語を定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-141">First, we need to define some OAuth2 terminology.</span></span>

- <span data-ttu-id="3d7a1-142">*リソース*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-142">*Resource*.</span></span> <span data-ttu-id="3d7a1-143">一部のデータを保護することができます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-143">Some piece of data that can be protected.</span></span>
- <span data-ttu-id="3d7a1-144">*リソース サーバー*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-144">*Resource server*.</span></span> <span data-ttu-id="3d7a1-145">リソースをホストするサーバー。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-145">The server that hosts the resource.</span></span>
- <span data-ttu-id="3d7a1-146">*リソース所有者*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-146">*Resource owner*.</span></span> <span data-ttu-id="3d7a1-147">リソースにアクセスする権限を与えることのできるエンティティです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-147">The entity that can grant permission to access a resource.</span></span> <span data-ttu-id="3d7a1-148">(通常はユーザーです。)</span><span class="sxs-lookup"><span data-stu-id="3d7a1-148">(Typically the user.)</span></span>
- <span data-ttu-id="3d7a1-149">*クライアント*: リソースへのアクセスを必要があるアプリ。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-149">*Client*: The app that wants access to the resource.</span></span> <span data-ttu-id="3d7a1-150">この記事では、クライアントは、web ブラウザーがします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-150">In this article, the client is a web browser.</span></span>
- <span data-ttu-id="3d7a1-151">*アクセス トークン*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-151">*Access token*.</span></span> <span data-ttu-id="3d7a1-152">リソースへのアクセスを付与するトークンです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-152">A token that grants access to a resource.</span></span>
- <span data-ttu-id="3d7a1-153">*ベアラー トークン*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-153">*Bearer token*.</span></span> <span data-ttu-id="3d7a1-154">特定の種類のすべてのユーザー トークンを使用できることプロパティを使用して、アクセス トークン。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-154">A particular type of access token, with the property that anyone can use the token.</span></span> <span data-ttu-id="3d7a1-155">つまり、クライアントは、暗号化キーまたはその他のシークレット ベアラー トークンを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-155">In other words, a client doesn't need a cryptographic key or other secret to use a bearer token.</span></span> <span data-ttu-id="3d7a1-156">そのため、ベアラー トークンは、HTTPS 経由でのみ使用する必要があり、比較的短い形式の有効期限の日時を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-156">For that reason, bearer tokens should only be used over a HTTPS, and should have relatively short expiration times.</span></span>
- <span data-ttu-id="3d7a1-157">*承認サーバー*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-157">*Authorization server*.</span></span> <span data-ttu-id="3d7a1-158">アクセス トークンを提供するサーバー。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-158">A server that gives out access tokens.</span></span>

<span data-ttu-id="3d7a1-159">アプリケーションは、承認サーバーとリソース サーバーの両方として機能できます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-159">An application can act as both authorization server and resource server.</span></span> <span data-ttu-id="3d7a1-160">Web API プロジェクト テンプレートでは、このパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-160">The Web API project template follows this pattern.</span></span>

## <a name="local-login-credential-flow"></a><span data-ttu-id="3d7a1-161">ローカルのログイン資格情報のフロー</span><span class="sxs-lookup"><span data-stu-id="3d7a1-161">Local Login Credential Flow</span></span>

<span data-ttu-id="3d7a1-162">ローカル ログインは、Web API を使用して、[リソース所有者のパスワード フロー](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) OAuth2 で定義されています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-162">For local login, Web API uses the [resource owner password flow](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) defined in OAuth2.</span></span>

1. <span data-ttu-id="3d7a1-163">ユーザーは、クライアント名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-163">The user enters a name and password into the client.</span></span>
2. <span data-ttu-id="3d7a1-164">クライアントは、承認サーバーにこれらの資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-164">The client sends these credentials to the authorization server.</span></span>
3. <span data-ttu-id="3d7a1-165">承認サーバーは、資格情報を認証し、アクセス トークンを返します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-165">The authorization server authenticates the credentials and returns an access token.</span></span>
4. <span data-ttu-id="3d7a1-166">クライアントには保護されたリソースにアクセスするには、HTTP 要求の承認ヘッダーにアクセス トークンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-166">To access a protected resource, the client includes the access token in the Authorization header of the HTTP request.</span></span>

![](individual-accounts-in-web-api/_static/image3.png)

<span data-ttu-id="3d7a1-167">選択すると**個々 のアカウント**Web API プロジェクト テンプレートでプロジェクトには、ユーザーの資格情報を検証し、トークンを発行する承認サーバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-167">When you select **Individual accounts** in the Web API project template, the project includes an authorization server that validates user credentials and issues tokens.</span></span> <span data-ttu-id="3d7a1-168">次の図は、Web API のコンポーネントの観点から、同じ資格情報フローを示しています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-168">The following diagram shows the same credential flow in terms of Web API components.</span></span>

![](individual-accounts-in-web-api/_static/image4.png)

<span data-ttu-id="3d7a1-169">このシナリオでは、Web API コント ローラーは、リソース サーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-169">In this scenario, Web API controllers act as resource servers.</span></span> <span data-ttu-id="3d7a1-170">認証フィルターがアクセス トークンを検証し、 **[Authorize]**属性を使用してリソースを保護します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-170">An authentication filter validates access tokens, and the **[Authorize]** attribute is used to protect a resource.</span></span> <span data-ttu-id="3d7a1-171">コント ローラーまたはアクションが持っている場合、 **[Authorize]**属性がすべての要求にそのコント ローラーまたはアクションを認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-171">When a controller or action has the **[Authorize]** attribute, all requests to that controller or action must be authenticated.</span></span> <span data-ttu-id="3d7a1-172">それ以外の場合、承認が拒否され、Web API には、401 (未承認) のエラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-172">Otherwise, authorization is denied, and Web API returns a 401 (Unauthorized) error.</span></span>

<span data-ttu-id="3d7a1-173">承認サーバーと両方呼び出せる認証フィルター、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)OAuth2 の詳細を処理するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-173">The authorization server and the authentication filter both call into an [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) component that handles the details of OAuth2.</span></span> <span data-ttu-id="3d7a1-174">このチュートリアルで後ほど詳しくデザインを説明します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-174">I'll describe the design in more detail later in this tutorial.</span></span>

## <a name="sending-an-unauthorized-request"></a><span data-ttu-id="3d7a1-175">承認されていない要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-175">Sending an Unauthorized Request</span></span>

<span data-ttu-id="3d7a1-176">最初に、アプリケーションを実行し、をクリックして、 **API を呼び出す**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-176">To get started, run the app and click the **Call API** button.</span></span> <span data-ttu-id="3d7a1-177">要求が完了したらのエラー メッセージが表示されます、**結果**ボックス。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-177">When the request completes, you should see an error message in the **Result** box.</span></span> <span data-ttu-id="3d7a1-178">要求がないため、アクセス トークン要求が承認されていないためにです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-178">That's because the request does not contain an access token, so the request is unauthorized.</span></span>

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

<span data-ttu-id="3d7a1-179">**API を呼び出す**ボタンに AJAX 要求を送信 ~/api/値、Web API コント ローラーのアクションを呼び出す。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-179">The **Call API** button sends an AJAX request to ~/api/values, which invokes a Web API controller action.</span></span> <span data-ttu-id="3d7a1-180">AJAX 要求を送信する JavaScript コードのセクションを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-180">Here is the section of JavaScript code that sends the AJAX request.</span></span> <span data-ttu-id="3d7a1-181">サンプル アプリですべてのアプリの JavaScript コードは Scripts\app.js ファイルにあります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-181">In the sample app, all of the JavaScript app code is located in the Scripts\app.js file.</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

<span data-ttu-id="3d7a1-182">ユーザーがログインするまではないベアラー トークンと要求の承認ヘッダーしたがってがありません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-182">Until the user logs in, there is no bearer token, and therefore no Authorization header in the request.</span></span> <span data-ttu-id="3d7a1-183">これは、ため、401 エラーを返す要求します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-183">This causes the request to return a 401 error.</span></span>

<span data-ttu-id="3d7a1-184">HTTP 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-184">Here is the HTTP request.</span></span> <span data-ttu-id="3d7a1-185">(を使用して[Fiddler](http://www.telerik.com/fiddler) HTTP トラフィックをキャプチャします)。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-185">(I used [Fiddler](http://www.telerik.com/fiddler) to capture the HTTP traffic.)</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

<span data-ttu-id="3d7a1-186">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="3d7a1-186">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

<span data-ttu-id="3d7a1-187">ベアラーに設定するという課題に Www 認証ヘッダーが応答が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-187">Notice that the response includes a Www-Authenticate header with the challenge set to Bearer.</span></span> <span data-ttu-id="3d7a1-188">サーバーにベアラー トークンが必要ですを示すです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-188">That indicates the server expects a bearer token.</span></span>

## <a name="register-a-user"></a><span data-ttu-id="3d7a1-189">ユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-189">Register a User</span></span>

<span data-ttu-id="3d7a1-190">**登録**セクションで、アプリの電子メールとパスワードを入力し、をクリックして、**登録**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-190">In the **Register** section of the app, enter an email and password, and click the **Register** button.</span></span>

<span data-ttu-id="3d7a1-191">このサンプルでは、有効な電子メール アドレスを使用する必要はありませんが、実際のアプリは、アドレスを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-191">You don't need to use a valid email address for this sample, but a real app would confirm the address.</span></span> <span data-ttu-id="3d7a1-192">(を参照してください[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md))。パスワードの大文字、小文字アルファベット、番号、および英数字以外の文字と"Password1!"のようなものを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-192">(See [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) For the password, use something like "Password1!", with an upper case letter, lower case letter, number, and non-alpha-numeric character.</span></span> <span data-ttu-id="3d7a1-193">アプリをシンプルにするには、クライアント側の検証を残しましたため、パスワードの形式に問題がある場合は、400 (Bad Request) のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-193">To keep the app simple, I left out client-side validation, so if there is a problem with the password format, you'll get a 400 (Bad Request) error.</span></span>

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

<span data-ttu-id="3d7a1-194">**登録**ボタン ~/api/Account/Register に POST 要求を送信する/です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-194">The **Register** button sends a POST request to ~/api/Account/Register/.</span></span> <span data-ttu-id="3d7a1-195">要求本文は、ユーザー名とパスワードを保持する JSON オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-195">The request body is a JSON object that holds the name and password.</span></span> <span data-ttu-id="3d7a1-196">要求を送信する JavaScript コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-196">Here is the JavaScript code that sends the request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

<span data-ttu-id="3d7a1-197">HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-197">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

<span data-ttu-id="3d7a1-198">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="3d7a1-198">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

<span data-ttu-id="3d7a1-199">この要求を処理、`AccountController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-199">This request is handled by the `AccountController` class.</span></span> <span data-ttu-id="3d7a1-200">内部的には、 `AccountController` ASP.NET の Id を使用して、メンバーシップ データベースを管理します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-200">Internally, `AccountController` uses ASP.NET Identity to manage the membership database.</span></span>

<span data-ttu-id="3d7a1-201">Visual Studio からアプリをローカルで実行する場合、ユーザー アカウントが AspNetUsers テーブルで LocalDB に格納されます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-201">If you run the app locally from Visual Studio, user accounts are stored in LocalDB, in the AspNetUsers table.</span></span> <span data-ttu-id="3d7a1-202">テーブルを表示する、Visual Studio で、をクリックして、**ビュー**メニューの **サーバー エクスプ ローラー**の順に展開**データ接続**です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-202">To view the tables in Visual Studio, click the **View** menu, select **Server Explorer**, then expand **Data Connections**.</span></span>

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a><span data-ttu-id="3d7a1-203">アクセス トークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-203">Get an Access Token</span></span>

<span data-ttu-id="3d7a1-204">これまでは実行していない、OAuth が、今すぐお見せ、OAuth 承認サーバーの動作、アクセス トークンを要求時にします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-204">So far we have not done any OAuth, but now we'll see the OAuth authorization server in action, when we request an access token.</span></span> <span data-ttu-id="3d7a1-205">**ログで**サンプル アプリの領域が電子メール アドレスとパスワードを入力し、クリックして**ログで**です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-205">In the **Log In** area of the sample app, enter the email and password and click **Log In**.</span></span>

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

<span data-ttu-id="3d7a1-206">**ログで**ボタンは、トークン エンドポイントに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-206">The **Log In** button sends a request to the token endpoint.</span></span> <span data-ttu-id="3d7a1-207">要求の本文には、次の形式で url でエンコードされたデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-207">The body of the request contains the following form-url-encoded data:</span></span>

- <span data-ttu-id="3d7a1-208">grant\_type: "password"</span><span class="sxs-lookup"><span data-stu-id="3d7a1-208">grant\_type: "password"</span></span>
- <span data-ttu-id="3d7a1-209">ユーザー名:&lt;ユーザーの電子メール&gt;</span><span class="sxs-lookup"><span data-stu-id="3d7a1-209">username: &lt;the user's email&gt;</span></span>
- <span data-ttu-id="3d7a1-210">password: &lt;password&gt;</span><span class="sxs-lookup"><span data-stu-id="3d7a1-210">password: &lt;password&gt;</span></span>

<span data-ttu-id="3d7a1-211">AJAX 要求を送信する JavaScript コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-211">Here is the JavaScript code that sends the AJAX request:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

<span data-ttu-id="3d7a1-212">要求が成功すると、承認サーバーは、応答本文にアクセス トークンを返します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-212">If the request succeeds, the authorization server returns an access token in the response body.</span></span> <span data-ttu-id="3d7a1-213">API に要求を送信するときに後で使用する、セッションの記憶域にトークンを格納することを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-213">Notice that we store the token in session storage, to use later when sending requests to the API.</span></span> <span data-ttu-id="3d7a1-214">(Cookie ベースの認証など) の認証の一部のフォームとは異なりブラウザーは自動的にアクセス トークンに含めません後続の要求。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-214">Unlike some forms of authentication (such as cookie-based authentication), the browser will not automatically include the access token in subsequent requests.</span></span> <span data-ttu-id="3d7a1-215">アプリケーションする必要があります明示的に行います。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-215">The application must do so explicitly.</span></span> <span data-ttu-id="3d7a1-216">制限されるため、良いことをある[CSRF 脆弱性](preventing-cross-site-request-forgery-csrf-attacks.md)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-216">That's a good thing, because it limits [CSRF vulnerabilities](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

<span data-ttu-id="3d7a1-217">HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-217">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

<span data-ttu-id="3d7a1-218">要求にユーザーの資格情報が含まれることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-218">You can see that the request contains the user's credentials.</span></span> <span data-ttu-id="3d7a1-219">*必要があります*HTTPS を使用して、トランスポート層セキュリティを提供します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-219">You *must* use HTTPS to provide transport layer security.</span></span>

<span data-ttu-id="3d7a1-220">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="3d7a1-220">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

<span data-ttu-id="3d7a1-221">読みやすさは、JSON をインデントし、これはかなり長いはアクセス トークンを切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-221">For readability, I indented the JSON and truncated the access token, which is a quite long.</span></span>

<span data-ttu-id="3d7a1-222">`access_token`、 `token_type`、および`expires_in`プロパティは、OAuth2 仕様によって定義されます。その他のプロパティ (`userName`、 `.issued`、および`.expires`) は情報提供を目的用だけです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-222">The `access_token`, `token_type`, and `expires_in` properties are defined by the OAuth2 spec. The other properties (`userName`, `.issued`, and `.expires`) are just for informational purposes.</span></span> <span data-ttu-id="3d7a1-223">これらのプロパティを追加するコードを検索することができます、 `TokenEndpoint` /Providers/ApplicationOAuthProvider.cs ファイル内のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-223">You can find the code that adds those additional properties in the `TokenEndpoint` method, in the /Providers/ApplicationOAuthProvider.cs file.</span></span>

## <a name="send-an-authenticated-request"></a><span data-ttu-id="3d7a1-224">認証された要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-224">Send an Authenticated Request</span></span>

<span data-ttu-id="3d7a1-225">ベアラー トークンである、API に対して認証された要求を行うおことができます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-225">Now that we have a bearer token, we can make an authenticated request to the API.</span></span> <span data-ttu-id="3d7a1-226">これは、要求で Authorization ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-226">This is done by setting the Authorization header in the request.</span></span> <span data-ttu-id="3d7a1-227">クリックして、 **API を呼び出す**ボタンをもう一度このを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-227">Click the **Call API** button again to see this.</span></span>

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

<span data-ttu-id="3d7a1-228">HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-228">HTTP request:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

<span data-ttu-id="3d7a1-229">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="3d7a1-229">HTTP response:</span></span>

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a><span data-ttu-id="3d7a1-230">ログアウトします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-230">Log Out</span></span>

<span data-ttu-id="3d7a1-231">ブラウザーでは、資格情報またはアクセス トークンをキャッシュしません、ためには、セッションの記憶域から削除して、トークンを「忘れた」だけログ記録です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-231">Because the browser does not cache the credentials or the access token, logging out is simply a matter of "forgetting" the token, by removing it from session storage:</span></span>

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a><span data-ttu-id="3d7a1-232">個々 のアカウントのプロジェクト テンプレートを理解します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-232">Understanding the Individual Accounts Project Template</span></span>

<span data-ttu-id="3d7a1-233">選択すると**個々 のアカウント**ASP.NET Web アプリケーション プロジェクト テンプレートで、プロジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-233">When you select **Individual Accounts** in the ASP.NET Web Application project template, the project includes:</span></span>

- <span data-ttu-id="3d7a1-234">OAuth2 承認サーバーの場合は。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-234">An OAuth2 authorization server.</span></span>
- <span data-ttu-id="3d7a1-235">ユーザー アカウントを管理するための Web API エンドポイント</span><span class="sxs-lookup"><span data-stu-id="3d7a1-235">A Web API endpoint for managing user accounts</span></span>
- <span data-ttu-id="3d7a1-236">ユーザー アカウントを格納するための EF モデルです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-236">An EF model for storing user accounts.</span></span>

<span data-ttu-id="3d7a1-237">次に、これらの機能を実装するアプリケーションのメイン クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-237">Here are the main application classes that implement these features:</span></span>

- <span data-ttu-id="3d7a1-238">`AccountController`。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-238">`AccountController`.</span></span> <span data-ttu-id="3d7a1-239">ユーザー アカウントを管理するためには、Web API エンドポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-239">Provides a Web API endpoint for managing user accounts.</span></span> <span data-ttu-id="3d7a1-240">`Register`アクションは、このチュートリアルで使用した 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-240">The `Register` action is the only one that we used in this tutorial.</span></span> <span data-ttu-id="3d7a1-241">クラスの他のメソッドは、パスワードのリセット、ソーシャル ログイン、およびその他の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-241">Other methods on the class support password reset, social logins, and other functionality.</span></span>
- <span data-ttu-id="3d7a1-242">`ApplicationUser`、/Models/IdentityModels.cs で定義されています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-242">`ApplicationUser`, defined in /Models/IdentityModels.cs.</span></span> <span data-ttu-id="3d7a1-243">このクラスは、メンバーシップ データベース内のユーザー アカウントの EF モデルです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-243">This class is the EF model for user accounts in the membership database.</span></span>
- <span data-ttu-id="3d7a1-244">`ApplicationUserManager`、/App で定義されている\_このクラスから派生 Start/IdentityConfig.cs [UserManager](https://msdn.microsoft.com/library/dn613290.aspx)パスワード、およびなどを確認する、新しいユーザーを作成するなどのユーザー アカウントの操作を実行し、自動的に保持し、データベースに変更します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-244">`ApplicationUserManager`, defined in /App\_Start/IdentityConfig.cs This class derives from [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) and performs operations on user accounts, such as creating a new user, verifying passwords, and so forth, and automatically persists changes to the database.</span></span>
- <span data-ttu-id="3d7a1-245">`ApplicationOAuthProvider`。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-245">`ApplicationOAuthProvider`.</span></span> <span data-ttu-id="3d7a1-246">このオブジェクトは、OWIN ミドルウェアに接続し、ミドルウェアによって生成されるイベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-246">This object plugs into the OWIN middleware, and processes events raised by the middleware.</span></span> <span data-ttu-id="3d7a1-247">派生して[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-247">It derives from [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).</span></span>

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a><span data-ttu-id="3d7a1-248">承認サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-248">Configuring the Authorization Server</span></span>

<span data-ttu-id="3d7a1-249">StartupAuth.cs では、次のコードは、OAuth2 承認サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-249">In StartupAuth.cs, the following code configures the OAuth2 authorization server.</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

<span data-ttu-id="3d7a1-250">`TokenEndpointPath`プロパティは、承認サーバーのエンドポイントへの URL パス。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-250">The `TokenEndpointPath` property is the URL path to the authorization server endpoint.</span></span> <span data-ttu-id="3d7a1-251">URL はそのアプリを使用して、ベアラー トークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-251">That's the URL that app uses to get the bearer tokens.</span></span>

<span data-ttu-id="3d7a1-252">`Provider`プロパティは、OWIN ミドルウェアに接続し、ミドルウェアによって生成されるイベントを処理するプロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-252">The `Provider` property specifies a provider that plugs into the OWIN middleware, and processes events raised by the middleware.</span></span>

<span data-ttu-id="3d7a1-253">アプリがトークンを取得するときに、基本的な流れを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-253">Here is the basic flow when the app wants to get a token:</span></span>

1. <span data-ttu-id="3d7a1-254">アプリが要求を送信するアクセス トークンを取得するには、~/トークンです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-254">To get an access token, the app sends a request to ~/Token.</span></span>
2. <span data-ttu-id="3d7a1-255">OAuth ミドルウェア呼び出し`GrantResourceOwnerCredentials`プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-255">The OAuth middleware calls `GrantResourceOwnerCredentials` on the provider.</span></span>
3. <span data-ttu-id="3d7a1-256">プロバイダーの呼び出し、`ApplicationUserManager`を資格情報を検証し、要求 id を作成します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-256">The provider calls the `ApplicationUserManager` to validate the credentials and create a claims identity.</span></span>
4. <span data-ttu-id="3d7a1-257">成功すると、プロバイダーは、トークンの生成に使用される認証チケットを作成します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-257">If that succeeds, the provider creates an authentication ticket, which is used to generate the token.</span></span>

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

<span data-ttu-id="3d7a1-258">OAuth ミドルウェアは、ユーザー アカウントの設定を認識しません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-258">The OAuth middleware doesn't know anything about the user accounts.</span></span> <span data-ttu-id="3d7a1-259">プロバイダーは、ミドルウェアと ASP.NET Identity の間で通信します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-259">The provider communicates between the middleware and ASP.NET Identity.</span></span> <span data-ttu-id="3d7a1-260">承認サーバーの実装の詳細については、次を参照してください。 [OWIN OAuth 2.0 承認サーバー](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-260">For more information about implementing the authorization server, see [OWIN OAuth 2.0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).</span></span>

### <a name="configuring-web-api-to-use-bearer-tokens"></a><span data-ttu-id="3d7a1-261">ベアラー トークンを使用する Web API を構成します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-261">Configuring Web API to use Bearer Tokens</span></span>

<span data-ttu-id="3d7a1-262">`WebApiConfig.Register`メソッド、次のコードは、Web API パイプラインの認証を設定します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-262">In the `WebApiConfig.Register` method, the following code sets up authentication for the Web API pipeline:</span></span>

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

<span data-ttu-id="3d7a1-263">**HostAuthenticationFilter**クラス ベアラー トークンを使用して認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-263">The **HostAuthenticationFilter** class enables authentication using bearer tokens.</span></span>

<span data-ttu-id="3d7a1-264">**SuppressDefaultHostAuthentication**メソッドが Web API、Web API パイプラインは、IIS または OWIN ミドルウェアのいずれかに到達する前に発生する任意の認証を無視するように指示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-264">The **SuppressDefaultHostAuthentication** method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware.</span></span> <span data-ttu-id="3d7a1-265">こうすれば、のみベアラー トークンを使用して認証を Web API を限定できます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-265">That way, we can restrict Web API to authenticate only using bearer tokens.</span></span>

> [!NOTE]
> <span data-ttu-id="3d7a1-266">具体的には、アプリの MVC 部分は、資格情報を cookie に格納フォーム認証を使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-266">In particular, the MVC portion of your app might use forms authentication, which stores credentials in a cookie.</span></span> <span data-ttu-id="3d7a1-267">Cookie ベースの認証には、CSRF 攻撃を防ぐため、偽造防止トークンの使用が必要です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-267">Cookie-based authentication requires the use of anti-forgery tokens, to prevent CSRF attacks.</span></span> <span data-ttu-id="3d7a1-268">偽造防止トークンをクライアントに送信する web API の便利な方法がないために、web Api では、問題があります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-268">That's a problem for web APIs, because there is no convenient way for the web API to send the anti-forgery token to the client.</span></span> <span data-ttu-id="3d7a1-269">(この問題の詳細については、次を参照してください[CSRF は Web API の攻撃の防止](preventing-cross-site-request-forgery-csrf-attacks.md)。)。呼び出す**SuppressDefaultHostAuthentication**で Web API が cookie に格納されている資格情報から CSRF 攻撃に対して脆弱なことはありません。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-269">(For more background on this issue, see [Preventing CSRF Attacks in Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Calling **SuppressDefaultHostAuthentication** ensures that Web API is not vulnerable to CSRF attacks from credentials stored in cookies.</span></span>


<span data-ttu-id="3d7a1-270">クライアントは、保護されたリソースを要求するときに次に、Web API パイプラインでの動作を示します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-270">When the client requests a protected resource, here is what happens in the Web API pipeline:</span></span>

1. <span data-ttu-id="3d7a1-271">**HostAuthentication**フィルターは、トークンを検証する OAuth ミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-271">The **HostAuthentication** filter calls the OAuth middleware to validate the token.</span></span>
2. <span data-ttu-id="3d7a1-272">ミドルウェアは、トークンを要求の id に変換します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-272">The middleware converts the token into a claims identity.</span></span>
3. <span data-ttu-id="3d7a1-273">この時点では、要求が*認証*ではなく*承認*です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-273">At this point, the request is *authenticated* but not *authorized*.</span></span>
4. <span data-ttu-id="3d7a1-274">承認フィルターは、要求の id を検査します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-274">The authorization filter examines the claims identity.</span></span> <span data-ttu-id="3d7a1-275">クレームは、そのリソースのユーザーを承認する要求が承認されています。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-275">If the claims authorize the user for that resource, the request is authorized.</span></span> <span data-ttu-id="3d7a1-276">既定では、 **[Authorize]**属性は、認証されているすべての要求が承認されます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-276">By default, the **[Authorize]** attribute will authorize any request that is authenticated.</span></span> <span data-ttu-id="3d7a1-277">ただし、ロールまたはその他の要求を承認できます。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-277">However, you can authorize by role or by other claims.</span></span> <span data-ttu-id="3d7a1-278">詳細については、次を参照してください。 [Web API で認証と承認](authentication-and-authorization-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-278">For more information, see [Authentication and Authorization in Web API](authentication-and-authorization-in-aspnet-web-api.md).</span></span>
5. <span data-ttu-id="3d7a1-279">前の手順が成功した場合は、コント ローラーは、保護されたリソースを返します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-279">If the previous steps are successful, the controller returns the protected resource.</span></span> <span data-ttu-id="3d7a1-280">それ以外の場合、クライアントは、401 (未承認) のエラーを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-280">Otherwise, the client receives a 401 (Unauthorized) error.</span></span>

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a><span data-ttu-id="3d7a1-281">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3d7a1-281">Additional Resources</span></span>

- [<span data-ttu-id="3d7a1-282">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="3d7a1-282">ASP.NET Identity</span></span>](../../../identity/index.md)
- <span data-ttu-id="3d7a1-283">[VS2013 RC の SPA テンプレートのセキュリティ機能の理解](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-283">[Understanding Security Features in the SPA Template for VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx).</span></span> <span data-ttu-id="3d7a1-284">MSDN のブログでは、Sun Hongye で投稿します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-284">MSDN blog post by Hongye Sun.</span></span>
- <span data-ttu-id="3d7a1-285">[アカウント、Web API の個々 の分解をテンプレート – パート 2: ローカル アカウント](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-285">[Dissecting the Web API Individual Accounts Template–Part 2: Local Accounts](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/).</span></span> <span data-ttu-id="3d7a1-286">ブログ投稿 Dominick Baier でします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-286">Blog post by Dominick Baier.</span></span>
- <span data-ttu-id="3d7a1-287">[ホストの認証および Web API OWIN の](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-287">[Host authentication and Web API with OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/).</span></span> <span data-ttu-id="3d7a1-288">説明については、`SuppressDefaultHostAuthentication`と`HostAuthenticationFilter`Brock Allen でします。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-288">A good explanation of `SuppressDefaultHostAuthentication` and `HostAuthenticationFilter` by Brock Allen.</span></span>
- <span data-ttu-id="3d7a1-289">[VS 2013 テンプレートの ASP.NET Identity のプロファイル情報をカスタマイズする](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-289">[Customizing profile information in ASP.NET Identity in VS 2013 templates](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx).</span></span> <span data-ttu-id="3d7a1-290">Pranav Rastogi が MSDN ブログの投稿。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-290">MSDN blog post by Pranav Rastogi.</span></span>
- <span data-ttu-id="3d7a1-291">[UserManager クラスは、ASP.NET Identity の要求の有効期間管理あたり](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-291">[Per request lifetime management for UserManager class in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).</span></span> <span data-ttu-id="3d7a1-292">適切な説明 Suhas Joshi で MSDN ブログの投稿、`UserManager`クラスです。</span><span class="sxs-lookup"><span data-stu-id="3d7a1-292">MSDN blog post by Suhas Joshi, with a good explanation of the `UserManager` class.</span></span>
