---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: オープン リダイレクト攻撃を防ぐ (c#) |Microsoft Docs
author: jongalloway
description: このチュートリアルでは、ASP.NET MVC アプリケーションでオープン リダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、加えられた変更について説明しています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 767f9c85527fbcdf34e700eb32fe0c6cad30bf0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832417"
---
<a name="preventing-open-redirection-attacks-c"></a><span data-ttu-id="4df02-104">オープン リダイレクト攻撃を防ぐ (c#)</span><span class="sxs-lookup"><span data-stu-id="4df02-104">Preventing Open Redirection Attacks (C#)</span></span>
====================
<span data-ttu-id="4df02-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4df02-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4df02-106">このチュートリアルでは、ASP.NET MVC アプリケーションでオープン リダイレクト攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4df02-106">This tutorial explains how you can prevent open redirection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="4df02-107">このチュートリアルでは、ASP.NET MVC 3 で AccountController で加えられた変更について説明し、既存の ASP.NET MVC 1.0 と 2 つのアプリケーションでこれらの変更を適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4df02-107">This tutorial discusses the changes that have been made in the AccountController in ASP.NET MVC 3 and demonstrates how you can apply these changes in your existing ASP.NET MVC 1.0 and 2 applications.</span></span>


## <a name="what-is-an-open-redirection-attack"></a><span data-ttu-id="4df02-108">オープン リダイレクト攻撃とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="4df02-108">What is an Open Redirection Attack?</span></span>

<span data-ttu-id="4df02-109">悪意のある、外部 URL にユーザーをリダイレクトする任意の web アプリケーションなど、クエリ文字列またはフォーム データ要求で指定されている URL にリダイレクトすることができます改ざんされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4df02-109">Any web application that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="4df02-110">この改ざんは、オープン リダイレクト攻撃と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="4df02-110">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="4df02-111">アプリケーション ロジックは、指定された URL にリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4df02-111">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="4df02-112">AccountController の既定の ASP.NET MVC 1.0 と ASP.NET MVC 2 の両方に使用されるログインは、開くリダイレクト攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="4df02-112">The login used in the default AccountController for both ASP.NET MVC 1.0 and ASP.NET MVC 2 is vulnerable to open redirection attacks.</span></span> <span data-ttu-id="4df02-113">さいわい、簡単に、修正の ASP.NET MVC 3 の Preview を使用する既存のアプリケーションを更新できますが。</span><span class="sxs-lookup"><span data-stu-id="4df02-113">Fortunately, it is easy to update your existing applications to use the corrections from the ASP.NET MVC 3 Preview.</span></span>

<span data-ttu-id="4df02-114">この脆弱性を理解するのには、既定の ASP.NET MVC 2 Web アプリケーション プロジェクトでのログイン リダイレクトのしくみを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4df02-114">To understand the vulnerability, let's look at how the login redirection works in a default ASP.NET MVC 2 Web Application project.</span></span> <span data-ttu-id="4df02-115">このアプリケーションで [Authorize] 属性を持つコント ローラー アクションにアクセスしようとしてにリダイレクトされます承認されていないユーザー/Account/LogOn ビュー。</span><span class="sxs-lookup"><span data-stu-id="4df02-115">In this application, attempting to visit a controller action that has the [Authorize] attribute will redirect unauthorized users to the /Account/LogOn view.</span></span> <span data-ttu-id="4df02-116">/Account/LogOn へのリダイレクトをこのユーザーが正常にログインした後、ユーザーが最初に要求された URL に返されるようにする returnUrl クエリ文字列パラメーターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4df02-116">This redirect to /Account/LogOn will include a returnUrl querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span>

<span data-ttu-id="4df02-117">次のスクリーン ショット ログインしていない場合は、/Account/ChangePassword ビューにアクセスしようとすると、/Account/LogOn にリダイレクトでに結果ことがわかりますか。ReturnUrl % 2faccount% 2fChangePassword %2f を = です。</span><span class="sxs-lookup"><span data-stu-id="4df02-117">In the screenshot below, we can see that an attempt to access the /Account/ChangePassword view when not logged in results in a redirect to /Account/LogOn?ReturnUrl=%2fAccount%2fChangePassword%2f.</span></span>

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

<span data-ttu-id="4df02-118">**図 01**: オープンのリダイレクトされたログイン ページ</span><span class="sxs-lookup"><span data-stu-id="4df02-118">**Figure 01**: Login page with an open redirection</span></span>

<span data-ttu-id="4df02-119">ReturnUrl クエリ文字列パラメーターが検証されていないため、攻撃者は、オープン リダイレクト攻撃を実行するパラメーターに任意の URL アドレスを挿入することを変更できます。</span><span class="sxs-lookup"><span data-stu-id="4df02-119">Since the ReturnUrl querystring parameter is not validated, an attacker can modify it to inject any URL address into the parameter to conduct an open redirection attack.</span></span> <span data-ttu-id="4df02-120">これを示すためには、ReturnUrl のパラメーターを変更できます[ http://bing.com](http://bing.com)のため結果として得られるログイン URL になります/アカウント/ログオン、ですか?ReturnUrl =<http://www.bing.com/>します。</span><span class="sxs-lookup"><span data-stu-id="4df02-120">To demonstrate this, we can modify the ReturnUrl parameter to [http://bing.com](http://bing.com), so the resulting login URL will be /Account/LogOn?ReturnUrl=<http://www.bing.com/>.</span></span> <span data-ttu-id="4df02-121">正常にサイトにログインをするとにリダイレクトされます。 [ http://bing.com](http://bing.com)します。</span><span class="sxs-lookup"><span data-stu-id="4df02-121">Upon successfully logging in to the site, we are redirected to [http://bing.com](http://bing.com).</span></span> <span data-ttu-id="4df02-122">このリダイレクトが検証されていないため代わりに、ユーザーをだましてしようとする悪意のあるサイトを指している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4df02-122">Since this redirection is not validated, it could instead point to a malicious site that attempts to trick the user.</span></span>

### <a name="a-more-complex-open-redirection-attack"></a><span data-ttu-id="4df02-123">複雑なオープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="4df02-123">A more complex Open Redirection Attack</span></span>

<span data-ttu-id="4df02-124">オープン リダイレクト攻撃は、攻撃者に対する脆弱性のおかげが特定の web サイトにログインしようとしていることを知っているために、特に危険を[フィッシング攻撃](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4df02-124">Open redirection attacks are especially dangerous because an attacker knows that we're trying to log into a specific website, which makes us vulnerable to a [phishing attack](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx).</span></span> <span data-ttu-id="4df02-125">たとえば、攻撃者は、自分のパスワードをキャプチャするために、web サイトのユーザーに悪意のある電子メールを送信でした。</span><span class="sxs-lookup"><span data-stu-id="4df02-125">For example, an attacker could send malicious emails to website users in an attempt to capture their passwords.</span></span> <span data-ttu-id="4df02-126">NerdDinner サイトでどのように見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4df02-126">Let's look at how this would work on the NerdDinner site.</span></span> <span data-ttu-id="4df02-127">(オープン リダイレクト攻撃から保護するライブ NerdDinner サイトが更新されたことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="4df02-127">(Note that the live NerdDinner site has been updated to protect against open redirection attacks.)</span></span>

<span data-ttu-id="4df02-128">最初に、攻撃者リンクが送信私たちが偽造ページへのリダイレクトを含む NerdDinner にログイン ページ。</span><span class="sxs-lookup"><span data-stu-id="4df02-128">First, an attacker sends us a link to the login page on NerdDinner that includes a redirect to their forged page:</span></span>

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

<span data-ttu-id="4df02-129">戻り先 URL が word dinner から nerddiner.com で、"n"がありませんを指すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4df02-129">Note that the return URL points to nerddiner.com, which is missing an "n" from the word dinner.</span></span> <span data-ttu-id="4df02-130">この例では、これは、攻撃者によって制御されるドメインです。</span><span class="sxs-lookup"><span data-stu-id="4df02-130">In this example, this is a domain that the attacker controls.</span></span> <span data-ttu-id="4df02-131">上記のリンクにアクセスするときに正当な NerdDinner.com ログイン ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="4df02-131">When we access the above link, we're taken to the legitimate NerdDinner.com login page.</span></span>

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

<span data-ttu-id="4df02-132">**図 02**: オープンのリダイレクトで NerdDinner ログイン ページ</span><span class="sxs-lookup"><span data-stu-id="4df02-132">**Figure 02**: NerdDinner login page with an open redirection</span></span>

<span data-ttu-id="4df02-133">誤ってログ、ASP.NET MVC AccountController のログオン操作は私たち returnUrl クエリ文字列パラメーターで指定された URL にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="4df02-133">When we correctly log in, the ASP.NET MVC AccountController's LogOn action redirects us to the URL specified in the returnUrl querystring parameter.</span></span> <span data-ttu-id="4df02-134">この場合、これは、攻撃者が入力すると、URL は[ http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)します。</span><span class="sxs-lookup"><span data-stu-id="4df02-134">In this case, it's the URL that the attacker has entered, which is [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn).</span></span> <span data-ttu-id="4df02-135">私たちは、攻撃者が必ず確認してくださいされているため特に、これを気付かない可能性がありますが、非常に常時しない限り、偽造ページは、正当なログイン ページとまったく同じように検索します。</span><span class="sxs-lookup"><span data-stu-id="4df02-135">Unless we're extremely watchful, it's very likely we won't notice this, especially because the attacker has been careful to make sure that their forged page looks exactly like the legitimate login page.</span></span> <span data-ttu-id="4df02-136">このログイン ページには、もう一度ログイン要求するエラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4df02-136">This login page includes an error message requesting that we login again.</span></span> <span data-ttu-id="4df02-137">少しぎこちない、パスワードをタイプミスしましたする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4df02-137">Clumsy us, we must have mistyped our password.</span></span>

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

<span data-ttu-id="4df02-138">**図 03**: 偽造 NerdDinner ログイン画面</span><span class="sxs-lookup"><span data-stu-id="4df02-138">**Figure 03**: Forged NerdDinner Login screen</span></span>

<span data-ttu-id="4df02-139">ユーザー名とパスワードを再入力し、偽造のログイン ページ情報を保存します正当な NerdDinner.com サイトにマイクロソフトへ送信されます。</span><span class="sxs-lookup"><span data-stu-id="4df02-139">When we retype our user name and password, the forged login page saves the information and sends us back to the legitimate NerdDinner.com site.</span></span> <span data-ttu-id="4df02-140">この時点で NerdDinner.com サイトが既に認証されて、偽造のログイン ページがそのページに直接リダイレクトできるようにします。</span><span class="sxs-lookup"><span data-stu-id="4df02-140">At this point, the NerdDinner.com site has already authenticated us, so the forged login page can redirect directly to that page.</span></span> <span data-ttu-id="4df02-141">最終的な結果は、攻撃者が、ユーザー名とパスワード、そのことも紹介それに対応していません。</span><span class="sxs-lookup"><span data-stu-id="4df02-141">The end result is that the attacker has our user name and password, and we are unaware that we've provided it to them.</span></span>

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a><span data-ttu-id="4df02-142">AccountController ログオン アクションで脆弱なコードを見る</span><span class="sxs-lookup"><span data-stu-id="4df02-142">Looking at the vulnerable code in the AccountController LogOn Action</span></span>

<span data-ttu-id="4df02-143">ASP.NET MVC 2 アプリケーションでのログオン操作のコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4df02-143">The code for the LogOn action in an ASP.NET MVC 2 application is shown below.</span></span> <span data-ttu-id="4df02-144">ログインに成功すると、コント ローラーに返されるリダイレクト、returnUrl に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4df02-144">Note that upon a successful login, the controller returns a redirect to the returnUrl.</span></span> <span data-ttu-id="4df02-145">ReturnUrl パラメーターに対して検証を実行しないことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4df02-145">You can see that no validation is being performed against the returnUrl parameter.</span></span>

<span data-ttu-id="4df02-146">**1 – で ASP.NET MVC 2 のログオン操作を一覧表示します。 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="4df02-146">**Listing 1 – ASP.NET MVC 2 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

<span data-ttu-id="4df02-147">これで、ASP.NET MVC 3 のログオン操作への変更内容を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4df02-147">Now let's look at the changes to the ASP.NET MVC 3 LogOn action.</span></span> <span data-ttu-id="4df02-148">このコードがという名前の System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって returnUrl パラメーターの検証に変更された`IsLocalUrl()`します。</span><span class="sxs-lookup"><span data-stu-id="4df02-148">This code has been changed to validate the returnUrl parameter by calling a new method in the System.Web.Mvc.Url helper class named `IsLocalUrl()`.</span></span>

<span data-ttu-id="4df02-149">**2 – で ASP.NET MVC 3 のログオン操作を一覧表示します。 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="4df02-149">**Listing 2 – ASP.NET MVC 3 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

<span data-ttu-id="4df02-150">これが、System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出して、戻り値 URL パラメーターの検証に変更された`IsLocalUrl()`します。</span><span class="sxs-lookup"><span data-stu-id="4df02-150">This has been changed to validate the return URL parameter by calling a new method in the System.Web.Mvc.Url helper class, `IsLocalUrl()`.</span></span>

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a><span data-ttu-id="4df02-151">ASP.NET MVC 1.0 と MVC 2 を保護するアプリケーション</span><span class="sxs-lookup"><span data-stu-id="4df02-151">Protecting Your ASP.NET MVC 1.0 and MVC 2 Applications</span></span>

<span data-ttu-id="4df02-152">ReturnUrl パラメーターを検証するログオン アクションの更新を IsLocalUrl() のヘルパー メソッドを追加して、既存の ASP.NET MVC 1.0 と 2 つのアプリケーションで ASP.NET MVC 3 の変更の利用できます。</span><span class="sxs-lookup"><span data-stu-id="4df02-152">We can take advantage of the ASP.NET MVC 3 changes in our existing ASP.NET MVC 1.0 and 2 applications by adding the IsLocalUrl() helper method and updating the LogOn action to validate the returnUrl parameter.</span></span>

<span data-ttu-id="4df02-153">この検証として、System.Web.WebPages 内のメソッドを呼び出す実際には、UrlHelper IsLocalUrl() メソッドは、ASP.NET Web Pages アプリケーションによっても使用されます。</span><span class="sxs-lookup"><span data-stu-id="4df02-153">The UrlHelper IsLocalUrl() method actually just calling into a method in System.Web.WebPages, as this validation is also used by ASP.NET Web Pages applications.</span></span>

<span data-ttu-id="4df02-154">**3 – ASP.NET MVC 3 の UrlHelper から IsLocalUrl() メソッドを一覧表示します。 `class`**</span><span class="sxs-lookup"><span data-stu-id="4df02-154">**Listing 3 – The IsLocalUrl() method from the ASP.NET MVC 3 UrlHelper `class`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

<span data-ttu-id="4df02-155">IsUrlLocalToHost メソッドには、リスト 4 に示すように、実際の検証ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4df02-155">The IsUrlLocalToHost method contains the actual validation logic, as shown in Listing 4.</span></span>

<span data-ttu-id="4df02-156">**4 – System.Web.WebPages RequestExtensions クラスから IsUrlLocalToHost() メソッドを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4df02-156">**Listing 4 – IsUrlLocalToHost() method from the System.Web.WebPages RequestExtensions class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

<span data-ttu-id="4df02-157">ASP.NET MVC 1.0 または 2 つのアプリケーションで、AccountController に IsLocalUrl() メソッドを追加しますが、可能であれば別のヘルパー クラスに追加することをお勧めしていること。</span><span class="sxs-lookup"><span data-stu-id="4df02-157">In our ASP.NET MVC 1.0 or 2 application, we'll add a IsLocalUrl() method to the AccountController, but you're encouraged to add it to a separate helper class if possible.</span></span> <span data-ttu-id="4df02-158">AccountController 内で動作するように IsLocalUrl() の ASP.NET MVC 3 バージョンに 2 つの小さな変更が行ったされます。</span><span class="sxs-lookup"><span data-stu-id="4df02-158">We will make two small changes to the ASP.NET MVC 3 version of IsLocalUrl() so that it will work inside the AccountController.</span></span> <span data-ttu-id="4df02-159">最初に、変更しますが、パブリック メソッドから、プライベート メソッドをコント ローラー内のパブリック メソッドをコント ローラー アクションとしてアクセスできますので。</span><span class="sxs-lookup"><span data-stu-id="4df02-159">First, we'll change it from a public method to a private method, since public methods in controllers can be accessed as controller actions.</span></span> <span data-ttu-id="4df02-160">次に、私たちに対してでは、アプリケーション ホスト URL のホストを確認する呼び出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="4df02-160">Second, we'll modify the call that checks the URL host against the application host.</span></span> <span data-ttu-id="4df02-161">呼び出しは、ローカル RequestContext の使用、UrlHelper クラスのフィールド。</span><span class="sxs-lookup"><span data-stu-id="4df02-161">That call makes use of a local RequestContext field in the UrlHelper class.</span></span> <span data-ttu-id="4df02-162">代わりに、これを使用します。RequestContext.HttpContext.Request.Url.Host、これを使用します。Request.Url.Host します。</span><span class="sxs-lookup"><span data-stu-id="4df02-162">Instead of using this.RequestContext.HttpContext.Request.Url.Host, we will use this.Request.Url.Host.</span></span> <span data-ttu-id="4df02-163">次のコードでは、ASP.NET MVC 1.0 と 2 つのアプリケーションでコント ローラー クラスで使用するための変更の IsLocalUrl() メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="4df02-163">The following code shows the modified IsLocalUrl() method for use with a controller class in ASP.NET MVC 1.0 and 2 applications.</span></span>

<span data-ttu-id="4df02-164">**この MVC コント ローラーのクラスを使用するため変更が 5 – IsLocalUrl() メソッドを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4df02-164">**Listing 5 – IsLocalUrl() method, which is modified for use with an MVC Controller class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

<span data-ttu-id="4df02-165">IsLocalUrl() メソッドが、これで呼び出せる returnUrl パラメーターを検証する、ログオン アクションから次のコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="4df02-165">Now that the IsLocalUrl() method is in place, we can call it from our LogOn action to validate the returnUrl parameter, as shown in the following code.</span></span>

<span data-ttu-id="4df02-166">**6 – returnUrl パラメーターを検証する更新されたログオン メソッドを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4df02-166">**Listing 6 – Updated LogOn method which validates the returnUrl parameter**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

<span data-ttu-id="4df02-167">外部の戻り先 URL を使用してログインしようとして、オープン リダイレクト攻撃をテストできます。</span><span class="sxs-lookup"><span data-stu-id="4df02-167">Now we can test an open redirection attack by attempting to log in using an external return URL.</span></span> <span data-ttu-id="4df02-168">/Account/LogOn を使用してみましょうか。ReturnUrl =<http://www.bing.com/>もう一度です。</span><span class="sxs-lookup"><span data-stu-id="4df02-168">Let's use /Account/LogOn?ReturnUrl=<http://www.bing.com/> again.</span></span>

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

<span data-ttu-id="4df02-169">**図 04**: 更新されたログオン操作をテストします。</span><span class="sxs-lookup"><span data-stu-id="4df02-169">**Figure 04**: Testing the updated LogOn Action</span></span>

<span data-ttu-id="4df02-170">正常にログインした後には、私たちは、外部 URL ではなく Home/index コント ローラー アクションにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="4df02-170">After successfully logging in, we are redirected to the Home/Index Controller action rather than the external URL.</span></span>

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

<span data-ttu-id="4df02-171">**図 05**: 敗北オープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="4df02-171">**Figure 05**: Open Redirection attack defeated</span></span>

## <a name="summary"></a><span data-ttu-id="4df02-172">まとめ</span><span class="sxs-lookup"><span data-stu-id="4df02-172">Summary</span></span>

<span data-ttu-id="4df02-173">オープン リダイレクト攻撃は、リダイレクト Url は、アプリケーションの URL をパラメーターとして渡されるときに発生します。</span><span class="sxs-lookup"><span data-stu-id="4df02-173">Open redirection attacks can occur when redirection URLs are passed as parameters in the URL for an application.</span></span> <span data-ttu-id="4df02-174">テンプレートにはから保護するためのコードが含まれています、ASP.NET MVC 3 では、リダイレクト攻撃を開きます。</span><span class="sxs-lookup"><span data-stu-id="4df02-174">The ASP.NET MVC 3 template includes code to protect against open redirection attacks.</span></span> <span data-ttu-id="4df02-175">ASP.NET MVC 1.0 と 2 つのアプリケーションを一部変更してこのコードを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="4df02-175">You can add this code with some modification to ASP.NET MVC 1.0 and 2 applications.</span></span> <span data-ttu-id="4df02-176">オープン リダイレクト攻撃に対する保護のため ASP.NET 1.0 と 2 つのアプリケーションにログインするとき、IsLocalUrl() メソッドを追加し、ログオン中 returnUrl パラメーターを検証します。</span><span class="sxs-lookup"><span data-stu-id="4df02-176">To protect against open redirection attacks when logging into ASP.NET 1.0 and 2 applications, add a IsLocalUrl() method and validate the returnUrl parameter in the LogOn action.</span></span>
