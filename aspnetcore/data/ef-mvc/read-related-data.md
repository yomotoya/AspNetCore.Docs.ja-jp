---
title: "ASP.NET MVC を持つコアを EF Core - 読み取り関連データ - 10 の 6"
author: tdykstra
description: "このチュートリアルでは、読み取りを関連データ--Entity Framework は、ナビゲーション プロパティに読み込まれるデータを表示します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 2333ac70c77847ece1f90c9ff22eec30bc35fea1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="10ce3-103">読み取りに関連したデータの ASP.NET Core MVC のチュートリアル (10 の 6) の EF コア</span><span class="sxs-lookup"><span data-stu-id="10ce3-103">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="10ce3-104">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10ce3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="10ce3-105">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="10ce3-106">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="10ce3-107">前のチュートリアルでは、データの School モデルを完了しました。</span><span class="sxs-lookup"><span data-stu-id="10ce3-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="10ce3-108">このチュートリアルを読み取るし、関連データ--Entity Framework は、ナビゲーション プロパティに読み込まれるデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="10ce3-109">次の図で作業をしているページを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-109">The following illustrations show the pages that you'll work with.</span></span>

![インデックス ページのコース](read-related-data/_static/courses-index.png)

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="10ce3-112">Eager、明示的なおよび関連するデータの読み込み遅延</span><span class="sxs-lookup"><span data-stu-id="10ce3-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="10ce3-113">いくつかの方法をオブジェクト リレーショナル マッピング (ORM) ソフトウェアなどは Entity Framework は、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="10ce3-114">一括読み込みします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-114">Eager loading.</span></span> <span data-ttu-id="10ce3-115">エンティティが読み込まれると、それに伴う関連データを取得します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="10ce3-116">通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="10ce3-117">使用して Entity Framework のコアで一括読み込みを指定する、`Include`と`ThenInclude`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![一括読み込みの例](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="10ce3-119">一部の個別のクエリでデータを取得するおよび EF「修正」ナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="10ce3-120">つまり、EF は、以前に取得したエンティティのナビゲーション プロパティで属しているとは別に取得したエンティティを自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="10ce3-121">関連するデータを取得するクエリでは、使用することができます、`Load`メソッドなど、リストまたはオブジェクトを返すメソッドではなく`ToList`または`Single`です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![別のクエリ例](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="10ce3-123">明示的な読み込みします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-123">Explicit loading.</span></span> <span data-ttu-id="10ce3-124">エンティティが読み込まれると、関連データが取得されていません。</span><span class="sxs-lookup"><span data-stu-id="10ce3-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="10ce3-125">関連するデータを取得するコードを記述すると、必要なかどうか。</span><span class="sxs-lookup"><span data-stu-id="10ce3-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="10ce3-126">メソッドに別々 にクエリで一括読み込みの場合、複数のクエリ結果の読み込み明示的なデータベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="10ce3-127">違いは、明示的な読み込みでのコードが指定ロードするナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="10ce3-128">Entity Framework Core 1.1 で使用することができます、`Load`明示的な読み込みを行うメソッドです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="10ce3-129">例:</span><span class="sxs-lookup"><span data-stu-id="10ce3-129">For example:</span></span>

  ![明示的な読み込みの例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="10ce3-131">遅延読み込みします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-131">Lazy loading.</span></span> <span data-ttu-id="10ce3-132">エンティティが読み込まれると、関連データが取得されていません。</span><span class="sxs-lookup"><span data-stu-id="10ce3-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="10ce3-133">ただし、ナビゲーション プロパティにアクセスしようとすると、最初にそのナビゲーション プロパティに必要なデータが自動的に取得します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="10ce3-134">クエリは、最初にナビゲーション プロパティからデータを取得しようとするたびにデータベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="10ce3-135">Entity Framework Core 1.0 では、遅延読み込みをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="10ce3-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="10ce3-136">パフォーマンスに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="10ce3-136">Performance considerations</span></span>

<span data-ttu-id="10ce3-137">データベースに送信する 1 つのクエリが通常エンティティごとに取得される個別のクエリよりも効率的であるために、多くの場合、一括読み込み関連データが必要なすべてのエンティティを取得する場合に最高のパフォーマンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="10ce3-138">たとえば、部門ごとに 10 個の関連コースがあるとします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="10ce3-139">すべての関連データの一括読み込みになる単なる (結合) を 1 つのクエリと 1 つのラウンド トリップをデータベースにします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="10ce3-140">各部門のコースの個別のクエリをデータベースに 11 個のラウンド トリップになります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="10ce3-141">データベースへの余分なラウンド トリップは、遅延が多場合に特にパフォーマンスを低下させます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="10ce3-142">その一方で、一部のシナリオで別々 にクエリが効率的です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="10ce3-143">1 つのクエリ内のすべての関連するデータの一括読み込みには、非常に複雑な結合を生成するのには SQL Server を効率的に処理できない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="10ce3-144">または、エンティティのナビゲーション プロパティを処理しているエンティティのセットのサブセットについてのみにアクセスする必要がある場合に別々 にクエリ パフォーマンスが向上するため、事前にすべての一括読み込みする必要がありますよりも多くのデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="10ce3-145">パフォーマンスが重要な場合は、パフォーマンスを最適な選択を行うために両方の方法をテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="10ce3-146">部門名が表示されるコース ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="10ce3-147">Course エンティティには、コースに割り当てられている部署の部門エンティティを含むナビゲーション プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="10ce3-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="10ce3-148">コースの一覧で、割り当てられている部門の名前を表示するのには、内にある [部門] エンティティの Name プロパティを取得する必要があります、`Course.Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="10ce3-149">CoursesController と同じオプションを使用して、Course エンティティ型の名前が付いたコント ローラーを作成、 **Entity Framework を使用して、ビューがある MVC コント ローラー**以前に行っていた生徒コント ローラーのように scaffolder、次の図では:</span><span class="sxs-lookup"><span data-stu-id="10ce3-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![コースのコント ローラーを追加します。](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="10ce3-151">開いている*CoursesController.cs*を調べると、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="10ce3-152">自動のスキャフォールディングがの一括読み込みを指定、`Department`ナビゲーション プロパティを使用して、`Include`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="10ce3-153">置換、`Index`メソッドを次のコードをより適切な名前を使用する、`IQueryable`コース エンティティを返す (`courses`の代わりに`schoolContext`)。</span><span class="sxs-lookup"><span data-stu-id="10ce3-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="10ce3-154">開いている*Views/Courses/Index.cshtml*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="10ce3-155">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="10ce3-156">スキャフォールディングのコードに、次の変更を加えた。</span><span class="sxs-lookup"><span data-stu-id="10ce3-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="10ce3-157">インデックスからのコースに見出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="10ce3-158">追加、**数**を表示する列、`CourseID`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="10ce3-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="10ce3-159">既定では、主キーはいないだ通常エンドユーザーにとって意味のないスキャフォールディングされました。</span><span class="sxs-lookup"><span data-stu-id="10ce3-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="10ce3-160">ただし、ここでは、主キーは無効と非表示にします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="10ce3-161">変更、**部門**部門名を表示する列。</span><span class="sxs-lookup"><span data-stu-id="10ce3-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="10ce3-162">コードの表示、`Name`に読み込まれる部門 エンティティのプロパティ、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="10ce3-163">アプリの実行を選択して、**コース**部署名を使用してリストを表示するタブです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![インデックス ページのコース](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="10ce3-165">コースおよびまで登録数を示す講習においてインストラクター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="10ce3-166">このセクションでは、講習においてインストラクター ページを表示するためにコント ローラーと Instructor エンティティのビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

<span data-ttu-id="10ce3-168">このページは、読み取りし、次のように関連するデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="10ce3-169">講習においてインストラクターの一覧には、OfficeAssignment エンティティから関連するデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="10ce3-170">0 または 1 を 1 つのリレーションシップは、インストラクター OfficeAssignment エンティティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="10ce3-171">OfficeAssignment エンティティの一括読み込みを使用します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="10ce3-172">前述のように、主テーブルの取得したすべての行に、関連するデータが必要なときに一括読み込みは通常より効率的です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="10ce3-173">この場合、表示されているすべてのインストラクターのオフィスの割り当てを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="10ce3-174">ユーザーは、あるインストラクターを選択するときに関連する一連のエンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="10ce3-175">多対多のリレーションシップは、インストラクター コースのエンティティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="10ce3-176">Course エンティティとそれらに関連する部署エンティティの一括読み込みを使用します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="10ce3-177">この場合、講師が選択されているためだけのコースを必要なので、別のクエリがより効率的な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="10ce3-178">ただし、この例は、ナビゲーション プロパティではエンティティ内でナビゲーション プロパティの一括読み込みを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="10ce3-179">選択すると、ユーザー、コース、登録のエンティティ セットから関連するデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="10ce3-180">コースと登録のエンティティは、一対多リレーションシップでです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="10ce3-181">登録のエンティティとその関連学生エンティティには、別々 にクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="10ce3-182">講師インデックス ビューのビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="10ce3-183">講習においてインストラクター ページでは、次の 3 つの異なるテーブルからデータを示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="10ce3-184">そのため、3 つのプロパティを含むテーブルの 1 つのデータを保持している各ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="10ce3-185">*SchoolViewModels*フォルダー作成*InstructorIndexData.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="10ce3-186">講師コント ローラーとビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="10ce3-187">EF 読み取り/書き込みアクションの次の図に示すように、インストラクター コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![講習においてインストラクター コント ローラーを追加します。](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="10ce3-189">開いている*InstructorsController.cs*を使用して、追加し、ViewModels 名前空間のステートメント。</span><span class="sxs-lookup"><span data-stu-id="10ce3-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="10ce3-190">インデックスのメソッドを関連するデータの一括読み込みを行うし、ビュー モデルに格納するには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="10ce3-191">このメソッドは省略可能なルート データを受け取ります (`id`) およびクエリ文字列パラメーター (`courseID`)、選択した講師と選択したコースの ID 値を提供します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="10ce3-192">パラメーターがによって提供される、**選択**ハイパーリンクをページ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="10ce3-193">コードは、ビュー モデルのインスタンスを作成し、インストラクターの一覧に配置して開始します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="10ce3-194">コードでの一括読み込みを指定、`Instructor.OfficeAssignment`と`Instructor.CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="10ce3-195">内で、`CourseAssignments`プロパティ、`Course`プロパティは、読み込まれた場合は、その、`Enrollments`と`Department`プロパティは、アンロード、および各`Enrollment`エンティティ、`Student`プロパティが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="10ce3-196">ビューには、OfficeAssignment エンティティが常に必要とするので、同じクエリでフェッチをより効率的です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="10ce3-197">Course エンティティは、web ページにあるインストラクターが選択されているため、ページが表示されるより多くの場合、コースよりも選択されている場合にのみ、1 つのクエリは複数のクエリよりも高くときに必要があります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="10ce3-198">コードが繰り返されます`CourseAssignments`と`Course`から 2 つのプロパティが必要なので`Course`です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="10ce3-199">最初の文字列`ThenInclude`取得を呼び出して`CourseAssignment.Course`、 `Course.Enrollments`、および`Enrollment.Student`です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="10ce3-200">その時点でコードでは、別`ThenInclude`のナビゲーション プロパティでは`Student`、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="10ce3-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="10ce3-201">呼び出し元が`Include`経由で始まる`Instructor`を経由するチェーンここでも、この時間を指定する必要があるため、プロパティ`Course.Department`の代わりに`Course.Enrollments`です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="10ce3-202">次のコードは、あるインストラクターが選択されたときに実行します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="10ce3-203">講習においてインストラクターにビューのモデルの一覧から選択した講師が取得されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="10ce3-204">ビュー モデルの`Courses`そのインストラクターから Course エンティティとプロパティが読み込まれて、`CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="10ce3-205">`Where`メソッドのコレクションを返しますが、ここでは、条件は、そのメソッドの結果で返される単一インストラクター エンティティのみを渡されます。 します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="10ce3-206">`Single`メソッドに変換し、コレクションにそのエンティティのアクセスできる単一の Instructor エンティティ`CourseAssignments`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="10ce3-207">`CourseAssignments`プロパティが含まれます`CourseAssignment`をのみ関連エンティティ`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="10ce3-208">使用する、`Single`メソッドのコレクションがわかっている場合にコレクションを 1 つの項目になります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="10ce3-209">渡されるコレクションが空の場合、または複数の項目がある場合、1 つのメソッドは例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="10ce3-210">代わりに`SingleOrDefault`既定の値を返す (この場合は null) コレクションが空の場合。</span><span class="sxs-lookup"><span data-stu-id="10ce3-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="10ce3-211">ただし、ここではするがまだ例外が発生 (を検索しようとしてから、`Courses`プロパティは null 参照を)、例外メッセージは、問題の原因を示す明確に小さいとします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="10ce3-212">呼び出すと、`Single`メソッドを渡すこともできます、Where 句で条件を呼び出す代わりに、`Where`メソッドとは別に。</span><span class="sxs-lookup"><span data-stu-id="10ce3-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="10ce3-213">これは次のコードの代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="10ce3-214">次に、コースが選択されている場合は、選択したコースがビューのモデルでコースの一覧から取得されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="10ce3-215">モデルの表示の`Enrollments`コースから登録エンティティとプロパティが読み込まれる`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="10ce3-216">講師インデックス ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="10ce3-217">*Views/Instructors/Index.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="10ce3-218">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="10ce3-219">既存のコードに、次の変更を加えた。</span><span class="sxs-lookup"><span data-stu-id="10ce3-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="10ce3-220">モデル クラスを変更`InstructorIndexData`です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="10ce3-221">ページのタイトルを変更**インデックス**に**講習においてインストラクター**です。</span><span class="sxs-lookup"><span data-stu-id="10ce3-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="10ce3-222">追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、 `item.OfficeAssignment` null でないです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="10ce3-223">(これは、0 または 1 を 1 つのリレーションシップであるため、ある可能性がありますいない関連 OfficeAssignment エンティティです。)</span><span class="sxs-lookup"><span data-stu-id="10ce3-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="10ce3-224">追加、**コース**コースを表示する列が各インストラクターを学習します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="10ce3-225">参照してください[行の明示的な移行`@:`](xref:mvc/views/razor#explicit-line-transition-with-)この razor 構文の詳細についてです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="10ce3-226">動的に追加するコードを追加しました`class="success"`を`tr`選択した講師の要素。</span><span class="sxs-lookup"><span data-stu-id="10ce3-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="10ce3-227">これは、ブートス トラップのクラスを使用して、選択した行の背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="10ce3-228">新しいラベル付けされるハイパーリンクを追加**選択**各行の他のリンクの直前に選択したインストラクター ID への送信を停止する、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="10ce3-229">アプリの実行を選択して、**講習においてインストラクター**タブです。関連 OfficeAssignment エンティティが存在しない場合は、関連 OfficeAssignment エンティティと空のテーブル セルの Location プロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![講習においてインストラクター インデックス ページが何も選択](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="10ce3-231">*Views/Instructors/Index.cshtml* (末尾の要素ファイルの) の表に、終了した後のファイルは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="10ce3-232">このコードは、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="10ce3-233">このコードを読み取り、`Courses`コースの一覧を表示するビューのモデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="10ce3-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="10ce3-234">用意されています、**選択**ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="10ce3-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="10ce3-235">ページを更新し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="10ce3-236">これで、選択したインストラクターに割り当てられているコースを表示するグリッドが表示し、する各コースに割り当てられている部署の名前を参照してください。</span><span class="sxs-lookup"><span data-stu-id="10ce3-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![講習においてインストラクター インデックス ページ講師が選択されています。](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="10ce3-238">追加したコード ブロックの後に、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="10ce3-239">コースが選択されているときにコースに受講登録された者のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="10ce3-240">このコードは、受講者コースに登録済みの一覧を表示するために、ビュー モデルの登録プロパティを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="10ce3-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="10ce3-241">ページをもう一度更新し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="10ce3-242">登録済みの受講者との成績の一覧を表示するコースを選択します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![講習においてインストラクター インデックス ページ インストラクターと選択したコース](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="10ce3-244">明示的読み込み</span><span class="sxs-lookup"><span data-stu-id="10ce3-244">Explicit loading</span></span>

<span data-ttu-id="10ce3-245">講習においてインストラクターの一覧を取得した*InstructorsController.cs*の一括読み込みを指定した、`CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10ce3-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="10ce3-246">まれでは、選択した講師コースの「登録」を参照するユーザーを意図したとします。</span><span class="sxs-lookup"><span data-stu-id="10ce3-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="10ce3-247">その場合は、要求された場合にのみ登録データを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="10ce3-248">明示的な読み込みを行う方法の例を表示するには、置換、`Index`メソッドを次のコードの登録の一括読み込みを削除し、そのプロパティを明示的に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="10ce3-249">コードの変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="10ce3-249">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="10ce3-250">新しいコードを削除、 *ThenInclude*インストラクター エンティティを取得するコードから登録データのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="10ce3-251">インストラクターとコースが選択されている場合、強調表示されたコードは、選択したコースの登録のエンティティと各登録のための学生エンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="10ce3-252">今すぐ講習においてインストラクター インデックス ページに移動して、アプリが表示されますなし ページで、表示される内容に違いが、データの取得方法を変更した実行します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="10ce3-253">まとめ</span><span class="sxs-lookup"><span data-stu-id="10ce3-253">Summary</span></span>

<span data-ttu-id="10ce3-254">ここで使用した一括読み込み 1 つのクエリを使用して、複数のクエリを使用してナビゲーション プロパティに関連するデータを読み取る。</span><span class="sxs-lookup"><span data-stu-id="10ce3-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="10ce3-255">次のチュートリアルでは、関連するデータを更新する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="10ce3-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="10ce3-256">[前へ](complex-data-model.md)
>[次へ](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="10ce3-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
