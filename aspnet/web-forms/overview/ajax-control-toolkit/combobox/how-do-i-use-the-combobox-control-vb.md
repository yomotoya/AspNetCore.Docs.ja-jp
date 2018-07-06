---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: コンボ ボックス コントロールの使用方法 (VB) |Microsoft Docs
author: microsoft
description: コンボ ボックスは、ユーザーが選択できるオプションの一覧と、テキスト ボックスの柔軟性を組み合わせた ASP.NET AJAX コントロールです。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3241641b3e136b24c8cff75026e496ddf8eb04ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371909"
---
<a name="how-do-i-use-the-combobox-control-vb"></a><span data-ttu-id="a9fa4-104">コンボ ボックス コントロールの使用方法</span><span class="sxs-lookup"><span data-stu-id="a9fa4-104">How do I use the ComboBox Control?</span></span> <span data-ttu-id="a9fa4-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-105">(VB)</span></span>
====================
<span data-ttu-id="a9fa4-106">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a9fa4-107">コンボ ボックスは、ユーザーが選択できるオプションの一覧と、テキスト ボックスの柔軟性を組み合わせた ASP.NET AJAX コントロールです。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-107">ComboBox is an ASP.NET AJAX control that combines the flexibility of a TextBox with a list of options from which users can choose.</span></span>


<span data-ttu-id="a9fa4-108">このチュートリアルの目的では、AJAX Control Toolkit のコンボ ボックス コントロールについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-108">The goal of this tutorial is to explain the AJAX Control Toolkit ComboBox control.</span></span> <span data-ttu-id="a9fa4-109">コンボ ボックスは、標準の ASP.NET DropDownList コントロールと TextBox コントロールの組み合わせのように動作します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-109">The ComboBox works like a combination between a standard ASP.NET DropDownList control and a TextBox control.</span></span> <span data-ttu-id="a9fa4-110">既存項目の一覧から選択するか、新しい項目を入力できます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-110">You can either select from a pre-existing list of items or enter a new item.</span></span>

<span data-ttu-id="a9fa4-111">コンボ ボックスは、オートコンプリート コントロール エクステンダーに似ていますが、さまざまなシナリオで、コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-111">The ComboBox is similar to the AutoComplete control extender, but the controls are used in different scenarios.</span></span> <span data-ttu-id="a9fa4-112">AutoComplete エクステンダーでは、一致するエントリを取得する web サービスを照会します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-112">The AutoComplete extender queries a web service to get matching entries.</span></span> <span data-ttu-id="a9fa4-113">これに対し、コンボ ボックス コントロールは、一連の項目で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-113">The ComboBox control, in contrast, is initialized with a set of items.</span></span> <span data-ttu-id="a9fa4-114">AutoComplete エクステンダーの使用を使用してコンボ ボックス コントロールを使用しているときに、多数のデータ (数百万の車の部分) を使用している場合意味データの小さなセットを使用する場合 (多数の車の部分)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-114">Using the AutoComplete extender makes sense when you are working with a large set of data (millions of car parts) while using the ComboBox control makes sense when working with a small set of data (dozens of car parts).</span></span>

## <a name="selecting-from-a-static-list-of-items"></a><span data-ttu-id="a9fa4-115">項目の静的リストから選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-115">Selecting from a Static List of Items</span></span>

<span data-ttu-id="a9fa4-116">S コンボ ボックス コントロールを使用して単純なサンプルを始めることができます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-116">Let�s start with a simple sample of using the ComboBox control.</span></span> <span data-ttu-id="a9fa4-117">ドロップダウン リストの項目の静的な一覧を表示することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-117">Imagine that you want to display a static list of items in a dropdown list.</span></span> <span data-ttu-id="a9fa4-118">ただし、リストが不完全で発生する可能性を開いたままにします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-118">However, you want to leave open the possibility that the list is not complete.</span></span> <span data-ttu-id="a9fa4-119">一覧にカスタム値を入力するユーザーを許可するには。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-119">You want to allow a user to enter a custom value into the list.</span></span>

