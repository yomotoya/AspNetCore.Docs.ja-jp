---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 8 部 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 545438a48c57bed00530f3d4cc143542d8999581
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826099"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="e5d1d-104">Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 8</span><span class="sxs-lookup"><span data-stu-id="e5d1d-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="e5d1d-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="e5d1d-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="e5d1d-107">チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="e5d1d-108">Dynamic Data 機能を使用して書式設定およびデータを検証するには</span><span class="sxs-lookup"><span data-stu-id="e5d1d-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="e5d1d-109">前のチュートリアルでは、ストアド プロシージャを実装します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="e5d1d-110">このチュートリアルでは説明 Dynamic Data 機能が次の利点を提供する方法。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="e5d1d-111">フィールドは自動的に、それらのデータ型に基づいて表示の書式設定します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="e5d1d-112">フィールドは、それらのデータ型に基づいて自動的に検証されます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="e5d1d-113">書式設定と検証の動作をカスタマイズするデータ モデルにメタデータを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="e5d1d-114">これを行うし、書式設定と検証ルールを追加するには、1 つだけで、すべての場所で自動的に適用する動的データ コントロールを使用してフィールドにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="e5d1d-115">この動作を確認するには、表示し、編集、既存のフィールドを使用するコントロールを変更します*Students.aspx*の名前と日付のフィールドを書式設定と検証のメタデータを追加します ページとする、`Student`エンティティ型。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="e5d1d-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="e5d1d-117">DynamicField および DynamicControl コントロールの使用</span><span class="sxs-lookup"><span data-stu-id="e5d1d-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="e5d1d-118">開く、 *Students.aspx*ページし、`StudentsGridView`コントロール置換、**名前**と**加入契約日**`TemplateField`を次のマークアップ要素。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="e5d1d-119">このマークアップでは、`DynamicControl`の代わりに制御`TextBox`と`Label`、学生のコントロール テンプレートのフィールドの名前を使用して、`DynamicField`登録日付コントロール。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="e5d1d-120">書式指定文字列が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-120">No format strings are specified.</span></span>

<span data-ttu-id="e5d1d-121">追加、`ValidationSummary`制御した後、`StudentsGridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="e5d1d-122">`SearchGridView`コントロールのマークアップを置き換える、**名前**と**加入契約日**で実行したとする列、`StudentsGridView`省略点を除いて、制御、`EditItemTemplate`要素。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="e5d1d-123">`Columns`の要素、`SearchGridView`コントロールが次のマークアップを含むようになりました。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="e5d1d-124">開いている*Students.aspx.cs*し、以下の追加`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="e5d1d-125">ページのハンドラーを追加`Init`イベント。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="e5d1d-126">このコードでは、動的なデータが書式設定とこれらのフィールドのデータ バインド コントロールで検証を提供するを指定します、`Student`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="e5d1d-127">次の例のようなエラー メッセージが表示されるは、ページを実行すると、通常場合を呼び出す忘れてしまった場合、`EnableDynamicData`メソッド`Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="e5d1d-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="e5d1d-128">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-128">Run the page.</span></span>

<span data-ttu-id="e5d1d-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="e5d1d-130">**加入契約日**列、プロパティの型があるため、日付と時刻が表示されます`DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="e5d1d-131">後でを修正します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-131">You'll fix that later.</span></span>

