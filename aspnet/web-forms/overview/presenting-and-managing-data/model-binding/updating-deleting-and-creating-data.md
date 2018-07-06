---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: 更新、削除、およびモデルのバインディングと web フォームでデータの作成 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 1cf9873db177b67927b579def1eedd08e3e9a762
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821633"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="946b6-104">更新、削除、およびモデルのバインディングと web フォームでデータを作成します。</span><span class="sxs-lookup"><span data-stu-id="946b6-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="946b6-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="946b6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="946b6-106">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="946b6-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="946b6-107">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="946b6-108">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="946b6-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="946b6-109">このチュートリアルでは、作成、更新、およびモデル バインドでデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="946b6-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="946b6-110">次のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="946b6-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="946b6-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="946b6-111">DeleteMethod</span></span>
> - <span data-ttu-id="946b6-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="946b6-112">InsertMethod</span></span>
> - <span data-ttu-id="946b6-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="946b6-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="946b6-114">これらのプロパティは、対応する操作を処理するメソッドの名前を受信します。</span><span class="sxs-lookup"><span data-stu-id="946b6-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="946b6-115">そのメソッド内でデータを操作するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="946b6-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="946b6-116">このチュートリアルは、最初に作成されたプロジェクトでビルド[一部](retrieving-data.md)シリーズの。</span><span class="sxs-lookup"><span data-stu-id="946b6-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="946b6-117">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト</span><span class="sxs-lookup"><span data-stu-id="946b6-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="946b6-118">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="946b6-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="946b6-119">これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="946b6-120">構築します</span><span class="sxs-lookup"><span data-stu-id="946b6-120">What you'll build</span></span>

<span data-ttu-id="946b6-121">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="946b6-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="946b6-122">動的なデータ テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="946b6-123">モデル バインド メソッドを介してデータを更新および削除を有効にします。</span><span class="sxs-lookup"><span data-stu-id="946b6-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="946b6-124">データ検証規則を適用 - データベースに新しいレコードの作成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="946b6-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="946b6-125">動的なデータ テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-125">Add dynamic data templates</span></span>

