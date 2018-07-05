---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: 単純な検証 (VB) を実行する |Microsoft Docs
author: StephenWalther
description: ASP.NET MVC アプリケーションで検証を実行する方法について説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして検証 HTML ヘルパーについて説明しています.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: a8320e87cb4ef418fe5c8308b9dacceb5d6bbac8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830786"
---
<a name="performing-simple-validation-vb"></a><span data-ttu-id="ecabd-104">単純な検証 (VB) を実行します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-104">Performing Simple Validation (VB)</span></span>
====================
<span data-ttu-id="ecabd-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ecabd-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ecabd-106">ASP.NET MVC アプリケーションで検証を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="ecabd-107">このチュートリアルでは、Stephen Walther は、モデルの状態にして、検証 HTML ヘルパーについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="ecabd-108">このチュートリアルの目的では、ASP.NET MVC アプリケーション内での検証を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="ecabd-109">たとえば、だれかが必要なフィールドの値を含まないフォームを送信するようにする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="ecabd-110">モデルの状態と検証の HTML ヘルパーを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="ecabd-111">モデルの状態について</span><span class="sxs-lookup"><span data-stu-id="ecabd-111">Understanding Model State</span></span>

<span data-ttu-id="ecabd-112">モデルの状態、またはより正確には、モデル状態ディクショナリを使用する検証エラーを表します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="ecabd-113">たとえば、リスト 1 で Create() アクションでは、Product クラスをデータベースに追加する前に Product クラスのプロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="ecabd-114">コント ローラーに、検証またはデータベース ロジックを追加することは推奨していませんいます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="ecabd-115">コント ローラーには、アプリケーション フローの制御に関連するロジックのみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecabd-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="ecabd-116">単純化へのショートカットを取得しています。</span><span class="sxs-lookup"><span data-stu-id="ecabd-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="ecabd-117">**1 - Controllers\ProductController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ecabd-117">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

<span data-ttu-id="ecabd-118">リストの 1 では、Product クラスの名前、説明、および UnitsInStock プロパティが検証されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="ecabd-119">これらのプロパティのいずれかの検証テストが失敗した場合、エラーは、(コント ローラー クラスの ModelState プロパティによって表される)、モデル状態ディクショナリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="ecabd-120">モデル状態エラーがある場合、ModelState.IsValid プロパティは false を返します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="ecabd-121">その場合は、新しい製品を作成するための HTML フォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="ecabd-122">それ以外の場合、検証エラーがない場合は、新しい製品がデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="ecabd-123">検証ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="ecabd-123">Using the Validation Helpers</span></span>

<span data-ttu-id="ecabd-124">ASP.NET MVC フレームワークには、2 つの検証ヘルパーが含まれています: Html.ValidationMessage() ヘルパーと Html.ValidationSummary() ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ecabd-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="ecabd-125">検証エラー メッセージを表示するのにには、ビューのこれら 2 つのヘルパーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="ecabd-126">Html.ValidationMessage() と Html.ValidationSummary() ヘルパーは、ASP.NET MVC のスキャフォールディングによって自動的に生成される Create と Edit ビューで使用されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="ecabd-127">作成ビューを生成するこれらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ecabd-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="ecabd-128">製品のコント ローラーで Create() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="ecabd-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="ecabd-129">**ビューの追加**ダイアログ ボックスで、というラベルの付いたチェック ボックスをオン**厳密に型指定されたビューを作成する**(図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="ecabd-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="ecabd-130">**データ クラスを表示**ドロップダウン リストで、Product クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="ecabd-131">**コンテンツを表示**ドロップダウン リストで、作成を選択します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="ecabd-132">**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ecabd-132">Click the **Add** button.</span></span>


