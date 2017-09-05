---
title: "ASP.NET MVC を持つコアを EF コア - 継承 - 9 10"
author: tdykstra
description: "このチュートリアルでは、ASP.NET Core アプリケーションに Entity Framework のコアを使用して、データ モデル内の継承を実装する方法を示します。"
keywords: "ASP.NET Core、Entity Framework のコアを継承"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 3c86dea145d2d4dec10c77e008f511cfe67975f9
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="efe89-104">継承の ASP.NET Core MVC のチュートリアル (10 の 9) と EF コア</span><span class="sxs-lookup"><span data-stu-id="efe89-104">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="efe89-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="efe89-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efe89-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="efe89-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="efe89-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="efe89-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="efe89-108">前のチュートリアルでは、同時実行例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="efe89-108">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="efe89-109">このチュートリアルでは、データ モデルでの継承を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="efe89-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="efe89-110">オブジェクト指向プログラミングでは、コードの再利用を容易に継承を使用できます。</span><span class="sxs-lookup"><span data-stu-id="efe89-110">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="efe89-111">このチュートリアルでは、変更、`Instructor`と`Student`クラスから派生するように、`Person`基底クラスなどのプロパティを含む`LastName`講習においてインストラクターと受講者の両方に共通であります。</span><span class="sxs-lookup"><span data-stu-id="efe89-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="efe89-112">されませんを追加または web ページは変更は、コードの一部を変更しますが、し、それらの変更がデータベースに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="efe89-113">継承をデータベース テーブルにマップするためのオプション</span><span class="sxs-lookup"><span data-stu-id="efe89-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="efe89-114">`Instructor`と`Student`School データ モデル内のクラスと同じではいくつかのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="efe89-114">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Student とインストラクター クラス](inheritance/_static/no-inheritance.png)