<span data-ttu-id="946b6-126">最適なユーザー エクスペリエンスを提供し、コードの繰り返しを最小限に抑えるには、動的なデータ テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="946b6-127">構築済みの動的なデータ テンプレートは、NuGet パッケージをインストールすることで、既存のサイトに簡単に統合できます。</span><span class="sxs-lookup"><span data-stu-id="946b6-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="946b6-128">**NuGet パッケージの管理**、インストール、 **DynamicDataTemplatesCS**します。</span><span class="sxs-lookup"><span data-stu-id="946b6-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![動的なデータ テンプレート](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="946b6-130">通知は、プロジェクトがという名前のフォルダーを今すぐに**DynamicData**します。</span><span class="sxs-lookup"><span data-stu-id="946b6-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="946b6-131">そのフォルダーでは、web フォームでのダイナミック コントロールに自動的に適用されているテンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="946b6-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![動的なデータ フォルダー](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="946b6-133">有効にする更新および削除</span><span class="sxs-lookup"><span data-stu-id="946b6-133">Enable updating and deleting</span></span>

<span data-ttu-id="946b6-134">ユーザーを更新し、データベース内のレコードを削除することは、データを取得するためのプロセスによく似ています。</span><span class="sxs-lookup"><span data-stu-id="946b6-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="946b6-135">**UpdateMethod**と**DeleteMethod**プロパティ、これらの操作を実行するメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="946b6-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="946b6-136">GridView コントロールとも編集の自動生成を指定し、ボタンを削除できます。</span><span class="sxs-lookup"><span data-stu-id="946b6-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="946b6-137">次の強調表示されたコードでは、GridView コードへの追加を示します。</span><span class="sxs-lookup"><span data-stu-id="946b6-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="946b6-138">分離コード ファイルに追加を使用して、ステートメントの**System.Data.Entity.Infrastructure**します。</span><span class="sxs-lookup"><span data-stu-id="946b6-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="946b6-139">次の更新プログラムを追加し、メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="946b6-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="946b6-140">**Tryupdatemodel に渡します**メソッドが web フォームからのデータ項目に一致するデータ バインドされた値を適用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="946b6-141">データ項目は、id パラメーターの値に基づいて取得されます。</span><span class="sxs-lookup"><span data-stu-id="946b6-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="946b6-142">検証の要件を適用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-142">Enforce validation requirements</span></span>

<span data-ttu-id="946b6-143">データを更新するときに、FirstName、LastName、および年の Student クラス プロパティに適用する検証属性が自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="946b6-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="946b6-144">DynamicField コントロールは、検証属性に基づくクライアントとサーバーの検証コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="946b6-145">FirstName および LastName プロパティが両方とも必要です。</span><span class="sxs-lookup"><span data-stu-id="946b6-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="946b6-146">FirstName が 20 文字を超えることはできず、LastName は 40 文字を超えることはできません。</span><span class="sxs-lookup"><span data-stu-id="946b6-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="946b6-147">年は AcademicYear 列挙体の有効な値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="946b6-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="946b6-148">ユーザーは、検証の要件のいずれかに違反する場合、更新プログラムは続行されません。</span><span class="sxs-lookup"><span data-stu-id="946b6-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="946b6-149">エラー メッセージを確認するには、GridView 上 ValidationSummary コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="946b6-150">モデル バインドからの検証エラーを表示するには、設定、 **ShowModelStateErrors**プロパティに設定**true**します。</span><span class="sxs-lookup"><span data-stu-id="946b6-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="946b6-151">Web アプリケーションを実行および更新したレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="946b6-151">Run the web application, and update and delete any of the records.</span></span>

![データの更新](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="946b6-153">編集モードで年のプロパティの値が自動的にドロップダウン リストとして表示する場合に注意します。</span><span class="sxs-lookup"><span data-stu-id="946b6-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="946b6-154">Year プロパティは、列挙値を列挙値の動的なデータ テンプレートが、ドロップダウン リストの編集を指定します。</span><span class="sxs-lookup"><span data-stu-id="946b6-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="946b6-155">開いて、そのテンプレートを見つけることができます、**列挙\_Edit.ascx**ファイル、 **DynamicData**/**FieldTemplates**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="946b6-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="946b6-156">有効な値を指定する場合、更新プログラムが正常に完了します。</span><span class="sxs-lookup"><span data-stu-id="946b6-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="946b6-157">検証の要件のいずれかに違反した場合、更新プログラムは続行されませんし、グリッドの上に、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="946b6-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![エラー メッセージ](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="946b6-159">新しいレコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-159">Add new records</span></span>

<span data-ttu-id="946b6-160">GridView コントロールは含まれません、 **InsertMethod**プロパティと、モデル バインドで新しいレコードを追加するため使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="946b6-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="946b6-161">InsertMethod プロパティを見つけることができます、 **FormView**、 **DetailsView**、または**ListView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="946b6-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="946b6-162">このチュートリアルでは、新しいレコードを追加するのに FormView コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="946b6-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="946b6-163">最初に、新しいレコードを追加用に作成する新しいページにリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="946b6-164">ValidationSummary を追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="946b6-165">新しいリンクは、学生のページのコンテンツの最上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="946b6-165">The new link will appear at the top of the content for the Students page.</span></span>

![新しいリンク](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="946b6-167">次に、マスター ページを使用して、新しい web フォームを追加し、名前**AddStudent**します。</span><span class="sxs-lookup"><span data-stu-id="946b6-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="946b6-168">マスター ページとして Site.Master を選択します。</span><span class="sxs-lookup"><span data-stu-id="946b6-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="946b6-169">使用して新しい受講者を追加するためのフィールドをレンダリングするが、 **DynamicEntity**コントロール。</span><span class="sxs-lookup"><span data-stu-id="946b6-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="946b6-170">DynamicEntity コントロールは、ItemType プロパティで指定されたクラスでその編集可能なプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="946b6-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="946b6-171">StudentID プロパティでマークを付けた、 **[ScaffoldColumn(false)]** 属性はレンダリングされませんので。</span><span class="sxs-lookup"><span data-stu-id="946b6-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="946b6-172">AddStudent ページの MainContent プレース ホルダーでは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="946b6-173">分離コード ファイル (AddStudent.aspx.cs) で、追加、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="946b6-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="946b6-174">次に、新しいレコード と キャンセル ボタンのイベント ハンドラーを挿入する方法を指定する次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="946b6-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="946b6-175">すべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="946b6-175">Save all of the changes.</span></span>

<span data-ttu-id="946b6-176">Web アプリケーションを実行し、新しい学生を作成します。</span><span class="sxs-lookup"><span data-stu-id="946b6-176">Run the web application and create a new student.</span></span>

![新しい学生を追加します。](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="946b6-178">クリックして**挿入**し、新しい学生が作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="946b6-178">Click **Insert** and notice the new student has been created.</span></span>

![新しい学生を表示します。](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="946b6-180">まとめ</span><span class="sxs-lookup"><span data-stu-id="946b6-180">Conclusion</span></span>

<span data-ttu-id="946b6-181">このチュートリアルでは、更新、削除、およびデータの作成を有効になります。</span><span class="sxs-lookup"><span data-stu-id="946b6-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="946b6-182">データを操作するときに検証規則が適用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="946b6-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="946b6-183">次の[チュートリアル](sorting-paging-and-filtering-data.md)このシリーズでは有効にした並べ替え、ページング、およびデータのフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="946b6-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="946b6-184">[前へ](retrieving-data.md)
> [次へ](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="946b6-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
