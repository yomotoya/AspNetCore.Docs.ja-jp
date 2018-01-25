---
title: "ASP.NET Core でクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止"
author: steve-smith
ms.author: riande
description: "ASP.NET Core でクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: 3831bf737186d10eb1b298f5ec2da1fd33ebedd9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="9188c-103">ASP.NET Core でクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="9188c-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="9188c-104">[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9188c-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="9188c-105">偽造はどのような攻撃を防止しますか。</span><span class="sxs-lookup"><span data-stu-id="9188c-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="9188c-106">クロスサイト リクエスト フォージェリ (とも呼ばれる XSRF または CSRF、発音*「surf*) は、web ホスト アプリケーションそれ悪意のある web サイト影響を与える、クライアント ブラウザーと web サイトを信頼する間の相互作用に対する攻撃そのブラウザー。</span><span class="sxs-lookup"><span data-stu-id="9188c-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="9188c-107">Web ブラウザーの web サイトにいくつかの種類の認証トークンに自動的にすべての要求を送信するため、このような攻撃が可能になります。</span><span class="sxs-lookup"><span data-stu-id="9188c-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="9188c-108">悪用のこのフォームのとも呼ばれる、 *1 回のクリック攻撃*または*乗るセッション*ユーザーの攻撃の活用セッションを認証していたため、します。</span><span class="sxs-lookup"><span data-stu-id="9188c-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="9188c-109">CSRF 攻撃の例:</span><span class="sxs-lookup"><span data-stu-id="9188c-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="9188c-110">ユーザーがログイン`www.example.com`、フォーム認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="9188c-111">サーバーは、ユーザーを認証し、認証 cookie を含んでいる応答を発行します。</span><span class="sxs-lookup"><span data-stu-id="9188c-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="9188c-112">ユーザーは、悪意のあるサイトにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="9188c-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="9188c-113">悪意のあるサイトには、次のような HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9188c-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="9188c-114">フォームのアクションが悪意のあるサイトではない、脆弱なサイトにポストすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9188c-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="9188c-115">これは、CSRF の「クロスサイト」の一部です。</span><span class="sxs-lookup"><span data-stu-id="9188c-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="9188c-116">ユーザーは、[送信] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9188c-116">The user clicks the submit button.</span></span> <span data-ttu-id="9188c-117">ブラウザーには、自動的に要求を使用して要求されたドメイン (この場合は脆弱なサイト) の認証クッキーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9188c-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="9188c-118">要求は、ユーザーの認証コンテキストを使用してサーバー上で実行し、は、認証されたユーザーの操作が許可されているものを何でも実行できます。</span><span class="sxs-lookup"><span data-stu-id="9188c-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="9188c-119">この例では、ユーザーがフォームのボタンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="9188c-120">悪意のあるページには次があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-120">The malicious page could:</span></span>

* <span data-ttu-id="9188c-121">フォームを自動的に送信するスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="9188c-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="9188c-122">フォームの送信を AJAX 要求として送信します。</span><span class="sxs-lookup"><span data-stu-id="9188c-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="9188c-123">CSS を非表示のフォームを使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="9188c-124">SSL を使用すると、CSRF 攻撃を防ぐことは、悪意のあるサイトが送信できる、`https://`要求します。</span><span class="sxs-lookup"><span data-stu-id="9188c-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="9188c-125">一部の攻撃対象が応答するサイトのエンドポイント`GET`要求、(この種の攻撃は、一般的なフォーラム サイトでイメージを許可するが、JavaScript をブロックする) 操作を実行する場合、イメージ タグを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9188c-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="9188c-126">使用して状態を変更するアプリケーション`GET`悪意のある攻撃からの要求を受けます。</span><span class="sxs-lookup"><span data-stu-id="9188c-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="9188c-127">CSRF 攻撃は、ブラウザーが web サイトに関連するすべての cookie を送信するために、認証の cookie を使用する web サイトに対して可能です。</span><span class="sxs-lookup"><span data-stu-id="9188c-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="9188c-128">ただし、CSRF 攻撃では、cookie の悪用に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="9188c-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="9188c-129">たとえば、基本認証とダイジェスト認証では、また脆弱です。</span><span class="sxs-lookup"><span data-stu-id="9188c-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="9188c-130">ユーザーは、基本認証またはダイジェスト認証を使用してログイン、ブラウザーは、セッションが終了するまで自動的に資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="9188c-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="9188c-131">注: このコンテキストで*セッション*をユーザーが認証されたクライアント側のセッションを参照します。</span><span class="sxs-lookup"><span data-stu-id="9188c-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="9188c-132">サーバー側のセッションに関連付けられていないか、[セッション ミドルウェア](xref:fundamentals/app-state)です。</span><span class="sxs-lookup"><span data-stu-id="9188c-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="9188c-133">ユーザーは、によって CSRF 脆弱性を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="9188c-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="9188c-134">Web サイトのログ出力をそれらの使用が終了します。</span><span class="sxs-lookup"><span data-stu-id="9188c-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="9188c-135">ブラウザーの cookie を定期的に削除します。</span><span class="sxs-lookup"><span data-stu-id="9188c-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="9188c-136">CSRF 脆弱性が根本的に、web アプリで、エンド ユーザーではない問題です。</span><span class="sxs-lookup"><span data-stu-id="9188c-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="9188c-137">ASP.NET Core MVC が CSRF に対処しますか。</span><span class="sxs-lookup"><span data-stu-id="9188c-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="9188c-138">Request 偽造対策を使用して ASP.NET Core を実装して、 [ASP.NET Core データ保護スタック](xref:security/data-protection/introduction)です。</span><span class="sxs-lookup"><span data-stu-id="9188c-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="9188c-139">ASP.NET Core データ保護は、サーバー ファームで動作するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="9188c-140">参照してください[データ保護を構成する](xref:security/data-protection/configuration/overview)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="9188c-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="9188c-141">ASP.NET Core request-偽造防止対策の既定のデータ保護の構成</span><span class="sxs-lookup"><span data-stu-id="9188c-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="9188c-142">ASP.NET Core MVC 2.0、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) HTML フォーム要素を偽造防止トークンを挿入します。</span><span class="sxs-lookup"><span data-stu-id="9188c-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="9188c-143">たとえば、Razor ファイルに次のマークアップでは、偽造防止トークンが自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="9188c-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="9188c-144">HTML フォーム要素を偽造防止トークンの自動生成の発生時にします。</span><span class="sxs-lookup"><span data-stu-id="9188c-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="9188c-145">`form`タグが含まれています、`method="post"`属性 AND</span><span class="sxs-lookup"><span data-stu-id="9188c-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="9188c-146">[アクション] 属性が空です。</span><span class="sxs-lookup"><span data-stu-id="9188c-146">The action attribute is empty.</span></span> <span data-ttu-id="9188c-147">( `action=""`) または</span><span class="sxs-lookup"><span data-stu-id="9188c-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="9188c-148">Action 属性が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="9188c-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="9188c-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="9188c-149">(`<form method="post">`)</span></span>

