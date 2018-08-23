---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC 5 アプリケーション (12 の 11) で Entity Framework 6 による継承の実装 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9bfe860761a18cb4a69d33336d64a169e3c834f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827855"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a><span data-ttu-id="93394-103">ASP.NET MVC 5 アプリケーション (12 の 11) で Entity Framework 6 による継承の実装</span><span class="sxs-lookup"><span data-stu-id="93394-103">Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application (11 of 12)</span></span>
====================
<span data-ttu-id="93394-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="93394-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="93394-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="93394-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="93394-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="93394-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="93394-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="93394-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="93394-108">前のチュートリアルでは、同時実行例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="93394-108">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="93394-109">このチュートリアルでは、データ モデルで継承を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="93394-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="93394-110">オブジェクト指向のプログラミングで使用できます[継承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))を容易にする[コードの再利用](http://en.wikipedia.org/wiki/Code_reuse)します。</span><span class="sxs-lookup"><span data-stu-id="93394-110">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="93394-111">このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。</span><span class="sxs-lookup"><span data-stu-id="93394-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="93394-112">どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="93394-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="93394-113">継承をデータベース テーブルにマップするためのオプション</span><span class="sxs-lookup"><span data-stu-id="93394-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="93394-114">`Instructor`と`Student`クラス、`School`データ モデルが同一であるいくつかのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="93394-114">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="93394-116">`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。</span><span class="sxs-lookup"><span data-stu-id="93394-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="93394-117">または、インストラクターと学生のどちらから名前を取得したかに関係なく、名前をフォーマットできるサービスを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="93394-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="93394-118">作成することが、`Person`基本クラスがそれらの共有プロパティだけが含まれているようにして、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="93394-118">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="93394-120">データベースでこの継承構造を表すことができるいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="93394-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="93394-121">割り当てることも、`Person`受講者とインストラクターによる 1 つのテーブルの両方に関する情報が含まれるテーブル。</span><span class="sxs-lookup"><span data-stu-id="93394-121">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="93394-122">インストラクターのみに適用の一部の列 (`HireDate`)、受講者のみに (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="93394-122">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="93394-123">通常があります、*識別子*各行を入力するかを示す列を表します。</span><span class="sxs-lookup"><span data-stu-id="93394-123">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="93394-124">たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="93394-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![テーブル-hierarchy_example ごと](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="93394-126">単一のデータベース テーブルからエンティティの継承構造を生成するには、このパターンと呼ばれます *- table-per-hierarchy* (TPH) 継承します。</span><span class="sxs-lookup"><span data-stu-id="93394-126">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="93394-127">代わりに、継承構造と同じように見えるデータベースを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="93394-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="93394-128">など名前フィールドのみがある可能性があります、`Person`テーブルいて個別`Instructor`と`Student`日付フィールドを持つテーブル。</span><span class="sxs-lookup"><span data-stu-id="93394-128">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="93394-130">各エンティティ クラスと呼ばれる、データベース テーブルを作成するこのパターン*table-per-type* (TPT) 継承します。</span><span class="sxs-lookup"><span data-stu-id="93394-130">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="93394-131">他のオプションとして、個々のテーブルにすべての非抽象型をマップすることもできます。</span><span class="sxs-lookup"><span data-stu-id="93394-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="93394-132">継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="93394-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="93394-133">このパターンは、Table-per-Concrete Class (TPC) 継承と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="93394-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="93394-134">TPC 継承を実装する場合、 `Person`、 `Student`、および`Instructor`クラス、前述のように、`Student`と`Instructor`継承を実装する前にテーブルに同じなります。</span><span class="sxs-lookup"><span data-stu-id="93394-134">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="93394-135">TPC および TPH 継承パターンは、TPT パターンが複雑な結合クエリのため、TPT 継承パターンよりも、Entity Framework で高いパフォーマンスを実現一般にします。</span><span class="sxs-lookup"><span data-stu-id="93394-135">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="93394-136">このチュートリアルでは、TPH 継承の実装方法を示します。</span><span class="sxs-lookup"><span data-stu-id="93394-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="93394-137">TPH は、Entity Framework で既定の継承パターンの作成は行う必要があるすべてを`Person`クラス、変更、`Instructor`と`Student`クラスから派生する`Person`、新しいクラスを追加、`DbContext`を作成し、移行します。</span><span class="sxs-lookup"><span data-stu-id="93394-137">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="93394-138">(その他の継承パターンを実装する方法については、次を参照してください[Table-type ごと (TPT) 継承のマッピング](https://msdn.microsoft.com/data/jj591617#2.5)と[テーブル-ごとの具象クラス (TPC) 継承のマッピング](https://msdn.microsoft.com/data/jj591617#2.6)msdn。Entity Framework ドキュメントです。)</span><span class="sxs-lookup"><span data-stu-id="93394-138">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="93394-139">Person クラスの作成</span><span class="sxs-lookup"><span data-stu-id="93394-139">Create the Person class</span></span>

<span data-ttu-id="93394-140">*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="93394-140">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="93394-141">Person クラスから Student クラスと Instructor クラスを派生させる</span><span class="sxs-lookup"><span data-stu-id="93394-141">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="93394-142">*Instructor.cs*、派生、`Instructor`クラスから、`Person`クラスし、キーと名前のフィールドを削除します。</span><span class="sxs-lookup"><span data-stu-id="93394-142">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="93394-143">コードは次の例のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="93394-143">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="93394-144">同様に変更する*Student.cs*します。</span><span class="sxs-lookup"><span data-stu-id="93394-144">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="93394-145">`Student`クラスは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="93394-145">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a><span data-ttu-id="93394-146">Person エンティティ型をモデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="93394-146">Add the Person Entity Type to the Model</span></span>

<span data-ttu-id="93394-147">*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。</span><span class="sxs-lookup"><span data-stu-id="93394-147">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="93394-148">Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。</span><span class="sxs-lookup"><span data-stu-id="93394-148">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="93394-149">わかる、データベースが更新されたとき、`Person`テーブルの代わりに、`Student`と`Instructor`テーブル。</span><span class="sxs-lookup"><span data-stu-id="93394-149">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="93394-150">作成し、移行ファイルを更新</span><span class="sxs-lookup"><span data-stu-id="93394-150">Create and Update a Migrations File</span></span>

<span data-ttu-id="93394-151">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="93394-151">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="93394-152">実行、 `Update-Database` PMC でコマンド。</span><span class="sxs-lookup"><span data-stu-id="93394-152">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="93394-153">移行が処理する方法を認識しない既存のデータがあるので、コマンドはこの時点で失敗します。</span><span class="sxs-lookup"><span data-stu-id="93394-153">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="93394-154">次のようなエラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="93394-154">You get an error message like the following one:</span></span>

> <span data-ttu-id="93394-155">*オブジェクトを削除できませんでした ' dbo します。Instructor' FOREIGN KEY 制約によって参照されているためです。*</span><span class="sxs-lookup"><span data-stu-id="93394-155">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="93394-156">開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="93394-156">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="93394-157">このコードは、次のデータベースの更新タスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="93394-157">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="93394-158">外部キー制約と Student テーブルをポイントするインデックスを削除します。</span><span class="sxs-lookup"><span data-stu-id="93394-158">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="93394-159">Instructor テーブルの名前の Person に変更し、Student データを格納するために必要な変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="93394-159">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="93394-160">受講者の null 許容 EnrollmentDate を追加します。</span><span class="sxs-lookup"><span data-stu-id="93394-160">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="93394-161">行が、受講者かインストラクターかを示すために識別子列を追加します。</span><span class="sxs-lookup"><span data-stu-id="93394-161">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="93394-162">受講者行には雇用日がないので HireDate を nul 許容にします。</span><span class="sxs-lookup"><span data-stu-id="93394-162">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="93394-163">受講者をポイントする外部キーの更新に使用する一時的なフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="93394-163">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="93394-164">Person テーブルに受講者をコピーする場合は、新しい主キー値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="93394-164">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="93394-165">Student テーブルから Person テーブルにデータをコピーします。</span><span class="sxs-lookup"><span data-stu-id="93394-165">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="93394-166">これにより、受講者に新しい主キー値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="93394-166">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="93394-167">受講者をポイントする外部キー値を修正します。</span><span class="sxs-lookup"><span data-stu-id="93394-167">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="93394-168">今は Person テーブルをポイントしている外部キー制約とインデックスを再作成します </span><span class="sxs-lookup"><span data-stu-id="93394-168">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="93394-169">(主キーの型として整数の代わりに GUID を使用した場合は、受講者の主キー値を変更する必要はなく、これらの手順のいくつかを省略できます)。</span><span class="sxs-lookup"><span data-stu-id="93394-169">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="93394-170">実行、`update-database`コマンドを再実行します。</span><span class="sxs-lookup"><span data-stu-id="93394-170">Run the `update-database` command again.</span></span>

<span data-ttu-id="93394-171">(実稼働システムでは対応に変更を加えるダウン メソッドを使用してデータベースの以前のバージョンに戻ることがある発生した場合。</span><span class="sxs-lookup"><span data-stu-id="93394-171">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="93394-172">このチュートリアルではありませんする方法を使用してダウンします。)</span><span class="sxs-lookup"><span data-stu-id="93394-172">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="93394-173">データとスキーマの変更を移行するときにその他のエラーが発生することになります。</span><span class="sxs-lookup"><span data-stu-id="93394-173">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="93394-174">移行エラーが発生する場合は解決できない場合、接続文字列を変更することで、チュートリアルを続けることができます、 *Web.config*ファイルまたはでデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="93394-174">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="93394-175">データベースの名前を変更する最も簡単なアプローチとしては、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="93394-175">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="93394-176">たとえば、次の例に示すようを ContosoUniversity2 にデータベース名を変更します。</span><span class="sxs-lookup"><span data-stu-id="93394-176">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> <span data-ttu-id="93394-177">新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="93394-177">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="93394-178">データベースを削除する方法の詳細については、次を参照してください。 [Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)します。</span><span class="sxs-lookup"><span data-stu-id="93394-178">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="93394-179">チュートリアルを続行するにはこの方法を実行する場合は、このチュートリアルの最後に、展開の手順をスキップまたは新しいサイトとデータベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="93394-179">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="93394-180">既にするデプロイされたしたのと同じサイトに更新プログラムを展開する場合の移行を自動的に実行時に EF は、同じエラーがありますを取得します。</span><span class="sxs-lookup"><span data-stu-id="93394-180">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="93394-181">移行エラーのトラブルシューティングを行う場合は、最適なリソースは、Entity Framework のフォーラムまたは StackOverflow.com のいずれか。</span><span class="sxs-lookup"><span data-stu-id="93394-181">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="93394-182">テスト</span><span class="sxs-lookup"><span data-stu-id="93394-182">Testing</span></span>

<span data-ttu-id="93394-183">サイトを実行し、さまざまなページをお試しください。</span><span class="sxs-lookup"><span data-stu-id="93394-183">Run the site and try various pages.</span></span> <span data-ttu-id="93394-184">すべてが前と同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="93394-184">Everything works the same as it did before.</span></span>

<span data-ttu-id="93394-185">**サーバー エクスプ ローラー**展開**データ Connections\SchoolContext**し**テーブル**、ことを確認して、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。</span><span class="sxs-lookup"><span data-stu-id="93394-185">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="93394-186">展開、 **Person**テーブルとするすべてに使用されていた列があるを参照してください、**学生**と**インストラクター**テーブル。</span><span class="sxs-lookup"><span data-stu-id="93394-186">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="93394-188">Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。</span><span class="sxs-lookup"><span data-stu-id="93394-188">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="93394-189">次の図は、新しい School データベースの構造を示しています。</span><span class="sxs-lookup"><span data-stu-id="93394-189">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="93394-191">Azure に配置する</span><span class="sxs-lookup"><span data-stu-id="93394-191">Deploy to Azure</span></span>

<span data-ttu-id="93394-192">このセクションでは、省略可能なが完了する必要があります**アプリを Azure にデプロイ**セクション[パート 3、並べ替え、フィルター処理、およびページング](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)このチュートリアル シリーズの。</span><span class="sxs-lookup"><span data-stu-id="93394-192">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="93394-193">ローカル プロジェクトでデータベースを削除することによって解決された移行エラーがある場合は、この手順では; をスキップします。または、新しいサイトとデータベースを作成し、新しい環境にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="93394-193">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="93394-194">Visual Studio でのプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択と**発行**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="93394-194">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![プロジェクトのコンテキスト メニューを発行します。](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. <span data-ttu-id="93394-196">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="93394-196">Click **Publish**.</span></span>  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   <span data-ttu-id="93394-198">Web アプリが既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="93394-198">The Web app will open in your default browser.</span></span>
3. <span data-ttu-id="93394-199">アプリケーションをテストして確認することが動作します。</span><span class="sxs-lookup"><span data-stu-id="93394-199">Test the application to verify it's working.</span></span>

    <span data-ttu-id="93394-200">初めてページを実行するデータベースにアクセスする、Entity Framework で実行されるすべての移行`Up`データベースの現在のデータ モデルで最新の状態に必要なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="93394-200">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="summary"></a><span data-ttu-id="93394-201">まとめ</span><span class="sxs-lookup"><span data-stu-id="93394-201">Summary</span></span>

<span data-ttu-id="93394-202">`Person`、`Student`、および `Instructor` クラスの Table-per-Hierarchy 継承を実装しました。</span><span class="sxs-lookup"><span data-stu-id="93394-202">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="93394-203">これと他の継承構造の詳細については、次を参照してください。 [TPT 継承パターン](https://msdn.microsoft.com/data/jj618293)と[TPH 継承パターン](https://msdn.microsoft.com/data/jj618292)msdn です。</span><span class="sxs-lookup"><span data-stu-id="93394-203">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="93394-204">次のチュートリアルでは、比較的高度なさまざまな Entity Framework のシナリオを処理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="93394-204">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

<span data-ttu-id="93394-205">その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="93394-205">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="93394-206">[前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="93394-206">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
