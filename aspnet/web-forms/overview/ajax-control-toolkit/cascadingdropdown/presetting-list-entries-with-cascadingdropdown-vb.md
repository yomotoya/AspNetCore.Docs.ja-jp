---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: CascadingDropDown (VB) で一覧のエントリを事前設定する |Microsoft Docs
author: wenz
description: CascadingDropDown コントロール、AJAX Control Toolkit では、anoth 内の値が 1 つの DropDownList の読み込みの変更に関連付けられているように DropDownList コントロールを拡張しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5133516311478d0a4faab45721c6b1d0a251b4b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817753"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="e1912-103">CascadingDropDown (VB) で一覧のエントリを事前に設定します。</span><span class="sxs-lookup"><span data-stu-id="e1912-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="e1912-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e1912-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e1912-105">[コードのダウンロード](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e1912-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="e1912-106">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e1912-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e1912-107">ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。</span><span class="sxs-lookup"><span data-stu-id="e1912-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="e1912-108">概要</span><span class="sxs-lookup"><span data-stu-id="e1912-108">Overview</span></span>

<span data-ttu-id="e1912-109">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e1912-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e1912-110">(たとえば、1 つのリストは、私たちの状態の一覧を提供します。 し、[次へ] の一覧は、その状態の主な都市で埋められます)。ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。</span><span class="sxs-lookup"><span data-stu-id="e1912-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="e1912-111">手順</span><span class="sxs-lookup"><span data-stu-id="e1912-111">Steps</span></span>

<span data-ttu-id="e1912-112">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="e1912-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="e1912-113">次に、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="e1912-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="e1912-114">このリストの CascadingDropDown エクステンダーが追加になり、web サービスの URL とメソッドの情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="e1912-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="e1912-115">CascadingDropDown エクステンダー非同期的に呼び出して、次のメソッド シグネチャを持つ web サービス。</span><span class="sxs-lookup"><span data-stu-id="e1912-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="e1912-116">CascadingDropDown 値の型の配列を返します。</span><span class="sxs-lookup"><span data-stu-id="e1912-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="e1912-117">リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。</span><span class="sxs-lookup"><span data-stu-id="e1912-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="e1912-118">3 番目の引数が設定されている場合は true、リストに要素が、ブラウザーで自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="e1912-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="e1912-119">ブラウザーでページの読み込みが挿入されますをドロップダウン リストに 3 つのベンダーでは、あらかじめ選択されている 2 つ目。</span><span class="sxs-lookup"><span data-stu-id="e1912-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="e1912-120">[![リストが入力され、自動的にあらかじめ選択されています](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e1912-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="e1912-121">リストが入力され、自動的にあらかじめ選択されています ([フルサイズの画像を表示する をクリックします](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="e1912-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1912-122">[前へ](using-cascadingdropdown-with-a-database-vb.md)
> [次へ](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e1912-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