<span data-ttu-id="a9fa4-120">Ll が新しい ASP.NET Web フォーム ページを作成し、ページで、コンボ ボックス コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-120">We�ll create a new ASP.NET Web Forms page and use the ComboBox control in the page.</span></span> <span data-ttu-id="a9fa4-121">新しい ASP.NET ページをプロジェクトに追加し、デザイン ビューに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-121">Add the new ASP.NET page to your project and switch to Design view.</span></span>

<span data-ttu-id="a9fa4-122">ページで、コンボ ボックス コントロールを使用する場合は、ページに ScriptManager コントロールを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-122">If you want to use the ComboBox control in the page then you must add a ScriptManager control to the page.</span></span> <span data-ttu-id="a9fa4-123">AJAX Extensions タブの下から、ScriptManager コントロールをデザイナー画面にドラッグします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-123">Drag the ScriptManager control from beneath the AJAX Extensions tab onto the Designer surface.</span></span> <span data-ttu-id="a9fa4-124">ページの上部にある、ScriptManager コントロールを追加する必要があります。開始サーバー側の下にすぐに追加できます&lt;フォーム&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-124">You should add the ScriptManager control at the top of the page; you can add it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="a9fa4-125">次に、コンボ ボックス コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-125">Next, drag the ComboBox control onto the page.</span></span> <span data-ttu-id="a9fa4-126">その他の AJAX Control Toolkit のコントロールとコントロールのエクステンダー (図 1 を参照してください) で、ツールボックスにコンボ ボックス コントロールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-126">You can find the ComboBox control in the Toolbox with the other AJAX Control Toolkit controls and control extenders (see figure1).</span></span>


