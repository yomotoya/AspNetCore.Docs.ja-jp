---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: 検証コントロール (VB) で TextBoxWatermark の使用 |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで TextBoxWatermark コントロールは、ボックス内でテキストが表示されないようにテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックしたときに.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="ac0b4-104">検証コントロール (VB) で TextBoxWatermark を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="ac0b4-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ac0b4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ac0b4-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ac0b4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="ac0b4-107">AJAX コントロールのツールキットで TextBoxWatermark コントロールは、ボックス内でテキストが表示されないようにテキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="ac0b4-108">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="ac0b4-109">ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="ac0b4-110">これは同じページで、ASP.NET の検証コントロールと競合する可能性がありますが、これらの問題を解決する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="ac0b4-111">概要</span><span class="sxs-lookup"><span data-stu-id="ac0b4-111">Overview</span></span>

<span data-ttu-id="ac0b4-112">`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX コントロール Toolkit でコントロールがテキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="ac0b4-113">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="ac0b4-114">ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="ac0b4-115">これは同じページで、ASP.NET の検証コントロールと競合する可能性がありますが、これらの問題を解決する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="ac0b4-116">手順</span><span class="sxs-lookup"><span data-stu-id="ac0b4-116">Steps</span></span>

<span data-ttu-id="ac0b4-117">このサンプルの基本的なセットアップは、次:`TextBox`を使用してコントロールを透かし、`TextBoxWatermarkExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="ac0b4-118">ボタンを使用して、ポストバックのトリガー、ページ上の検証コントロールのトリガーを後で使用されます。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="ac0b4-119">また、`ScriptManager`コントロールは ASP.NET AJAX を初期化するために必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="ac0b4-120">ここで追加、`RequiredFieldValidator`コントロールかどうかがテキスト フィールドに、フォームが送信されるときに確認します。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="ac0b4-121">`InitialValue`検証コントロールのプロパティで使用されるものと同じ値に設定する必要があります、`TextBoxWatermarkExtender`コントロール: 変更なしのテキスト ボックスの値は、その中レベルのウォーターマーク値でフォームが送信されるときに。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="ac0b4-122">このアプローチの 1 つの問題: 場合は、クライアントには、JavaScript が無効化、テキスト フィールドされません事前入力透かしのテキストをそのため、`RequiredFieldValidator`エラー メッセージは発生しません。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="ac0b4-123">そのため、1 秒あたり`RequiredFieldValidator`制御が必要なを空のテキスト ボックスのチェック (を省略すると、`InitialValue`属性)。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="ac0b4-124">両方の検証コントロールを使用するので`Display` =`"Dynamic"`エンドユーザーとを区別できない、視覚的な外観が 2 つの検証コントロールが発生しました。 代わりに、ようそれらの 1 つだけを使用する必要がありました。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="ac0b4-125">最後に、検証コントロールにエラー メッセージが発行されたない場合は、フィールド内のテキストを出力するには、いくつかサーバー側コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ac0b4-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="ac0b4-126">[![検証コントロールがないテキスト フィールドにエラーが発生します。](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ac0b4-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="ac0b4-127">検証エラーが発生 フィールドにテキストがないこと ([フルサイズのイメージを表示するをクリックして](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ac0b4-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac0b4-128">前へ</span><span class="sxs-lookup"><span data-stu-id="ac0b4-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
