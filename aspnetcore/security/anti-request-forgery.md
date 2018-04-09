---
title: ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃
author: steve-smith
description: 悪意のある web サイトがクライアント ブラウザーとアプリ間の相互作用を与えることができますの web アプリに対する攻撃を防止する方法を検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="a6113-103">ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃</span><span class="sxs-lookup"><span data-stu-id="a6113-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="a6113-104">によって[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6113-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6113-105">クロスサイト リクエスト フォージェリ (とも呼ばれる XSRF または CSRF、発音*「surf*) は、これにより、悪意のある web アプリに影響を与える、クライアント ブラウザーと web アプリを信頼している間の相互作用 web ホスト型アプリへの攻撃ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="a6113-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="a6113-106">Web ブラウザーの web サイトにいくつかの種類の認証トークンに自動的にすべての要求を送信するため、このような攻撃が可能です。</span><span class="sxs-lookup"><span data-stu-id="a6113-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="a6113-107">この形式の脆弱性攻撃とも呼ばれます、 *1 回のクリック攻撃*または*乗るセッション*セッション、ユーザーの認証に以前の攻撃を活用するためです。</span><span class="sxs-lookup"><span data-stu-id="a6113-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="a6113-108">CSRF 攻撃の例:</span><span class="sxs-lookup"><span data-stu-id="a6113-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="a6113-109">ユーザーにサインイン`www.good-banking-site.com`フォーム認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="a6113-110">サーバーは、ユーザーを認証し、認証 cookie を含んでいる応答を発行します。</span><span class="sxs-lookup"><span data-stu-id="a6113-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="a6113-111">サイトは、有効な認証 cookie で受信したすべての要求を信頼しているために攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="a6113-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="a6113-112">ユーザーが、悪意のあるサイトを訪問`www.bad-crook-site.com`です。</span><span class="sxs-lookup"><span data-stu-id="a6113-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="a6113-113">悪意のあるサイト`www.bad-crook-site.com`次のような HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a6113-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="a6113-114">注意して、フォームの`action`悪意のあるサイトではなく、脆弱なサイトに投稿します。</span><span class="sxs-lookup"><span data-stu-id="a6113-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a6113-115">これは、CSRF の「クロスサイト」の一部です。</span><span class="sxs-lookup"><span data-stu-id="a6113-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="a6113-116">ユーザーは、[送信] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="a6113-116">The user selects the submit button.</span></span> <span data-ttu-id="a6113-117">ブラウザーが、要求を行うし、要求されたドメインの認証クッキーを自動的に含まれます`www.good-banking-site.com`です。</span><span class="sxs-lookup"><span data-stu-id="a6113-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="a6113-118">要求が実行されて、`www.good-banking-site.com`ユーザーの認証コンテキストを持つサーバーと、認証されたユーザーの実行が許可されているすべてのアクションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="a6113-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="a6113-119">ユーザーがフォームを送信するボタンを選択すると、悪意のあるサイトことができます。</span><span class="sxs-lookup"><span data-stu-id="a6113-119">When the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="a6113-120">フォームを自動的に送信するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="a6113-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="a6113-121">フォームの送信を AJAX 要求として送信します。</span><span class="sxs-lookup"><span data-stu-id="a6113-121">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="a6113-122">CSS を非表示のフォームを使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-122">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="a6113-123">HTTPS を使用して CSRF 攻撃を阻止しません。</span><span class="sxs-lookup"><span data-stu-id="a6113-123">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="a6113-124">悪意のあるサイトが送信できる、`https://www.good-banking-site.com/`同じくらい簡単に保護されていない要求を送信できるように要求します。</span><span class="sxs-lookup"><span data-stu-id="a6113-124">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="a6113-125">一部の攻撃は、イメージ タグを使用して、操作を実行する場合、GET 要求に応答するエンドポイントを対象します。</span><span class="sxs-lookup"><span data-stu-id="a6113-125">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="a6113-126">この種の攻撃は、イメージの許可は、JavaScript をブロックするフォーラム サイトに共通です。</span><span class="sxs-lookup"><span data-stu-id="a6113-126">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="a6113-127">ここで変数またはリソースが変更、GET 要求に状態を変更するアプリは、悪意のある攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="a6113-127">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="a6113-128">**GET 要求の状態を変更するには安全ではありません。GET 要求の状態を決して変更しないことをお勧めします。**</span><span class="sxs-lookup"><span data-stu-id="a6113-128">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="a6113-129">CSRF 攻撃があるために、認証の cookie を使用する web アプリに対する考えられます。</span><span class="sxs-lookup"><span data-stu-id="a6113-129">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="a6113-130">ブラウザーでは、web アプリによって発行された cookie を保存します。</span><span class="sxs-lookup"><span data-stu-id="a6113-130">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="a6113-131">ストアドの cookie には、認証されたユーザーのセッションの cookie が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a6113-131">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="a6113-132">ブラウザーでは、すべての cookie ドメインに関連付けられた、web アプリに、ブラウザー内アプリケーションへの要求を生成する方法に関係なくすべての要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="a6113-132">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="a6113-133">ただし、CSRF 攻撃は制限に cookie を利用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-133">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="a6113-134">たとえば、基本認証とダイジェスト認証では、また脆弱です。</span><span class="sxs-lookup"><span data-stu-id="a6113-134">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a6113-135">ブラウザーが、セッションまで、資格情報を自動的に送信するユーザーは、基本認証またはダイジェスト認証を使用してサインイン後、&dagger;を終了します。</span><span class="sxs-lookup"><span data-stu-id="a6113-135">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="a6113-136">&dagger;このコンテキストで*セッション*をユーザーが認証されたクライアント側のセッションを参照します。</span><span class="sxs-lookup"><span data-stu-id="a6113-136">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="a6113-137">サーバー側のセッションに関連付けられていないか、 [ASP.NET Core セッション ミドルウェア](xref:fundamentals/app-state)です。</span><span class="sxs-lookup"><span data-stu-id="a6113-137">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="a6113-138">ユーザーは、対策を講じること、CSRF 脆弱性を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="a6113-138">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="a6113-139">使用してそれらを完了すると、web アプリからサインインします。</span><span class="sxs-lookup"><span data-stu-id="a6113-139">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="a6113-140">オフのブラウザーの cookie 定期的にします。</span><span class="sxs-lookup"><span data-stu-id="a6113-140">Clear browser cookies periodically.</span></span>

