---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: CascadingDropDown (c#) で一覧のエントリを事前設定する |Microsoft Docs
author: wenz
description: CascadingDropDown コントロール、AJAX Control Toolkit では、anoth 内の値が 1 つの DropDownList の読み込みの変更に関連付けられているように DropDownList コントロールを拡張しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 56093ad098d69039aafa9b4e6ba4e4c2ad91e49b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362000"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="2a6fa-103">CascadingDropDown (c#) で一覧のエントリを事前に設定します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-103">Presetting List Entries with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="2a6fa-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2a6fa-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2a6fa-105">[コードのダウンロード](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2a6fa-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="2a6fa-106">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2a6fa-107">ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="2a6fa-108">概要</span><span class="sxs-lookup"><span data-stu-id="2a6fa-108">Overview</span></span>

<span data-ttu-id="2a6fa-109">CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2a6fa-110">(たとえば、1 つのリストは、私たちの状態の一覧を提供します。 し、[次へ] の一覧は、その状態の主な都市で埋められます)。ほんの少しのコード リストの要素は、データが動的に読み込まれると既に選択されてことができます。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="2a6fa-111">手順</span><span class="sxs-lookup"><span data-stu-id="2a6fa-111">Steps</span></span>

<span data-ttu-id="2a6fa-112">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="2a6fa-113">次に、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="2a6fa-114">このリストの CascadingDropDown エクステンダーが追加になり、web サービスの URL とメソッドの情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="2a6fa-115">CascadingDropDown エクステンダー非同期的に呼び出して、次のメソッド シグネチャを持つ web サービス。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="2a6fa-116">CascadingDropDown 値の型の配列を返します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="2a6fa-117">リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="2a6fa-118">3 番目の引数が設定されている場合は true、リストに要素が、ブラウザーで自動的に選択します。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="2a6fa-119">ブラウザーでページの読み込みが挿入されますをドロップダウン リストに 3 つのベンダーでは、あらかじめ選択されている 2 つ目。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="2a6fa-120">[![リストが入力され、自動的にあらかじめ選択されています](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a6fa-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="2a6fa-121">リストが入力され、自動的にあらかじめ選択されています ([フルサイズの画像を表示する をクリックします](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="2a6fa-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a6fa-122">[前へ](using-cascadingdropdown-with-a-database-cs.md)
> [次へ](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2a6fa-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
