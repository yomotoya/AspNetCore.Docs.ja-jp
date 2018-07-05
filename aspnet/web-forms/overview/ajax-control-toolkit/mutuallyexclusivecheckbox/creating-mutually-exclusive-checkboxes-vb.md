---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: 相互に排他的なチェック ボックス (VB) を作成する |Microsoft Docs
author: wenz
description: '一連のオプションの 1 つだけが選択されるときに、オプション ボタンが通常使用されます。 ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択すると、.'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 6badd005ed4775cb248784f3e9991132db1b3969
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828800"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="e5f8a-104">相互に排他的なチェック ボックス (VB) を作成します。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="e5f8a-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e5f8a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e5f8a-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e5f8a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="e5f8a-107">一連のオプションの 1 つだけが選択されるときに、オプション ボタンが通常使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="e5f8a-108">ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択すると後、は、すべてのオプション ボタンをオフにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="e5f8a-109">チェック ボックスは、ただし相互に排他的ではありません、いつでもチェックできます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="e5f8a-110">このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="e5f8a-111">概要</span><span class="sxs-lookup"><span data-stu-id="e5f8a-111">Overview</span></span>

<span data-ttu-id="e5f8a-112">一連のオプションの 1 つだけが選択されるときに、オプション ボタンが通常使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="e5f8a-113">ただし、欠点がある: グループ内の 1 つのオプション ボタンを選択すると後、は、すべてのオプション ボタンをオフにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="e5f8a-114">チェック ボックスは、ただし相互に排他的ではありません、いつでもチェックできます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="e5f8a-115">このチュートリアルでは、両方のアプローチの最適な: チェック ボックスは相互に排他的です。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="e5f8a-116">手順</span><span class="sxs-lookup"><span data-stu-id="e5f8a-116">Steps</span></span>

<span data-ttu-id="e5f8a-117">ASP.NET AJAX Control Toolkit には、MutuallyExclusiveCheckBox エクステンダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="e5f8a-118">これにより、プログラマはグループ名にすべてのチェック ボックスを割り当てる (`Key`属性)。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="e5f8a-119">同じグループ内のすべてのチェック ボックスは、1 つだけを一度に選択できます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="e5f8a-120">新しい ASP.NET ページに 2 つのチェック ボックスを配置することから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="e5f8a-121">以上、存在することができますが、原則を示すためにそのうち 2 つで十分です。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="e5f8a-122">両方のチェック ボックスのページに MutuallyExclusiveCheckBoxExtender コントロールを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="e5f8a-123">両方のキー属性は、HTML ラジオ ボタンの要素の属性は、所属するグループを示すのと同じである必要があります値と同様、同じ値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="e5f8a-124">エクステンダーの TargetControlID プロパティは、チェック ボックスの ID を指します。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="e5f8a-125">最後に、ASP.NET AJAX を含める`ScriptManager`ASP.NET AJAX Control Toolkit のすべての要素で必要です。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="e5f8a-126">保存し、ページの実行: を確認し、両方のチェック ボックスをオフにします。 ただしない時に両方のチェック ボックス チェックできます。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="e5f8a-127">[![一度に 1 つだけのチェック ボックスをオンすることができます。](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e5f8a-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="e5f8a-128">一度に 1 つだけのチェック ボックスをオンすることができます ([フルサイズの画像を表示する をクリックします](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="e5f8a-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e5f8a-129">前へ</span><span class="sxs-lookup"><span data-stu-id="e5f8a-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