<span data-ttu-id="e5d1d-132">ここでは、動的なデータが自動的に検証の基本的なデータを提供することを確認します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="e5d1d-133">たとえば、をクリックして**編集**、日付フィールドをクリア、 をクリックして**Update**としていること動的データに自動的にこの必須のフィールド値がデータ モデルで許容されないためです。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="e5d1d-134">ページには、フィールドと、エラー メッセージの後にアスタリスクが表示されます、`ValidationSummary`コントロール。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="e5d1d-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="e5d1d-136">省略することもできます、`ValidationSummary`もエラー メッセージが表示するには、アスタリスクをマウス ポインターを格納できるため、制御します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="e5d1d-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="e5d1d-138">動的データで入力したデータの検証も、**加入契約日**フィールドが有効な日付。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="e5d1d-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="e5d1d-140">ご覧のとおり、これは、一般的なエラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="e5d1d-141">次のセクションでは、メッセージ、検証、および書式指定規則をカスタマイズする方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="e5d1d-142">データ モデルにメタデータの追加</span><span class="sxs-lookup"><span data-stu-id="e5d1d-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="e5d1d-143">通常、動的なデータによって提供される機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="e5d1d-144">たとえば、データの表示方法、およびエラー メッセージの内容を変更する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="e5d1d-145">通常もカスタマイズする動的なデータより多くの機能を提供するデータの検証規則のデータ型に基づいて自動的に。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="e5d1d-146">これを行うには、エンティティ型に対応する部分クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="e5d1d-147">**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**参照の追加**への参照を追加および`System.ComponentModel.DataAnnotations`します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="e5d1d-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="e5d1d-149">*DAL*フォルダー、新しいクラス ファイルを作成、名前を付けます*Student.cs*、し、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="e5d1d-150">このコードの部分クラスを作成し、`Student`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="e5d1d-151">`MetadataType`この部分クラスに適用される属性を使用してメタデータを指定するクラスを識別します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="e5d1d-152">メタデータ クラスは、任意の名前を持つことができますが、一般的な方法は、エンティティ名と「メタデータ」を使用します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="e5d1d-153">メタデータ クラスのプロパティに適用される属性は、検証、ルール、およびエラー メッセージの書式設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="e5d1d-154">ここに示す属性は、次の結果になります。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="e5d1d-155">`EnrollmentDate` 日付 (時刻) のないとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="e5d1d-156">両方の名前フィールドは 25 文字である必要があります。 または小さい長、およびカスタム エラー メッセージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="e5d1d-157">両方の名前フィールドは必須であり、カスタム エラー メッセージを提供します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="e5d1d-158">実行、 *Students.aspx*ページをもう一度、および、時刻なしの日付が表示されますを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="e5d1d-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="e5d1d-160">行を編集し、名前フィールドの値のクリアを試します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="e5d1d-161">クリックする前に、フィールドのままにすると、すぐに表示されるフィールドのエラーを示すアスタリスク**Update**します。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="e5d1d-162">クリックすると**Update**ページには、指定したエラー メッセージ テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="e5d1d-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="e5d1d-164">25 文字より長い名前を入力すると、クリックして**更新**、し、ページには、指定したエラー メッセージ テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="e5d1d-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="e5d1d-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="e5d1d-166">ようになりましたデータ モデルのメタデータでこれらの書式設定と検証ルールを設定すると、規則が自動的に適用されることを表示したり、これらのフィールドを変更できるようにするすべてのページを使用する限り、`DynamicControl`または`DynamicField`コントロール。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="e5d1d-167">これは、量が減り、プログラミングおよびテストしやすいため、冗長なコードを記述する必要があるし、データの書式設定と検証が、アプリケーション全体で一貫性のあることになります。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="e5d1d-168">説明</span><span class="sxs-lookup"><span data-stu-id="e5d1d-168">More Information</span></span>

<span data-ttu-id="e5d1d-169">これには、Entity Framework の概要チュートリアルのこのシリーズで終了です。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="e5d1d-170">Entity Framework を使用する方法の学習に役立つその他のリソースを引き続き[Entity Framework の次のチュートリアル シリーズの最初のチュートリアル](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)か、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5d1d-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="e5d1d-171">Entity Framework に関する FAQ</span><span class="sxs-lookup"><span data-stu-id="e5d1d-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="e5d1d-172">Entity Framework チームのブログ</span><span class="sxs-lookup"><span data-stu-id="e5d1d-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="e5d1d-173">MSDN ライブラリの entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5d1d-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="e5d1d-174">MSDN データ デベロッパー センターで entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5d1d-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="e5d1d-175">EntityDataSource Web サーバー コントロール、MSDN ライブラリの概要</span><span class="sxs-lookup"><span data-stu-id="e5d1d-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="e5d1d-176">EntityDataSource コントロール、MSDN ライブラリの API リファレンス</span><span class="sxs-lookup"><span data-stu-id="e5d1d-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="e5d1d-177">MSDN の entity Framework フォーラム</span><span class="sxs-lookup"><span data-stu-id="e5d1d-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="e5d1d-178">Julie Lerman のブログ</span><span class="sxs-lookup"><span data-stu-id="e5d1d-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="e5d1d-179">前へ</span><span class="sxs-lookup"><span data-stu-id="e5d1d-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
