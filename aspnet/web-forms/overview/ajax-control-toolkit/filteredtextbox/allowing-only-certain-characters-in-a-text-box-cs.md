---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: (C#) のテキスト ボックス内の特定の文字のみを許可する |Microsoft ドキュメント
author: wenz
description: ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。 ただしこのまだでも、ユーザー入力が無効です.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d2ffc4b741bd0c7f9c456b6e76017f5350ab6378
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="728cd-104">(C#) のテキスト ボックス内の特定の文字のみを許可します。</span><span class="sxs-lookup"><span data-stu-id="728cd-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="728cd-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="728cd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="728cd-106">[コードをダウンロードする](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="728cd-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="728cd-107">ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="728cd-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="728cd-108">ただしこれができないユーザーが無効な文字を入力し、フォームを送信しようとしています。</span><span class="sxs-lookup"><span data-stu-id="728cd-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="728cd-109">概要</span><span class="sxs-lookup"><span data-stu-id="728cd-109">Overview</span></span>

<span data-ttu-id="728cd-110">ASP.NET の検証コントロールは、ユーザー入力の特定の文字のみが許可されることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="728cd-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="728cd-111">ただしこれができないユーザーが無効な文字を入力し、フォームを送信しようとしています。</span><span class="sxs-lookup"><span data-stu-id="728cd-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="728cd-112">手順</span><span class="sxs-lookup"><span data-stu-id="728cd-112">Steps</span></span>

<span data-ttu-id="728cd-113">ASP.NET AJAX コントロールのツールキットに含まれています、`FilteredTextBox`をテキスト ボックスを拡張するコントロール。</span><span class="sxs-lookup"><span data-stu-id="728cd-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="728cd-114">アクティブ化されると、フィールドに文字の特定のセットのみを入力することがあります。</span><span class="sxs-lookup"><span data-stu-id="728cd-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="728cd-115">これを行うには、まず必要がありますいつものように ASP.NET AJAX `ScriptManager` ASP.NET AJAX コントロール Toolkit で使用されるも JavaScript ライブラリが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="728cd-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="728cd-116">次に、テキスト ボックスは必要があります。</span><span class="sxs-lookup"><span data-stu-id="728cd-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="728cd-117">最後に、`FilteredTextBoxExtender`コントロールは、ユーザーが入力できる文字の制限の対処します。</span><span class="sxs-lookup"><span data-stu-id="728cd-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="728cd-118">まず、設定、`TargetControlID`属性を`ID`の`TextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="728cd-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="728cd-119">次に、使用可能ないずれかを選択`FilterType`値。</span><span class="sxs-lookup"><span data-stu-id="728cd-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="728cd-120">`Custom` 既定値です。有効な文字の一覧を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="728cd-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="728cd-121">`LowercaseLetters` 小文字のみ</span><span class="sxs-lookup"><span data-stu-id="728cd-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="728cd-122">`Numbers` 数字のみ</span><span class="sxs-lookup"><span data-stu-id="728cd-122">`Numbers` digits only</span></span>
- <span data-ttu-id="728cd-123">`UppercaseLetters` 大文字の使用のみ</span><span class="sxs-lookup"><span data-stu-id="728cd-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="728cd-124">場合、`Custom FilterType`を使用する、`ValidChars`プロパティが設定して型を指定することがありますの文字の一覧を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="728cd-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="728cd-125">方法: テキスト ボックスにテキストを貼り付けるしようとすると、すべての無効な文字が削除されます。</span><span class="sxs-lookup"><span data-stu-id="728cd-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="728cd-126">マークアップをここでは、`FilteredTextBoxExtender`桁の数字のみを実行できるコントロール (も必要であったと考えられるもの`FilterType="Numbers"`)。</span><span class="sxs-lookup"><span data-stu-id="728cd-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="728cd-127">ページを起動して、JavaScript が有効になっている場合、文字を入力してくださいは機能しません。ただし、桁の数字は、ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="728cd-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="728cd-128">注意して、保護`FilteredTextBox`提供強固ではありません: 場合は JavaScript が有効になっている、検証を追加する方法、つまり ASP を使用する必要があるために、テキスト ボックスで、すべてのデータを入力します。NET の検証コントロール。</span><span class="sxs-lookup"><span data-stu-id="728cd-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="728cd-129">[![数字のみを入力することがあります。](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="728cd-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="728cd-130">数字のみを入力することがあります ([フルサイズのイメージを表示するをクリックして](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="728cd-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="728cd-131">次へ</span><span class="sxs-lookup"><span data-stu-id="728cd-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
