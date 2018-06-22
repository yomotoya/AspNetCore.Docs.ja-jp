---
title: ASP.NET Core で開いているリダイレクト攻撃を防止します。
author: ardalis
description: ASP.NET Core アプリケーションに対して開くリダイレクト攻撃を防止する方法を示します
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 75591e37753c24bc959b3a96a54abebb51728364
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278299"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="abab6-103">ASP.NET Core で開いているリダイレクト攻撃を防止します。</span><span class="sxs-lookup"><span data-stu-id="abab6-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="abab6-104">クエリ文字列またはフォームのデータなどの要求で指定されている URL にリダイレクトする web アプリ可能性のある改ざんされるおそれがユーザーをリダイレクトする、悪意のある外部の url。</span><span class="sxs-lookup"><span data-stu-id="abab6-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="abab6-105">このによって改ざんされると、開いているリダイレクト攻撃が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="abab6-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="abab6-106">アプリケーション ロジックは、指定された URL へリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="abab6-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="abab6-107">ASP.NET Core では、アプリを開いているリダイレクト (開いているとも呼ばれるリダイレクト) 攻撃から保護するための組み込みの機能を持ちます。</span><span class="sxs-lookup"><span data-stu-id="abab6-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="abab6-108">開いているリダイレクト攻撃とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="abab6-108">What is an open redirect attack?</span></span>

<span data-ttu-id="abab6-109">Web アプリケーションは、認証を必要とするリソースにアクセスするとき頻繁にユーザーとログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="abab6-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="abab6-110">リダイレクト typlically が含まれています、 `returnUrl` querystring パラメーターが正常にログインした後、ユーザーが最初に要求された URL に返されるようにします。</span><span class="sxs-lookup"><span data-stu-id="abab6-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="abab6-111">ユーザーが認証されると、ユーザーが最初に要求した URL にリダイレクトしているされます。</span><span class="sxs-lookup"><span data-stu-id="abab6-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="abab6-112">リンク先の URL が指定されて、要求のクエリ文字列の悪意のあるユーザーが、クエリ文字列を改ざん可能性があります。</span><span class="sxs-lookup"><span data-stu-id="abab6-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="abab6-113">改ざんされたクエリ文字列には、外部、悪意のあるサイトにユーザーをリダイレクトするサイトを許可できます。</span><span class="sxs-lookup"><span data-stu-id="abab6-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="abab6-114">この手法は、開いているリダイレクト (またはリダイレクト) 攻撃と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="abab6-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="abab6-115">例攻撃</span><span class="sxs-lookup"><span data-stu-id="abab6-115">An example attack</span></span>

