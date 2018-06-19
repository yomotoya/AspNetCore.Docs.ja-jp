---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (10 の 8) で Entity Framework による継承の実装 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874006"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="fb71f-103">ASP.NET MVC アプリケーション (10 の 8) で Entity Framework による継承の実装</span><span class="sxs-lookup"><span data-stu-id="fb71f-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="fb71f-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fb71f-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="fb71f-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="fb71f-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="fb71f-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="fb71f-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fb71f-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="fb71f-108">一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="fb71f-109">解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="fb71f-110">一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="fb71f-111">一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="fb71f-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="fb71f-112">前のチュートリアルでは、同時実行例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="fb71f-113">このチュートリアルでは、データ モデルで継承を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="fb71f-114">オブジェクト指向プログラミングでは、冗長なコードを除去するのに継承を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="fb71f-115">このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="fb71f-116">どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="fb71f-117">テーブルの種類ごとの継承ではなくテーブルは階層ごと</span><span class="sxs-lookup"><span data-stu-id="fb71f-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="fb71f-118">オブジェクト指向プログラミングでは、関連するクラスを使用しやすく継承を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="fb71f-119">たとえば、`Instructor`と`Student`内のクラス、`School`データ モデルは冗長なコードでこの結果、複数のプロパティを共有します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="fb71f-121">`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="fb71f-122">作成することが、`Person`プロパティ、その共有だけが含まれているクラスを基本にしてください、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="fb71f-124">データベースでこの継承構造を表すことができるいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="fb71f-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="fb71f-125">割り当てることも、`Person`受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むテーブル。</span><span class="sxs-lookup"><span data-stu-id="fb71f-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="fb71f-126">講習においてインストラクターにのみ適用できなかった一部の列 (`HireDate`)、受講者にのみ一部 (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="fb71f-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="fb71f-127">通常、する必要があります、*識別子*各行を入力するかを示す列を表します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="fb71f-128">たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="fb71f-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="fb71f-130">1 つのデータベース テーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*テーブルの階層あたり*(TPH) 継承します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="fb71f-131">代わりに、継承構造と同じように見えるデータベースを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="fb71f-132">たとえば name フィールドのみを持つことが、`Person`テーブルし、は独立した`Instructor`と`Student`日付フィールドを持つテーブルです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="fb71f-134">各エンティティ クラスと呼ばれるデータベース テーブルのこのパターン*型ごとにテーブル*(TPT) 継承します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="fb71f-135">一般に TPH 継承パターンでは TPT パターンが複雑な結合クエリのためにパフォーマンスを向上させる TPT 継承のパターンよりも、Entity Framework でに配信しています。</span><span class="sxs-lookup"><span data-stu-id="fb71f-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="fb71f-136">このチュートリアルでは、TPH 継承の実装方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="fb71f-137">次の手順を実行することによってを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="fb71f-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="fb71f-138">作成、`Person`クラスし、変更、`Instructor`と`Student`クラスから派生する`Person`です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="fb71f-139">データベース コンテキスト クラスには、データベースへのモデルのマッピング コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="fb71f-140">変更`InstructorID`と`StudentID`プロジェクト全体にわたる参照`PersonID`です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="fb71f-141">ユーザー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-141">Creating the Person Class</span></span>

 <span data-ttu-id="fb71f-142">注: これらのクラスを使用するコント ローラーを更新するまでは、次のクラスを作成した後、プロジェクトをコンパイルできません。</span><span class="sxs-lookup"><span data-stu-id="fb71f-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="fb71f-143">*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="fb71f-144">*Instructor.cs*、派生、`Instructor`クラス、`Person`クラスし、キーと名前のフィールドを削除します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="fb71f-145">コードは次の例のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="fb71f-146">同様に変更を*Student.cs*です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="fb71f-147">`Student`クラスは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="fb71f-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="fb71f-148">Person エンティティ型をモデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="fb71f-149">*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。</span><span class="sxs-lookup"><span data-stu-id="fb71f-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="fb71f-150">Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="fb71f-151">わかる、データベースが再作成された場合、`Person`の代わりにテーブル、`Student`と`Instructor`テーブル。</span><span class="sxs-lookup"><span data-stu-id="fb71f-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="fb71f-152">PersonID に InstructorID と学生 Id を変更します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="fb71f-153">*SchoolContext.cs*、ステートメントでは、インストラクター コース マッピング変更`MapRightKey("InstructorID")`に`MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="fb71f-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="fb71f-154">この変更は必要はありません。多対多の結合テーブル内の InstructorID 列の名前を変更するだけです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="fb71f-155">InstructorID と名前のままの場合、アプリケーションが正しく機能もします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="fb71f-156">ここでは、完成した*SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb71f-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="fb71f-157">次を変更する必要があります`InstructorID`に`PersonID`と`StudentID`に`PersonID`プロジェクト全体にわたって***を除く***内の移行のタイムスタンプ付きのファイルで、*移行*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="fb71f-158">これを行うにするを検索し、開きますを変更する必要のあるファイルのみファイルが開かれてでグローバル変更を実行します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="fb71f-159">唯一のファイル、*移行*フォルダーを変更する必要がありますが*Migrations\Configuration.cs です。*</span><span class="sxs-lookup"><span data-stu-id="fb71f-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="fb71f-160">Visual Studio で開いているすべてのファイルを終了して作業を開始します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="fb71f-161">をクリックして**検索し置換--すべてのファイルを検索する**で、**編集**メニューのおよびを含む、プロジェクト内のすべてのファイルを検索`InstructorID`です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="fb71f-162">内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_.cs* で移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="fb71f-163">開く、**ファイル内の置換**ダイアログと変更**ファイルの場所**に**すべての開いているドキュメント**です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="fb71f-164">使用して、**ファイル内の置換**すべて変更するためのダイアログ`InstructorID`に `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="fb71f-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="fb71f-165">プロジェクトが含まれているすべてのファイルを検索`StudentID`です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="fb71f-166">内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_\*.cs*移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="fb71f-167">開く、**ファイル内の置換**ダイアログと変更**ファイルの場所**に**すべての開いているドキュメント**です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="fb71f-168">使用して、**ファイル内の置換**すべて変更するためのダイアログ`StudentID`に`PersonID`です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="fb71f-169">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-169">Build the project.</span></span>

