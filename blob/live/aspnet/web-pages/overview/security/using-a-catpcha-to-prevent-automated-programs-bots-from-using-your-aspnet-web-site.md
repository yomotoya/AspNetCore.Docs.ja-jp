---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "サイトのコンポーネントを使用できるように、ASP.NET Web Razor、CAPTCHA を使用して) |Microsoft ドキュメント"
author: microsoft
description: "ReCaptcha (セキュリティ メジャー) を使用して自動プログラム (bot) がタスクで、ASP.NET Web Pages (Razor) を実行することを防止する方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="2698f-103">サイトのコンポーネントを使用できるように、ASP.NET Web Razor、CAPTCHA を使用)</span><span class="sxs-lookup"><span data-stu-id="2698f-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="2698f-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2698f-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2698f-105">この記事では、ReCaptcha (セキュリティ メジャー) を使用して自動プログラム (bot) が ASP.NET Web Pages (Razor) の web サイトにタスクを実行することを防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2698f-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="2698f-106">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="2698f-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="2698f-107">CAPTCHA テストをサイトに追加する方法。</span><span class="sxs-lookup"><span data-stu-id="2698f-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="2698f-108">アーティクルで導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2698f-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="2698f-109">`ReCaptcha`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="2698f-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2698f-110">この記事の情報は、ASP.NET Web Pages 1.0 および Web Pages 2 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="2698f-111">な Captcha について</span><span class="sxs-lookup"><span data-stu-id="2698f-111">About CAPTCHAs</span></span>

<span data-ttu-id="2698f-112">いつでも、サイトまたはのみでもユーザーに登録を許可するには、URL (ブログのコメントのような) と名前を入力してください、膨大な量の偽の名前を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2698f-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="2698f-113">これらは多くの場合、確認することができるすべての web サイトで Url を保持しようとする自動プログラム (bot) で残されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="2698f-114">(一般的な動機は販売製品の Url をポストします。)</span><span class="sxs-lookup"><span data-stu-id="2698f-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="2698f-115">必ず、ユーザーが実際のユーザーとコンピューター プログラムではなくを使用していただくこと、 *CAPTCHA*登録またはそれ以外の場合、名前とサイトを入力したときにユーザーを検証します。</span><span class="sxs-lookup"><span data-stu-id="2698f-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="2698f-116">CAPTCHA をパブリック チューリングを完全に自動化されたテスト コンピューターと人間間隔を表します。</span><span class="sxs-lookup"><span data-stu-id="2698f-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="2698f-117">CAPTCHA が、*チャレンジ/レスポンス*を促すメッセージが処理を行うには簡単に個人を実行するプログラムの自動化を行うにはハード テストします。</span><span class="sxs-lookup"><span data-stu-id="2698f-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="2698f-118">CAPTCHA の最も一般的な型はゆがんで文字を表示し、それらの入力を求められる配置します。</span><span class="sxs-lookup"><span data-stu-id="2698f-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="2698f-119">(ゆがみなって bot 文字を解読するが困難にします。)</span><span class="sxs-lookup"><span data-stu-id="2698f-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="2698f-120">ReCaptcha テストを追加します。</span><span class="sxs-lookup"><span data-stu-id="2698f-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="2698f-121">ASP.NET ページで、使用することができます、 `ReCaptcha` 、ReCaptcha サービスに基づいている CAPTCHA テストを表示するためのヘルパー ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="2698f-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="2698f-122">`ReCaptcha`ヘルパーはユーザーがページを検証する前に、正しく入力する必要がある 2 つの変形単語のイメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="2698f-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="2698f-123">ユーザーの応答が ReCaptcha.Net サービスによって検証されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="2698f-124">ReCaptcha.Net で web サイトを登録 ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="2698f-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="2698f-125">登録が完了したら、公開キーと秘密キーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="2698f-126">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="2698f-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="2698f-127">まだしていない場合、  *\_AppStart.cshtml*ファイル、という名前のファイルを作成、web サイトのルート フォルダーに *\_AppStart.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="2698f-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="2698f-128">次の追加`Recaptcha`ヘルパーの設定、  *\_AppStart.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2698f-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="2698f-129">設定、`PublicKey`と`PrivateKey`プロパティが、独自のパブリックおよびプライベート キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2698f-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="2698f-130">保存、  *\_AppStart.cshtml*ファイルして、閉じます。</span><span class="sxs-lookup"><span data-stu-id="2698f-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="2698f-131">という名前の新しいページを作成、web サイトのルート フォルダーに*Recaptcha.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="2698f-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="2698f-132">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2698f-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="2698f-133">実行、 *Recaptcha.cshtml*ブラウザーのページです。</span><span class="sxs-lookup"><span data-stu-id="2698f-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="2698f-134">場合、`PrivateKey`値が有効では、ページは、ReCaptcha コントロールとボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="2698f-135">キーをグローバルに設定した場合は *\_AppStart.html*ページにエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="2698f-136">テストの単語を入力します。</span><span class="sxs-lookup"><span data-stu-id="2698f-136">Enter the words for the test.</span></span> <span data-ttu-id="2698f-137">ReCaptcha テストに合格する場合、メッセージにそれを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2698f-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="2698f-138">それ以外の場合、エラー メッセージを表示し、ReCaptcha コントロールが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="2698f-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="2698f-139">コンピューターがプロキシ サーバーを使用しているドメイン上にある場合は、構成する必要があります、`defaultproxy`の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2698f-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="2698f-140">次の例は、 *Web.config*ファイルと、 `defaultproxy` ReCaptcha サービスを動作を有効にするように構成要素。</span><span class="sxs-lookup"><span data-stu-id="2698f-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2698f-141">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2698f-141">Additional Resources</span></span>


- [<span data-ttu-id="2698f-142">ASP.NET Web Pages サイトのサイト全体の動作のカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="2698f-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="2698f-143">ReCaptcha サイト</span><span class="sxs-lookup"><span data-stu-id="2698f-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
