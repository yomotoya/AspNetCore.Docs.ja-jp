---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止 |Microsoft Docs
author: MikeWasson
description: クロスサイト リクエスト フォージェリ (CSRF) 攻撃と ASP.NET Web API での CSRF 対策メジャーを実装する方法について説明します。
ms.author: aspnetcontent
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: b8cb42120822c56f620f22a4c7fcbc4bfc5141d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838531"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a><span data-ttu-id="84aab-103">ASP.NET Web API でクロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="84aab-103">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>
====================
<span data-ttu-id="84aab-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="84aab-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="84aab-105">クロスサイト リクエスト フォージェリ (CSRF) は、攻撃で、ユーザーが現在ログインしている脆弱性のあるサイトに、悪意のあるサイトが要求を送信する場所</span><span class="sxs-lookup"><span data-stu-id="84aab-105">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in</span></span>

<span data-ttu-id="84aab-106">CSRF 攻撃の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="84aab-106">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="84aab-107">ユーザーがログイン`www.example.com`フォーム認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="84aab-107">A user logs into `www.example.com` using forms authentication.</span></span>
2. <span data-ttu-id="84aab-108">サーバーは、ユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="84aab-108">The server authenticates the user.</span></span> <span data-ttu-id="84aab-109">サーバーからの応答には、認証 cookie が含まれています。</span><span class="sxs-lookup"><span data-stu-id="84aab-109">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="84aab-110">ログアウトすると、しなくてもユーザー悪意のある web サイトにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="84aab-110">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="84aab-111">この悪意のあるサイトには、次の HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="84aab-111">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    <span data-ttu-id="84aab-112">フォームのアクションが悪意のあるサイトではない、脆弱性のあるサイトに投稿することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="84aab-112">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="84aab-113">これは、CSRF の"cross-site"の一部です。</span><span class="sxs-lookup"><span data-stu-id="84aab-113">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="84aab-114">ユーザーは、[送信] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="84aab-114">The user clicks the submit button.</span></span> <span data-ttu-id="84aab-115">ブラウザーには、要求の認証クッキーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="84aab-115">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="84aab-116">要求はユーザーの認証コンテキストを持つサーバーで実行されには、認証されたユーザーが許可されているものを何でも実行できます。</span><span class="sxs-lookup"><span data-stu-id="84aab-116">The request runs on the server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="84aab-117">この例には、ユーザーがフォームのボタンをクリックする必要がありますが、悪意のあるページがフォームを自動的に送信するスクリプトを簡単に実行する場合と同様でした。</span><span class="sxs-lookup"><span data-stu-id="84aab-117">Although this example requires the user to click the form button, the malicious page could just as easily run a script that submits the form automatically.</span></span> <span data-ttu-id="84aab-118">さらに、SSL を使用しても、CSRF 攻撃、悪意のあるサイトは、"https://"要求を送信できるので。</span><span class="sxs-lookup"><span data-stu-id="84aab-118">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="84aab-119">通常、CSRF 攻撃は、認証 cookie を使用する web サイトに対して可能なブラウザーすべての関連クッキーを模擬 web サイトに送信するためです。</span><span class="sxs-lookup"><span data-stu-id="84aab-119">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="84aab-120">ただし、CSRF 攻撃では、cookie の利用に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="84aab-120">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="84aab-121">たとえば、基本認証とダイジェスト認証では、また脆弱です。</span><span class="sxs-lookup"><span data-stu-id="84aab-121">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="84aab-122">基本認証またはダイジェスト認証を使用してログオンした後</span><span class="sxs-lookup"><span data-stu-id="84aab-122">After a user logs in with Basic or Digest authentication.</span></span> <span data-ttu-id="84aab-123">セッションが終了するまで、ブラウザーは資格情報を自動的に送信します。</span><span class="sxs-lookup"><span data-stu-id="84aab-123">the browser automatically sends the credentials until the session ends.</span></span>