<span data-ttu-id="ecabd-133">ビューを追加する前にアプリケーションをビルドすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="ecabd-134">それ以外の場合、クラスの一覧に表示されません、**データ クラスを表示**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="ecabd-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="ecabd-135">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ecabd-135">[![The New Project dialog box](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="ecabd-136">**図 01**: ビューの追加 ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="ecabd-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="ecabd-137">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ecabd-137">[![The New Project dialog box](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span></span>

<span data-ttu-id="ecabd-138">**図 02**: 厳密に型指定されたビューを作成する ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="ecabd-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-vb/_static/image4.png))</span></span>


<span data-ttu-id="ecabd-139">次の手順を完了すると、リスト 2 でビューを作成するを取得します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="ecabd-140">**2 - Views\Product\Create.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ecabd-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

<span data-ttu-id="ecabd-141">リストの 2 では、HTML フォームの上、Html.ValidationSummary() ヘルパーが直ちに呼び出さします。</span><span class="sxs-lookup"><span data-stu-id="ecabd-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="ecabd-142">このヘルパーを使用して、検証エラー メッセージの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="ecabd-143">Html.ValidationSummary() ヘルパーは、箇条書きリストにエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="ecabd-144">Html.ValidationMessage() ヘルパーは HTML フォームのフィールドの横にある呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="ecabd-145">このヘルパーを使用して、フォーム フィールドのすぐ隣に、エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="ecabd-146">リストの 2 の場合、Html.ValidationMessage() ヘルパーでは、エラーがある場合にアスタリスクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="ecabd-147">図 3 のページは、不足しているフィールドと無効な値で、フォームが送信されたときに、検証ヘルパーによって表示されるエラー メッセージを示しています。</span><span class="sxs-lookup"><span data-stu-id="ecabd-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="ecabd-148">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ecabd-148">[![The New Project dialog box](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span></span>

<span data-ttu-id="ecabd-149">**図 03**: 問題を含めて提出して、作成ビュー ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="ecabd-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-vb/_static/image6.png))</span></span>


<span data-ttu-id="ecabd-150">HTML の外観は、検証エラーがある場合にもフィールドが変更された入力に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ecabd-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="ecabd-151">Html.TextBox() ヘルパーのレンダリングを*クラス =「の入力検証エラー」* Html.TextBox() ヘルパーによって表示されるプロパティに関連付けられている属性の検証エラーがある場合。</span><span class="sxs-lookup"><span data-stu-id="ecabd-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="ecabd-152">検証エラーの外観の制御に使用される 3 つのカスケード スタイル シート クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="ecabd-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="ecabd-153">適用される入力の検証エラー -、&lt;入力&gt;タグ Html.TextBox() ヘルパーによってレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="ecabd-154">適用されるフィールドの検証エラー -、 &lt;span&gt;タグ Html.ValidationMessage() ヘルパーによってレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="ecabd-155">適用される検証の概要-エラー -、 &lt;ul&gt;タグ Html.ValidationSumamry() ヘルパーによってレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="ecabd-156">これらカスケード スタイル シートのクラスを変更し、コンテンツのフォルダーにある Site.css ファイルを変更することでそのため、検証エラーの外観を変更できます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ecabd-157">HtmlHelper クラスには、CSS を関連する検証の名前を取得する読み取り専用の静的プロパティが含まれますクラス。</span><span class="sxs-lookup"><span data-stu-id="ecabd-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="ecabd-158">これらの静的プロパティは、ValidationInputCssClassName、ValidationFieldCssClassName、および ValidationSummaryCssClassName 名前です。</span><span class="sxs-lookup"><span data-stu-id="ecabd-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="ecabd-159">Prebinding 検証と Postbinding の検証</span><span class="sxs-lookup"><span data-stu-id="ecabd-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="ecabd-160">製品を作成するための HTML フォームを送信する、[price] フィールドと UnitsInStock フィールドの値に無効な値を入力すると、図 4 に表示される検証メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="ecabd-161">これらの検証エラー メッセージからどのようになるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="ecabd-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="ecabd-162">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ecabd-162">[![The New Project dialog box](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span></span>

<span data-ttu-id="ecabd-163">**図 04**: Prebinding 検証エラー ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="ecabd-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-vb/_static/image8.png))</span></span>


<span data-ttu-id="ecabd-164">実際に 2 種類の検証エラー メッセージの HTML フォームのフィールドは、クラスにバインドされ、フォームのフィールドがクラスにバインドされている後に生成された前に生成されたものがあります。</span><span class="sxs-lookup"><span data-stu-id="ecabd-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="ecabd-165">つまり、あるは検証エラーを prebinding と postbinding 検証エラー。</span><span class="sxs-lookup"><span data-stu-id="ecabd-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="ecabd-166">リスト 1 で製品のコント ローラーによって公開される Create() アクションは、Product クラスのインスタンスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="ecabd-167">Create メソッドのシグネチャのようになります。</span><span class="sxs-lookup"><span data-stu-id="ecabd-167">The signature of the Create method looks like this:</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

<span data-ttu-id="ecabd-168">作成フォームから HTML フォーム フィールドの値は、モデル バインダーと呼ばれるもので、productToCreate クラスにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ecabd-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="ecabd-169">既定のモデル バインダー エラー メッセージに追加モデルの状態に自動的にフォーム フィールド、フォームのプロパティにバインドできないときにします。</span><span class="sxs-lookup"><span data-stu-id="ecabd-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="ecabd-170">既定のモデル バインダーは、Product クラスの価格のプロパティに文字列"apple"をバインドできません。</span><span class="sxs-lookup"><span data-stu-id="ecabd-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="ecabd-171">文字列は、10 進数のプロパティに割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="ecabd-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="ecabd-172">そのため、モデル バインダーでは、モデルの状態をエラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="ecabd-173">既定のモデル バインダーことはできませんを割り当てることも、値 Nothing はない値をそのまま何もするプロパティ。</span><span class="sxs-lookup"><span data-stu-id="ecabd-173">The default model binder also cannot assign the value Nothing to a property that does not accept the value Nothing.</span></span> <span data-ttu-id="ecabd-174">具体的には、モデル バインダーに代入できません値 Nothing UnitsInStock プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ecabd-174">In particular, the model binder cannot assign the value Nothing to the UnitsInStock property.</span></span> <span data-ttu-id="ecabd-175">ここでも、モデル バインダーを放棄し、モデルの状態をエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="ecabd-176">エラー メッセージを prebinding これらの外観をカスタマイズする場合は、これらのメッセージのリソース文字列を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecabd-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="ecabd-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="ecabd-177">Summary</span></span>

<span data-ttu-id="ecabd-178">このチュートリアルの目的は、ASP.NET MVC フレームワークでの検証の基本的なしくみを説明することでした。</span><span class="sxs-lookup"><span data-stu-id="ecabd-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="ecabd-179">モデルの状態と検証の HTML ヘルパーを使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="ecabd-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="ecabd-180">Prebinding と検証を postbinding の違いを説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="ecabd-181">他のチュートリアルでは、コント ローラーからモデル クラスに検証コードを移動するためのさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ecabd-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ecabd-182">[前へ](displaying-a-table-of-database-data-vb.md)
> [次へ](validating-with-the-idataerrorinfo-interface-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ecabd-182">[Previous](displaying-a-table-of-database-data-vb.md)
[Next](validating-with-the-idataerrorinfo-interface-vb.md)</span></span>
