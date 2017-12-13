---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "更新、削除、およびデータのモデル バインディングと web フォームを作成 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c2c95-104">更新、削除、およびモデル バインディングと web フォームをデータの作成</span><span class="sxs-lookup"><span data-stu-id="c2c95-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="c2c95-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c2c95-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c2c95-106">このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c2c95-107">モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。</span><span class="sxs-lookup"><span data-stu-id="c2c95-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c2c95-108">この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c2c95-109">このチュートリアルでは、作成、更新、およびモデル バインディングでデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="c2c95-110">次のプロパティが設定されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="c2c95-111">deleteMethod</span><span class="sxs-lookup"><span data-stu-id="c2c95-111">DeleteMethod</span></span>
> - <span data-ttu-id="c2c95-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="c2c95-112">InsertMethod</span></span>
> - <span data-ttu-id="c2c95-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="c2c95-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="c2c95-114">これらのプロパティは、対応する操作を処理するメソッドの名前を受信します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="c2c95-115">メソッドは、内では、データと対話するため、ロジックを指定します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="c2c95-116">このチュートリアルは、最初に作成されたプロジェクトに基づいて[一部](retrieving-data.md)シリーズのです。</span><span class="sxs-lookup"><span data-stu-id="c2c95-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="c2c95-117">実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c2c95-118">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c2c95-119">これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="c2c95-120">新機能のビルド</span><span class="sxs-lookup"><span data-stu-id="c2c95-120">What you'll build</span></span>

<span data-ttu-id="c2c95-121">このチュートリアルでは、次の手順を。</span><span class="sxs-lookup"><span data-stu-id="c2c95-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c2c95-122">動的なデータ テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="c2c95-123">モデル バインド メソッドを介してデータを更新し、削除を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c2c95-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="c2c95-124">データ検証ルールを適用 - 新しいレコードを作成するデータベースで有効にします。</span><span class="sxs-lookup"><span data-stu-id="c2c95-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="c2c95-125">動的なデータ テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-125">Add dynamic data templates</span></span>