<span data-ttu-id="9188c-150">HTML フォーム要素を偽造防止トークンの自動生成を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="9188c-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="9188c-151">明示的に無効にすると`asp-antiforgery`です。</span><span class="sxs-lookup"><span data-stu-id="9188c-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="9188c-152">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="9188c-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="9188c-153">タグ ヘルパーの使用を form 要素タグ ヘルパー外で選択[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out)です。</span><span class="sxs-lookup"><span data-stu-id="9188c-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="9188c-154">削除、`FormTagHelper`ビューからです。</span><span class="sxs-lookup"><span data-stu-id="9188c-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="9188c-155">削除することができます、 `FormTagHelper` Razor ビューに次のディレクティブを追加することによってビューから。</span><span class="sxs-lookup"><span data-stu-id="9188c-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="9188c-156">[Razor ページ](xref:mvc/razor-pages/index)XSRF/CSRF から自動的に保護されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="9188c-157">追加のコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9188c-157">You don't have to write any additional code.</span></span> <span data-ttu-id="9188c-158">参照してください[XSRF/CSRF および Razor ページ](xref:mvc/razor-pages/index#xsrf)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="9188c-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="9188c-159">CSRF 攻撃から保護する最も一般的な方法は、シンクロナイザー トークン パターン (STP) です。</span><span class="sxs-lookup"><span data-stu-id="9188c-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="9188c-160">STP は、ユーザーがフォームのデータを含むページを要求したときに使用される手法です。</span><span class="sxs-lookup"><span data-stu-id="9188c-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="9188c-161">サーバーは、クライアントに現在のユーザーの id に関連付けられたトークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="9188c-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="9188c-162">クライアントは、検証用のサーバーに、トークンでから返送されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="9188c-163">受信した場合、サーバーが認証されたユーザーの id と一致しないトークン、要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="9188c-164">トークンは、一意で予期しないです。</span><span class="sxs-lookup"><span data-stu-id="9188c-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="9188c-165">トークンは、一連の要求 (確保ページ 1 の前にページ 3 の前にページ 2) が適切な順序を確実にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="9188c-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="9188c-166">ASP.NET Core MVC テンプレート内のすべてのフォームは、antiforgery トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="9188c-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="9188c-167">ビュー ロジックの次の 2 つの例では、antiforgery トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="9188c-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="9188c-168">Antiforgery トークンを明示的に追加したことができます、 ``<form>`` HTML ヘルパーとタグ ヘルパーを使用せず要素``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="9188c-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="9188c-169">各ケースの前のでは、ASP.NET Core は、次のような隠しフォーム フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9188c-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="9188c-170">ASP.NET Core を含む 3 つ[フィルター](xref:mvc/controllers/filters) antiforgery トークンを使用するため: ``ValidateAntiForgeryToken``、 ``AutoValidateAntiforgeryToken``、および``IgnoreAntiforgeryToken``です。</span><span class="sxs-lookup"><span data-stu-id="9188c-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="9188c-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="9188c-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="9188c-172">``ValidateAntiForgeryToken``個々 のアクション、コント ローラーに適用できるアクション フィルターは、またはグローバルにします。</span><span class="sxs-lookup"><span data-stu-id="9188c-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="9188c-173">要求が有効な antiforgery トークンを含まない限り、このフィルターが適用されているアクションに対する要求がブロックされます。</span><span class="sxs-lookup"><span data-stu-id="9188c-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="9188c-174">``ValidateAntiForgeryToken``属性トークンを必要とするための要求のアクション メソッドを含む、修飾`HTTP GET`要求します。</span><span class="sxs-lookup"><span data-stu-id="9188c-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="9188c-175">広範に適用する場合にできるメソッドをオーバーライドすると、``IgnoreAntiforgeryToken``属性。</span><span class="sxs-lookup"><span data-stu-id="9188c-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="9188c-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="9188c-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="9188c-177">通常、ASP.NET Core アプリケーションは、HTTP セーフ メソッド (GET、HEAD、オプション、およびトレース) に対する antiforgery トークンを生成しません。</span><span class="sxs-lookup"><span data-stu-id="9188c-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="9188c-178">広範に適用する代わりに、``ValidateAntiForgeryToken``属性とし、オーバーライドすることで``IgnoreAntiforgeryToken``使用することができます、属性、``AutoValidateAntiforgeryToken``属性。</span><span class="sxs-lookup"><span data-stu-id="9188c-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="9188c-179">この属性の動作と同じように、``ValidateAntiForgeryToken``次の HTTP メソッドを使用して行われる要求のトークンを必要としないためする点を除いて、属性します。</span><span class="sxs-lookup"><span data-stu-id="9188c-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="9188c-180">GET</span><span class="sxs-lookup"><span data-stu-id="9188c-180">GET</span></span>
* <span data-ttu-id="9188c-181">HEAD、</span><span class="sxs-lookup"><span data-stu-id="9188c-181">HEAD</span></span>
* <span data-ttu-id="9188c-182">オプション</span><span class="sxs-lookup"><span data-stu-id="9188c-182">OPTIONS</span></span>
* <span data-ttu-id="9188c-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="9188c-183">TRACE</span></span>