<span data-ttu-id="a6113-141">CSRF 脆弱性が根本的に、web アプリで、エンド ユーザーではない問題です。</span><span class="sxs-lookup"><span data-stu-id="a6113-141">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="a6113-142">認証の基本事項</span><span class="sxs-lookup"><span data-stu-id="a6113-142">Authentication fundamentals</span></span>

<span data-ttu-id="a6113-143">Cookie ベースの認証は、認証の一般的な形式です。</span><span class="sxs-lookup"><span data-stu-id="a6113-143">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="a6113-144">トークン ベースの認証システム普及しつつ、特に単一ページ アプリケーション (SPAs) の場合。</span><span class="sxs-lookup"><span data-stu-id="a6113-144">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="a6113-145">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="a6113-145">Cookie-based authentication</span></span>

<span data-ttu-id="a6113-146">ユーザーを認証ユーザー名とパスワードを使用して、ときに認証と承認に使用できる認証チケットを含む、トークンが発行するしています。</span><span class="sxs-lookup"><span data-stu-id="a6113-146">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="a6113-147">クライアントのすべての要求に付属している cookie が行うと、トークンが格納されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-147">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="a6113-148">生成して、この cookie を検証していますが、Cookie 認証ミドルウェアによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-148">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="a6113-149">[ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="a6113-149">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="a6113-150">以降の要求でミドルウェア cookie を検証、プリンシパルが再作成およびするプリンシパルが割り当てられます、[ユーザー](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)プロパティ[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)です。</span><span class="sxs-lookup"><span data-stu-id="a6113-150">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="a6113-151">トークン ベースの認証</span><span class="sxs-lookup"><span data-stu-id="a6113-151">Token-based authentication</span></span>

<span data-ttu-id="a6113-152">ユーザーが認証されると、トークン (antiforgery トークンされません) が発行するしています。</span><span class="sxs-lookup"><span data-stu-id="a6113-152">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="a6113-153">トークンには形式でユーザー情報が含まれます[クレーム](/dotnet/framework/security/claims-based-identity-model)またはアプリで管理ユーザーの状態をアプリを指す参照トークンです。</span><span class="sxs-lookup"><span data-stu-id="a6113-153">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="a6113-154">ユーザーが認証を必要とするリソースにアクセスしようとした場合、トークンはベアラー トークンの形式で、追加の承認ヘッダーを使用してアプリに送信されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-154">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="a6113-155">これにより、アプリはステートレスです。</span><span class="sxs-lookup"><span data-stu-id="a6113-155">This makes the app stateless.</span></span> <span data-ttu-id="a6113-156">後続の要求、トークンは、サーバー側の検証の要求で渡されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-156">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="a6113-157">このトークンがない*暗号化*; が*エンコード*です。</span><span class="sxs-lookup"><span data-stu-id="a6113-157">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="a6113-158">サーバーで、その情報へのアクセス トークンがデコードされます。</span><span class="sxs-lookup"><span data-stu-id="a6113-158">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="a6113-159">トークンに後続の要求を送信するには、ブラウザーのローカル ストレージにトークンを格納します。</span><span class="sxs-lookup"><span data-stu-id="a6113-159">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="a6113-160">トークンが、ブラウザーのローカル ストレージに格納されている場合、CSRF 脆弱性を懸念いけません。</span><span class="sxs-lookup"><span data-stu-id="a6113-160">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="a6113-161">CSRF は問題にトークンが、cookie に格納されている場合です。</span><span class="sxs-lookup"><span data-stu-id="a6113-161">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="a6113-162">1 つのドメインでホストされている複数のアプリ</span><span class="sxs-lookup"><span data-stu-id="a6113-162">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="a6113-163">共有ホスティング環境は、セッションの乗っ取り、ログイン CSRF、およびその他の攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="a6113-163">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="a6113-164">`example1.contoso.net`と`example2.contoso.net`別のホストは、下にあるホスト間で暗黙的な信頼関係がある、`*.contoso.net`ドメイン。</span><span class="sxs-lookup"><span data-stu-id="a6113-164">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="a6113-165">この暗黙的な信頼関係は、それぞれの他のクッキー (HTTP クッキーには、同じオリジンのポリシーを AJAX 要求は該当しないとは限りません) に影響を与える可能性のある信頼されていないホストできます。</span><span class="sxs-lookup"><span data-stu-id="a6113-165">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="a6113-166">ドメインを共有していないでは、同じドメインでホストされているアプリ間で信頼されている cookie を利用する攻撃を防止できます。</span><span class="sxs-lookup"><span data-stu-id="a6113-166">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="a6113-167">各アプリケーションが独自のドメインでホストされている場合を悪用する cookie の暗黙的な信頼関係はありません。</span><span class="sxs-lookup"><span data-stu-id="a6113-167">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="a6113-168">ASP.NET Core antiforgery 構成</span><span class="sxs-lookup"><span data-stu-id="a6113-168">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="a6113-169">ASP.NET Core を実装を使用して antiforgery [ASP.NET Core データ保護](xref:security/data-protection/introduction)です。</span><span class="sxs-lookup"><span data-stu-id="a6113-169">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="a6113-170">データ保護スタックは、サーバー ファームで動作するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-170">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="a6113-171">参照してください[データ保護を構成する](xref:security/data-protection/configuration/overview)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="a6113-171">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="a6113-172">ASP.NET Core 2.0 以降では、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery トークンでは HTML フォーム要素に挿入します。</span><span class="sxs-lookup"><span data-stu-id="a6113-172">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="a6113-173">Razor ファイルに次のマークアップでは、antiforgery トークンが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-173">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="a6113-174">同様に、 [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)フォームのメソッドが GET でない場合、既定では antiforgery トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="a6113-174">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="a6113-175">HTML フォーム要素 antiforgery トークンの自動生成の発生時に、`<form>`タグが含まれています、`method="post"`属性と、次のいずれかに該当します。</span><span class="sxs-lookup"><span data-stu-id="a6113-175">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="a6113-176">アクションの属性が空白 (`action=""`)。</span><span class="sxs-lookup"><span data-stu-id="a6113-176">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="a6113-177">Action 属性が指定されていない (`<form method="post">`)。</span><span class="sxs-lookup"><span data-stu-id="a6113-177">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="a6113-178">HTML フォーム要素の antiforgery トークンの自動生成を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="a6113-178">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="a6113-179">Antiforgery トークンを明示的に無効化、`asp-antiforgery`属性。</span><span class="sxs-lookup"><span data-stu-id="a6113-179">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="a6113-180">Form 要素はオプトアウト タグ ヘルパーのタグ ヘルパーの使用によって[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="a6113-180">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="a6113-181">削除、`FormTagHelper`ビューからです。</span><span class="sxs-lookup"><span data-stu-id="a6113-181">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="a6113-182">`FormTagHelper` Razor ビューに次のディレクティブを追加することによってビューから削除することができます。</span><span class="sxs-lookup"><span data-stu-id="a6113-182">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="a6113-183">[Razor ページ](xref:mvc/razor-pages/index)XSRF/CSRF から自動的に保護されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-183">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="a6113-184">詳細については、次を参照してください。 [XSRF/CSRF および Razor ページ](xref:mvc/razor-pages/index#xsrf)です。</span><span class="sxs-lookup"><span data-stu-id="a6113-184">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="a6113-185">CSRF 攻撃から保護する最も一般的な方法は、使用する、*シンクロナイザー トークン パターン*(STP)。</span><span class="sxs-lookup"><span data-stu-id="a6113-185">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="a6113-186">STP は、ユーザーがフォームのデータを含むページを要求するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-186">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="a6113-187">サーバーは、クライアントに現在のユーザーの id に関連付けられたトークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="a6113-187">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="a6113-188">クライアントは、検証用のサーバーに、トークンでから返送されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-188">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="a6113-189">受信した場合、サーバーが認証されたユーザーの id と一致しないトークン、要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-189">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="a6113-190">トークンは、一意で予期しないです。</span><span class="sxs-lookup"><span data-stu-id="a6113-190">The token is unique and unpredictable.</span></span> <span data-ttu-id="a6113-191">一連の要求が適切な順序を確認トークンを使用することができますも (たとえば、要求シーケンスのことを確認: 1 ページ&ndash;ページ 2 &ndash; 3 ページ)。</span><span class="sxs-lookup"><span data-stu-id="a6113-191">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="a6113-192">ASP.NET Core MVC および Razor ページ テンプレートではフォームのすべての antiforgery トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="a6113-192">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="a6113-193">次の 2 つのビューの例では、antiforgery トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="a6113-193">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="a6113-194">Antiforgery トークンを明示的に追加、 `<form>` HTML ヘルパーとタグ ヘルパーを使用せず要素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="a6113-194">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="a6113-195">各ケースの前のでは、ASP.NET Core には、次のような隠しフォーム フィールドが追加されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-195">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="a6113-196">ASP.NET Core を含む 3 つ[フィルター](xref:mvc/controllers/filters) antiforgery トークンを使用するため。</span><span class="sxs-lookup"><span data-stu-id="a6113-196">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="a6113-197">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="a6113-197">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="a6113-198">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a6113-198">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="a6113-199">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="a6113-199">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="a6113-200">Antiforgery オプション</span><span class="sxs-lookup"><span data-stu-id="a6113-200">Antiforgery options</span></span>

<span data-ttu-id="a6113-201">カスタマイズ[antiforgery オプション](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a6113-201">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="a6113-202">オプション</span><span class="sxs-lookup"><span data-stu-id="a6113-202">Option</span></span> | <span data-ttu-id="a6113-203">説明</span><span class="sxs-lookup"><span data-stu-id="a6113-203">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="a6113-204">Cookie</span><span class="sxs-lookup"><span data-stu-id="a6113-204">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="a6113-205">Antiforgery クッキーを作成するために使用する設定を決定します。</span><span class="sxs-lookup"><span data-stu-id="a6113-205">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="a6113-206">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="a6113-206">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="a6113-207">Cookie のドメイン。</span><span class="sxs-lookup"><span data-stu-id="a6113-207">The domain of the cookie.</span></span> <span data-ttu-id="a6113-208">既定値は `null` です。</span><span class="sxs-lookup"><span data-stu-id="a6113-208">Defaults to `null`.</span></span> <span data-ttu-id="a6113-209">このプロパティは廃止されており、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="a6113-209">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a6113-210">Cookie.Domain お勧めします。</span><span class="sxs-lookup"><span data-stu-id="a6113-210">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="a6113-211">CookieName</span><span class="sxs-lookup"><span data-stu-id="a6113-211">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="a6113-212">Cookie の名前。</span><span class="sxs-lookup"><span data-stu-id="a6113-212">The name of the cookie.</span></span> <span data-ttu-id="a6113-213">設定されていないシステム生成一意な名前の先頭に、 [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("です。AspNetCore.Antiforgery。") です。</span><span class="sxs-lookup"><span data-stu-id="a6113-213">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="a6113-214">このプロパティは廃止されており、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="a6113-214">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a6113-215">Cookie.Name お勧めします。</span><span class="sxs-lookup"><span data-stu-id="a6113-215">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="a6113-216">CookiePath</span><span class="sxs-lookup"><span data-stu-id="a6113-216">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="a6113-217">Cookie の設定のパス。</span><span class="sxs-lookup"><span data-stu-id="a6113-217">The path set on the cookie.</span></span> <span data-ttu-id="a6113-218">このプロパティは廃止されており、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="a6113-218">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a6113-219">Cookie.Path お勧めします。</span><span class="sxs-lookup"><span data-stu-id="a6113-219">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="a6113-220">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="a6113-220">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="a6113-221">ビューで antiforgery トークンを表示するために、antiforgery システムで使用される隠しフォーム フィールドの名前。</span><span class="sxs-lookup"><span data-stu-id="a6113-221">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="a6113-222">HeaderName</span><span class="sxs-lookup"><span data-stu-id="a6113-222">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="a6113-223">Antiforgery システムによって使用されるヘッダーの名前。</span><span class="sxs-lookup"><span data-stu-id="a6113-223">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="a6113-224">場合`null`フォームのデータのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-224">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="a6113-225">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="a6113-225">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="a6113-226">Antiforgery システムでは SSL が必要かどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="a6113-226">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="a6113-227">場合`true`、SSL 以外の要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="a6113-227">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="a6113-228">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a6113-228">Defaults to `false`.</span></span> <span data-ttu-id="a6113-229">このプロパティは廃止されており、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="a6113-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="a6113-230">Cookie.SecurePolicy を設定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a6113-230">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="a6113-231">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="a6113-231">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="a6113-232">生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="a6113-232">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="a6113-233">既定では、ヘッダーには"SAMEORIGIN"の値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-233">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="a6113-234">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="a6113-234">Defaults to `false`.</span></span> |

<span data-ttu-id="a6113-235">詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)です。</span><span class="sxs-lookup"><span data-stu-id="a6113-235">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="a6113-236">IAntiforgery で antiforgery 機能を構成します。</span><span class="sxs-lookup"><span data-stu-id="a6113-236">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="a6113-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery 機能を構成する API を提供します。</span><span class="sxs-lookup"><span data-stu-id="a6113-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="a6113-238">`IAntiforgery` 要求されることができます、`Configure`のメソッド、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="a6113-238">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="a6113-239">次の例では、antiforgery トークンを生成し、(このトピックの後半で説明、既定値の角度の名前付け規則を使用) がクッキーとしての応答で送信するアプリのホーム ページからのミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-239">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="a6113-240">Antiforgery 検証を要求します。</span><span class="sxs-lookup"><span data-stu-id="a6113-240">Require antiforgery validation</span></span>

<span data-ttu-id="a6113-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)個々 のアクション、コント ローラーに適用できるアクション フィルターは、またはグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="a6113-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="a6113-242">要求が有効な antiforgery トークンを含まない限り、このフィルターが適用されているアクションに対する要求がブロックされます。</span><span class="sxs-lookup"><span data-stu-id="a6113-242">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="a6113-243">`ValidateAntiForgeryToken`属性には、HTTP GET 要求を含む、修飾アクション メソッドへの要求に対してトークンが必要です。</span><span class="sxs-lookup"><span data-stu-id="a6113-243">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="a6113-244">場合、`ValidateAntiForgeryToken`アプリのコント ローラー間で属性が適用されるでオーバーライドできる、`IgnoreAntiforgeryToken`属性。</span><span class="sxs-lookup"><span data-stu-id="a6113-244">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="a6113-245">ASP.NET Core では、GET 要求を antiforgery トークンを自動的に追加することをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="a6113-245">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="a6113-246">自動的に安全でない HTTP メソッドにのみの antiforgery のトークンを検証します。</span><span class="sxs-lookup"><span data-stu-id="a6113-246">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="a6113-247">ASP.NET Core アプリケーションは、安全な HTTP メソッド (GET、HEAD、オプション、およびトレース) に対する antiforgery トークンを生成しません。</span><span class="sxs-lookup"><span data-stu-id="a6113-247">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="a6113-248">広範に適用する代わりに、`ValidateAntiForgeryToken`属性とし、オーバーライドすることで`IgnoreAntiforgeryToken`、属性、 [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a6113-248">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="a6113-249">この属性の動作と同じように、`ValidateAntiForgeryToken`次の HTTP メソッドを使用して行われる要求のトークンを必要としないためする点を除いて、属性します。</span><span class="sxs-lookup"><span data-stu-id="a6113-249">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="a6113-250">GET</span><span class="sxs-lookup"><span data-stu-id="a6113-250">GET</span></span>
* <span data-ttu-id="a6113-251">HEAD、</span><span class="sxs-lookup"><span data-stu-id="a6113-251">HEAD</span></span>
* <span data-ttu-id="a6113-252">オプション</span><span class="sxs-lookup"><span data-stu-id="a6113-252">OPTIONS</span></span>
* <span data-ttu-id="a6113-253">TRACE</span><span class="sxs-lookup"><span data-stu-id="a6113-253">TRACE</span></span>

<span data-ttu-id="a6113-254">使用をお勧め`AutoValidateAntiforgeryToken`非 API のシナリオを広範にします。</span><span class="sxs-lookup"><span data-stu-id="a6113-254">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="a6113-255">これにより、後の操作が既定では保護されています。</span><span class="sxs-lookup"><span data-stu-id="a6113-255">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="a6113-256">代替手段は、しない限り、既定では、antiforgery トークンを無視するのには`ValidateAntiForgeryToken`は個別のアクション メソッドに適用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-256">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="a6113-257">可能性が高い POST アクション メソッドのままにするのには、このシナリオでは、保護されていない、誤って、アプリの CSRF 攻撃に対して脆弱なままです。</span><span class="sxs-lookup"><span data-stu-id="a6113-257">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="a6113-258">すべての投稿では、antiforgery トークンを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-258">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="a6113-259">Api は、トークンのクッキー以外の一部を送信するための自動メカニズムはありません。</span><span class="sxs-lookup"><span data-stu-id="a6113-259">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="a6113-260">実装は、おそらくクライアント コードの実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="a6113-260">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="a6113-261">いくつかの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a6113-261">Some examples are shown below:</span></span>

<span data-ttu-id="a6113-262">クラス レベルの例:</span><span class="sxs-lookup"><span data-stu-id="a6113-262">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="a6113-263">グローバルの例:</span><span class="sxs-lookup"><span data-stu-id="a6113-263">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="a6113-264">上書きのグローバルまたはコント ローラー antiforgery 属性</span><span class="sxs-lookup"><span data-stu-id="a6113-264">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="a6113-265">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)フィルターを使用して、特定のアクション (またはコント ローラー) の antiforgery トークンの必要性を排除します。</span><span class="sxs-lookup"><span data-stu-id="a6113-265">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="a6113-266">適用すると、このフィルターをオーバーライド`ValidateAntiForgeryToken`と`AutoValidateAntiforgeryToken`(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。</span><span class="sxs-lookup"><span data-stu-id="a6113-266">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="a6113-267">更新トークンを認証した後</span><span class="sxs-lookup"><span data-stu-id="a6113-267">Refresh tokens after authentication</span></span>

<span data-ttu-id="a6113-268">ビューまたは Razor ページのページにユーザーをリダイレクトすることで、ユーザーが認証されると、トークンを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-268">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="a6113-269">JavaScript、AJAX、および SPAs</span><span class="sxs-lookup"><span data-stu-id="a6113-269">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="a6113-270">従来の HTML ベースのアプリでは、antiforgery トークンは隠しフォーム フィールドを使用してサーバーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="a6113-270">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="a6113-271">JavaScript ベースの最新のアプリと SPAs で多数の要求は、プログラムにより行われます。</span><span class="sxs-lookup"><span data-stu-id="a6113-271">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="a6113-272">これらの AJAX 要求は、要求ヘッダーまたはクッキー) などその他の手法を使用して、トークンを送信する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-272">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="a6113-273">認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合は、潜在的な問題は CSRF です。</span><span class="sxs-lookup"><span data-stu-id="a6113-273">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="a6113-274">トークンを格納するローカル ストレージを使用すると、すべての要求でサーバーにローカル ストレージからの値が自動的に送信されないために、CSRF 脆弱性を軽減する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-274">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="a6113-275">したがって、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨される方法として、トークンを送信する antiforgery トークンを格納します。</span><span class="sxs-lookup"><span data-stu-id="a6113-275">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="a6113-276">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a6113-276">JavaScript</span></span>

<span data-ttu-id="a6113-277">JavaScript を使用して、ビューで、トークンを作成できますビュー内からサービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-277">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="a6113-278">挿入、 [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)サービス ビューと呼び出しを[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="a6113-278">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="a6113-279">この方法では、サーバーから cookie の設定や、クライアントからの読み取りを直接処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-279">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="a6113-280">前の例では、AJAX の POST ヘッダーを非表示フィールドの値を読み取り、JavaScript を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-280">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="a6113-281">JavaScript のアクセス cookie トークンと cookie の内容を使用して、トークンの値を持つヘッダーを作成することができますも。</span><span class="sxs-lookup"><span data-stu-id="a6113-281">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="a6113-282">というヘッダーにトークンを送信する要求、スクリプトを想定して`X-CSRF-TOKEN`を探して antiforgery サービスの構成、`X-CSRF-TOKEN`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="a6113-282">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="a6113-283">次の例では、JavaScript を使用して適切なヘッダーを持つ AJAX 要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="a6113-283">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="a6113-284">AngularJS</span><span class="sxs-lookup"><span data-stu-id="a6113-284">AngularJS</span></span>

<span data-ttu-id="a6113-285">AngularJS は、アドレス CSRF に規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-285">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="a6113-286">サーバーが名前を持つ cookie を送信する場合`XSRF-TOKEN`、AngularJS`$http`サービスがサーバーに要求を送信するときに、ヘッダーに cookie の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="a6113-286">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="a6113-287">このプロセスは自動です。</span><span class="sxs-lookup"><span data-stu-id="a6113-287">This process is automatic.</span></span> <span data-ttu-id="a6113-288">ヘッダーは、明示的に設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a6113-288">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="a6113-289">ヘッダー名は`X-XSRF-TOKEN`します。</span><span class="sxs-lookup"><span data-stu-id="a6113-289">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="a6113-290">サーバーは、このヘッダーを検出し、その内容を検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6113-290">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="a6113-291">ASP.NET Core API のこの規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6113-291">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="a6113-292">呼ばれる cookie のトークンを提供するアプリの構成`XSRF-TOKEN`です。</span><span class="sxs-lookup"><span data-stu-id="a6113-292">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="a6113-293">サービスを構成します antiforgery という名前のヘッダーを検索する`X-XSRF-TOKEN`です。</span><span class="sxs-lookup"><span data-stu-id="a6113-293">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="a6113-294">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a6113-294">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="a6113-295">Antiforgery を拡張します。</span><span class="sxs-lookup"><span data-stu-id="a6113-295">Extend antiforgery</span></span>

<span data-ttu-id="a6113-296">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、各トークンの追加データのラウンド トリップしてアンチ CSRF システムの動作を拡張する開発者です。</span><span class="sxs-lookup"><span data-stu-id="a6113-296">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="a6113-297">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)メソッドが呼び出された各フィールド トークンが生成され、戻り値が生成されたトークンに含まれています。</span><span class="sxs-lookup"><span data-stu-id="a6113-297">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="a6113-298">実装者が、タイムスタンプ、nonce、またはその他の値を返すし、呼び出すでした[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)トークンが検証されると、このデータを検証します。</span><span class="sxs-lookup"><span data-stu-id="a6113-298">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="a6113-299">生成されたトークンに埋め込まれているクライアントのユーザー名が既にするためにこの情報を含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a6113-299">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="a6113-300">トークンは、補足データですが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足データが検証されません。</span><span class="sxs-lookup"><span data-stu-id="a6113-300">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6113-301">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a6113-301">Additional resources</span></span>

* <span data-ttu-id="a6113-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web アプリケーション プロジェクトを開きセキュリティ](https://www.owasp.org/index.php/Main_Page)(OWASP)。</span><span class="sxs-lookup"><span data-stu-id="a6113-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
