---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: 相互に排他的なチェック ボックス (c#) を作成 |Microsoft ドキュメント
author: wenz
description: '一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。 ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択すると、.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871806"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="da3af-104">相互に排他的なチェック ボックス (c#) を作成します。</span><span class="sxs-lookup"><span data-stu-id="da3af-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="da3af-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="da3af-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="da3af-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="da3af-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="da3af-107">一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。</span><span class="sxs-lookup"><span data-stu-id="da3af-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="da3af-108">ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択するはすべてのオプション ボタンをクリックしてオフにします。</span><span class="sxs-lookup"><span data-stu-id="da3af-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="da3af-109">チェック ボックスは、ただしは相互に排他的でない、いつでもチェックできます。</span><span class="sxs-lookup"><span data-stu-id="da3af-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="da3af-110">このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="da3af-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="da3af-111">概要</span><span class="sxs-lookup"><span data-stu-id="da3af-111">Overview</span></span>

<span data-ttu-id="da3af-112">一連のオプションの 1 つのみが選択されるときに、オプション ボタンが通常使用されます。</span><span class="sxs-lookup"><span data-stu-id="da3af-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="da3af-113">ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択するはすべてのオプション ボタンをクリックしてオフにします。</span><span class="sxs-lookup"><span data-stu-id="da3af-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="da3af-114">チェック ボックスは、ただしは相互に排他的でない、いつでもチェックできます。</span><span class="sxs-lookup"><span data-stu-id="da3af-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="da3af-115">このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="da3af-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="da3af-116">手順</span><span class="sxs-lookup"><span data-stu-id="da3af-116">Steps</span></span>

<span data-ttu-id="da3af-117">ASP.NET AJAX コントロール Toolkit には、MutuallyExclusiveCheckBox extender が含まれています。</span><span class="sxs-lookup"><span data-stu-id="da3af-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="da3af-118">これにより、プログラマは、グループ名にすべてのチェック ボックスを割り当てる (`Key`属性)。</span><span class="sxs-lookup"><span data-stu-id="da3af-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="da3af-119">同じグループ内のすべてのチェック ボックスは、1 つだけを一度に 1 つ選択する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="da3af-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="da3af-120">新しい ASP.NET ページ上の 2 つのチェック ボックスを配置することから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="da3af-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="da3af-121">以上、存在することができますですが、プリンシパルを示すために十分 2 つの。</span><span class="sxs-lookup"><span data-stu-id="da3af-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="da3af-122">両方のチェック ボックス、ページで MutuallyExclusiveCheckBoxExtender コントロールに配置されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="da3af-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="da3af-123">両方のキー属性は、HTML ラジオ ボタンの要素の属性は、所属するグループを示すと同じである必要があります値と同様に、同じ値を持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="da3af-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="da3af-124">Extender の TargetControlID プロパティは、チェック ボックスの ID を指します。</span><span class="sxs-lookup"><span data-stu-id="da3af-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="da3af-125">最後に、ASP.NET AJAX を含める`ScriptManager`ASP.NET AJAX コントロール Toolkit のすべての要素で必要となります。</span><span class="sxs-lookup"><span data-stu-id="da3af-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="da3af-126">保存し、ページの実行: を確認し、両方のチェック ボックスをオフにただしない時に両方のチェック ボックス チェックできます。</span><span class="sxs-lookup"><span data-stu-id="da3af-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="da3af-127">[![一度に 1 つだけのチェック ボックスを選択することができます。](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da3af-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="da3af-128">一度に 1 つだけのチェック ボックスを選択することができます ([フルサイズのイメージを表示するをクリックして](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="da3af-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="da3af-129">次へ</span><span class="sxs-lookup"><span data-stu-id="da3af-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
