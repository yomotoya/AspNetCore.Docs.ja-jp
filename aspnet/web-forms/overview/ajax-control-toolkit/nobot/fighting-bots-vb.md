---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: (VB) のボットと戦う |Microsoft Docs
author: wenz
description: 自動のボット雇いスパム、ユーザーが介入せずコメント フォームを送信すると、web ログやその他の web サイトを設置します。 ASP.NET AJAX の Con で NoBot コントロール.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f919d130cc97b5f892cc01d58a0bcba790a98e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362519"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="706af-104">(VB) のボットと戦う</span><span class="sxs-lookup"><span data-stu-id="706af-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="706af-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="706af-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="706af-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="706af-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="706af-107">自動のボット雇いスパム、ユーザーが介入せずコメント フォームを送信すると、web ログやその他の web サイトを設置します。</span><span class="sxs-lookup"><span data-stu-id="706af-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="706af-108">ASP.NET AJAX Control Toolkit で NoBot コントロールは、こうしたボットの対策に役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="706af-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="706af-109">概要</span><span class="sxs-lookup"><span data-stu-id="706af-109">Overview</span></span>

<span data-ttu-id="706af-110">自動のボット雇いスパム、ユーザーが介入せずコメント フォームを送信すると、web ログやその他の web サイトを設置します。</span><span class="sxs-lookup"><span data-stu-id="706af-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="706af-111">ASP.NET AJAX Control Toolkit で NoBot コントロールは、こうしたボットの対策に役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="706af-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="706af-112">手順</span><span class="sxs-lookup"><span data-stu-id="706af-112">Steps</span></span>

<span data-ttu-id="706af-113">ボットを無効にする 1 つの一般的なアプローチでは、コンピューターと人間離れているかを知らせるな Captcha 完全に自動化されたパブリック チューリング テストを使用します。</span><span class="sxs-lookup"><span data-stu-id="706af-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="706af-114">チューリング テストがもともとテスト通信パートナーが、人間またはマシンかどうかを判断だれかが必要な場所。</span><span class="sxs-lookup"><span data-stu-id="706af-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="706af-115">Web、CAPTCHA は一部の歪んで表示される文字を使用してイメージの通常されています。</span><span class="sxs-lookup"><span data-stu-id="706af-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="706af-116">考え方としては OCR アルゴリズムは失敗しますが、人間だけが、イメージ上の文字を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="706af-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="706af-117">いくつかの利点と、このアプローチに欠点があるが、これの詳細については、このチュートリアルの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="706af-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="706af-118">ただしは同様のアプローチを提供する ASP.NET AJAX Control toolkit コントロール:`NoBot`します。</span><span class="sxs-lookup"><span data-stu-id="706af-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="706af-119">場所と見なされます成功最も迷惑メールの送信試行のブログなどの web サイトで非常に優れた運賃は行われず、および、CAPTCHA よりを克服する方が簡単ですが、非常に簡単に使用される、`NoBot`制御を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="706af-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="706af-120">`NoBot` これらの条件の少なくとも 1 つが満たされる場合は、現在の ASP.NET web フォームのポストバックをインターセプトします。</span><span class="sxs-lookup"><span data-stu-id="706af-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="706af-121">ブラウザーが JavaScript パズルを解くに失敗した (たとえば JavaScript が非アクティブ化時に)</span><span class="sxs-lookup"><span data-stu-id="706af-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="706af-122">ユーザーには、高速にフォームが送信されました</span><span class="sxs-lookup"><span data-stu-id="706af-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="706af-123">クライアントの IP アドレスは、一定期間で多くの場合、フォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="706af-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="706af-124">これらの条件を確認するには、`NoBot`コントロールには、これらの属性 (すべての省略可能) が必要です。</span><span class="sxs-lookup"><span data-stu-id="706af-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="706af-125">`ResponseMinimumDelaySeconds` 最小限のポストバック間隔 (秒)</span><span class="sxs-lookup"><span data-stu-id="706af-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="706af-126">`CutoffWindowSeconds` 1 つの IP からのポストバックがメジャーとなる時間間隔の長さ</span><span class="sxs-lookup"><span data-stu-id="706af-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="706af-127">`CutoffMaximumInstances` 最長 1 時間間隔 (秒)</span><span class="sxs-lookup"><span data-stu-id="706af-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="706af-128">ポストバック間で、次のマークアップ要求するには少なくとも 2 秒が経過し、5 つのみのポストバックが 30 秒の間隔内で以下。</span><span class="sxs-lookup"><span data-stu-id="706af-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="706af-129">通常どおりを含めるように、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるように。</span><span class="sxs-lookup"><span data-stu-id="706af-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="706af-130">チェックの多く`NoBot`が行っている、サーバー側で発生する、これらの検証の結果を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="706af-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="706af-131">これを行う呼び出し`NoBot`の`IsValid()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="706af-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="706af-132">1 つの引数がある (として、`out`パラメーター/`ByRef`パラメーター) 型の`NoBotState`します。</span><span class="sxs-lookup"><span data-stu-id="706af-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="706af-133">チェックが失敗したときに、その文字列形式には理由が含まれますと`Valid`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="706af-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="706af-134">次のコードによるのメッセージを出力する`NoBot`'s が発生します。</span><span class="sxs-lookup"><span data-stu-id="706af-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="706af-135">最後に、フォームを送信して、メッセージを出力するラベル要素が必要ですし、完了したら!</span><span class="sxs-lookup"><span data-stu-id="706af-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="706af-136">このスクリプトを実行し、JavaScript を非アクティブ化または最初の 2 秒内で、フォームの送信や 7 回 30 秒以内にフォームを送信、ときに、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="706af-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="706af-137">このコントロールを使用して、賢明なただし、そのためユーザーの 5 ~ 10% が失敗ユーザーの約 90 ~ 95% の JavaScript のアクティブ化されるため、 `NoBot`'s をテストします。</span><span class="sxs-lookup"><span data-stu-id="706af-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="706af-138">[![このエラー メッセージをボットで発生する可能性があります。](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="706af-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="706af-139">このエラー メッセージをボットで発生する可能性があります ([フルサイズの画像を表示する をクリックします](fighting-bots-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="706af-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="706af-140">前へ</span><span class="sxs-lookup"><span data-stu-id="706af-140">Previous</span></span>](fighting-bots-cs.md)
