---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: 開いているリダイレクト攻撃 (c#) |Microsoft ドキュメント
author: jongalloway
description: このチュートリアルでは、ASP.NET MVC アプリケーションで開いているリダイレクト攻撃を防止する方法について説明します。 このチュートリアルでは、加えられた変更について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: ec1cd1791eb6d32e7c1ea50bc6626929cad2960e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879674"
---
<a name="preventing-open-redirection-attacks-c"></a><span data-ttu-id="6b9cb-104">開いているリダイレクト攻撃 (c#)</span><span class="sxs-lookup"><span data-stu-id="6b9cb-104">Preventing Open Redirection Attacks (C#)</span></span>
====================
<span data-ttu-id="6b9cb-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6b9cb-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6b9cb-106">このチュートリアルでは、ASP.NET MVC アプリケーションで開いているリダイレクト攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-106">This tutorial explains how you can prevent open redirection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="6b9cb-107">このチュートリアルでは、ASP.NET MVC 3 で AccountController で加えられた変更について説明し、既存の ASP.NET MVC 1.0 と 2 のアプリケーションでこれらの変更を適用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-107">This tutorial discusses the changes that have been made in the AccountController in ASP.NET MVC 3 and demonstrates how you can apply these changes in your existing ASP.NET MVC 1.0 and 2 applications.</span></span>


## <a name="what-is-an-open-redirection-attack"></a><span data-ttu-id="6b9cb-108">開いているリダイレクト攻撃とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-108">What is an Open Redirection Attack?</span></span>

<span data-ttu-id="6b9cb-109">クエリ文字列またはフォームのデータなどの要求で指定されている URL にリダイレクトする任意の web アプリケーション可能性のある改ざんされるおそれがユーザーをリダイレクトする、悪意のある外部の url。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-109">Any web application that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="6b9cb-110">このによって改ざんされると、開いているリダイレクト攻撃が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-110">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="6b9cb-111">アプリケーション ロジックは、指定された URL へリダイレクトされるたびに、リダイレクト URL 改ざんされていないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-111">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="6b9cb-112">ASP.NET MVC 1.0 と ASP.NET MVC 2 の両方の既定 AccountController で使用するログインを開くにはリダイレクト攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-112">The login used in the default AccountController for both ASP.NET MVC 1.0 and ASP.NET MVC 2 is vulnerable to open redirection attacks.</span></span> <span data-ttu-id="6b9cb-113">しかし、ASP.NET MVC 3 Preview からの修正を使用する既存のアプリケーションを更新する簡単です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-113">Fortunately, it is easy to update your existing applications to use the corrections from the ASP.NET MVC 3 Preview.</span></span>

<span data-ttu-id="6b9cb-114">この脆弱性を理解するのには、ログインのリダイレクトが、既定の ASP.NET MVC 2 Web アプリケーション プロジェクトでどのように動作するかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-114">To understand the vulnerability, let's look at how the login redirection works in a default ASP.NET MVC 2 Web Application project.</span></span> <span data-ttu-id="6b9cb-115">このアプリケーションで [Authorize] 属性を持つコント ローラーのアクションにアクセスしようとしています。 リダイレクト承認されていないユーザー/Account/LogOn ビューにします。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-115">In this application, attempting to visit a controller action that has the [Authorize] attribute will redirect unauthorized users to the /Account/LogOn view.</span></span> <span data-ttu-id="6b9cb-116">このリダイレクト/Account/LogOn に、ログインに成功した後、ユーザーが最初に要求された URL に返されるようにする returnUrl querystring パラメーターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-116">This redirect to /Account/LogOn will include a returnUrl querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span>

<span data-ttu-id="6b9cb-117">次のスクリーン ショット ログオンしていないとき、/Account/ChangePassword ビューにアクセスしようとすると、結果をするリダイレクト/Account/LogOn が確認できますか。ReturnUrl % 2faccount% 2fChangePassword %2f を = です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-117">In the screenshot below, we can see that an attempt to access the /Account/ChangePassword view when not logged in results in a redirect to /Account/LogOn?ReturnUrl=%2fAccount%2fChangePassword%2f.</span></span>

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

<span data-ttu-id="6b9cb-118">**図 01**: 開いているリダイレクトとログイン ページ</span><span class="sxs-lookup"><span data-stu-id="6b9cb-118">**Figure 01**: Login page with an open redirection</span></span>

<span data-ttu-id="6b9cb-119">ReturnUrl querystring パラメーターが検証されていないため、攻撃者は、開いているリダイレクト攻撃を実行するためにパラメーターに任意の URL アドレスを挿入することを変更できます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-119">Since the ReturnUrl querystring parameter is not validated, an attacker can modify it to inject any URL address into the parameter to conduct an open redirection attack.</span></span> <span data-ttu-id="6b9cb-120">これを示すためには、ReturnUrl パラメーターを変更できる[ http://bing.com](http://bing.com)ので、結果として得られるログイン URL になります/アカウント/ログオンですか?ReturnUrl =<http://www.bing.com/>です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-120">To demonstrate this, we can modify the ReturnUrl parameter to [http://bing.com](http://bing.com), so the resulting login URL will be /Account/LogOn?ReturnUrl=<http://www.bing.com/>.</span></span> <span data-ttu-id="6b9cb-121">ログインすると、サイトに、私たちにリダイレクト[ http://bing.com](http://bing.com)です。このリダイレクトが検証されていないため代わりに、ユーザーを騙そうしようとする悪意のあるサイトを指している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-121">Upon successfully logging in to the site, we are redirected to [http://bing.com](http://bing.com). Since this redirection is not validated, it could instead point to a malicious site that attempts to trick the user.</span></span>

### <a name="a-more-complex-open-redirection-attack"></a><span data-ttu-id="6b9cb-122">複雑なオープン リダイレクト攻撃</span><span class="sxs-lookup"><span data-stu-id="6b9cb-122">A more complex Open Redirection Attack</span></span>

<span data-ttu-id="6b9cb-123">開いているリダイレクト攻撃は、攻撃者がに対する脆弱性と、お客様を特定の web サイトにログインしようとしていることを認識しているために、特に危険な[フィッシング攻撃](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-123">Open redirection attacks are especially dangerous because an attacker knows that we're trying to log into a specific website, which makes us vulnerable to a [phishing attack](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx).</span></span> <span data-ttu-id="6b9cb-124">たとえば、攻撃者で、自分のパスワードをキャプチャするために、web サイトのユーザーに悪意のある電子メールを送信でしたできます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-124">For example, an attacker could send malicious emails to website users in an attempt to capture their passwords.</span></span> <span data-ttu-id="6b9cb-125">これを NerdDinner サイトでは機能するしくみを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-125">Let's look at how this would work on the NerdDinner site.</span></span> <span data-ttu-id="6b9cb-126">(開いているリダイレクト攻撃を防ぐために、ライブ NerdDinner サイトが更新されたことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-126">(Note that the live NerdDinner site has been updated to protect against open redirection attacks.)</span></span>

<span data-ttu-id="6b9cb-127">最初に、攻撃者は、送信 us リンク NerdDinner が偽造ページへのリダイレクトを含む、ログイン ページにします。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-127">First, an attacker sends us a link to the login page on NerdDinner that includes a redirect to their forged page:</span></span>

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

<span data-ttu-id="6b9cb-128">戻り先 URL が word dinner からは"n"のない nerddiner.com を指していることを注意してください。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-128">Note that the return URL points to nerddiner.com, which is missing an "n" from the word dinner.</span></span> <span data-ttu-id="6b9cb-129">この例では、これは、攻撃者を制御するドメインです。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-129">In this example, this is a domain that the attacker controls.</span></span> <span data-ttu-id="6b9cb-130">上記のリンクにアクセスするときに、正当な NerdDinner.com ログイン ページが表示されるおです。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-130">When we access the above link, we're taken to the legitimate NerdDinner.com login page.</span></span>

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

<span data-ttu-id="6b9cb-131">**図 02**: 開いているリダイレクトと NerdDinner ログイン ページ</span><span class="sxs-lookup"><span data-stu-id="6b9cb-131">**Figure 02**: NerdDinner login page with an open redirection</span></span>

<span data-ttu-id="6b9cb-132">ログイン時に正しく、ASP.NET MVC AccountController のログオン操作は私たちを returnUrl querystring パラメーターで指定された URL にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-132">When we correctly log in, the ASP.NET MVC AccountController's LogOn action redirects us to the URL specified in the returnUrl querystring parameter.</span></span> <span data-ttu-id="6b9cb-133">この場合、これは、攻撃者が入った URL は[ http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-133">In this case, it's the URL that the attacker has entered, which is [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn).</span></span> <span data-ttu-id="6b9cb-134">私たちは、攻撃者ことを確認するように注意されているため特にお気付きませんこれには、多くの場合は、非常に高く万一しない限り、偽造ページは、正当なログイン ページと同じように検索します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-134">Unless we're extremely watchful, it's very likely we won't notice this, especially because the attacker has been careful to make sure that their forged page looks exactly like the legitimate login page.</span></span> <span data-ttu-id="6b9cb-135">このログイン ページには、もう一度ログインして要求するエラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-135">This login page includes an error message requesting that we login again.</span></span> <span data-ttu-id="6b9cb-136">扱いにくい、パスワードを間違ってお必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-136">Clumsy us, we must have mistyped our password.</span></span>

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

<span data-ttu-id="6b9cb-137">**図 03**: 偽造 NerdDinner ログイン画面</span><span class="sxs-lookup"><span data-stu-id="6b9cb-137">**Figure 03**: Forged NerdDinner Login screen</span></span>

<span data-ttu-id="6b9cb-138">ユーザー名とパスワードを再入力してと偽造ログイン ページ情報を保存することを正当な NerdDinner.com サイトに送信します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-138">When we retype our user name and password, the forged login page saves the information and sends us back to the legitimate NerdDinner.com site.</span></span> <span data-ttu-id="6b9cb-139">この時点では、NerdDinner.com サイトが既に認証 us, ため、そのページに直接偽造ログイン ページにリダイレクトできます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-139">At this point, the NerdDinner.com site has already authenticated us, so the forged login page can redirect directly to that page.</span></span> <span data-ttu-id="6b9cb-140">最終的な結果は、攻撃者は、ユーザー名とパスワード、および、提供されていますがそれらに対応していないことです。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-140">The end result is that the attacker has our user name and password, and we are unaware that we've provided it to them.</span></span>

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a><span data-ttu-id="6b9cb-141">AccountController ログオン アクションで脆弱なコードを見る</span><span class="sxs-lookup"><span data-stu-id="6b9cb-141">Looking at the vulnerable code in the AccountController LogOn Action</span></span>

<span data-ttu-id="6b9cb-142">ASP.NET MVC 2 アプリケーションのログオン操作のコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-142">The code for the LogOn action in an ASP.NET MVC 2 application is shown below.</span></span> <span data-ttu-id="6b9cb-143">ログインに成功すると、コント ローラーに返されるリダイレクト returnUrl に注意してください。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-143">Note that upon a successful login, the controller returns a redirect to the returnUrl.</span></span> <span data-ttu-id="6b9cb-144">ReturnUrl パラメーターに対して検証を実行するないことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-144">You can see that no validation is being performed against the returnUrl parameter.</span></span>

<span data-ttu-id="6b9cb-145">**1 – で ASP.NET MVC 2 のログオン操作を一覧表示します。 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-145">**Listing 1 – ASP.NET MVC 2 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

<span data-ttu-id="6b9cb-146">これで、ASP.NET MVC 3 のログオン操作への変更を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-146">Now let's look at the changes to the ASP.NET MVC 3 LogOn action.</span></span> <span data-ttu-id="6b9cb-147">このコードがという名前の System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって returnUrl パラメーターの検証に変更された`IsLocalUrl()`です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-147">This code has been changed to validate the returnUrl parameter by calling a new method in the System.Web.Mvc.Url helper class named `IsLocalUrl()`.</span></span>

<span data-ttu-id="6b9cb-148">**2 – で ASP.NET MVC 3 のログオン操作を一覧表示します。 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-148">**Listing 2 – ASP.NET MVC 3 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

<span data-ttu-id="6b9cb-149">これに変わりました System.Web.Mvc.Url ヘルパー クラスの新しいメソッドを呼び出すことによって、戻り値 URL パラメーターを検証する`IsLocalUrl()`です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-149">This has been changed to validate the return URL parameter by calling a new method in the System.Web.Mvc.Url helper class, `IsLocalUrl()`.</span></span>

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a><span data-ttu-id="6b9cb-150">ASP.NET MVC 1.0 と MVC 2 を保護するアプリケーション</span><span class="sxs-lookup"><span data-stu-id="6b9cb-150">Protecting Your ASP.NET MVC 1.0 and MVC 2 Applications</span></span>

<span data-ttu-id="6b9cb-151">IsLocalUrl() ヘルパー メソッドを追加し、returnUrl パラメーターを検証するログオン操作を更新して、既存の ASP.NET MVC 1.0 と 2 のアプリケーションで ASP.NET MVC 3 の変更の利用できます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-151">We can take advantage of the ASP.NET MVC 3 changes in our existing ASP.NET MVC 1.0 and 2 applications by adding the IsLocalUrl() helper method and updating the LogOn action to validate the returnUrl parameter.</span></span>

<span data-ttu-id="6b9cb-152">この検証として、System.Web.WebPages 内のメソッドを呼び出す実際には、UrlHelper IsLocalUrl() メソッドは、ASP.NET Web Pages アプリケーションによっても使用されます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-152">The UrlHelper IsLocalUrl() method actually just calling into a method in System.Web.WebPages, as this validation is also used by ASP.NET Web Pages applications.</span></span>

<span data-ttu-id="6b9cb-153">**3 – ASP.NET MVC 3 UrlHelper から IsLocalUrl() メソッドを一覧表示します。 `class`**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-153">**Listing 3 – The IsLocalUrl() method from the ASP.NET MVC 3 UrlHelper `class`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

<span data-ttu-id="6b9cb-154">IsUrlLocalToHost メソッドには、リスト 4 に示すように、実際の検証ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-154">The IsUrlLocalToHost method contains the actual validation logic, as shown in Listing 4.</span></span>

<span data-ttu-id="6b9cb-155">**4 – System.Web.WebPages RequestExtensions クラスから IsUrlLocalToHost() メソッドを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-155">**Listing 4 – IsUrlLocalToHost() method from the System.Web.WebPages RequestExtensions class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

<span data-ttu-id="6b9cb-156">ASP.NET MVC 1.0 または 2 つのアプリケーションでは IsLocalUrl() メソッドを追加、AccountController が可能であれば別のヘルパー クラスに追加することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-156">In our ASP.NET MVC 1.0 or 2 application, we'll add a IsLocalUrl() method to the AccountController, but you're encouraged to add it to a separate helper class if possible.</span></span> <span data-ttu-id="6b9cb-157">2 つの小さな変更、AccountController 内に動作するように IsLocalUrl() の ASP.NET MVC 3 バージョンに実行されます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-157">We will make two small changes to the ASP.NET MVC 3 version of IsLocalUrl() so that it will work inside the AccountController.</span></span> <span data-ttu-id="6b9cb-158">最初に変更しますが、パブリック メソッドからプライベート メソッド コント ローラー内のパブリック メソッドは、コント ローラーのアクションとしてアクセスできるためです。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-158">First, we'll change it from a public method to a private method, since public methods in controllers can be accessed as controller actions.</span></span> <span data-ttu-id="6b9cb-159">次に、アプリケーションのホストに対して URL のホストを確認する呼び出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-159">Second, we'll modify the call that checks the URL host against the application host.</span></span> <span data-ttu-id="6b9cb-160">呼び出しがローカル RequestContext を使用すること、UrlHelper クラスのフィールドです。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-160">That call makes use of a local RequestContext field in the UrlHelper class.</span></span> <span data-ttu-id="6b9cb-161">代わりに、この設定を使用します。RequestContext.HttpContext.Request.Url.Host、これを使用します。Request.Url.Host です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-161">Instead of using this.RequestContext.HttpContext.Request.Url.Host, we will use this.Request.Url.Host.</span></span> <span data-ttu-id="6b9cb-162">次のコードは、ASP.NET MVC 1.0 と 2 のアプリケーションで、コント ローラー クラスで使用するため、変更された IsLocalUrl() メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-162">The following code shows the modified IsLocalUrl() method for use with a controller class in ASP.NET MVC 1.0 and 2 applications.</span></span>

<span data-ttu-id="6b9cb-163">**5 – IsLocalUrl() メソッドを一覧表示するユーザーが変更、MVC コント ローラー クラスで使用します。**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-163">**Listing 5 – IsLocalUrl() method, which is modified for use with an MVC Controller class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

<span data-ttu-id="6b9cb-164">IsLocalUrl() メソッドは、配置では、これでお呼び出すことができます、returnUrl パラメーターを検証する、ログオン アクションから次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-164">Now that the IsLocalUrl() method is in place, we can call it from our LogOn action to validate the returnUrl parameter, as shown in the following code.</span></span>

<span data-ttu-id="6b9cb-165">**6 – returnUrl パラメーターを検証する方法で更新されたログオンを一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="6b9cb-165">**Listing 6 – Updated LogOn method which validates the returnUrl parameter**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

<span data-ttu-id="6b9cb-166">外部戻り先 URL を使用してログインしようとして、開いているリダイレクト攻撃をテストできます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-166">Now we can test an open redirection attack by attempting to log in using an external return URL.</span></span> <span data-ttu-id="6b9cb-167">/Account/LogOn を利用してみましょう。ReturnUrl =<http://www.bing.com/>もう一度です。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-167">Let's use /Account/LogOn?ReturnUrl=<http://www.bing.com/> again.</span></span>

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

<span data-ttu-id="6b9cb-168">**図 04**: 更新済みのログオン操作のテスト</span><span class="sxs-lookup"><span data-stu-id="6b9cb-168">**Figure 04**: Testing the updated LogOn Action</span></span>

<span data-ttu-id="6b9cb-169">ログインすると、後に、外部 URL ではなく Home/index コント ローラーのアクションにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-169">After successfully logging in, we are redirected to the Home/Index Controller action rather than the external URL.</span></span>

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

<span data-ttu-id="6b9cb-170">**図 05**: 開いているリダイレクト攻撃を阻止</span><span class="sxs-lookup"><span data-stu-id="6b9cb-170">**Figure 05**: Open Redirection attack defeated</span></span>

## <a name="summary"></a><span data-ttu-id="6b9cb-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="6b9cb-171">Summary</span></span>

<span data-ttu-id="6b9cb-172">リダイレクト Url は、アプリケーションの URL をパラメーターとして渡される、開いているリダイレクト攻撃が発生します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-172">Open redirection attacks can occur when redirection URLs are passed as parameters in the URL for an application.</span></span> <span data-ttu-id="6b9cb-173">テンプレートを防ぐためにコードが含まれています。 ASP.NET MVC 3 では、リダイレクト攻撃を開きます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-173">The ASP.NET MVC 3 template includes code to protect against open redirection attacks.</span></span> <span data-ttu-id="6b9cb-174">ASP.NET MVC 1.0 と 2 つのアプリケーションをいくつかの変更でこのコードを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-174">You can add this code with some modification to ASP.NET MVC 1.0 and 2 applications.</span></span> <span data-ttu-id="6b9cb-175">ASP.NET 1.0 と 2 つのアプリケーションにログインするときにを開いているリダイレクト攻撃を防ぐためするには、IsLocalUrl() メソッドを追加し、ログオン アクションで returnUrl パラメーターを検証します。</span><span class="sxs-lookup"><span data-stu-id="6b9cb-175">To protect against open redirection attacks when logging into ASP.NET 1.0 and 2 applications, add a IsLocalUrl() method and validate the returnUrl parameter in the LogOn action.</span></span>