<span data-ttu-id="9188c-184">使用することをお勧め``AutoValidateAntiforgeryToken``非 API のシナリオを広範にします。</span><span class="sxs-lookup"><span data-stu-id="9188c-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="9188c-185">これにより、既定では、後の操作が保護されています。</span><span class="sxs-lookup"><span data-stu-id="9188c-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="9188c-186">代替手段は、しない限り、既定では、antiforgery トークンを無視するのには``ValidateAntiForgeryToken``個々 のアクション メソッドに適用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="9188c-187">より多くする POST アクション メソッドのこのシナリオでは左、保護されていないアプリの CSRF 攻撃に対して脆弱なままです。</span><span class="sxs-lookup"><span data-stu-id="9188c-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="9188c-188">匿名投稿は、antiforgery トークンを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="9188c-189">注: Api はトークン; の非 cookie の一部を送信するための自動メカニズムがありません。実装は、クライアント コードの実装に左右されます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="9188c-190">いくつかの例は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="9188c-190">Some examples are shown below.</span></span>


<span data-ttu-id="9188c-191">例 (クラス レベル):</span><span class="sxs-lookup"><span data-stu-id="9188c-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="9188c-192">例 (グローバル):</span><span class="sxs-lookup"><span data-stu-id="9188c-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="9188c-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="9188c-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="9188c-194">``IgnoreAntiforgeryToken``フィルターを使用して、指定したアクションまたはコント ローラー) に存在する antiforgery トークンの必要性を排除します。</span><span class="sxs-lookup"><span data-stu-id="9188c-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="9188c-195">適用すると、このフィルターはオーバーライドされます``ValidateAntiForgeryToken``や``AutoValidateAntiforgeryToken``(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。</span><span class="sxs-lookup"><span data-stu-id="9188c-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="9188c-196">JavaScript、AJAX、および SPAs</span><span class="sxs-lookup"><span data-stu-id="9188c-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="9188c-197">従来の HTML ベースのアプリケーションで antiforgery トークンは、隠しフォーム フィールドを使用してサーバーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="9188c-198">JavaScript ベースの最新のアプリと単一ページ アプリケーション (SPAs) で多数の要求は、プログラムにより行われます。</span><span class="sxs-lookup"><span data-stu-id="9188c-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="9188c-199">これらの AJAX 要求は、要求ヘッダーまたはクッキー) などその他の手法を使用して、トークンを送信する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="9188c-200">認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合 CSRF は潜在的な問題にします。</span><span class="sxs-lookup"><span data-stu-id="9188c-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="9188c-201">ただし場合は、トークンを格納するローカル ストレージを使用すると、CSRF 脆弱性軽減することが、すべての新しい要求を使用してサーバーをローカル ストレージからの値が自動的に送信しないためです。</span><span class="sxs-lookup"><span data-stu-id="9188c-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="9188c-202">したがって、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨される方法として、トークンを送信する antiforgery トークンを格納します。</span><span class="sxs-lookup"><span data-stu-id="9188c-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="9188c-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="9188c-203">AngularJS</span></span>

