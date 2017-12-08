---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: "単純な検証 (c#) を実行する |Microsoft ドキュメント"
author: StephenWalther
description: "ASP.NET MVC アプリケーションで検証を実行する方法を説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして検証 HTML ヘルパーを紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 005872308d9d4d8ac7feb12dd5ab1fc463d0140e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="5cae6-104">単純な検証 (c#) を実行します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="5cae6-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5cae6-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5cae6-106">ASP.NET MVC アプリケーションで検証を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="5cae6-107">このチュートリアルでは、Stephen Walther は、モデルの状態にして、検証 HTML ヘルパーを紹介します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="5cae6-108">このチュートリアルの目的では、ASP.NET MVC アプリケーション内での検証を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="5cae6-109">たとえば、必要なフィールドの値を含まないフォームの送信を禁止する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="5cae6-110">モデルの状態および検証する HTML ヘルパーを使用する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="5cae6-111">モデルの状態について</span><span class="sxs-lookup"><span data-stu-id="5cae6-111">Understanding Model State</span></span>

<span data-ttu-id="5cae6-112">検証エラーを表すモデルの状態、またはより正確には、モデル状態ディクショナリを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="5cae6-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="5cae6-113">たとえば、リスト 1 の Create() アクションは、Product クラスをデータベースに追加する前に製品クラスのプロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="5cae6-114">コント ローラーに、検証やデータベースのロジックを追加することを推奨してはしていません。</span><span class="sxs-lookup"><span data-stu-id="5cae6-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="5cae6-115">コント ローラーには、アプリケーションのフロー制御に関連するロジックのみを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cae6-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="5cae6-116">複雑にならないへのショートカットを実行しています。</span><span class="sxs-lookup"><span data-stu-id="5cae6-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="5cae6-117">**1 - Controllers\ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="5cae6-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="5cae6-118">リストの 1 では、Product クラスの名前、説明、および UnitsInStock プロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="5cae6-119">これらのプロパティのいずれかの検証テストに失敗した場合、エラーは、(コント ローラー クラスの ModelState プロパティによって表される)、モデル状態ディクショナリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="5cae6-120">モデル状態エラーがある場合、ModelState.IsValid プロパティは false を返します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="5cae6-121">その場合は、新しい製品を作成するための HTML フォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="5cae6-122">それ以外の場合、検証エラーがない場合は、新しい製品がデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="5cae6-123">検証ヘルパーの使用</span><span class="sxs-lookup"><span data-stu-id="5cae6-123">Using the Validation Helpers</span></span>

<span data-ttu-id="5cae6-124">ASP.NET MVC フレームワークには、次の 2 つの検証ヘルパーが含まれています: Html.ValidationMessage() ヘルパーと Html.ValidationSummary() ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="5cae6-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="5cae6-125">検証エラー メッセージを表示するのにには、ビューでこれら 2 つのヘルパーを使用します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="5cae6-126">Html.ValidationMessage() と Html.ValidationSummary() ヘルパーは、ASP.NET MVC のスキャフォールディングによって自動的に生成される作成および編集ビューで使用されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="5cae6-127">作成ビューを生成するこれらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="5cae6-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="5cae6-128">製品のコント ローラーの Create() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="5cae6-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="5cae6-129">**ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**(図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="5cae6-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="5cae6-130">**データ クラスを表示** ドロップダウン リストで、Product クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="5cae6-131">**コンテンツを表示**ドロップダウン リストで、選択を作成します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="5cae6-132">**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5cae6-132">Click the **Add** button.</span></span>