<span data-ttu-id="efe89-116">共有されるプロパティを冗長なコードを削除すると、`Instructor`と`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="efe89-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="efe89-117">または、インストラクターまたは受講者から名前が付属しているかどうかをかけることがなく名を書式設定できるサービスを記述します。</span><span class="sxs-lookup"><span data-stu-id="efe89-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="efe89-118">作成することが、`Person`基底クラスのプロパティ、その共有だけが含まれているし、`Instructor`と`Student`クラスは、次の図に示すように、その基底クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="efe89-118">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![ユーザー クラスから派生する student とインストラクター クラス](inheritance/_static/inheritance.png)

<span data-ttu-id="efe89-120">これには、データベースでこの継承構造を表すことができますいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="efe89-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="efe89-121">Person テーブル受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むことができます。</span><span class="sxs-lookup"><span data-stu-id="efe89-121">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="efe89-122">一部の列は、インストラクター (HireDate)、受講者 (EnrollmentDate) 両方 (姓、名) にいくつかにのみ一部にのみ適用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="efe89-122">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="efe89-123">通常、識別子の列を各行を表す種類を示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe89-123">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="efe89-124">たとえば、識別子の列は、受講者のインストラクターと「生徒」「インストラクター」があります。</span><span class="sxs-lookup"><span data-stu-id="efe89-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![テーブルの階層あたりの例](inheritance/_static/tph.png)

<span data-ttu-id="efe89-126">このパターンの 1 つのデータベース テーブルからエンティティ継承構造を生成するには、テーブルの階層あたり (TPH) の継承が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-126">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="efe89-127">代わりに、継承構造と同じように、データベースの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="efe89-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="efe89-128">たとえば、Person テーブルの名前フィールドしかありませんでした、日付フィールドを持つ別のインストラクターと Student テーブルを持つとします。</span><span class="sxs-lookup"><span data-stu-id="efe89-128">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Table-Per-Type 継承](inheritance/_static/tpt.png)

<span data-ttu-id="efe89-130">このエンティティ クラスごとにデータベース テーブルのパターンは、型 (TPT) の継承ごとにテーブルと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="efe89-130">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="efe89-131">まだ他のオプションは、個々 のテーブルにすべての非抽象型をマップするです。</span><span class="sxs-lookup"><span data-stu-id="efe89-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="efe89-132">継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップします。</span><span class="sxs-lookup"><span data-stu-id="efe89-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="efe89-133">このパターンは、具象型でのテーブルごとのクラス (TPC) の継承と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="efe89-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="efe89-134">前に示したように、ユーザー、学生、およびインストラクター クラスの継承を TPC が実装されている場合、Student テーブルとインストラクター テーブルはよりも長く前に、継承の実装後にまったく同じなります。</span><span class="sxs-lookup"><span data-stu-id="efe89-134">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="efe89-135">TPC と TPH の継承パターンは、TPT パターンが複雑な結合クエリのため通常 TPT 継承のパターンよりも優れたパフォーマンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="efe89-135">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="efe89-136">このチュートリアルでは、TPH 継承の実装方法を示します。</span><span class="sxs-lookup"><span data-stu-id="efe89-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="efe89-137">TPH は、Entity Framework のコアをサポートする唯一の継承パターンです。</span><span class="sxs-lookup"><span data-stu-id="efe89-137">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="efe89-138">作成を行いますが、`Person`クラス、変更、`Instructor`と`Student`クラスから派生する`Person`、新しいクラスを追加、 `DbContext`、し、移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="efe89-138">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="efe89-139">次の変更を加える前に、プロジェクトのコピーを保存することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="efe89-139">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="efe89-140">問題と最初からやり直す必要性に発生した場合、一連の先頭にこのチュートリアルの完了手順を反転することや継続的ではなく保存されているプロジェクトから開始しやすくがされます。</span><span class="sxs-lookup"><span data-stu-id="efe89-140">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="efe89-141">ユーザー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="efe89-141">Create the Person class</span></span>

<span data-ttu-id="efe89-142">Models フォルダーに Person.cs を作成し、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="efe89-142">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

<span data-ttu-id="efe89-143">[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]</span><span class="sxs-lookup"><span data-stu-id="efe89-143">[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]</span></span>

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="efe89-144">ユーザーから継承 Student とインストラクター クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="efe89-144">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="efe89-145">*Instructor.cs*Person クラスから派生して、インストラクター クラス、およびキーと名前のフィールドを削除します。</span><span class="sxs-lookup"><span data-stu-id="efe89-145">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="efe89-146">コードは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="efe89-146">The code will look like the following example:</span></span>

<span data-ttu-id="efe89-147">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="efe89-147">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]</span></span>

<span data-ttu-id="efe89-148">同じ変更を加え*Student.cs*です。</span><span class="sxs-lookup"><span data-stu-id="efe89-148">Make the same changes in *Student.cs*.</span></span>

<span data-ttu-id="efe89-149">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="efe89-149">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]</span></span>

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="efe89-150">Person エンティティ型をデータ モデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="efe89-150">Add the Person entity type to the data model</span></span>

<span data-ttu-id="efe89-151">ユーザーのエンティティ型を追加*SchoolContext.cs*です。</span><span class="sxs-lookup"><span data-stu-id="efe89-151">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="efe89-152">新しい行が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-152">The new lines are highlighted.</span></span>

<span data-ttu-id="efe89-153">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]</span><span class="sxs-lookup"><span data-stu-id="efe89-153">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]</span></span>

<span data-ttu-id="efe89-154">これは、Entity Framework は、テーブルの階層あたりの継承を構成するために必要なです。</span><span class="sxs-lookup"><span data-stu-id="efe89-154">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="efe89-155">表示されます、データベースが更新されたときに、Student テーブルとインストラクター テーブルの代わりに、Person テーブルがあります。</span><span class="sxs-lookup"><span data-stu-id="efe89-155">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="efe89-156">作成し、移行のコードをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="efe89-156">Create and customize migration code</span></span>

<span data-ttu-id="efe89-157">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="efe89-157">Save your changes and build the project.</span></span> <span data-ttu-id="efe89-158">プロジェクト フォルダー内のコマンド ウィンドウを開くし、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="efe89-158">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="efe89-159">実行しない、`database update`まだコマンドします。</span><span class="sxs-lookup"><span data-stu-id="efe89-159">Don't run the `database update` command yet.</span></span> <span data-ttu-id="efe89-160">インストラクター テーブルを削除し、ユーザーに、Student テーブルの名前を変更ことはために、そのコマンドはデータの損失になります。</span><span class="sxs-lookup"><span data-stu-id="efe89-160">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="efe89-161">既存のデータを保持するためにカスタム コードを提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe89-161">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="efe89-162">開いている*移行/\<タイムスタンプ > _Inheritance.cs*と置換、`Up`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="efe89-162">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

<span data-ttu-id="efe89-163">[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]</span><span class="sxs-lookup"><span data-stu-id="efe89-163">[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]</span></span>

<span data-ttu-id="efe89-164">このコードは、次のデータベースの更新タスクの処理します。</span><span class="sxs-lookup"><span data-stu-id="efe89-164">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="efe89-165">外部キー制約と Student テーブルをポイントするインデックスを削除します。</span><span class="sxs-lookup"><span data-stu-id="efe89-165">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="efe89-166">個人として教師テーブルの名前を変更し、受講者用のデータを格納するために必要な変更を行います。</span><span class="sxs-lookup"><span data-stu-id="efe89-166">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="efe89-167">受講者の null 許容 EnrollmentDate を追加します。</span><span class="sxs-lookup"><span data-stu-id="efe89-167">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="efe89-168">行が、受講者または講師がかどうかを示すために識別子列を追加します。</span><span class="sxs-lookup"><span data-stu-id="efe89-168">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="efe89-169">により HireDate null 許容ため学生行は、雇用日を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="efe89-169">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="efe89-170">受講者をポイントする外部キーの更新に使用される一時的なフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="efe89-170">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="efe89-171">Person テーブルに受講者をコピーするときに、新しい主キー値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-171">When you copy students into the Person table they'll get new primary key values.</span></span>

* <span data-ttu-id="efe89-172">Person テーブルに、Student テーブルからデータをコピーします。</span><span class="sxs-lookup"><span data-stu-id="efe89-172">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="efe89-173">これにより、受講者が割り当てられている新しい主キー値を取得します。</span><span class="sxs-lookup"><span data-stu-id="efe89-173">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="efe89-174">受講者をポイントする外部キー値を修正します。</span><span class="sxs-lookup"><span data-stu-id="efe89-174">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="efe89-175">外部キー制約と今すぐ Person テーブルをポイントして、インデックスを再作成されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-175">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="efe89-176">(学生主キーの値を変更する必要はない場合は、プライマリ キーの種類として、整数ではなく GUID を使用した、および省略されていることができいくつかの手順です。)</span><span class="sxs-lookup"><span data-stu-id="efe89-176">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="efe89-177">実行、`database update`コマンド。</span><span class="sxs-lookup"><span data-stu-id="efe89-177">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="efe89-178">(実稼働システムで対応する変更を行い、`Down`メソッドの場合も、以前のデータベース バージョンに戻るに使用しなければならなかったことです。</span><span class="sxs-lookup"><span data-stu-id="efe89-178">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="efe89-179">このチュートリアルでは使用しない、`Down`メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="efe89-179">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="efe89-180">既存のデータを含むデータベースでスキーマ変更を行うときにその他のエラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="efe89-180">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="efe89-181">解決できない場合は移行エラーが発生した場合か、接続文字列でデータベース名を変更したり、データベースを削除できます。</span><span class="sxs-lookup"><span data-stu-id="efe89-181">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="efe89-182">新しいデータベースを移行するデータが存在しないと、データベースの更新コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="efe89-182">With a new database, there is no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="efe89-183">データベースを削除する SSOX を使用してまたはを実行、 `database drop` CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="efe89-183">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="efe89-184">継承の実装をテストします。</span><span class="sxs-lookup"><span data-stu-id="efe89-184">Test with inheritance implemented</span></span>

<span data-ttu-id="efe89-185">サイトを実行して、さまざまなページを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="efe89-185">Run the site and try various pages.</span></span> <span data-ttu-id="efe89-186">前に、と同じに動作します。</span><span class="sxs-lookup"><span data-stu-id="efe89-186">Everything works the same as it did before.</span></span>

<span data-ttu-id="efe89-187">**SQL Server オブジェクト エクスプ ローラー**、展開**データ接続/SchoolContext**し**テーブル**、Student テーブルとインストラクター テーブルに置換されたことを確認して、Person テーブル。</span><span class="sxs-lookup"><span data-stu-id="efe89-187">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="efe89-188">Person テーブル デザイナーを開くし、Student テーブルとインストラクター テーブルに存在するために使用される列のすべてがあるを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efe89-188">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![SSOX で person テーブル](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="efe89-190">Person テーブルを右クリックし、をクリックして**テーブル データの表示**識別子列を表示します。</span><span class="sxs-lookup"><span data-stu-id="efe89-190">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![SSOX - テーブルのデータで person テーブル](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="efe89-192">概要</span><span class="sxs-lookup"><span data-stu-id="efe89-192">Summary</span></span>

<span data-ttu-id="efe89-193">テーブルの階層あたりの継承を実装したら、 `Person`、 `Student`、および`Instructor`クラスです。</span><span class="sxs-lookup"><span data-stu-id="efe89-193">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="efe89-194">Entity Framework Core での継承の詳細については、次を参照してください。[継承](https://docs.microsoft.com/ef/core/modeling/inheritance)です。</span><span class="sxs-lookup"><span data-stu-id="efe89-194">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="efe89-195">次のチュートリアルでは、さまざまな Entity Framework の比較的高度なシナリオを処理する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efe89-195">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="efe89-196">[前へ](concurrency.md)
[次へ](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="efe89-196">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