## <a name="anti-forgery-tokens"></a><span data-ttu-id="84aab-124">偽造防止トークン</span><span class="sxs-lookup"><span data-stu-id="84aab-124">Anti-Forgery Tokens</span></span>

<span data-ttu-id="84aab-125">CSRF 攻撃を防ぐため、ASP.NET MVC とも呼ばれる、偽造防止トークンを使用して*検証トークンを要求*します。</span><span class="sxs-lookup"><span data-stu-id="84aab-125">To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called *request verification tokens*.</span></span>

1. <span data-ttu-id="84aab-126">クライアントは、フォームを含む HTML ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="84aab-126">The client requests an HTML page that contains a form.</span></span>
2. <span data-ttu-id="84aab-127">サーバーには、応答内の 2 つのトークンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="84aab-127">The server includes two tokens in the response.</span></span> <span data-ttu-id="84aab-128">1 つのトークンは cookie として送信されます。</span><span class="sxs-lookup"><span data-stu-id="84aab-128">One token is sent as a cookie.</span></span> <span data-ttu-id="84aab-129">隠しフォーム フィールド、もう一方が配置されます。</span><span class="sxs-lookup"><span data-stu-id="84aab-129">The other is placed in a hidden form field.</span></span> <span data-ttu-id="84aab-130">トークンは、敵対者は、値を推測できないように、ランダムに生成されます。</span><span class="sxs-lookup"><span data-stu-id="84aab-130">The tokens are generated randomly so that an adversary cannot guess the values.</span></span>
3. <span data-ttu-id="84aab-131">クライアントは、フォームを送信する、サーバーに両方のトークンが送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84aab-131">When the client submits the form, it must send both tokens back to the server.</span></span> <span data-ttu-id="84aab-132">クライアントが、cookie として cookie トークンを送信し、フォーム データ内で、フォーム トークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="84aab-132">The client sends the cookie token as a cookie, and it sends the form token inside the form data.</span></span> <span data-ttu-id="84aab-133">(ブラウザー クライアントが自動的にこのユーザーがフォームを送信します。)</span><span class="sxs-lookup"><span data-stu-id="84aab-133">(A browser client automatically does this when the user submits the form.)</span></span>
4. <span data-ttu-id="84aab-134">要求に両方のトークンが含まれていない場合、サーバーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="84aab-134">If a request does not include both tokens, the server disallows the request.</span></span>

<span data-ttu-id="84aab-135">隠しフォーム トークンを使用して HTML フォームの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="84aab-135">Here is an example of an HTML form with a hidden form token:</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

<span data-ttu-id="84aab-136">偽造防止トークンは、悪意のあるページが同一オリジン ポリシーにより、ユーザーのトークンを読み取ることができませんので機能します。</span><span class="sxs-lookup"><span data-stu-id="84aab-136">Anti-forgery tokens work because the malicious page cannot read the user's tokens, due to same-origin policies.</span></span> <span data-ttu-id="84aab-137">([同一オリジン ポリシー](http://www.w3.org/Security/wiki/Same_Origin_Policy)から他のコンテンツにアクセスする 2 つの異なるサイトでホストされているドキュメントを防止します。</span><span class="sxs-lookup"><span data-stu-id="84aab-137">([Same-origin policies](http://www.w3.org/Security/wiki/Same_Origin_Policy) prevent documents hosted on two different sites from accessing each other's content.</span></span> <span data-ttu-id="84aab-138">ように、前の例で、悪意のあるページは、example.com に要求を送信できますが、応答を読み取ることができません)。</span><span class="sxs-lookup"><span data-stu-id="84aab-138">So in the earlier example, the malicious page can send requests to example.com, but it cannot read the response.)</span></span>

