---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: "Bot (VB) の戦い |Microsoft ドキュメント"
author: wenz
description: "自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。 ASP.NET AJAX Con で NoBot 制御しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b786fd8605c7521a4aae8e49ca236363a71b572
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-vb"></a><span data-ttu-id="46a7e-104">戦闘 Bot (VB)</span><span class="sxs-lookup"><span data-stu-id="46a7e-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="46a7e-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="46a7e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="46a7e-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="46a7e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="46a7e-107">自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="46a7e-108">ASP.NET AJAX コントロールのツールキットで NoBot 管理は、具体的には、bot 対策に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="46a7e-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="46a7e-109">概要</span><span class="sxs-lookup"><span data-stu-id="46a7e-109">Overview</span></span>

<span data-ttu-id="46a7e-110">自動のボット雇い迷惑メール、ユーザーが介入せずコメント フォームの送信と、web ログやその他の web サイトを設置します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="46a7e-111">ASP.NET AJAX コントロールのツールキットで NoBot 管理は、具体的には、bot 対策に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="46a7e-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="46a7e-112">手順</span><span class="sxs-lookup"><span data-stu-id="46a7e-112">Steps</span></span>

<span data-ttu-id="46a7e-113">Bot を阻止する 1 つの一般的な方法では、コンピューターおよび人間間隔を通知するな Captcha 完全に自動化されたパブリック チューリング テストを使用します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="46a7e-114">チューリング テストがもともとテスト通信パートナーは人間またはコンピューターがあるかどうかを決定する必要な他のユーザー場所です。</span><span class="sxs-lookup"><span data-stu-id="46a7e-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="46a7e-115">Web、CAPTCHA 通常構成一部ゆがんでの文字を使用してイメージをします。</span><span class="sxs-lookup"><span data-stu-id="46a7e-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="46a7e-116">概念は人間のみが、イメージ上の文字を読み取ることができますに対しです OCR アルゴリズムは失敗します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="46a7e-117">いくつかの長所と短所、この方法にが、これの詳細については、このチュートリアルの範囲を超えては。</span><span class="sxs-lookup"><span data-stu-id="46a7e-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="46a7e-118">ただしが、同様のアプローチを提供する ASP.NET AJAX コントロール Toolkit でコントロール:`NoBot`です。</span><span class="sxs-lookup"><span data-stu-id="46a7e-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="46a7e-119">場所と見なされます成功ほとんど迷惑メールの送信試行ブログなどの web サイトで、特によく運賃は行われず、し、CAPTCHA よりを克服する方が簡単ですがは非常に簡単に使用される、`NoBot`制御を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="46a7e-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="46a7e-120">`NoBot`これらの条件の少なくとも 1 つが満たされる場合は、現在の ASP.NET web フォームのポストバックを途中受信します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="46a7e-121">ブラウザーが JavaScript パズルを解く失敗 (たとえば JavaScript が非アクティブ化時に)</span><span class="sxs-lookup"><span data-stu-id="46a7e-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="46a7e-122">ユーザーには、高速にフォームが送信されました</span><span class="sxs-lookup"><span data-stu-id="46a7e-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="46a7e-123">クライアントの IP アドレスでは、一定期間で多くの場合、フォームが送信されます。</span><span class="sxs-lookup"><span data-stu-id="46a7e-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="46a7e-124">これらの条件を確認するために、`NoBot`コントロールには、これらの属性 (すべての省略可能) が必要です。</span><span class="sxs-lookup"><span data-stu-id="46a7e-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="46a7e-125">`ResponseMinimumDelaySeconds`最小限のポストバック間隔 (秒)</span><span class="sxs-lookup"><span data-stu-id="46a7e-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="46a7e-126">`CutoffWindowSeconds`1 つの IP からのポストバックがメジャーとなる時間間隔の長さ</span><span class="sxs-lookup"><span data-stu-id="46a7e-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="46a7e-127">`CutoffMaximumInstances`1 時間間隔の秒の最大量</span><span class="sxs-lookup"><span data-stu-id="46a7e-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="46a7e-128">ポストバック間で次のマークアップ要求するには、少なくとも 2 秒が経過し、5 つのポストバックがあること、または 30 秒間隔に含まれる小さい。</span><span class="sxs-lookup"><span data-stu-id="46a7e-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="46a7e-129">通常どおりを含めるように、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="46a7e-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="46a7e-130">ほとんどの確認のため`NoBot`を行ってこれらの検証の結果を確認する必要がありますが、サーバー側で発生します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="46a7e-131">呼び出すことによってこれ行う`NoBot`の`IsValid()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="46a7e-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="46a7e-132">1 つの引数がある (として、`out`パラメーター/`ByRef`パラメーター) が型である`NoBotState`です。</span><span class="sxs-lookup"><span data-stu-id="46a7e-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="46a7e-133">チェックが失敗したときに、文字列形式には理由が含まれますと`Valid`それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="46a7e-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="46a7e-134">次のコードによるとメッセージを出力する`NoBot`'s が発生します。</span><span class="sxs-lookup"><span data-stu-id="46a7e-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="46a7e-135">最後に、フォームを送信して、メッセージを出力する label 要素が必要し、し終わったら!</span><span class="sxs-lookup"><span data-stu-id="46a7e-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="46a7e-136">このスクリプトを実行し、JavaScript を非アクティブ化または最初の 2 秒以内に、フォームを送信または送信する際、フォーム 30 秒以内に 7 回、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="46a7e-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="46a7e-137">ただし、このコントロールは慎重に使用すると、ユーザーの約 90 ~ 95% があるためアクティブ化された JavaScript、にしたがって 5 ~ 10% のユーザー失敗`NoBot`'s をテストします。</span><span class="sxs-lookup"><span data-stu-id="46a7e-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="46a7e-138">[![このエラー メッセージを bot によって発生する可能性があります。](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="46a7e-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="46a7e-139">このエラー メッセージを bot によって発生する可能性があります ([フルサイズのイメージを表示するをクリックして](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="46a7e-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="46a7e-140">前へ</span><span class="sxs-lookup"><span data-stu-id="46a7e-140">Previous</span></span>](fighting-bots-cs.md)
