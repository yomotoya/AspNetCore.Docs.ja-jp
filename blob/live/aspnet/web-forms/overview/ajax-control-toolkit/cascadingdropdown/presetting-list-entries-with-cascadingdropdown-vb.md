---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: "一覧のエントリ CascadingDropDown (VB) を事前に設定する |Microsoft ドキュメント"
author: wenz
description: "CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: c28c7893c39d9ba9f828c34da7ffdce525ee248e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="c03ed-103">一覧のエントリ CascadingDropDown (VB) を事前に設定します。</span><span class="sxs-lookup"><span data-stu-id="c03ed-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="c03ed-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c03ed-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c03ed-105">[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c03ed-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="c03ed-106">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="c03ed-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c03ed-107">少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。</span><span class="sxs-lookup"><span data-stu-id="c03ed-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="c03ed-108">概要</span><span class="sxs-lookup"><span data-stu-id="c03ed-108">Overview</span></span>

<span data-ttu-id="c03ed-109">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="c03ed-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c03ed-110">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。少しのコードがデータが動的に読み込まれた後にリスト要素があらかじめ選択されていることもできます。</span><span class="sxs-lookup"><span data-stu-id="c03ed-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="c03ed-111">手順</span><span class="sxs-lookup"><span data-stu-id="c03ed-111">Steps</span></span>

<span data-ttu-id="c03ed-112">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="c03ed-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="c03ed-113">その後、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="c03ed-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="c03ed-114">このリストの CascadingDropDown extender が追加され、web サービスの URL とメソッドの情報。</span><span class="sxs-lookup"><span data-stu-id="c03ed-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="c03ed-115">CascadingDropDown extender し、非同期的に呼び出します次のメソッド シグネチャを持つ web サービス。</span><span class="sxs-lookup"><span data-stu-id="c03ed-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="c03ed-116">メソッドは、型 CascadingDropDown 値の配列を返します。</span><span class="sxs-lookup"><span data-stu-id="c03ed-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="c03ed-117">リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。</span><span class="sxs-lookup"><span data-stu-id="c03ed-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="c03ed-118">3 番目の引数が設定されている場合、true、リストに要素が自動的に、ブラウザーで選択します。</span><span class="sxs-lookup"><span data-stu-id="c03ed-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="c03ed-119">ブラウザーでページの読み込みがいっぱいに 3 つのベンダーと、ドロップダウン リスト、もう 1 つが既に選択されています。</span><span class="sxs-lookup"><span data-stu-id="c03ed-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="c03ed-120">[![リストが入力され、自動的に事前に選択](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c03ed-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="c03ed-121">リストが入力され、自動的に事前に選択された ([フルサイズのイメージを表示するをクリックして](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c03ed-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c03ed-122">[前へ](using-cascadingdropdown-with-a-database-vb.md)
[次へ](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c03ed-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