<span data-ttu-id="84aab-139">CSRF 攻撃を防ぐため、ブラウザーがサイレント モードで送信任意の認証プロトコルで偽造防止トークンを使用して、ユーザーがログインした後、資格情報します。</span><span class="sxs-lookup"><span data-stu-id="84aab-139">To prevent CSRF attacks, use anti-forgery tokens with any authentication protocol where the browser silently sends credentials after the user logs in.</span></span> <span data-ttu-id="84aab-140">これは、フォーム認証など、cookie ベースの認証プロトコルを含むだけでなく基本認証とダイジェスト認証などのプロトコルします。</span><span class="sxs-lookup"><span data-stu-id="84aab-140">This includes cookie-based authentication protocols, such as forms authentication, as well as protocols such as Basic and Digest authentication.</span></span>

<span data-ttu-id="84aab-141">Nonsafe メソッド (POST、PUT、DELETE) の偽造防止トークンを要求する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84aab-141">You should require anti-forgery tokens for any nonsafe methods (POST, PUT, DELETE).</span></span> <span data-ttu-id="84aab-142">また、安全なメソッド (GET、HEAD) には、副作用がないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="84aab-142">Also, make sure that safe methods (GET, HEAD) do not have any side effects.</span></span> <span data-ttu-id="84aab-143">さらに、CORS または JSONP などのクロス ドメインのサポートを有効にした場合も安全な GET メソッドは、CSRF 攻撃の可能性がある機密データを閲覧できるようにする可能性のある脆弱であります。</span><span class="sxs-lookup"><span data-stu-id="84aab-143">Moreover, if you enable cross-domain support, such as CORS or JSONP, then even safe methods like GET are potentially vulnerable to CSRF attacks, allowing the attacker to read potentially sensitive data.</span></span>

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a><span data-ttu-id="84aab-144">ASP.NET MVC で偽造防止トークン</span><span class="sxs-lookup"><span data-stu-id="84aab-144">Anti-Forgery Tokens in ASP.NET MVC</span></span>

<span data-ttu-id="84aab-145">Razor ページに、偽造防止トークンを追加するには、使用、 **HtmlHelper.AntiForgeryToken**ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="84aab-145">To add the anti-forgery tokens to a Razor page, use the **HtmlHelper.AntiForgeryToken** helper method:</span></span>

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

<span data-ttu-id="84aab-146">このメソッドは、隠しフォーム フィールドを追加し、また cookie トークンを設定します。</span><span class="sxs-lookup"><span data-stu-id="84aab-146">This method adds the hidden form field and also sets the cookie token.</span></span>

## <a name="anti-csrf-and-ajax"></a><span data-ttu-id="84aab-147">CSRF 対策と AJAX</span><span class="sxs-lookup"><span data-stu-id="84aab-147">Anti-CSRF and AJAX</span></span>

<span data-ttu-id="84aab-148">フォームのトークンは、JSON データを HTML フォーム データではなく、AJAX 要求を送信するため、AJAX 要求に対して問題にすることができます。</span><span class="sxs-lookup"><span data-stu-id="84aab-148">The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="84aab-149">1 つのソリューションでは、カスタム HTTP ヘッダーでトークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="84aab-149">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="84aab-150">次のコードでは、Razor 構文を使用して、トークンを生成し、AJAX 要求にトークンが追加されます。</span><span class="sxs-lookup"><span data-stu-id="84aab-150">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> <span data-ttu-id="84aab-151">トークンが呼び出すことによって、サーバーで生成された**AntiForgery.GetTokens**します。</span><span class="sxs-lookup"><span data-stu-id="84aab-151">The tokens are generated at the server by calling **AntiForgery.GetTokens**.</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

<span data-ttu-id="84aab-152">要求を処理するときに、要求ヘッダーからトークンを抽出します。</span><span class="sxs-lookup"><span data-stu-id="84aab-152">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="84aab-153">呼び出して、 **AntiForgery.Validate**トークンを検証するメソッド。</span><span class="sxs-lookup"><span data-stu-id="84aab-153">Then call the **AntiForgery.Validate** method to validate the tokens.</span></span> <span data-ttu-id="84aab-154">**検証**トークンが有効でない場合、メソッドが例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="84aab-154">The **Validate** method throws an exception if the tokens are not valid.</span></span>

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