<span data-ttu-id="abab6-116">悪意のあるユーザーは、悪意のあるユーザー、ユーザーの資格情報やアプリで機密情報へのアクセスを許可するためのもので、攻撃を開発できます。</span><span class="sxs-lookup"><span data-stu-id="abab6-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="abab6-117">サイトのログイン ページへのリンクをクリックして、ユーザーを誘導する攻撃を開始する、 `returnUrl` querystring 値の URL に追加します。</span><span class="sxs-lookup"><span data-stu-id="abab6-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="abab6-118">たとえば、 [NerdDinner.com](http://nerddinner.com) (ASP.NET MVC の記述) サンプル アプリケーションには、このようなログイン ページは、ここが含まれています:`http://nerddinner.com/Account/LogOn?returnUrl=/Home/About`です。</span><span class="sxs-lookup"><span data-stu-id="abab6-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: `http://nerddinner.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="abab6-119">攻撃には、これらの手順を実行し、次に示します。</span><span class="sxs-lookup"><span data-stu-id="abab6-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="abab6-120">ユーザーへのリンクをクリックした`http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`(に注意してください、2 番目の URL は nerddi**n**er、いない nerddi**nn**er)。</span><span class="sxs-lookup"><span data-stu-id="abab6-120">User clicks a link to `http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="abab6-121">ユーザーが正常にログインします。</span><span class="sxs-lookup"><span data-stu-id="abab6-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="abab6-122">ユーザーがリダイレクト (サイト) によって`http://nerddiner.com/Account/LogOn`(実際のサイトのような悪意のあるサイト)。</span><span class="sxs-lookup"><span data-stu-id="abab6-122">The user is redirected (by the site) to `http://nerddiner.com/Account/LogOn` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="abab6-123">ユーザーがもう一度ログイン (自分の資格情報をサイト悪意のあること) と実際のサイトにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="abab6-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="abab6-124">ユーザーは可能性がありますと考えるログインに、最初の試行が失敗したし、その 2 つ目が成功しました。</span><span class="sxs-lookup"><span data-stu-id="abab6-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="abab6-125">対応していない可能性があります残ります自分の資格情報が侵害されていることができます。</span><span class="sxs-lookup"><span data-stu-id="abab6-125">They will most likely remain unaware their credentials have been compromised.</span></span>

![開いているリダイレクト攻撃プロセス](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="abab6-127">ログイン ページだけでなく、一部のサイトはリダイレクト ページまたはエンドポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="abab6-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="abab6-128">アプリが開いているリダイレクトでは、含まれているページを想像`/Home/Redirect`です。</span><span class="sxs-lookup"><span data-stu-id="abab6-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="abab6-129">攻撃者を作成して、たとえばに移動する電子メールのリンク`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`です。</span><span class="sxs-lookup"><span data-stu-id="abab6-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="abab6-130">一般的なユーザーは、URL を見るし、サイト名で始まるを参照してください。</span><span class="sxs-lookup"><span data-stu-id="abab6-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="abab6-131">信頼する、リンクがクリックしてされます。</span><span class="sxs-lookup"><span data-stu-id="abab6-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="abab6-132">開いているリダイレクトは自分のものと同じでは、フィッシング詐欺サイトにユーザーに送信し、およびユーザーがそうなと考える内容へのログインは、サイトです。</span><span class="sxs-lookup"><span data-stu-id="abab6-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="abab6-133">開いているリダイレクト攻撃から保護します。</span><span class="sxs-lookup"><span data-stu-id="abab6-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="abab6-134">Web アプリケーションを開発する場合は、すべてのユーザーが入力したデータを信頼できないとして扱います。</span><span class="sxs-lookup"><span data-stu-id="abab6-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="abab6-135">アプリケーションに、URL の内容に基づく、ユーザーをリダイレクトする機能がある場合は、このようなリダイレクトはのみ行うことローカルで、アプリ内で (または、既知の url、クエリ文字列で指定することがありますいない任意の URL) を確認します。</span><span class="sxs-lookup"><span data-stu-id="abab6-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="abab6-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="abab6-136">LocalRedirect</span></span>

<span data-ttu-id="abab6-137">使用して、`LocalRedirect`ベースからのヘルパー メソッド`Controller`クラス。</span><span class="sxs-lookup"><span data-stu-id="abab6-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="abab6-138">`LocalRedirect` 非ローカル URL が指定されている場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="abab6-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="abab6-139">それ以外の場合、動作と同じように、`Redirect`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="abab6-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="abab6-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="abab6-140">IsLocalUrl</span></span>

<span data-ttu-id="abab6-141">使用して、 [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)をリダイレクトする前に Url をテストするメソッド。</span><span class="sxs-lookup"><span data-stu-id="abab6-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="abab6-142">次の例では、リダイレクトする前に、URL がローカルかどうかをチェックする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="abab6-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="abab6-143">`IsLocalUrl`メソッドは、悪意のあるサイトにリダイレクトされない誤ってユーザーを保護します。</span><span class="sxs-lookup"><span data-stu-id="abab6-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="abab6-144">ローカルの URL を予期したような状況で非ローカル URL が指定されている場合に指定された URL の詳細を記録できます。</span><span class="sxs-lookup"><span data-stu-id="abab6-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="abab6-145">ログイン リダイレクト Url ではリダイレクト攻撃を診断する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="abab6-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
