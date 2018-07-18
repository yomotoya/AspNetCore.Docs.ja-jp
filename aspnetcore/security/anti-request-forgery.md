---
title: ASP.NET Core で防ぐクロスサイト リクエスト フォージェリ (XSRF または CSRF) 攻撃
author: steve-smith
description: 悪意のある web サイトがクライアント ブラウザーとアプリ間のやり取りに影響する web アプリに対する攻撃を防ぐ方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6a30e1e2321ca3a81d6e1a320d1d87dddb3033c7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095789"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="c4dc6-103">ASP.NET Core で防ぐクロスサイト リクエスト フォージェリ (XSRF または CSRF) 攻撃</span><span class="sxs-lookup"><span data-stu-id="c4dc6-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="c4dc6-104">によって[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4dc6-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4dc6-105">クロスサイト リクエスト フォージェリ (XSRF または CSRF の発音とも呼ばれます *「サーフ*) は、悪意ある web アプリ影響を与える、クライアント ブラウザーと web アプリを信頼している間の相互作用 web でホストされたアプリに対する攻撃ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="c4dc6-106">Web ブラウザーの web サイトにすべての要求で自動的に認証トークンの一部の種類を送信するため、これらの攻撃は可能です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="c4dc6-107">悪用のこのフォームとも呼ばれますが、*ワンクリックによる攻撃*または*乗るセッション*セッション、ユーザーの認証に以前の攻撃を利用するためです。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="c4dc6-108">CSRF 攻撃の例:</span><span class="sxs-lookup"><span data-stu-id="c4dc6-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="c4dc6-109">ユーザーにサインイン`www.good-banking-site.com`フォーム認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="c4dc6-110">サーバーは、ユーザーを認証し、認証 cookie を含む応答を発行します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="c4dc6-111">サイトは、有効な認証 cookie を受信するすべての要求を信頼しているために攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="c4dc6-112">ユーザーにアクセスする悪意のあるサイトでは、`www.bad-crook-site.com`します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="c4dc6-113">悪意のあるサイト、`www.bad-crook-site.com`次のような HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="c4dc6-114">注意して、フォームの`action`悪意のあるサイトではなく、脆弱性のあるサイトに投稿します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="c4dc6-115">これは、CSRF の"cross-site"の一部です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="c4dc6-116">ユーザーは、[送信] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-116">The user selects the submit button.</span></span> <span data-ttu-id="c4dc6-117">ブラウザーが、要求を行うし、要求されたドメインの認証クッキーを自動的に含まれます`www.good-banking-site.com`します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="c4dc6-118">要求の実行、`www.good-banking-site.com`ユーザーの認証コンテキストを持つサーバー、認証されたユーザーの実行が許可されている任意のアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="c4dc6-119">ユーザーがフォームを送信するボタンを選択する場合、だけでなく、悪意のあるサイトでは次のことができます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="c4dc6-120">フォームを自動的に送信するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="c4dc6-121">フォームの送信は、AJAX 要求として送信します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="c4dc6-122">CSS を使用してフォームを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-122">Hide the form using CSS.</span></span>

<span data-ttu-id="c4dc6-123">これらの代替シナリオは、すべての操作や悪意のあるサイトに最初にアクセスする以外のユーザーからの入力を必要としません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="c4dc6-124">HTTPS を使用して CSRF 攻撃を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="c4dc6-125">悪意のあるサイトに送信できる、`https://www.good-banking-site.com/`簡単、安全でない要求を送信できるように要求します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="c4dc6-126">一部の攻撃は、イメージ タグを使用して、操作を実行する場合、GET 要求に応答するエンドポイントを対象します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="c4dc6-127">この種の攻撃は、イメージを許可しますが、JavaScript をブロックするフォーラム サイトが一般的です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="c4dc6-128">変数またはリソースの変更で、GET 要求の状態を変更するアプリは、悪意のある攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="c4dc6-129">**状態を変更する GET 要求では、安全です。GET 要求の状態が変更されないことをお勧めします。**</span><span class="sxs-lookup"><span data-stu-id="c4dc6-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="c4dc6-130">CSRF 攻撃は cookie 認証を使用するため、web アプリに対して使用できます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="c4dc6-131">ブラウザーでは、web アプリによって発行されたクッキーを格納します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="c4dc6-132">ストアドの cookie には、認証されたユーザーのセッションの cookie が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="c4dc6-133">ブラウザーでは、すべての cookie に関連付けられているドメイン、web アプリをブラウザー内でアプリに要求が生成する方法に関係なくすべての要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="c4dc6-134">ただし、CSRF 攻撃に制限されない cookie を利用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="c4dc6-135">たとえば、基本認証とダイジェスト認証では、また脆弱です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="c4dc6-136">ブラウザーが、セッションまで、資格情報を自動的に送信するユーザーが基本認証またはダイジェスト認証を使ってサインインした後&dagger;が終了します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="c4dc6-137">&dagger;このコンテキストで*セッション*ユーザーを認証するクライアント側のセッションを指します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="c4dc6-138">これは、サーバー側のセッションに関連しないまたは[ASP.NET Core のセッション ミドルウェア](xref:fundamentals/app-state)します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="c4dc6-139">ユーザーは、CSRF の脆弱性に対する対策を講じることで防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="c4dc6-140">使用してそれらを完了すると、web アプリからサインインします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="c4dc6-141">クリア ブラウザー cookie 定期的にします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="c4dc6-142">ただし、CSRF の脆弱性、基本的に web アプリをエンドユーザーではない問題です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="c4dc6-143">認証の基礎</span><span class="sxs-lookup"><span data-stu-id="c4dc6-143">Authentication fundamentals</span></span>

<span data-ttu-id="c4dc6-144">Cookie ベースの認証は、認証の一般的な形式です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="c4dc6-145">トークン ベースの認証システムがシングル ページ アプリケーション (Spa) の特に普及しつつ、成長しています。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="c4dc6-146">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="c4dc6-146">Cookie-based authentication</span></span>

<span data-ttu-id="c4dc6-147">ユーザー認証、ユーザー名とパスワードを使用すると、それらがトークンを認証と承認のために使用できる認証チケットを格納しているに発行しています。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="c4dc6-148">トークンは、すべての要求、クライアントに付属している cookie として格納されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="c4dc6-149">生成して、この cookie を検証していますが、Cookie 認証ミドルウェアによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="c4dc6-150">[ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="c4dc6-151">以降の要求で、ミドルウェア、cookie を検証、プリンシパルが再作成や、プリンシパルに割り当てます、[ユーザー](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)プロパティの[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="c4dc6-152">トークン ベースの認証</span><span class="sxs-lookup"><span data-stu-id="c4dc6-152">Token-based authentication</span></span>

<span data-ttu-id="c4dc6-153">ユーザーが認証されるときに、発行されたトークン (偽造防止トークンされません) がいます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="c4dc6-154">トークンには形式でユーザー情報が含まれています[クレーム](/dotnet/framework/security/claims-based-identity-model)またはアプリで管理ユーザーの状態をアプリを指す参照トークンです。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="c4dc6-155">ユーザーが認証を必要とするリソースにアクセスしようとした場合、トークンはベアラー トークンの形式で追加の承認ヘッダーを使ってアプリに送信されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="c4dc6-156">これにより、アプリはステートレスです。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-156">This makes the app stateless.</span></span> <span data-ttu-id="c4dc6-157">後続の要求ごとに、サーバー側の検証のための要求でトークンが渡されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="c4dc6-158">このトークンが*暗号化*; が*エンコード*します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="c4dc6-159">サーバーで、その情報にアクセス トークンがデコードされます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="c4dc6-160">後続の要求でトークンを送信するには、ブラウザーのローカル ストレージに格納します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="c4dc6-161">トークンがブラウザーのローカル ストレージに格納されている場合、CSRF の脆弱性について心配いけません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="c4dc6-162">CSRF 問題では、トークンがクッキーに格納されている場合です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="c4dc6-163">1 つのドメインでホストされている複数のアプリ</span><span class="sxs-lookup"><span data-stu-id="c4dc6-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="c4dc6-164">共有ホスティング環境は、セッション ハイジャック、ログイン CSRF、およびその他の攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="c4dc6-165">`example1.contoso.net`と`example2.contoso.net`は別のホストのホスト間で暗黙的な信頼リレーションシップがある、`*.contoso.net`ドメイン。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="c4dc6-166">この暗黙的な信頼関係は、(HTTP クッキーには、AJAX 要求を管理する同一オリジン ポリシーは適用しないとは限りません)、互いの cookie に影響を与える可能性のある信頼されていないホストを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="c4dc6-167">ドメインが共有されない、同じドメインでホストされているアプリ間の信頼された cookie を悪用した攻撃を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="c4dc6-168">各アプリは、独自のドメインでホストされている、ときに、悪用する cookie の暗黙的な信頼関係はありません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="c4dc6-169">ASP.NET Core の偽造防止構成</span><span class="sxs-lookup"><span data-stu-id="c4dc6-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="c4dc6-170">ASP.NET Core では、偽造防止を使用して実装する[ASP.NET Core データ保護](xref:security/data-protection/introduction)します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="c4dc6-171">データ保護スタックは、サーバー ファームで動作するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="c4dc6-172">参照してください[データ保護の構成](xref:security/data-protection/configuration/overview)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="c4dc6-173">ASP.NET Core 2.0 以降では、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)は HTML フォーム要素に偽造防止トークンを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="c4dc6-174">Razor ファイルで次のマークアップでは、偽造防止トークンが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="c4dc6-175">同様に、 [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)フォームのメソッドが GET でない場合は、既定で偽造防止トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="c4dc6-176">偽造防止トークンの自動生成の HTML フォーム要素の発生時に、`<form>`タグが含まれています、`method="post"`属性と、次のいずれかに該当します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="c4dc6-177">Action 属性が空 (`action=""`)。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="c4dc6-178">Action 属性が指定されていない (`<form method="post">`)。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="c4dc6-179">HTML フォーム要素を偽造防止トークンの自動生成を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="c4dc6-180">偽造防止トークンを明示的に無効化、`asp-antiforgery`属性。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="c4dc6-181">フォーム要素はオプトアウトのタグ ヘルパーのタグ ヘルパーを使用して[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="c4dc6-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="c4dc6-182">削除、`FormTagHelper`ビューから。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="c4dc6-183">`FormTagHelper` Razor ビューに、次のディレクティブを追加することで、ビューから削除できます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="c4dc6-184">[Razor ページ](xref:razor-pages/index)XSRF/CSRF から自動的に保護されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="c4dc6-185">詳細については、次を参照してください。 [XSRF/CSRF と Razor ページ](xref:razor-pages/index#xsrf)します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="c4dc6-186">CSRF 攻撃から保護する最も一般的なアプローチは、使用する、*シンクロナイザー トークン パターン*(STP)。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="c4dc6-187">ユーザーがフォーム データを含むページを要求したときに、STP が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="c4dc6-188">サーバーは、クライアントの現在のユーザーの id に関連付けられたトークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="c4dc6-189">クライアントは、検証のためのサーバーにトークンで送信バックアップします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="c4dc6-190">サーバーは、認証されたユーザーの id とも一致しないトークンを受信する場合は、要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="c4dc6-191">トークンは、一意かつ予測不可能です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="c4dc6-192">トークンが一連の要求が適切な順序を確認することもできます (たとえば、要求シーケンスのことを確認: 1 ページ&ndash;2 ページ目&ndash;ページ 3)。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="c4dc6-193">ASP.NET Core MVC と Razor ページ テンプレート内のフォームのすべての偽造防止トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="c4dc6-194">次の 2 つのビューの例では、偽造防止トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="c4dc6-195">偽造防止トークンを明示的に追加、 `<form>` HTML ヘルパーとタグ ヘルパーを使用することがなく要素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="c4dc6-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="c4dc6-196">前のケースの各では、ASP.NET Core は、次のような隠しフォーム フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="c4dc6-197">ASP.NET Core には、3 つが含まれます[フィルター](xref:mvc/controllers/filters)の偽造防止トークンを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="c4dc6-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4dc6-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="c4dc6-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4dc6-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="c4dc6-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="c4dc6-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="c4dc6-201">偽造防止オプション</span><span class="sxs-lookup"><span data-stu-id="c4dc6-201">Antiforgery options</span></span>

<span data-ttu-id="c4dc6-202">カスタマイズ[偽造防止オプション](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c4dc6-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

| <span data-ttu-id="c4dc6-203">オプション</span><span class="sxs-lookup"><span data-stu-id="c4dc6-203">Option</span></span> | <span data-ttu-id="c4dc6-204">説明</span><span class="sxs-lookup"><span data-stu-id="c4dc6-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="c4dc6-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="c4dc6-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="c4dc6-206">偽造防止 cookie を作成するために使用する設定を決定します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="c4dc6-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="c4dc6-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="c4dc6-208">Cookie のドメイン。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-208">The domain of the cookie.</span></span> <span data-ttu-id="c4dc6-209">既定値は `null` です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-209">Defaults to `null`.</span></span> <span data-ttu-id="c4dc6-210">このプロパティは廃止され、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="c4dc6-211">Cookie.Domain お勧めします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="c4dc6-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="c4dc6-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="c4dc6-213">クッキーの名前。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-213">The name of the cookie.</span></span> <span data-ttu-id="c4dc6-214">設定されていないシステム生成一意の名前の先頭に、 [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("。AspNetCore.Antiforgery。")。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="c4dc6-215">このプロパティは廃止され、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="c4dc6-216">Cookie.Name お勧めします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="c4dc6-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="c4dc6-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="c4dc6-218">Cookie の設定のパス。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-218">The path set on the cookie.</span></span> <span data-ttu-id="c4dc6-219">このプロパティは廃止され、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="c4dc6-220">Cookie.Path お勧めします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="c4dc6-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="c4dc6-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="c4dc6-222">ビューで偽造防止トークンを表示するために、偽造防止システムによって使用される隠しフォーム フィールドの名前。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="c4dc6-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="c4dc6-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="c4dc6-224">偽造防止システムによって使用されるヘッダーの名前。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="c4dc6-225">場合`null`フォームのデータのみが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="c4dc6-226">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="c4dc6-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="c4dc6-227">偽造防止システムでは SSL が必要かどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="c4dc6-228">場合`true`、非 SSL 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="c4dc6-229">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-229">Defaults to `false`.</span></span> <span data-ttu-id="c4dc6-230">このプロパティは廃止され、今後のバージョンで削除される予定です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="c4dc6-231">Cookie.SecurePolicy を設定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="c4dc6-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="c4dc6-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="c4dc6-233">生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="c4dc6-234">既定では、"SAMEORIGIN"の値を持つヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="c4dc6-235">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-235">Defaults to `false`.</span></span> |

<span data-ttu-id="c4dc6-236">詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="c4dc6-237">IAntiforgery 偽造防止機能を構成します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="c4dc6-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)偽造防止機能を構成する API を提供します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="c4dc6-239">`IAntiforgery` 要求することができます、`Configure`のメソッド、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="c4dc6-240">次の例では、偽造防止トークンを生成し、(このトピックの後半で説明、既定の Angular の名前付け規則を使用して) クッキーとしての応答で送信するアプリのホーム ページからミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="c4dc6-241">偽造防止検証が必要</span><span class="sxs-lookup"><span data-stu-id="c4dc6-241">Require antiforgery validation</span></span>

<span data-ttu-id="c4dc6-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)個々 のアクション、コント ローラーに適用できるアクション フィルターは、グローバルにします。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="c4dc6-243">要求に有効な偽造防止トークンは、しない限り、このフィルターを適用する操作に対して行われた要求はブロックされます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="c4dc6-244">`ValidateAntiForgeryToken`属性には、HTTP GET 要求を含む、修飾アクション メソッドへの要求のトークンが必要です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="c4dc6-245">場合、`ValidateAntiForgeryToken`アプリのコント ローラー間で属性が適用されるで上書きされること、`IgnoreAntiforgeryToken`属性。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="c4dc6-246">ASP.NET Core では、GET 要求を偽造防止トークンを自動的に追加することをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="c4dc6-247">自動的に安全でない HTTP メソッドのみの偽造防止トークンを検証します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="c4dc6-248">ASP.NET Core アプリは、安全な HTTP メソッド (GET、HEAD、オプション、およびトレース) の偽造防止トークンを生成しません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="c4dc6-249">幅広く適用する代わりに、`ValidateAntiForgeryToken`属性とそれをオーバーライドする`IgnoreAntiforgeryToken`、属性、 [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="c4dc6-250">この属性の動作と同様に、`ValidateAntiForgeryToken`属性は、次の HTTP メソッドを使用して行われる要求のトークンをする必要がない点が異なります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="c4dc6-251">GET</span><span class="sxs-lookup"><span data-stu-id="c4dc6-251">GET</span></span>
* <span data-ttu-id="c4dc6-252">HEAD、</span><span class="sxs-lookup"><span data-stu-id="c4dc6-252">HEAD</span></span>
* <span data-ttu-id="c4dc6-253">オプション</span><span class="sxs-lookup"><span data-stu-id="c4dc6-253">OPTIONS</span></span>
* <span data-ttu-id="c4dc6-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="c4dc6-254">TRACE</span></span>

<span data-ttu-id="c4dc6-255">使用をお勧め`AutoValidateAntiforgeryToken`API 以外のシナリオの広範です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="c4dc6-256">これにより、後の操作が既定では保護されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="c4dc6-257">しない限り、既定では、偽造防止トークンを無視する代替手段です`ValidateAntiForgeryToken`個々 のアクション メソッドに適用されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="c4dc6-258">これは、多くの場合のままにする POST アクション メソッドには、このシナリオで保護されていないを誤って、アプリの CSRF 攻撃に対して脆弱なままです。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="c4dc6-259">すべての投稿では、偽造防止トークンを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="c4dc6-260">Api は、トークンの非 cookie の一部を送信するための自動メカニズムはありません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="c4dc6-261">おそらく、実装は、クライアント コードの実装に依存します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="c4dc6-262">いくつかの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-262">Some examples are shown below:</span></span>

<span data-ttu-id="c4dc6-263">クラス レベルの例:</span><span class="sxs-lookup"><span data-stu-id="c4dc6-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="c4dc6-264">グローバルの例:</span><span class="sxs-lookup"><span data-stu-id="c4dc6-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="c4dc6-265">上書きのグローバルまたはコント ローラーの偽造防止属性</span><span class="sxs-lookup"><span data-stu-id="c4dc6-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="c4dc6-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)フィルターを使用して、特定のアクション (またはコント ローラー) の偽造防止トークンの必要性を排除します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="c4dc6-267">このフィルターを適用すると、オーバーライド`ValidateAntiForgeryToken`と`AutoValidateAntiforgeryToken`(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="c4dc6-268">更新トークンを認証した後</span><span class="sxs-lookup"><span data-stu-id="c4dc6-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="c4dc6-269">ビューまたは Razor ページにユーザーをリダイレクトすることにより、ユーザーが認証された後、トークンを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="c4dc6-270">JavaScript、AJAX、および Spa</span><span class="sxs-lookup"><span data-stu-id="c4dc6-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="c4dc6-271">従来の HTML ベースのアプリで偽造防止トークンは、隠しフォーム フィールドを使用して、サーバーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="c4dc6-272">最新の JavaScript ベースのアプリと Spa では、多数の要求が行われるプログラムを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="c4dc6-273">これらの AJAX 要求は、(要求ヘッダー、cookie など) には、その他の手法を使用して、トークンを送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="c4dc6-274">認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合は、潜在的な問題は CSRF です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="c4dc6-275">トークンを格納するローカル ストレージを使用する場合は、すべての要求でサーバーにローカル ストレージからの値が自動的に送信されないため、CSRF の脆弱性を軽減する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="c4dc6-276">そのため、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨されるアプローチとしては、トークンを送信するで偽造防止トークンを格納します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="c4dc6-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="c4dc6-277">JavaScript</span></span>

<span data-ttu-id="c4dc6-278">JavaScript は、ビューを使用して、トークンを作成できますからビュー内のサービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="c4dc6-279">挿入、 [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)サービスを確認して呼び出したり[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="c4dc6-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="c4dc6-280">このアプローチでは、サーバーから cookie を設定またはクライアントからの読み取りを直接処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="c4dc6-281">上記の例では、JavaScript を使用して、AJAX の POST ヘッダーの非表示フィールドの値を読み取る。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="c4dc6-282">JavaScript は、アクセス トークンを cookie にも、cookie の内容を使用して、トークンの値を持つヘッダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="c4dc6-283">という名前のヘッダーでトークンを送信する要求と仮定すると、スクリプト`X-CSRF-TOKEN`、偽造防止を探してサービスを構成、`X-CSRF-TOKEN`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="c4dc6-284">次の例では、適切なヘッダーでの AJAX 要求を作成するのに JavaScript を使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="c4dc6-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="c4dc6-285">AngularJS</span></span>

<span data-ttu-id="c4dc6-286">AngularJS は、アドレス CSRF に規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="c4dc6-287">サーバーが名前を持つクッキーを送信する場合`XSRF-TOKEN`、AngularJS`$http`サービスがサーバーに要求を送信するときに、ヘッダーに cookie の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="c4dc6-288">このプロセスは自動です。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-288">This process is automatic.</span></span> <span data-ttu-id="c4dc6-289">ヘッダーを明示的に設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="c4dc6-290">ヘッダー名は`X-XSRF-TOKEN`します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="c4dc6-291">サーバーは、このヘッダーを検出し、その内容を検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="c4dc6-292">ASP.NET Core API のこの規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="c4dc6-293">Cookie のトークンを提供するアプリの構成`XSRF-TOKEN`します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="c4dc6-294">偽造防止という名前のヘッダーを検索するサービスを構成`X-XSRF-TOKEN`します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="c4dc6-295">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="c4dc6-296">偽造防止を拡張します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-296">Extend antiforgery</span></span>

<span data-ttu-id="c4dc6-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、開発者は各トークン内の追加データのラウンド トリップで CSRF 対策システムの動作を拡張します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="c4dc6-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)メソッドが呼び出されるフィールドのトークンが生成され、戻り値が生成されたトークン内に埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="c4dc6-299">実装するときのタイムスタンプ、nonce、またはその他の値を返すを呼び出してでした[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)トークンが検証されると、このデータを検証します。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="c4dc6-300">この情報を含める必要はありませんので、クライアントのユーザー名は、生成されたトークンに埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="c4dc6-301">補足データ。 ただし、トークンが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足的なデータが検証されません。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4dc6-302">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c4dc6-302">Additional resources</span></span>

* <span data-ttu-id="c4dc6-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web Application Security Project を開いて](https://www.owasp.org/index.php/Main_Page)(OWASP)。</span><span class="sxs-lookup"><span data-stu-id="c4dc6-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
