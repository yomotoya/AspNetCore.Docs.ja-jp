---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: ColorPicker コントロール エクステンダー (VB) を使用して |Microsoft ドキュメント
author: microsoft
description: ColorPicker は、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。 すべての ASP.NET にアタッチできます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="1e950-104">ColorPicker コントロール エクステンダー (VB) を使用します。</span><span class="sxs-lookup"><span data-stu-id="1e950-104">Using the ColorPicker Control Extender (VB)</span></span>
====================
<span data-ttu-id="1e950-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1e950-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1e950-106">ColorPicker は、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。</span><span class="sxs-lookup"><span data-stu-id="1e950-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="1e950-107">これは、ASP.NET のテキスト ボックス コントロールにアタッチできます。</span><span class="sxs-lookup"><span data-stu-id="1e950-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="1e950-108">です。</span><span class="sxs-lookup"><span data-stu-id="1e950-108">It.</span></span>


<span data-ttu-id="1e950-109">このチュートリアルの目的では、AJAX コントロール Toolkit ColorPicker コントロール エクステンダーの使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1e950-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="1e950-110">ColorPicker コントロール エクステンダーは、色を選択することができますポップアップ ダイアログ ボックスを表示します。</span><span class="sxs-lookup"><span data-stu-id="1e950-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="1e950-111">ColorPicker は、色を選択するユーザーの直感的なユーザー インターフェイスを提供する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="1e950-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="1e950-112">ColorPicker コントロール エクステンダーを持つテキスト ボックス コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1e950-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="1e950-113">たとえば、ユーザーがカスタマイズされたビジネス カードを作成できるようにする web サイトを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="1e950-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="1e950-114">閲覧者は、名刺のテキストを入力し、色を選択します。</span><span class="sxs-lookup"><span data-stu-id="1e950-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="1e950-115">リスト 1 の ASP.NET ページには、2 つのテキスト ボックス コントロールが txtCardText および txtCardColor という名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1e950-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="1e950-116">フォームを送信すると、選択した値が表示されます (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1e950-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="1e950-117">[![名刺を作成するための簡単なフォーム](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1e950-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span></span>

<span data-ttu-id="1e950-118">**図 01**: 名刺を作成するための単純な形式 ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="1e950-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


<span data-ttu-id="1e950-119">**1 - CreateCard.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1e950-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="1e950-120">フォーム リスト 1 動作しますが、これには、優れたユーザー エクスペリエンスを提供しません。</span><span class="sxs-lookup"><span data-stu-id="1e950-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="1e950-121">テキスト ボックスに、色を入力するユーザーがいます。</span><span class="sxs-lookup"><span data-stu-id="1e950-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="1e950-122">ユーザーが特殊な色の場合など、pea 緑では、ユーザーの右側の網かけだけ考える必要がありますのヘルプを表示しない HTML 色コード。</span><span class="sxs-lookup"><span data-stu-id="1e950-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="1e950-123">ColorPicker コントロール エクステンダーを使用するより優れたユーザー エクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="1e950-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="1e950-124">テキスト ボックス コントロールにフォーカスを移動すると、ColorPicker に色のダイアログが表示されます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1e950-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="1e950-125">[![ColorPicker コントロール エクステンダー](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1e950-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span></span>

<span data-ttu-id="1e950-126">**図 02**:「ColorPicker コントロール エクステンダー ([フルサイズ イメージを表示するに、をクリックして](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="1e950-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="1e950-127">1 の一覧で、フォームを ColorPicker コントロール エクステンダーを使用する 2 つの手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e950-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="1e950-128">ページに ScriptManager コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="1e950-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="1e950-129">ColorPicker コントロール エクステンダーをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="1e950-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="1e950-130">ColorPicker を使用するには、ページに ScriptManager を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e950-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="1e950-131">開くサーバー側のすぐ下は、ScriptManager を追加するに適して&lt;フォーム&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="1e950-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="1e950-132">([AJAX Extensions] タブでは、ScriptManager があります)、ツールボックスから、ScriptManager をページにドラッグできます。</span><span class="sxs-lookup"><span data-stu-id="1e950-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="1e950-133">また、開始サーバー側 form タグの下にソース ビューに、次のタグを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="1e950-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="1e950-134">&lt;asp ScriptManager: ID ="ScriptManager1"runat ="server"/&gt;</span><span class="sxs-lookup"><span data-stu-id="1e950-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="1e950-135">ColorPicker コントロール エクステンダーをページに追加する最も簡単な方法は、デザイン ビューでです。</span><span class="sxs-lookup"><span data-stu-id="1e950-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="1e950-136">マウス txtCardColor ボックスにポインターを移動する場合は、スマート タスク オプションは、ではで表示されます。 extender を追加できます (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1e950-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="1e950-137">このオプションを選択した場合、Extender ウィザードでは、(図 4 を参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e950-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="1e950-138">[![Extender の追加](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1e950-138">[![Adding an extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span></span>

<span data-ttu-id="1e950-139">**図 03**: extender の追加 ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1e950-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="1e950-140">[![Extender のウィザードを使用してコントロール エクステンダーを選択します。](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1e950-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="1e950-141">**図 04**: Extender ウィザードを使用してコントロール エクステンダーを選択すると ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="1e950-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="1e950-142">ColorPicker エクステンダー txtCardColor テキスト ボックスを拡張する ColorPicker extender を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="1e950-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="1e950-143">ダイアログ ボックスを閉じるには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e950-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="1e950-144">これらの変更を加えた後、ページのソースを一覧表示する 2 に似ています。</span><span class="sxs-lookup"><span data-stu-id="1e950-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="1e950-145">**2 - (ColorPicker) を持つ CreateCard.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1e950-145">**Listing 2 - CreateCard.aspx (with ColorPicker)**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="1e950-146">ページに今すぐ txtCardColor TextBox コントロールのすぐ下に表示されている ColorPickerExtender コントロールが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1e950-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="1e950-147">ColorPickerExtender コントロールは、カラー ピッカー ダイアログ ボックスを表示するように、txtCardColor コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1e950-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="1e950-148">カラー ピッカー ダイアログを起動するボタンを使用してください。</span><span class="sxs-lookup"><span data-stu-id="1e950-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="1e950-149">ColorPicker extender は、次のプロパティをサポートします。</span><span class="sxs-lookup"><span data-stu-id="1e950-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="1e950-150">PopupButtonId - カラー ピッカー ダイアログを表示すると、ページ上のボタンの ID。</span><span class="sxs-lookup"><span data-stu-id="1e950-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="1e950-151">PopupPosition - カラー ピッカー ダイアログ ボックスのターゲット コントロールに対する相対的な位置です。</span><span class="sxs-lookup"><span data-stu-id="1e950-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="1e950-152">使用可能な値は絶対パス、Center、斜め、BottomRight、左上端、TopRight、右、および左 (既定では斜め) です。</span><span class="sxs-lookup"><span data-stu-id="1e950-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="1e950-153">SampleControlId - 選択した色を表示するコントロールの ID。</span><span class="sxs-lookup"><span data-stu-id="1e950-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="1e950-154">SelectedColor -、ColorPicker が選択した最初の色。</span><span class="sxs-lookup"><span data-stu-id="1e950-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="1e950-155">これらのプロパティを使用して、カラー ピッカー ダイアログ ボックスを表示する方法と、選択した色の表示方法をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="1e950-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="1e950-156">ページ 3 の一覧表示するのには、これらのプロパティのいくつかを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1e950-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="1e950-157">**3 - CreateCardButton.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1e950-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="1e950-158">一覧表示する 3 ページには、色選択にはが含まれています (図 5 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e950-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="1e950-159">このボタンをクリックすると、テキスト ボックスの上カラー ピッカー ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e950-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="1e950-160">ダイアログ ボックスで色を選択する場合は、lblSample ラベル コントロールの背景色として選択した色が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e950-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="1e950-161">ColorPicker PopupButtonID プロパティを使用して、色の選択 ボタンを関連付ける ColorPicker extender を使用します。</span><span class="sxs-lookup"><span data-stu-id="1e950-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="1e950-162">PopupButtonID プロパティの値を指定するときに、カラー ピッカー ダイアログは表示されなくなりますターゲット コントロールにフォーカスがある場合。</span><span class="sxs-lookup"><span data-stu-id="1e950-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="1e950-163">ダイアログ ボックスを表示するボタンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e950-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="1e950-164">SampleControlID プロパティは、ColorPicker で選択した色を表示するコントロールを関連付けるために使用します。</span><span class="sxs-lookup"><span data-stu-id="1e950-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="1e950-165">ColorPicker は、現在選択されている色に、このコントロールの背景色を変更します。</span><span class="sxs-lookup"><span data-stu-id="1e950-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="1e950-166">[![カラー ピッカー ダイアログ ボタンを表示します。](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="1e950-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span></span>

<span data-ttu-id="1e950-167">**図 05**: カラー ピッカー ダイアログ ボタンを表示する ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="1e950-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="1e950-168">まとめ</span><span class="sxs-lookup"><span data-stu-id="1e950-168">Summary</span></span>

<span data-ttu-id="1e950-169">このチュートリアルでは、ColorPicker コントロール エクステンダーを使用して、ポップアップ カラー ピッカー ダイアログ ボックスを表示する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1e950-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="1e950-170">最初に、テキスト ボックス コントロールにフォーカスを移動、ダイアログ ボックスを表示する方法調べられます。</span><span class="sxs-lookup"><span data-stu-id="1e950-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="1e950-171">次に、ボタンがクリックされたときに、カラー ピッカー ダイアログ ボックスを表示するボタンを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1e950-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1e950-172">前へ</span><span class="sxs-lookup"><span data-stu-id="1e950-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)