<span data-ttu-id="a9fa4-127">[![ビジネスのカードを作成するための簡単なフォーム](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-127">[![Simple form for creating a business card](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="a9fa4-128">**図 01**: コンボ ボックス コントロールをツールボックスから選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-128">**Figure 01**: Selecting the ComboBox control from the toolbox ([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="a9fa4-129">私たちの選択肢の静的な一覧を表示するコンボ ボックス コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-129">We�ll use the ComboBox control to display a static list of choices.</span></span> <span data-ttu-id="a9fa4-130">ユーザーは、3 つの選択肢の一覧から、食品の spiciness の特定のレベルを選択できます: 軽度、メディア、およびホット (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-130">The user can select a particular level of spiciness for their food from a list of three choices: Mild, Medium, and Hot (see Figure 2).</span></span>


<span data-ttu-id="a9fa4-131">[![項目の静的リストから選択します。](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-131">[![Selecting from a static list of items](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="a9fa4-132">**図 02**: 項目の静的な一覧から選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-132">**Figure 02**: Selecting from a static list of items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="a9fa4-133">コンボ ボックス コントロールにこれらの選択肢を追加するには、2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-133">There are two ways that you can add these choices to the ComboBox control.</span></span> <span data-ttu-id="a9fa4-134">まず、デザイン ビューでコントロールの上にマウス ポインターを置くオプションの編集タスク オプションを選択し、項目エディターを開きます (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-134">First, you select the Edit Options task option when hovering your mouse over the control in Design view and open the Item Editor (see Figure 3).</span></span>


<span data-ttu-id="a9fa4-135">[![コンボ ボックス項目の編集](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-135">[![Editing ComboBox items](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="a9fa4-136">**図 03**: 編集コンボ ボックス アイテム ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-136">**Figure 03**: Editing ComboBox items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="a9fa4-137">2 番目のオプションは、開始タグと終了の間にある項目の一覧を追加することです。 &lt;asp: コンボ ボックス&gt;ソース ビュー内のタグ。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-137">The second option is to add the list of items in between the opening and closing &lt;asp:ComboBox&gt; tags in Source view.</span></span> <span data-ttu-id="a9fa4-138">リスト 1 で、ページには、項目の一覧を含む更新された ComboBox が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-138">The page in Listing 1 contains the updated ComboBox that has the list of items.</span></span>

<span data-ttu-id="a9fa4-139">**1 - Static.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9fa4-139">**Listing 1 - Static.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

<span data-ttu-id="a9fa4-140">リスト 1 で、ページを開くと、コンボ ボックスから既存のオプションのいずれかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-140">When you open the page in Listing 1, you can select one of the pre-existing options from the ComboBox.</span></span> <span data-ttu-id="a9fa4-141">つまり、コンボ ボックスは、DropDownList コントロールと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-141">In other words, the ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="a9fa4-142">ただし、既存のリストに含まれていない新しい選択肢 (たとえば、スーパー スパイシー) を入力するオプションもあります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-142">However, you also have the option of entering a new choice (for example, Super Spicy) that is not in the existing list.</span></span> <span data-ttu-id="a9fa4-143">そのため、コンボ ボックスは、TextBox コントロールのようにも機能します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-143">So, the ComboBox also works like a TextBox control.</span></span>

<span data-ttu-id="a9fa4-144">既存を選択するかどうかに関係なく項目またはするカスタム項目をとき、入力フォームを送信すると、選択したラベル コントロールに表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-144">Regardless of whether you pick a pre-existing item or you enter a custom item, when you submit the form, your choice appears in the label control.</span></span> <span data-ttu-id="a9fa4-145">BtnSubmit、フォームを送信するとき\_ハンドラーを実行し、ラベルを更新 をクリックします (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-145">When you submit the form, the btnSubmit\_Click handler executes and updates the label (see Figure 4).</span></span>


<span data-ttu-id="a9fa4-146">[![選択した項目を表示します。](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-146">[![Displaying the selected item](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="a9fa4-147">**図 04**: 選択した項目を表示する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-147">**Figure 04**: Displaying the selected item([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="a9fa4-148">コンボ ボックスには、フォームが送信された後に、選択した項目を取得するための DropDownList コントロールと同じプロパティがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-148">The ComboBox supports the same properties as the DropDownList control for retrieving the selected item after a form is submitted:</span></span>

- <span data-ttu-id="a9fa4-149">SelectedItem.Text - には、選択した項目の Text プロパティの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-149">SelectedItem.Text - Displays the value of the Text property of the selected item.</span></span>
- <span data-ttu-id="a9fa4-150">SelectedItem.Value - は、選択した項目の Value プロパティの値を表示またはコンボ ボックスに入力されたテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-150">SelectedItem.Value - Displays the value of the Value property of the selected item or displays the text typed into the ComboBox.</span></span>
- <span data-ttu-id="a9fa4-151">SelectedValue - このプロパティを使用する既定の (初期) 選択した項目を指定する点を除いて SelectedItem.Value と同じです。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-151">SelectedValue - Same as SelectedItem.Value except that this property enables you to specify the default (initial) selected item.</span></span>

<span data-ttu-id="a9fa4-152">入力した場合、カスタム選択し、コンボ ボックスにカスタムの選択肢は SelectedItem.Text と SelectedItem.Value の両方のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-152">If you type a custom choice into the ComboBox then the custom choice is assigned to both the SelectedItem.Text and SelectedItem.Value properties.</span></span>

## <a name="selecting-the-list-of-items-from-the-database"></a><span data-ttu-id="a9fa4-153">データベースから項目の一覧を選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-153">Selecting the List of Items from the Database</span></span>

<span data-ttu-id="a9fa4-154">コンボ ボックスを表示する項目の一覧は、データベースから取得できます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-154">You can retrieve the list of items that the ComboBox displays from a database.</span></span> <span data-ttu-id="a9fa4-155">たとえば、SqlDataSource コントロール、ObjectDataSource コントロールの LinqDataSource または、EntityDataSource コンボ ボックスをバインドできます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-155">For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.</span></span>

<span data-ttu-id="a9fa4-156">コンボ ボックスで、ムービーの一覧を表示することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-156">Imagine that you want to display a list of movies in a ComboBox.</span></span> <span data-ttu-id="a9fa4-157">映画データベース テーブルからムービーの一覧を取得するには。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-157">You want to retrieve the list of movies from the Movies database table.</span></span> <span data-ttu-id="a9fa4-158">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-158">Follow these steps:</span></span>

1. <span data-ttu-id="a9fa4-159">Movies.aspx という名前のページを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-159">Create a page named Movies.aspx</span></span>
2. <span data-ttu-id="a9fa4-160">ページには、ツールボックスで、[AJAX Extensions] タブの下から、scriptmanager コントロールをドラッグして、ページに ScriptManager コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-160">Add a ScriptManager control to the page by dragging the ScriptManager from under the AJAX Extensions tab in the Toolbox onto the page.</span></span>
3. <span data-ttu-id="a9fa4-161">ページに、ページに、コンボ ボックスをドラッグして、コンボ ボックス コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-161">Add a ComboBox control to the page by dragging the ComboBox onto the page.</span></span>
4. <span data-ttu-id="a9fa4-162">デザイン ビューでは、コンボ ボックス コントロールの上にマウスを移動し、選択、**データ ソースの選択**タスク オプション (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-162">In Design view, hover your mouse over the ComboBox control and select the **Choose Data Source** task option (see Figure 5).</span></span> <span data-ttu-id="a9fa4-163">データ ソース構成ウィザードを起動します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-163">The Data Source Configuration Wizard is launched.</span></span>
5. <span data-ttu-id="a9fa4-164">**データ ソースの選択**手順で、&lt;新しいデータ ソース&gt;オプション。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-164">In the **Choose a Data Source** step, select the &lt;New data source�&gt; option.</span></span>
6. <span data-ttu-id="a9fa4-165">**データ ソースの種類を選択**ステップで、データベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-165">In the **Choose a Data Source Type** step, select Database.</span></span>
7. <span data-ttu-id="a9fa4-166">**データ接続の選択**ステップで、データベース (たとえば、MoviesDB.mdf) を選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-166">In the **Choose Your Data Connection** step, select your database (for example, MoviesDB.mdf).</span></span>
8. <span data-ttu-id="a9fa4-167">**アプリケーション構成ファイルへの接続文字列を保存**ステップで、接続文字列を保存するオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-167">In the **Save the Connection String to the Application Configuration File** step, select the option to save your connection string.</span></span>
9. <span data-ttu-id="a9fa4-168">**の Select ステートメントを構成する**ステップ、映画データベース テーブルを選択し、すべての列を選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-168">In the **Configure the Select Statement** step, select the Movies database table and select all of the columns.</span></span>
10. <span data-ttu-id="a9fa4-169">**テスト クエリ**ステップで、[完了] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-169">In the **Test Query** step, click the Finish button.</span></span>
11. <span data-ttu-id="a9fa4-170">戻り、**データ ソースの選択**手順で、表示するフィールドのタイトルの列とデータの Id 列のフィールド (図を参照してください) を選択します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-170">Back in the **Choose Data Source** step, select the Title column for the field to display and the Id column for the data field (see Figure).</span></span>
12. <span data-ttu-id="a9fa4-171">ウィザードを閉じる [ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-171">Click the OK button to close the wizard.</span></span>


<span data-ttu-id="a9fa4-172">[![データ ソースの選択](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-172">[![Choosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="a9fa4-173">**図 05**: データ ソースの選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-173">**Figure 05**: Choosing a data source([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="a9fa4-174">[![データのテキストと値のフィールドの選択](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-174">[![Choosing the data text and value fields](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span></span>

<span data-ttu-id="a9fa4-175">**図 06**: データのテキストと値のフィールドの選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-175">**Figure 06**: Choosing the data text and value fields([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span></span>


<span data-ttu-id="a9fa4-176">上記の手順を完了すると、コンボ ボックスは、映画データベース テーブルから、ムービーを表す、SqlDataSource コントロールにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-176">After you complete the steps above, the ComboBox is bound to a SqlDataSource control that represents the movies from the Movies database table.</span></span> <span data-ttu-id="a9fa4-177">ページのソースは、リスト 2 が (私はもう少し書式をクリーンアップ) のようになります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-177">The source for the page looks like Listing 2 (I cleaned up the formatting a little bit).</span></span>

<span data-ttu-id="a9fa4-178">**2 - Movies.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a9fa4-178">**Listing 2 - Movies.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

<span data-ttu-id="a9fa4-179">コンボ ボックス コントロールに SqlDataSource コントロールを指す DataSourceID プロパティがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-179">Notice that the ComboBox control has a DataSourceID property that points to the SqlDataSource control.</span></span> <span data-ttu-id="a9fa4-180">ブラウザーでページを開くと、データベースからムービーの一覧が表示されます (図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-180">When you open the page in a browser, the list of movies from the database is displayed (see Figure 7).</span></span> <span data-ttu-id="a9fa4-181">いずれかを選択、一覧からビデオを実行できますか、コンボ ボックスに、ムービーを入力して、新しいムービーを入力します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-181">You can either a pick a movie from the list or enter a new movie by typing the movie into the ComboBox.</span></span>


<span data-ttu-id="a9fa4-182">[![ムービーの一覧を表示します。](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-182">[![Displaying a list of movies](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span></span>

<span data-ttu-id="a9fa4-183">**図 07**: ムービーの一覧を表示する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image14.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-183">**Figure 07**: Displaying a list of movies([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span></span>


## <a name="setting-the-dropdownstyle"></a><span data-ttu-id="a9fa4-184">設定、DropDownStyle</span><span class="sxs-lookup"><span data-stu-id="a9fa4-184">Setting the DropDownStyle</span></span>

<span data-ttu-id="a9fa4-185">コンボ ボックス DropDownStyle プロパティを使用して、コンボ ボックスの動作を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-185">You can use the ComboBox DropDownStyle property to change the behavior of the ComboBox.</span></span> <span data-ttu-id="a9fa4-186">このプロパティではある使用可能な値。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-186">This property accepts there possible values:</span></span>

- <span data-ttu-id="a9fa4-187">ドロップダウン - (既定値)、コンボ ボックスを表示して、矢印をクリックすると、ドロップダウン ボックスの一覧は、カスタム値を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-187">DropDown - (default value) The ComboBox displays a dropdown list when you click the arrow and you can enter a custom value.</span></span>
- <span data-ttu-id="a9fa4-188">シンプル - コンボ ボックスが自動的にドロップダウン リストを表示し、カスタム値を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-188">Simple - The ComboBox displays a dropdown list automatically and you can enter a custom value.</span></span>
- <span data-ttu-id="a9fa4-189">DropDownList のコンボ ボックスを DropDownList コントロールと同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-189">DropDownList - The ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="a9fa4-190">項目の一覧が表示される場合は、さまざまなドロップダウン間、および単純なのです。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-190">The different between DropDown and Simple is when the list of items is displayed.</span></span> <span data-ttu-id="a9fa4-191">単純な場合は、コンボ ボックスにフォーカスを移動するとすぐに一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-191">In the case of Simple, the list is displayed immediately when you move focus to the ComboBox.</span></span> <span data-ttu-id="a9fa4-192">ドロップダウン リストの場合は、項目の一覧を表示する矢印をクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-192">In the case of DropDown, you must click the arrow to see the list of items.</span></span>

<span data-ttu-id="a9fa4-193">DropDownList の値により、標準の DropDownList コントロールと同様に動作するコンボ ボックス コントロール。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-193">The DropDownList value causes the ComboBox control to work just like a standard DropDownList control.</span></span> <span data-ttu-id="a9fa4-194">ただし、ここで重要な違いがあります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-194">However, there is an important difference here.</span></span> <span data-ttu-id="a9fa4-195">以前のバージョンの Internet Explorer は、その前に配置される、コントロールの前に、コントロールに表示されます、無限の z インデックスを持つ DropDownList コントロールを表示します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-195">Older versions of Internet Explorer display a DropDownList control with an infinite z-index so the control will appear in front of any control placed in front of it.</span></span> <span data-ttu-id="a9fa4-196">コンボ ボックス、HTML をレンダリングするため、 &lt;div&gt;タグ、HTML ではなく&lt;選択&gt;タグ、コンボ ボックス正しく尊重 z オーダーします。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-196">Because the ComboBox renders an HTML &lt;div&gt; tag instead of an HTML &lt;select&gt; tag, the ComboBox correctly respects z-ordering.</span></span>

## <a name="setting-the-autocompletemode"></a><span data-ttu-id="a9fa4-197">設定、AutoCompleteMode</span><span class="sxs-lookup"><span data-stu-id="a9fa4-197">Setting the AutoCompleteMode</span></span>

<span data-ttu-id="a9fa4-198">コンボ ボックスの AutoCompleteMode プロパティを使用して、テキスト、コンボ ボックスに入力すると動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-198">You use the ComboBox AutoCompleteMode property to specify what happens when someone types text into the ComboBox.</span></span> <span data-ttu-id="a9fa4-199">このプロパティは、次の値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-199">This property accepts the following possible values:</span></span>

- <span data-ttu-id="a9fa4-200">None - (既定値)、コンボ ボックスは、オート コンプリートの動作を提供していません。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-200">None - (default value) The ComboBox does not provide any auto-complete behavior.</span></span>
- <span data-ttu-id="a9fa4-201">提案のコンボ ボックスに一覧が表示され、リストに一致する項目が強調表示されます (図 8 参照)。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-201">Suggest - The ComboBox displays the list and it highlights the matching item in the list (see Figure 8).</span></span>
- <span data-ttu-id="a9fa4-202">追加し、コンボ ボックスで、一覧が表示されない - をリストから入力した内容が (図 9 参照) に一致する項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-202">Append - The ComboBox does not display the list and it appends the matching item from the list onto what you have typed (see Figure 9).</span></span>
- <span data-ttu-id="a9fa4-203">パネルのコンボ ボックス両方は、一覧が表示されをリストから入力した内容が (図 10 参照) に一致する項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-203">SuggestAppend - The ComboBox both displays the list and appends the matching item from the list onto what you have typed (see Figure 10).</span></span>


<span data-ttu-id="a9fa4-204">[![コンボ ボックスは、修正案](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-204">[![The ComboBox makes a suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span></span>

<span data-ttu-id="a9fa4-205">**図 08**: コンボ ボックスは、提案 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image16.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-205">**Figure 08**: The ComboBox makes a suggestion([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span></span>


<span data-ttu-id="a9fa4-206">[![コンボ ボックスは、一致するテキストを追加します。](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-206">[![ComboBox appends matching text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span></span>

<span data-ttu-id="a9fa4-207">**図 09**: コンボ ボックスは、一致するテキストを追加します ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image18.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-207">**Figure 09**: ComboBox appends matching text([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span></span>


<span data-ttu-id="a9fa4-208">[![コンボ ボックスの内容が提示され、追加します](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="a9fa4-208">[![The ComboBox suggests and appends](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span></span>

<span data-ttu-id="a9fa4-209">**図 10**: コンボ ボックスが提示され、追加します ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image20.png))。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-209">**Figure 10**: The ComboBox suggests and appends([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span></span>


## <a name="summary"></a><span data-ttu-id="a9fa4-210">まとめ</span><span class="sxs-lookup"><span data-stu-id="a9fa4-210">Summary</span></span>

<span data-ttu-id="a9fa4-211">このチュートリアルでは、コンボ ボックス コントロールを使用して、項目の固定セットを表示する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-211">In this tutorial, you learned how to use the ComboBox control to display a fixed set of items.</span></span> <span data-ttu-id="a9fa4-212">バインドされた項目の設定を静的なデータベース テーブルに両方のコンボ ボックス コントロール。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-212">We bound the ComboBox control both to a static set of items and to a database table.</span></span> <span data-ttu-id="a9fa4-213">最後に、その DropDownStyle と AutoCompleteMode プロパティを設定して、コンボ ボックスの動作を変更する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="a9fa4-213">Finally, you learned how to modify the behavior of the ComboBox by setting its DropDownStyle and AutoCompleteMode properties.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a9fa4-214">前へ</span><span class="sxs-lookup"><span data-stu-id="a9fa4-214">Previous</span></span>](how-do-i-use-the-combobox-control-cs.md)
