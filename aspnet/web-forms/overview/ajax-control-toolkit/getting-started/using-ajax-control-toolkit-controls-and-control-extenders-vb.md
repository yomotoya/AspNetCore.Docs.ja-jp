---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "AJAX コントロール Toolkit コントロールおよびコントロール エクステンダー (VB) を使用して |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET ページにコントロールの AJAX コントロール ツールキットおよびエクステンダーを追加する方法を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="869e7-103">AJAX コントロール Toolkit コントロールおよびコントロール エクステンダー (VB) を使用してください。</span><span class="sxs-lookup"><span data-stu-id="869e7-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="869e7-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="869e7-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="869e7-105">ASP.NET ページにコントロールの AJAX コントロール ツールキットおよびエクステンダーを追加する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="869e7-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="869e7-106">AJAX コントロール Toolkit には、コントロールおよびコントロール エクステンダーのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="869e7-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="869e7-107">この簡単なチュートリアルでは、ASP.NET ページにコントロールおよびコントロール エクステンダーの両方を追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="869e7-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="869e7-108">AJAX コントロール Toolkit をインストールして、AJAX コントロール Toolkit を Visual Studio または Visual Web Developer ツールボックスに追加する手順については、チュートリアルを参照してください[AJAX コントロール ツールキットを使用して開始](get-started-with-the-ajax-control-toolkit-vb.md)です。</span><span class="sxs-lookup"><span data-stu-id="869e7-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="869e7-109">AJAX コントロール Toolkit コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="869e7-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="869e7-110">AJAX コントロール Toolkit コントロールは、通常の ASP.NET コントロールと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="869e7-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="869e7-111">ASP.NET ページには、ツールボックスからコントロールをドラッグすることができます。</span><span class="sxs-lookup"><span data-stu-id="869e7-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="869e7-112">デザイン ビューまたはソース ビュー内のページにコントロールを追加できます。</span><span class="sxs-lookup"><span data-stu-id="869e7-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="869e7-113">1 つの特別な要件がある、AJAX コントロール Toolkit からコントロールを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="869e7-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="869e7-114">ページには、ScriptManager コントロールを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="869e7-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="869e7-115">ScriptManager コントロールは、AJAX コントロール Toolkit コントロールに必要なために必要な JavaScript のすべてを含む担当します。</span><span class="sxs-lookup"><span data-stu-id="869e7-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="869e7-116">たとえば、AJAX コントロール ツールキット タブには、エディター コントロールをという名前のコントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="869e7-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="869e7-117">このコントロールには、豊富な HTML エディターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="869e7-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="869e7-118">エディター コントロールをページに追加する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="869e7-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="869e7-119">ShowEditor.aspx をという名前の新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="869e7-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="869e7-120">ツールボックスで、[AJAX Extensions] タブの下から ScriptManager コントロールを選択し、コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="869e7-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="869e7-121">ツールボックスで、AJAX コントロール ツールキット タブの下からエディター コントロールを選択し、コントロールをページにドラッグ (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="869e7-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="869e7-122">デザイナーは、図 2 のようになります。</span><span class="sxs-lookup"><span data-stu-id="869e7-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="869e7-123">メニュー オプションを選択して、web サイトを実行**デバッグ、デバッグの開始** F5 キーを押すか。</span><span class="sxs-lookup"><span data-stu-id="869e7-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="869e7-124">図 3 にページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="869e7-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="869e7-125">[![HTML エディター コントロールを選択します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="869e7-126">**図 01**: HTML エディター コントロールを選択すると ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="869e7-127">[![ScriptManager およびエディット コントロールで visual Studio デザイナー](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="869e7-128">**図 02**: ScriptManager およびエディット コントロールで Visual Studio デザイナー ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="869e7-129">[![DisplayEditor.aspx ページ](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="869e7-130">**図 03**:「DisplayEditor.aspx ページ ([フルサイズのイメージを表示するには、をクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="869e7-131">AJAX コントロール Toolkit コントロール エクステンダーを使用してください。</span><span class="sxs-lookup"><span data-stu-id="869e7-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="869e7-132">AJAX コントロール Toolkit には、コントロール エクステンダーも含まれています。</span><span class="sxs-lookup"><span data-stu-id="869e7-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="869e7-133">その名前からわかるように、コントロール エクステンダーは既存のコントロールの機能を拡張します。</span><span class="sxs-lookup"><span data-stu-id="869e7-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="869e7-134">たとえば、ConfirmButton コントロール エクステンダーは、標準の ASP.NET ボタン コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="869e7-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="869e7-135">Extender は、ボタンがクリックすると、確認のダイアログ ボックスを表示できるように、ボタン コントロールの動作を変更します。</span><span class="sxs-lookup"><span data-stu-id="869e7-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="869e7-136">AJAX コントロール Toolkit コントロールと同じように、コントロールの拡張には、ScriptManager コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="869e7-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="869e7-137">ページでのコントロール エクステンダーの使用を開始する前に、ページに ScriptManager コントロールを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="869e7-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="869e7-138">ConfirmButton コントロール エクステンダーを使用して次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="869e7-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="869e7-139">ShowConfirmButton.aspx をという名前の新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="869e7-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="869e7-140">[AJAX Extensions] タブの下からページにコントロールをドラッグして、ページに ScriptManager コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="869e7-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="869e7-141">デザイナー画面には、ツールボックスで、[標準] タブの下からボタンをドラッグして、ページに標準のボタン コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="869e7-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="869e7-142">クリックして、 **Extender の追加**オプションのタスク (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="869e7-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="869e7-143">エクステンダーの選択 ダイアログ ボックスで選択 ConfirmButtonExtender (図 5 を参照してください)、ok ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="869e7-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="869e7-144">デザイナーでボタン コントロールを選択し、Extender、Button1 を展開\_ConfirmButtonExtender ノード プロパティ ウィンドウ (図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="869e7-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="869e7-145">値を割り当てる*本当に?* ConfirmText プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="869e7-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="869e7-146">メニュー オプションを選択して、ページの実行**デバッグ、デバッグの開始**か、F5 キーを押すとします。</span><span class="sxs-lookup"><span data-stu-id="869e7-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="869e7-147">[![Extender の追加タスク オプション](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="869e7-148">**図 04**: Extender の追加タスク オプション ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="869e7-149">[![ConfirmButton コントロール エクステンダーを選択します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="869e7-150">**図 05**: ConfirmButton コントロール エクステンダーを選択すると ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="869e7-151">[![ConfirmButton プロパティの設定](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="869e7-152">**図 06**: ConfirmButton プロパティの設定 ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="869e7-153">ページが開いたら、ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="869e7-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="869e7-154">ボタンをクリックすると、図 7 に確認のダイアログ ボックスを取得します。</span><span class="sxs-lookup"><span data-stu-id="869e7-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="869e7-155">[![確認のダイアログ ボックスを表示します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="869e7-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="869e7-156">**図 07**: 確認のダイアログ ボックスを表示する ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="869e7-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="869e7-157">通常ドラッグしないコントロール エクステンダーをページに注意してください。</span><span class="sxs-lookup"><span data-stu-id="869e7-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="869e7-158">代わりに、使用する、 **Extender の追加**エクステンダーをページに既に追加したコントロールに追加するオプションのタスクです。</span><span class="sxs-lookup"><span data-stu-id="869e7-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="869e7-159">さらに、あるプロパティを設定するコントロール エクステンダー拡張されるコントロールのプロパティ シートを開いてに注意してください。</span><span class="sxs-lookup"><span data-stu-id="869e7-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="869e7-160">1 つの ASP.NET コントロールは、複数のコントロール エクステンダーによって拡張できます。</span><span class="sxs-lookup"><span data-stu-id="869e7-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="869e7-161">拡張されるコントロールのプロパティ シートでは、コントロールに関連付けられたコントロール エクステンダーのすべてを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="869e7-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="869e7-162">[前へ](get-started-with-the-ajax-control-toolkit-vb.md)
[次へ](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="869e7-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