<span data-ttu-id="9188c-204">AngularJS は、アドレス CSRF に規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="9188c-205">サーバーが名前を持つ cookie を送信する場合``XSRF-TOKEN``、角速度``$http``サービスに追加されます値この cookie からヘッダーをこのサーバーに要求を送信するとき。</span><span class="sxs-lookup"><span data-stu-id="9188c-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="9188c-206">このプロセスは自動です。ヘッダーを明示的に設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9188c-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="9188c-207">ヘッダー名は``X-XSRF-TOKEN``します。</span><span class="sxs-lookup"><span data-stu-id="9188c-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="9188c-208">サーバーは、このヘッダーを検出し、その内容を検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="9188c-209">ASP.NET Core API のこの規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="9188c-210">呼ばれる cookie のトークンを提供するアプリを構成します。``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="9188c-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="9188c-211">Antiforgery という名前のヘッダーを検索するサービスを構成します。``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="9188c-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="9188c-212">[サンプルを表示](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)です。</span><span class="sxs-lookup"><span data-stu-id="9188c-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="9188c-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9188c-213">JavaScript</span></span>

<span data-ttu-id="9188c-214">でビューを JavaScript を使用して、ビュー内からサービスを使用してトークンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9188c-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="9188c-215">挿入するためには、`Microsoft.AspNetCore.Antiforgery.IAntiforgery`サービス ビューと呼び出しを`GetAndStoreTokens`ように。</span><span class="sxs-lookup"><span data-stu-id="9188c-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="9188c-216">この方法では、サーバーから cookie の設定や、クライアントからの読み取りを直接処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9188c-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="9188c-217">JavaScript では、また、cookie で提供されるトークンにアクセスでき、次に示すように、トークンの値を持つヘッダーを作成する cookie の内容を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="9188c-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="9188c-218">というヘッダーにトークンを送信する要求、スクリプトを構築すると仮定した場合、``X-CSRF-TOKEN``を探して antiforgery サービスの構成、``X-CSRF-TOKEN``ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="9188c-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="9188c-219">次の例では、jQuery を使用して、適切なヘッダーを持つ AJAX 要求を行います。</span><span class="sxs-lookup"><span data-stu-id="9188c-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="9188c-220">Antiforgery を構成します。</span><span class="sxs-lookup"><span data-stu-id="9188c-220">Configuring Antiforgery</span></span>