<span data-ttu-id="5cae6-133">ビューを追加する前に、アプリケーションをビルドすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="5cae6-134">クラスの一覧には表示されませんそれ以外の場合、**データ クラスを表示**ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="5cae6-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="5cae6-135">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cae6-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="5cae6-136">**図 01**: ビューの追加 ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5cae6-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="5cae6-137">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5cae6-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="5cae6-138">**図 02**: 厳密に型指定されたビューを作成する ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="5cae6-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="5cae6-139">次の手順を完了すると、リスト 2 で作成ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="5cae6-140">**2 - Views\Product\Create.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="5cae6-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="5cae6-141">リストの 2 では、HTML フォームの上、Html.ValidationSummary() ヘルパーが直ちに呼び出さです。</span><span class="sxs-lookup"><span data-stu-id="5cae6-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="5cae6-142">このヘルパーを使用して、検証エラー メッセージの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="5cae6-143">Html.ValidationSummary() ヘルパーは、箇条書きリスト内のエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="5cae6-144">各 HTML フォーム フィールドの横にある Html.ValidationMessage() ヘルパーと呼びます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="5cae6-145">このヘルパーを使用して、フォーム フィールドの横にエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="5cae6-146">一覧表示する 2 の場合、Html.ValidationMessage() ヘルパーでは、エラーがある場合にアスタリスクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="5cae6-147">図 3 に、ページは、不足しているフィールドと無効な値で、フォームが送信された場合、検証ヘルパーによって表示されるエラー メッセージを示しています。</span><span class="sxs-lookup"><span data-stu-id="5cae6-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="5cae6-148">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5cae6-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="5cae6-149">**図 03**: 問題と共に送信される、Create view ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5cae6-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="5cae6-150">HTML の外観入力フィールドは、検証エラーがある場合にも変更にすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5cae6-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="5cae6-151">Html.TextBox() ヘルパー レンダリング、*クラス =「の入力の検証エラー」* Html.TextBox() ヘルパーによって表示されるプロパティに関連付けられている検証エラーが発生するときに属性。</span><span class="sxs-lookup"><span data-stu-id="5cae6-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="5cae6-152">検証エラーの外観の制御に使用される 3 つのカスケード スタイル シート クラスにがあります。</span><span class="sxs-lookup"><span data-stu-id="5cae6-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="5cae6-153">適用される入力の検証エラー -、&lt;入力&gt;Html.TextBox() ヘルパーによって表示されるタグ。</span><span class="sxs-lookup"><span data-stu-id="5cae6-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="5cae6-154">適用されるフィールドの検証エラー -、 &lt;span&gt; Html.ValidationMessage() ヘルパーによって表示されるタグ。</span><span class="sxs-lookup"><span data-stu-id="5cae6-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="5cae6-155">適用される検証の概要-エラー -、 &lt;ul&gt; Html.ValidationSumamry() ヘルパーによって表示されるタグ。</span><span class="sxs-lookup"><span data-stu-id="5cae6-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="5cae6-156">これらカスケード スタイル シートのクラスを変更し、そのため、コンテンツ フォルダーにある Site.css ファイルを変更することによって検証エラーの外観を変更できます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5cae6-157">HtmlHelper クラスが含まれています読み取り専用の静的プロパティには検証の名前を取得して関連する CSS クラス。</span><span class="sxs-lookup"><span data-stu-id="5cae6-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="5cae6-158">これらの静的プロパティには、ValidationInputCssClassName、ValidationFieldCssClassName、ValidationSummaryCssClassName 名前です。</span><span class="sxs-lookup"><span data-stu-id="5cae6-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="5cae6-159">検証および Postbinding prebinding</span><span class="sxs-lookup"><span data-stu-id="5cae6-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="5cae6-160">場合は、製品を作成するための HTML フォームを送信する、price フィールドおよび UnitsInStock フィールドの値に無効な値を入力して、図 4 に表示される検証メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="5cae6-161">ここではこれらの検証エラー メッセージから取得されるか</span><span class="sxs-lookup"><span data-stu-id="5cae6-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="5cae6-162">[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="5cae6-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="5cae6-163">**図 04**: Prebinding 妥当性確認エラー ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="5cae6-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="5cae6-164">実際には 2 種類の検証エラー メッセージは HTML フォームのフィールドは、クラスにバインドされはフォームのフィールドは、クラスにバインドされた後に生成される前に生成されるものがあります。</span><span class="sxs-lookup"><span data-stu-id="5cae6-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="5cae6-165">つまり、ある prebinding 検証エラーと検証エラーを postbinding です。</span><span class="sxs-lookup"><span data-stu-id="5cae6-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="5cae6-166">1 の一覧で製品のコント ローラーによって公開される Create() アクションは、Product クラスのインスタンスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="5cae6-167">Create メソッドのシグネチャは、このようになります。</span><span class="sxs-lookup"><span data-stu-id="5cae6-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="5cae6-168">フォームの作成から HTML フォーム フィールドの値は、モデル バインダーと呼ばれるもので、productToCreate クラスにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="5cae6-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="5cae6-169">既定のモデル バインダー エラー メッセージを追加モデルの状態を自動的に、フォーム フィールド、フォームのプロパティにバインドできないときにします。</span><span class="sxs-lookup"><span data-stu-id="5cae6-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="5cae6-170">既定のモデル バインダーは、Product クラスの価格プロパティに文字列"apple"をバインドできません。</span><span class="sxs-lookup"><span data-stu-id="5cae6-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="5cae6-171">Decimal プロパティに文字列を割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="5cae6-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="5cae6-172">そのため、モデル バインダーは、モデルの状態をエラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="5cae6-173">既定のモデル バインダーは、null 値を受け付けないことプロパティに null 値を割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="5cae6-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="5cae6-174">具体的には、モデル バインダーは、UnitsInStock プロパティに null 値を割り当てることはできません。</span><span class="sxs-lookup"><span data-stu-id="5cae6-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="5cae6-175">もう一度、モデル バインダーを放棄し、モデルの状態をエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="5cae6-176">エラー メッセージを prebinding これらの外観をカスタマイズする場合は、これらのメッセージのリソース文字列を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cae6-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="5cae6-177">概要</span><span class="sxs-lookup"><span data-stu-id="5cae6-177">Summary</span></span>

<span data-ttu-id="5cae6-178">このチュートリアルの目的は、ASP.NET MVC フレームワークでの検証の基本的なしくみを説明することでした。</span><span class="sxs-lookup"><span data-stu-id="5cae6-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="5cae6-179">モデルの状態および検証する HTML ヘルパーを使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="5cae6-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="5cae6-180">私たちも prebinding と検証を postbinding の違いを説明します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="5cae6-181">他のチュートリアルでは、コント ローラー アウトし、モデルのクラスに検証コードを移動するためのさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="5cae6-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5cae6-182">[前へ](displaying-a-table-of-database-data-cs.md)
[次へ](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5cae6-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
