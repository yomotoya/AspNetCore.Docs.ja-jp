---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: データ注釈検証コントロールに (c#) を使用した検証 |Microsoft Docs
author: microsoft
description: ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーの活用します。 検証コントロールのさまざまな種類を使用する方法を説明してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 484d836c3865ca3df684a379585b71138d42ab7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378218"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="b8bad-104">データ注釈検証コントロールに (c#) を使用した検証</span><span class="sxs-lookup"><span data-stu-id="b8bad-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="b8bad-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b8bad-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b8bad-106">ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーの活用します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="b8bad-107">検証属性の種類を使用して Microsoft Entity Framework に処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="b8bad-108">このチュートリアルでは、データ注釈検証コントロールを使用して、ASP.NET MVC アプリケーションで検証を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b8bad-109">データ注釈検証コントロールを使用する利点は、– などに必要な 1 つまたは複数の属性や StringLength 属性を追加するだけで、– クラス プロパティには、検証を実行することを有効にすることです。</span><span class="sxs-lookup"><span data-stu-id="b8bad-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="b8bad-110">データ注釈検証コントロールを使用するには、データ注釈のモデル バインダーをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="b8bad-111">CodePlex の web サイトからデータ注釈のモデル バインダーのサンプルをダウンロードするにはクリックして[ここ](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="b8bad-112">データ注釈のモデル バインダーは、Microsoft ASP.NET MVC フレームワークの公式の一部ではないこと理解に重要です。</span><span class="sxs-lookup"><span data-stu-id="b8bad-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="b8bad-113">データ注釈のモデル バインダーが、Microsoft ASP.NET MVC チームによって作成されますが、データ注釈のモデル バインダーの公式の製品サポートの説明し、このチュートリアルで使用される Microsoft は提供されません。</span><span class="sxs-lookup"><span data-stu-id="b8bad-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="b8bad-114">データ注釈のモデル バインダーを使用してください。</span><span class="sxs-lookup"><span data-stu-id="b8bad-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="b8bad-115">ASP.NET MVC アプリケーションでデータ注釈のモデル バインダーを使用するには、まず Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="b8bad-116">メニュー オプションを選択**プロジェクトで、参照の追加**します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="b8bad-117">次に、**参照**タブし、データ注釈モデル バインダーのサンプルのダウンロード (および解凍) どこの場所を参照 (を参照してください**図 1**)。</span><span class="sxs-lookup"><span data-stu-id="b8bad-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="b8bad-118">**図 1**: データの注釈のモデル バインダーへの参照を追加する ([フルサイズの画像を表示する をクリックします](validation-with-the-data-annotation-validators-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="b8bad-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="b8bad-119">Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリの両方を選択し、クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b8bad-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="b8bad-120">データ注釈のモデル バインダーでは、.NET Framework Service Pack 1 に含まれている system.componentmodel.dataannotations.dll へのアセンブリを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b8bad-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="b8bad-121">データ注釈モデル バインダーのサンプル ダウンロードに含まれている system.componentmodel.dataannotations.dll へのアセンブリのバージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="b8bad-122">最後に、Global.asax ファイルで、DataAnnotations のモデル バインダーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="b8bad-123">アプリケーションに次のコード行を追加\_Start() イベント ハンドラーようにアプリケーション\_Start() メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="b8bad-124">次のコードでは、ASP.NET MVC アプリケーション全体の既定のモデル バインダーとして、ataAnnotationsModelBinder を登録します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="b8bad-125">データ注釈検証コントロールの属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="b8bad-126">データ注釈のモデル バインダーを使用する場合は、検証を実行する検証属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="b8bad-127">System.ComponentModel.DataAnnotations 名前空間には、次の検証属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b8bad-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="b8bad-128">範囲は – プロパティの値が、指定した値の範囲があるかどうかを検証することができます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="b8bad-129">– 正規表現では、プロパティの値が指定した正規表現パターンに一致するかどうかを検証することができます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="b8bad-130">必要な – 必要に応じてプロパティをマークすることができます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="b8bad-131">StringLength – では、文字列プロパティの最大長を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="b8bad-132">検証: すべての検証属性の基本クラス。</span><span class="sxs-lookup"><span data-stu-id="b8bad-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b8bad-133">標準の検証コントロールのいずれかによって、検証のニーズが満たされない場合、常がある新しい検証コントロールの属性ベースの検証属性から継承することによって、カスタム検証属性を作成するオプション。</span><span class="sxs-lookup"><span data-stu-id="b8bad-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="b8bad-134">Product クラス**リスト 1**これらの検証属性を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="b8bad-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="b8bad-135">名前、説明、および UnitPrice プロパティがマークされている必須とします。</span><span class="sxs-lookup"><span data-stu-id="b8bad-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="b8bad-136">Name プロパティには、文字列の長さが 10 より小さい文字である必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="b8bad-137">最後に、UnitPrice プロパティは、通貨金額を表す正規表現パターンと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="b8bad-138">**リスト 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="b8bad-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="b8bad-139">Product クラスは、1 つの追加属性を使用する方法を示しています。 DisplayName 属性。</span><span class="sxs-lookup"><span data-stu-id="b8bad-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="b8bad-140">DisplayName 属性には、エラー メッセージが表示されたら、プロパティ、プロパティの名前を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="b8bad-141">「UnitPrice フィールドが必要です」エラー メッセージを表示する代わりには「価格フィールドが必要です」エラー メッセージ表示できます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b8bad-142">検証コントロールによって表示されるエラー メッセージを完全にカスタマイズする場合は、このような検証コントロールのエラー メッセージのプロパティに、カスタム エラー メッセージを割り当てることができます。 `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="b8bad-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="b8bad-143">Product クラスを使用する**リスト 1**で Create() コント ローラー アクションで**リスト 2**。</span><span class="sxs-lookup"><span data-stu-id="b8bad-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="b8bad-144">このコント ローラー アクションでは、モデルの状態には、すべてのエラーが含まれている場合に、作成ビューが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="b8bad-145">**リスト 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="b8bad-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="b8bad-146">ビューを作成する最後に、**リスト 3** Create() アクションを右クリックし、メニュー オプションを選択して**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="b8bad-147">製品クラスを使用して、モデル クラスとして厳密に型指定されたビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="b8bad-148">選択**作成**ビューのコンテンツのドロップダウン リストから (を参照してください**図 2**)。</span><span class="sxs-lookup"><span data-stu-id="b8bad-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="b8bad-149">**図 2**: 作成ビューの追加</span><span class="sxs-lookup"><span data-stu-id="b8bad-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="b8bad-150">**リスト 3.**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="b8bad-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b8bad-151">によって生成されるフォームを作成するから、Id フィールドを削除、**ビューの追加**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="b8bad-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="b8bad-152">Id フィールドが Id 列に対応していますので、このフィールドの値を入力するユーザーを許可したくないです。</span><span class="sxs-lookup"><span data-stu-id="b8bad-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="b8bad-153">成果物を作成するためのフォームを送信するかどうかと必須のフィールドの値を入力しないでください、検証エラー メッセージが**図 3**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="b8bad-154">**図 3**: 必要なフィールドがないです。</span><span class="sxs-lookup"><span data-stu-id="b8bad-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="b8bad-155">無効な金額でエラー メッセージを入力する場合**図 4**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="b8bad-156">**図 4**: 無効な通貨金額</span><span class="sxs-lookup"><span data-stu-id="b8bad-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="b8bad-157">Entity Framework でのデータ注釈検証コントロールの使用</span><span class="sxs-lookup"><span data-stu-id="b8bad-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="b8bad-158">Microsoft Entity Framework を使用すると、データ モデル クラスを生成している場合は、クラスに直接検証属性を適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b8bad-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="b8bad-159">Entity Framework デザイナーでは、モデル クラスを生成するため、モデル クラスに加えた変更はすべてには、次に、デザイナーで変更を加えるときが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="b8bad-160">Entity Framework によって生成されたクラスで、検証コントロールを使用する場合は、メタ データ クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="b8bad-161">検証コントロールを実際のクラスに検証コントロールを適用する代わりにメタ データ クラスに適用します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="b8bad-162">たとえば、ムービーを Entity Framework を使用してクラスを作成した (を参照してください**図 5**)。</span><span class="sxs-lookup"><span data-stu-id="b8bad-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="b8bad-163">さらに、ディレクター、ムービーのタイトルのプロパティの必須プロパティを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="b8bad-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="b8bad-164">部分クラスとメタ データ クラスを作成する場合、**リスト 4**します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="b8bad-165">**図 5**: Entity Framework によって生成されたムービー クラス</span><span class="sxs-lookup"><span data-stu-id="b8bad-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="b8bad-166">**リスト 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="b8bad-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="b8bad-167">ファイル**リスト 4**ムービーと MovieMetaData という 2 つのクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b8bad-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="b8bad-168">ムービー クラスは、部分クラスです。</span><span class="sxs-lookup"><span data-stu-id="b8bad-168">The Movie class is a partial class.</span></span> <span data-ttu-id="b8bad-169">これは、DataModel.Designer.vb ファイル内の Entity Framework によって生成された部分クラスに対応します。</span><span class="sxs-lookup"><span data-stu-id="b8bad-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="b8bad-170">現時点では、.NET framework には、一部のプロパティはできません。</span><span class="sxs-lookup"><span data-stu-id="b8bad-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="b8bad-171">そのため、ファイル内に定義されているムービー クラスのプロパティに検証属性を適用することで、DataModel.Designer.vb ファイルで定義されているムービー クラスのプロパティに検証属性を適用する方法はありません**リスト4**.</span><span class="sxs-lookup"><span data-stu-id="b8bad-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="b8bad-172">ムービーの部分クラスを指し示す MovieMetaData クラス MetadataType 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b8bad-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="b8bad-173">MovieMetaData クラスには、プロパティ、Movie クラスのプロキシのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b8bad-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="b8bad-174">検証属性は、MovieMetaData クラスのプロパティに適用されます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="b8bad-175">タイトル、監督、および DateReleased プロパティは、すべての必須プロパティとしてマークされます。</span><span class="sxs-lookup"><span data-stu-id="b8bad-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="b8bad-176">ディレクター プロパティには、5 未満の場合の文字を含む文字列を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="b8bad-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="b8bad-177">DisplayName 属性を「リリース済みの日付フィールドが必要です」ようなエラー メッセージを表示する DateReleased プロパティに適用する最後に、</span><span class="sxs-lookup"><span data-stu-id="b8bad-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="b8bad-178">エラーではなく「DateReleased フィールドが必要です。」</span><span class="sxs-lookup"><span data-stu-id="b8bad-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b8bad-179">MovieMetaData クラスでプロキシのプロパティが、Movie クラス内の対応するプロパティと同じ型を表す必要がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b8bad-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="b8bad-180">たとえば、ディレクター プロパティは、ムービー クラスの文字列プロパティと MovieMetaData クラスでオブジェクトのプロパティが。</span><span class="sxs-lookup"><span data-stu-id="b8bad-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="b8bad-181">内のページ**図 6**ムービーのプロパティに無効な値を入力するときに返されるエラー メッセージを示しています。</span><span class="sxs-lookup"><span data-stu-id="b8bad-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="b8bad-182">**図 6**: Entity Framework での検証コントロールの使用 ([フルサイズの画像を表示する をクリックします](validation-with-the-data-annotation-validators-cs/_static/image14.png))。</span><span class="sxs-lookup"><span data-stu-id="b8bad-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="b8bad-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="b8bad-183">Summary</span></span>

<span data-ttu-id="b8bad-184">このチュートリアルでは、ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーを活用する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="b8bad-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="b8bad-185">など、必要な検証属性や StringLength 属性の種類を使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="b8bad-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="b8bad-186">Microsoft Entity Framework を使用する場合は、これらの属性を使用する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="b8bad-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8bad-187">[前へ](validating-with-a-service-layer-cs.md)
> [次へ](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b8bad-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