<span data-ttu-id="9188c-221">`IAntiforgery`antiforgery システムを構成する API を提供します。</span><span class="sxs-lookup"><span data-stu-id="9188c-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="9188c-222">要求することができます、`Configure`のメソッド、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="9188c-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="9188c-223">次の例では、antiforgery トークンを生成し、(上記の既定値の角度の名前付け規則を使用) がクッキーとしての応答で送信するアプリのホーム ページからのミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="9188c-224">オプション</span><span class="sxs-lookup"><span data-stu-id="9188c-224">Options</span></span>

<span data-ttu-id="9188c-225">カスタマイズできる[antiforgery オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary)で`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9188c-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="9188c-226">オプション</span><span class="sxs-lookup"><span data-stu-id="9188c-226">Option</span></span>        | <span data-ttu-id="9188c-227">説明</span><span class="sxs-lookup"><span data-stu-id="9188c-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="9188c-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="9188c-228">CookieDomain</span></span>  | <span data-ttu-id="9188c-229">Cookie のドメイン。</span><span class="sxs-lookup"><span data-stu-id="9188c-229">The domain of the cookie.</span></span> <span data-ttu-id="9188c-230">既定値は `null` です。</span><span class="sxs-lookup"><span data-stu-id="9188c-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="9188c-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="9188c-231">CookieName</span></span>    | <span data-ttu-id="9188c-232">Cookie の名前。</span><span class="sxs-lookup"><span data-stu-id="9188c-232">The name of the cookie.</span></span> <span data-ttu-id="9188c-233">設定されていない、システムは一意の名前の先頭を生成、 `DefaultCookiePrefix` ("です。AspNetCore.Antiforgery。") です。</span><span class="sxs-lookup"><span data-stu-id="9188c-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="9188c-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="9188c-234">CookiePath</span></span>    | <span data-ttu-id="9188c-235">Cookie の設定のパス。</span><span class="sxs-lookup"><span data-stu-id="9188c-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="9188c-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="9188c-236">FormFieldName</span></span> | <span data-ttu-id="9188c-237">ビューで antiforgery トークンを表示するために、antiforgery システムで使用される隠しフォーム フィールドの名前。</span><span class="sxs-lookup"><span data-stu-id="9188c-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="9188c-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="9188c-238">HeaderName</span></span>    | <span data-ttu-id="9188c-239">Antiforgery システムによって使用されるヘッダーの名前。</span><span class="sxs-lookup"><span data-stu-id="9188c-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="9188c-240">場合`null`システムには、フォームのデータのみが検討してください。</span><span class="sxs-lookup"><span data-stu-id="9188c-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="9188c-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="9188c-241">RequireSsl</span></span>    | <span data-ttu-id="9188c-242">Antiforgery システムでは SSL が必要かどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="9188c-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="9188c-243">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="9188c-243">Defaults to `false`.</span></span> <span data-ttu-id="9188c-244">場合`true`、SSL 以外の要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="9188c-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="9188c-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="9188c-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="9188c-246">生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="9188c-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="9188c-247">既定では、ヘッダーには"SAMEORIGIN"の値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="9188c-248">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="9188c-248">Defaults to `false`.</span></span> |

<span data-ttu-id="9188c-249">詳細については https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9188c-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="9188c-250">Antiforgery を拡張します。</span><span class="sxs-lookup"><span data-stu-id="9188c-250">Extending Antiforgery</span></span>

<span data-ttu-id="9188c-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、各トークン内の追加データのラウンド トリップで ANTI-XSRF システムの動作を拡張する開発者です。</span><span class="sxs-lookup"><span data-stu-id="9188c-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="9188c-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_)メソッドが呼び出された各フィールド トークンが生成され、戻り値が生成されたトークンに含まれています。</span><span class="sxs-lookup"><span data-stu-id="9188c-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="9188c-253">実装者が、タイムスタンプ、nonce、またはその他の値を返すし、呼び出すでした[ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_)トークンが検証されると、このデータを検証します。</span><span class="sxs-lookup"><span data-stu-id="9188c-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="9188c-254">生成されたトークンに埋め込まれているクライアントのユーザー名が既にするためにこの情報を含める必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9188c-254">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="9188c-255">トークンは、補足データですが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足データが検証されません。</span><span class="sxs-lookup"><span data-stu-id="9188c-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="9188c-256">Fundamentals</span><span class="sxs-lookup"><span data-stu-id="9188c-256">Fundamentals</span></span>

<span data-ttu-id="9188c-257">CSRF 攻撃は、ドメインのドメインをそのドメインに加えられたすべての要求に関連付けられた cookie を送信する既定のブラウザーの動作に依存します。</span><span class="sxs-lookup"><span data-stu-id="9188c-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="9188c-258">これらの cookie は、ブラウザー内で格納されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="9188c-259">頻繁に認証されたユーザーのセッションの cookie が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9188c-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="9188c-260">Cookie ベースの認証は、認証の一般的な形式です。</span><span class="sxs-lookup"><span data-stu-id="9188c-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="9188c-261">トークン ベースの認証システムがされて普及しつつ、特に、SPAs やその他の「スマート クライアント」シナリオです。</span><span class="sxs-lookup"><span data-stu-id="9188c-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="9188c-262">Cookie ベースの認証</span><span class="sxs-lookup"><span data-stu-id="9188c-262">Cookie-based authentication</span></span>

<span data-ttu-id="9188c-263">ユーザー名とパスワードを使用して、ユーザーが認証されたらを識別および認証されていることを検証するために使用するトークンが発行するしています。</span><span class="sxs-lookup"><span data-stu-id="9188c-263">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="9188c-264">クライアントのすべての要求に付属している cookie が行うと、トークンが格納されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="9188c-265">生成して、この cookie を検証していますが、cookie 認証ミドルウェアによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="9188c-266">ASP.NET Core 提供 cookie[ミドルウェア](../fundamentals/middleware.md)暗号化された cookie にユーザー プリンシパルをシリアル化し、次に、後続の要求の cookie を検証するプリンシパルを再作成し、それを`User`プロパティ`HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="9188c-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="9188c-267">Cookie を使用すると、認証 cookie、フォーム認証チケットのコンテナーだけです。</span><span class="sxs-lookup"><span data-stu-id="9188c-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="9188c-268">チケットは、各要求と一緒にフォーム認証 cookie の値として渡され、サーバー上のフォーム認証で認証されたユーザーを識別するために使用します。</span><span class="sxs-lookup"><span data-stu-id="9188c-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="9188c-269">ユーザーがシステムにログインした場合、ユーザー セッションはサーバー側でが作成され、データベースまたはその他の永続的なストアに格納されてです。</span><span class="sxs-lookup"><span data-stu-id="9188c-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="9188c-270">システムがデータ ストアの実際のセッションを指すセッション キーを生成し、クライアント側の cookie として送信されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-270">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="9188c-271">Web サーバーでは、ユーザーの要求が承認を必要なリソース、いつでもこのセッション キーがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="9188c-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="9188c-272">システムでは、関連付けられたユーザー セッションが要求されたリソースにアクセスするための権限を持つかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="9188c-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="9188c-273">場合は、要求が続行されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-273">If so, the request continues.</span></span> <span data-ttu-id="9188c-274">それ以外の場合、要求は、権限がありませんとして返されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="9188c-275">この方法で cookie を使用して、ステートフルなように見えるアプリケーションを作成する、ユーザーがサーバーで以前に認証できない「ことを忘れないでください」であるためです。</span><span class="sxs-lookup"><span data-stu-id="9188c-275">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="9188c-276">ユーザー トークン</span><span class="sxs-lookup"><span data-stu-id="9188c-276">User tokens</span></span>