<span data-ttu-id="fb71f-170">(これを示す注、*欠点*の`classnameID`主キーの名前付けパターン。</span><span class="sxs-lookup"><span data-stu-id="fb71f-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="fb71f-171">クラスの名前を付けることがなく主キー ID の名前を付けていた場合*ありません*名前を変更する必要があるようになりました)。</span><span class="sxs-lookup"><span data-stu-id="fb71f-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="fb71f-172">作成し、移行ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="fb71f-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="fb71f-173">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="fb71f-174">実行、 `Update-Database` PMC コマンド。</span><span class="sxs-lookup"><span data-stu-id="fb71f-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="fb71f-175">コマンドは、既存のデータ移行を処理する方法を知らないにしているために、この時点で失敗します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="fb71f-176">次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-176">You get the following error:</span></span>

<span data-ttu-id="fb71f-177">*ALTER TABLE ステートメントは、FOREIGN KEY 制約と競合して"FK\_dbo します。部門\_dbo します。Person\_PersonID"です。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。人"、列 'PersonID' です。*</span><span class="sxs-lookup"><span data-stu-id="fb71f-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="fb71f-178">開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="fb71f-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="fb71f-179">実行、`update-database`もう一度コマンドします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="fb71f-180">データとスキーマの変更を移行するときにその他のエラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="fb71f-181">移行エラーが発生する場合は解決できない場合、内の接続文字列を変更することで、チュートリアルを続行することができます、 *Web.config*ファイルまたはデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="fb71f-182">最も簡単な方法が内のデータベースの名前を変更するには、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="fb71f-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="fb71f-183">たとえば、CU にデータベース名を変更\_の次の例に示すようにテストします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="fb71f-184">新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="fb71f-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="fb71f-185">データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="fb71f-186">チュートリアルを続行するためにこの方法を採用する場合は、移行を自動的に実行時に同じエラーが発生は、配置サイトから、このチュートリアルの最後の配置手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="fb71f-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="fb71f-187">移行エラーのトラブルシューティングする場合に最適なリソースは Entity Framework のフォーラムや StackOverflow.com のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="fb71f-188">テスト中</span><span class="sxs-lookup"><span data-stu-id="fb71f-188">Testing</span></span>

<span data-ttu-id="fb71f-189">サイトを実行して、さまざまなページを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="fb71f-189">Run the site and try various pages.</span></span> <span data-ttu-id="fb71f-190">すべてが前と同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="fb71f-191">**サーバー エクスプ ローラーで、** 展開**SchoolContext**し**テーブル**、されたことを確認し、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。</span><span class="sxs-lookup"><span data-stu-id="fb71f-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="fb71f-192">展開、 **Person**テーブルとしているときに存在するために使用される列のすべて、**学生**と**インストラクター**テーブル。</span><span class="sxs-lookup"><span data-stu-id="fb71f-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="fb71f-194">Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。</span><span class="sxs-lookup"><span data-stu-id="fb71f-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="fb71f-195">次の図は、新しい School データベースの構造を示しています。</span><span class="sxs-lookup"><span data-stu-id="fb71f-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="fb71f-197">まとめ</span><span class="sxs-lookup"><span data-stu-id="fb71f-197">Summary</span></span>

<span data-ttu-id="fb71f-198">テーブルの階層あたりの継承が実装されたようになりました、 `Person`、 `Student`、および`Instructor`クラスです。</span><span class="sxs-lookup"><span data-stu-id="fb71f-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="fb71f-199">このおよびその他の継承構造の詳細については、次を参照してください。[継承のマッピング方法](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi ブログ。</span><span class="sxs-lookup"><span data-stu-id="fb71f-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="fb71f-200">次のチュートリアルでは、リポジトリと作業パターンの単位を実装するいくつかの方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fb71f-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="fb71f-201">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="fb71f-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb71f-202">[前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="fb71f-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
