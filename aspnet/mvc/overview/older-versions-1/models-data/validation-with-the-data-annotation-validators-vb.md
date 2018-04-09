---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: データ注釈検証コントロール (VB) を使用した検証 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーの手順を利用します。 さまざまな種類の検証コントロールを使用する方法を説明してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="58ffa-104">データ注釈検証コントロール (VB) を使用した検証</span><span class="sxs-lookup"><span data-stu-id="58ffa-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="58ffa-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="58ffa-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="58ffa-106">ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーの手順を利用します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="58ffa-107">さまざまな種類の検証属性を使用し、Microsoft Entity Framework に処理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="58ffa-108">このチュートリアルでは、データ注釈検証コントロールを使用して、ASP.NET MVC アプリケーションで検証を実行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="58ffa-109">データ注釈検証コントロールを使用する利点は、化など、必要な – 1 つまたは複数の属性や StringLength 属性を追加するだけで – クラス プロパティに検証を実行できることです。</span><span class="sxs-lookup"><span data-stu-id="58ffa-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="58ffa-110">データ注釈検証コントロールを使用することができます、前に、データ注釈のモデル バインダーをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="58ffa-111">CodePlex web サイトからデータ注釈のモデル バインダーのサンプルをダウンロードする をクリックして[ここ](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="58ffa-112">ことが重要データ注釈のモデル バインダーは、Microsoft ASP.NET MVC フレームワークの公式の一部ではないことを理解します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="58ffa-113">データ注釈のモデル バインダーが、Microsoft ASP.NET MVC チームによって作成されますが、Microsoft がデータ注釈のモデル バインダーの正式な製品のサポートが説明されているし、このチュートリアルで使用される提供しません。</span><span class="sxs-lookup"><span data-stu-id="58ffa-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="58ffa-114">データ注釈のモデル バインダーを使用してください。</span><span class="sxs-lookup"><span data-stu-id="58ffa-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="58ffa-115">ASP.NET MVC アプリケーションでデータの注釈のモデル バインダーを使用するのには、まず、Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="58ffa-116">メニュー オプションを選択**プロジェクトで、参照の追加**です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="58ffa-117">次へ をクリックして、**参照** タブを見つけて、データ注釈モデル バインダーのサンプルのダウンロード (および解凍)、(を参照してください**図 1**)。</span><span class="sxs-lookup"><span data-stu-id="58ffa-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="58ffa-118">**図 1**: データの注釈のモデル バインダーへの参照を追加する ([フルサイズのイメージを表示するをクリックして](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="58ffa-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="58ffa-119">Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へアセンブリの両方を選択し、クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="58ffa-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="58ffa-120">データ注釈のモデル バインダーを備えた .NET Framework Service Pack 1 に含まれている system.componentmodel.dataannotations.dll へのアセンブリを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="58ffa-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="58ffa-121">データ注釈モデル バインダー サンプル ダウンロードに含まれている system.componentmodel.dataannotations.dll へアセンブリのバージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="58ffa-122">最後に、Global.asax ファイルで DataAnnotations モデル バインダーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="58ffa-123">アプリケーションに次のコード行を追加\_Start() イベント ハンドラーをアプリケーション\_Start() メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="58ffa-124">次のコード行は、ASP.NET MVC アプリケーション全体の既定のモデル バインダーとして、DataAnnotationsModelBinder を登録します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="58ffa-125">データ注釈検証コントロールの属性の使用</span><span class="sxs-lookup"><span data-stu-id="58ffa-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="58ffa-126">データ注釈のモデル バインダーを使用する場合は、検証を実行するバリデーター属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="58ffa-127">System.ComponentModel.DataAnnotations 名前空間には、次の検証属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="58ffa-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="58ffa-128">範囲は – 指定された範囲の値の間に、プロパティの値があるかどうかを検証することができます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="58ffa-129">ReqularExpression – では、プロパティの値が指定した正規表現パターンに一致するかどうかを検証することができます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="58ffa-130">必要な – 必要に応じてプロパティをマークすることができます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="58ffa-131">StringLength – では、文字列プロパティの最大長を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="58ffa-132">検証: すべての検証属性の基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="58ffa-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="58ffa-133">標準の検証コントロールのいずれかによって、検証ニーズが満たされない場合、常がある基本検証属性を新しいバリデーター属性を継承してカスタム検証属性を作成するオプションです。</span><span class="sxs-lookup"><span data-stu-id="58ffa-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="58ffa-134">製品クラス**リスト 1**これらバリデーター属性を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="58ffa-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="58ffa-135">名前、説明、および UnitPrice プロパティもマークとして必要です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="58ffa-136">Name プロパティには、10 文字未満である文字列の長さが必要です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="58ffa-137">最後に、UnitPrice プロパティは、金額を表す正規表現パターンに一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="58ffa-138">**1.**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="58ffa-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="58ffa-139">Product クラスが 1 つの追加属性を使用する方法を示しています。 DisplayName 属性。</span><span class="sxs-lookup"><span data-stu-id="58ffa-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="58ffa-140">DisplayName 属性では、エラー メッセージが表示されたら、プロパティ、プロパティの名前を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="58ffa-141">「UnitPrice フィールドが必要」というエラー メッセージを表示する代わりには「Price フィールドが必要」というエラー メッセージ表示できます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="58ffa-142">検証コントロールによって表示されるエラー メッセージを完全にカスタマイズする場合は、次のように ErrorMessage プロパティの検証コントロールのカスタム エラー メッセージを割り当てることができます。 `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="58ffa-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="58ffa-143">Product クラスを使用することができます**リスト 1**の Create() コント ローラーのアクションと**を一覧表示する 2**です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="58ffa-144">このコント ローラーのアクションでは、モデルの状態には、すべてのエラーが含まれている場合、ビューを作成するが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="58ffa-145">**2 を一覧表示する**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="58ffa-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="58ffa-146">ビューを作成する最後に、**リスト 3** Create() アクションを右クリックし、メニュー オプションを選択して**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="58ffa-147">製品クラスを使用したモデル クラスとして厳密に型指定されたビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="58ffa-148">選択**作成**ビューのコンテンツのドロップダウン リストから (を参照してください**図 2**)。</span><span class="sxs-lookup"><span data-stu-id="58ffa-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="58ffa-149">**図 2**: 作成ビューの追加</span><span class="sxs-lookup"><span data-stu-id="58ffa-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="58ffa-150">**3 を一覧表示する**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="58ffa-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="58ffa-151">によって生成されたフォームの作成から、Id フィールドを削除、**ビューの追加**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="58ffa-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="58ffa-152">Id フィールドは、Id 列に対応しているために、このフィールドの値を入力できるようにするしたくありません。</span><span class="sxs-lookup"><span data-stu-id="58ffa-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="58ffa-153">製品を作成するフォームを送信して、必要なフィールドの値を入力しない場合検証エラー メッセージで**図 3**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="58ffa-154">**図 3**: 必要なフィールドがありません</span><span class="sxs-lookup"><span data-stu-id="58ffa-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="58ffa-155">無効な金額、内のエラー メッセージを入力すると**図 4**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="58ffa-156">**図 4**: 無効な金額</span><span class="sxs-lookup"><span data-stu-id="58ffa-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="58ffa-157">Entity Framework でのデータ注釈検証コントロールの使用</span><span class="sxs-lookup"><span data-stu-id="58ffa-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="58ffa-158">Microsoft Entity Framework を使用して、データ モデル クラスを生成する場合は、クラスに直接バリデーター属性を適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="58ffa-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="58ffa-159">Entity Framework Designer は、モデル クラスを生成するため、モデルのクラスに加えた変更はすべてには、次回、デザイナーで変更を加えるが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="58ffa-160">Entity Framework によって生成されたクラスでの検証コントロールを使用する場合は、メタ データ クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="58ffa-161">検証コントロールを実際のクラスに適用する代わりにメタ データ クラスには、検証コントロールを適用します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="58ffa-162">たとえば、Entity Framework を使用して、ムービー クラスが作成されたこと (を参照してください**図 5**)。</span><span class="sxs-lookup"><span data-stu-id="58ffa-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="58ffa-163">さらに、ムービー タイトルとディレクター プロパティの必須プロパティを作成することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="58ffa-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="58ffa-164">部分クラスと内のメタ データ クラスを作成する場合は、**リスト 4**です。</span><span class="sxs-lookup"><span data-stu-id="58ffa-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="58ffa-165">**図 5**: Entity Framework によって生成されたムービー クラス</span><span class="sxs-lookup"><span data-stu-id="58ffa-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="58ffa-166">**リスト 4**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="58ffa-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="58ffa-167">内のファイル**リスト 4**ムービーおよび MovieMetaData という名前の 2 つのクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="58ffa-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="58ffa-168">ムービー クラスは、部分クラスです。</span><span class="sxs-lookup"><span data-stu-id="58ffa-168">The Movie class is a partial class.</span></span> <span data-ttu-id="58ffa-169">これは、DataModel.Designer.vb ファイルに含まれている Entity Framework によって生成される部分クラスに対応します。</span><span class="sxs-lookup"><span data-stu-id="58ffa-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="58ffa-170">現時点では、.NET framework は一部のプロパティをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="58ffa-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="58ffa-171">そのため、ファイルで定義されているムービー クラスのプロパティに検証属性を適用することによって、DataModel.Designer.vb ファイルで定義されているムービー クラスのプロパティに検証属性を適用する方法はありません**リスト 4**.</span><span class="sxs-lookup"><span data-stu-id="58ffa-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="58ffa-172">ムービーの部分クラスが MovieMetaData クラスを指している MetadataType 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="58ffa-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="58ffa-173">MovieMetaData クラスには、ムービー クラスのプロパティのプロキシのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="58ffa-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="58ffa-174">バリデーター属性は、MovieMetaData クラスのプロパティに適用されます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="58ffa-175">タイトルやディレクター、DateReleased プロパティは、すべての必須プロパティとしてマークされます。</span><span class="sxs-lookup"><span data-stu-id="58ffa-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="58ffa-176">ディレクター プロパティには、5 未満の値の文字を含む文字列を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="58ffa-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="58ffa-177">最後に、DisplayName 属性がプロパティに適用される、DateReleased 必須「リリース済みの日付フィールドを行うことです」と同じように、エラー メッセージを表示するには</span><span class="sxs-lookup"><span data-stu-id="58ffa-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="58ffa-178">エラーの代わりに「DateReleased フィールドが必要です。」</span><span class="sxs-lookup"><span data-stu-id="58ffa-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="58ffa-179">MovieMetaData クラスのプロキシのプロパティがムービー クラスで対応するプロパティと同じ型を表す必要がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="58ffa-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="58ffa-180">たとえば、ディレクター プロパティは、ムービー クラス内の文字列プロパティ MovieMetaData クラス内のオブジェクト プロパティです。</span><span class="sxs-lookup"><span data-stu-id="58ffa-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="58ffa-181">内のページ**図 6**ムービーのプロパティに無効な値を入力するときに返されるエラー メッセージを示しています。</span><span class="sxs-lookup"><span data-stu-id="58ffa-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="58ffa-182">**図 6**: Entity Framework での検証コントロールの使用 ([フルサイズのイメージを表示するをクリックして](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="58ffa-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="58ffa-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="58ffa-183">Summary</span></span>

<span data-ttu-id="58ffa-184">このチュートリアルでは、ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーを活用する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="58ffa-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="58ffa-185">など、必要な検証属性や StringLength 属性の種類を使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="58ffa-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="58ffa-186">また、Microsoft の Entity Framework を使用する場合は、これらの属性を使用する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="58ffa-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="58ffa-187">前へ</span><span class="sxs-lookup"><span data-stu-id="58ffa-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