<span data-ttu-id="9188c-277">トークン ベースの認証は、サーバーにセッションを格納しません。</span><span class="sxs-lookup"><span data-stu-id="9188c-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="9188c-278">代わりログオンしているときにしている発行されるトークン (antiforgery トークンされません)。</span><span class="sxs-lookup"><span data-stu-id="9188c-278">Instead, when a user is logged in they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="9188c-279">このトークンは、トークンを検証するために必要なすべてのデータを保持します。</span><span class="sxs-lookup"><span data-stu-id="9188c-279">This token holds all the data that's required to validate the token.</span></span> <span data-ttu-id="9188c-280">形式で、ユーザーの情報も含まれています。[クレーム](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model)です。</span><span class="sxs-lookup"><span data-stu-id="9188c-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="9188c-281">ユーザーは、認証を必要とするサーバーのリソースにアクセスする場合、トークンはベアラー {トークン} の形式で追加の承認ヘッダーを持つサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="9188c-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="9188c-282">これにより、アプリケーションを後続の要求では、要求でトークンを渡さサーバー側の検証のためステートレスです。</span><span class="sxs-lookup"><span data-stu-id="9188c-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="9188c-283">このトークンがない*暗号化*; むしろ*エンコード*です。</span><span class="sxs-lookup"><span data-stu-id="9188c-283">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="9188c-284">サーバー側では、トークン内の未加工の情報にアクセスするトークンをデコードすることができます。</span><span class="sxs-lookup"><span data-stu-id="9188c-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="9188c-285">トークンを以降の要求を送信するには、ことができますか、保存するブラウザーのローカル記憶域またはでクッキー。</span><span class="sxs-lookup"><span data-stu-id="9188c-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="9188c-286">トークンが、ローカル ストレージに格納されているが、トークンが、cookie に格納されている場合、問題がある場合に XSRF の脆弱性について心配する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9188c-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="9188c-287">複数のアプリケーションが 1 つのドメインでホストされています。</span><span class="sxs-lookup"><span data-stu-id="9188c-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="9188c-288">にもかかわらず`example1.cloudapp.net`と`example2.cloudapp.net`別のホストは、下にあるすべてのホスト間で暗黙的な信頼関係がある、`*.cloudapp.net`ドメイン。</span><span class="sxs-lookup"><span data-stu-id="9188c-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="9188c-289">この暗黙的な信頼関係は、それぞれの他のクッキー (HTTP クッキーには、同じオリジンのポリシーを AJAX 要求は該当しないとは限りません) に影響を与える可能性のある信頼されていないホストできます。</span><span class="sxs-lookup"><span data-stu-id="9188c-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="9188c-290">ASP.NET Core ランタイムは、フィールド トークンにユーザー名が埋め込まれている悪意のあるサブドメインがセッション トークンを上書きできない場合でもそのことはできません、ユーザーの有効なフィールド トークンを生成するために、いくつかの対策を提供します。</span><span class="sxs-lookup"><span data-stu-id="9188c-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="9188c-291">ただし、このような環境でホストされている場合、組み込みの ANTI-XSRF ルーチンまだことはできません攻撃を防ぐセッションの乗っ取りまたはログイン CSRF です。</span><span class="sxs-lookup"><span data-stu-id="9188c-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="9188c-292">共有ホスティング環境とは、セッションの乗っ取り、ログイン CSRF、およびその他の攻撃に vunerable です。</span><span class="sxs-lookup"><span data-stu-id="9188c-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="9188c-293">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="9188c-293">Additional Resources</span></span>

* <span data-ttu-id="9188c-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web アプリケーション プロジェクトを開きセキュリティ](https://www.owasp.org/index.php/Main_Page)(OWASP)。</span><span class="sxs-lookup"><span data-stu-id="9188c-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