<span data-ttu-id="c2c95-126">最適なユーザー エクスペリエンスを提供し、コードの繰り返しを最小限に抑えるには、動的なデータ テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="c2c95-127">構築済みの動的なデータ テンプレートは、NuGet パッケージをインストールして、既存のサイトに簡単に統合できます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="c2c95-128">**NuGet パッケージの管理**、インストール、 **DynamicDataTemplatesCS**です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![動的なデータ テンプレート](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="c2c95-130">プロジェクトに今すぐという名前のフォルダーが含まれることに注意してください**ダイナミック**です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="c2c95-131">そのフォルダーでは、web フォームでのダイナミック コントロールに自動的に適用されているテンプレートが見つかります。</span><span class="sxs-lookup"><span data-stu-id="c2c95-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![動的なデータ フォルダー](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="c2c95-133">有効にする更新および削除</span><span class="sxs-lookup"><span data-stu-id="c2c95-133">Enable updating and deleting</span></span>

<span data-ttu-id="c2c95-134">更新およびデータベース内のレコードを削除するユーザーの有効化は、データを取得するためのプロセスによく似ています。</span><span class="sxs-lookup"><span data-stu-id="c2c95-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="c2c95-135">**UpdateMethod**と**DeleteMethod**プロパティ、これらの操作を実行するメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="c2c95-136">GridView コントロールにも編集の自動生成を指定し、ボタンを削除できます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="c2c95-137">次の強調表示されたコードでは、GridView コードの追加機能を示します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="c2c95-138">分離コード ファイル内の追加を使用して、ステートメントの**System.Data.Entity.Infrastructure**です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="c2c95-139">次の更新プログラムを追加し、メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="c2c95-140">**TryUpdateModel**メソッドは、一致するデータ バインドされた値を web フォームからのデータ項目に適用します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="c2c95-141">Id パラメーターの値に基づいて、データ項目が取得されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="c2c95-142">検証要件を強制します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-142">Enforce validation requirements</span></span>

<span data-ttu-id="c2c95-143">データを更新するときに、Student クラスでは、FirstName、LastName、および年のプロパティに適用される検証の属性が自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="c2c95-144">DynamicField コントロールは、検証の属性に基づくクライアントとサーバーの検証コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="c2c95-145">FirstName および LastName から構成プロパティの両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="c2c95-146">FirstName が 20 文字を超えることはできませんし、LastName は 40 文字を超えることはできません。</span><span class="sxs-lookup"><span data-stu-id="c2c95-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="c2c95-147">年は AcademicYear 列挙の有効な値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2c95-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="c2c95-148">ユーザーは、検証の要件のいずれかに違反する場合、更新プログラムは続行されません。</span><span class="sxs-lookup"><span data-stu-id="c2c95-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="c2c95-149">エラー メッセージを表示するには、GridView 上 ValidationSummary コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="c2c95-150">モデル バインディングの検証エラーを表示する設定、 **ShowModelStateErrors**プロパティに設定**true**です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="c2c95-151">Web アプリケーションを実行および更新し、任意のレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-151">Run the web application, and update and delete any of the records.</span></span>

![データの更新](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="c2c95-153">編集モードで年プロパティの値が自動的にドロップダウン リストとして表示する場合は、ことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="c2c95-154">年プロパティは、列挙値を列挙値の動的なデータ テンプレートは、ドロップダウン リストを編集するためを指定します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="c2c95-155">そのテンプレートを検索するには、開く、**列挙\_Edit.ascx**ファイルを**ダイナミック**/**FieldTemplates**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c2c95-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="c2c95-156">有効な値を指定する場合、更新プログラムが正常に完了します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="c2c95-157">検証の要件のいずれかに違反した場合、更新プログラムは続行されませんし、グリッドの上に、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![エラー メッセージ](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="c2c95-159">新しいレコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-159">Add new records</span></span>

<span data-ttu-id="c2c95-160">GridView コントロールは含まれません、 **InsertMethod**プロパティ モデル バインディングで、新しいレコードを追加するため使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="c2c95-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="c2c95-161">InsertMethod のプロパティを見つけることができます、 **FormView**、 **DetailsView**、または**ListView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="c2c95-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="c2c95-162">このチュートリアルでは、FormView コントロールを使用して、新しいレコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="c2c95-163">最初に、新しいレコードを追加するを作成する新しいページにリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="c2c95-164">ValidationSummary を追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="c2c95-165">新しいリンクは、受講者 ページのコンテンツの上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2c95-165">The new link will appear at the top of the content for the Students page.</span></span>

![新しいリンク](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="c2c95-167">次に、マスター ページを使用して新しい web フォームを追加し、名前**AddStudent**です。</span><span class="sxs-lookup"><span data-stu-id="c2c95-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="c2c95-168">マスター ページとして Site.Master を選択します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="c2c95-169">使用して、新しい受講者を追加するためのフィールドを表示する、 **DynamicEntity**コントロール。</span><span class="sxs-lookup"><span data-stu-id="c2c95-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="c2c95-170">DynamicEntity コントロールは、その ItemType プロパティで指定されたクラスの編集可能なプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="c2c95-171">学生 Id プロパティとして設定されていた、 **[ScaffoldColumn(false)]**属性のためはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="c2c95-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="c2c95-172">メインのプレース ホルダー AddStudent ページのでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="c2c95-173">分離コード ファイル (AddStudent.aspx.cs) で、追加、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="c2c95-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="c2c95-174">次に、新しいレコード と キャンセル ボタンのイベント ハンドラーを挿入する方法を指定する次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="c2c95-175">すべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-175">Save all of the changes.</span></span>

<span data-ttu-id="c2c95-176">Web アプリケーションを実行し、新しい学生を作成します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-176">Run the web application and create a new student.</span></span>

![新しい学生を追加します。](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="c2c95-178">をクリックして**挿入**し新しい学生が作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-178">Click **Insert** and notice the new student has been created.</span></span>

![新しい学生を表示します。](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="c2c95-180">まとめ</span><span class="sxs-lookup"><span data-stu-id="c2c95-180">Conclusion</span></span>

<span data-ttu-id="c2c95-181">このチュートリアルでは、更新、削除、およびデータを作成して有効になります。</span><span class="sxs-lookup"><span data-stu-id="c2c95-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="c2c95-182">データを操作するときに検証規則を適用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="c2c95-183">次の[チュートリアル](sorting-paging-and-filtering-data.md)この系列には有効にした並べ替え、ページング、およびデータのフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="c2c95-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2c95-184">[前へ](retrieving-data.md)
[次へ](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="c2c95-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
