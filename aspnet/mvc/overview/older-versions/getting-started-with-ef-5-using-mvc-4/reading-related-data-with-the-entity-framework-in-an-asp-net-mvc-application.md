---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 5) で、Entity Framework と関連するデータの読み取り |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9093fb90a52b297f173c5cddb6f332d2d1a25135
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a><span data-ttu-id="99002-103">関連する with ASP.NET MVC アプリケーション (10 の 5) で Entity Framework データの読み取り</span><span class="sxs-lookup"><span data-stu-id="99002-103">Reading Related Data with the Entity Framework in an ASP.NET MVC Application (5 of 10)</span></span>
====================
<span data-ttu-id="99002-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="99002-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="99002-105">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="99002-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="99002-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="99002-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="99002-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="99002-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="99002-108">一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="99002-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="99002-109">解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="99002-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="99002-110">一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。</span><span class="sxs-lookup"><span data-stu-id="99002-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="99002-111">一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="99002-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="99002-112">前のチュートリアルでは、データの School モデルを完了しました。</span><span class="sxs-lookup"><span data-stu-id="99002-112">In the previous tutorial you completed the School data model.</span></span> <span data-ttu-id="99002-113">このチュートリアルではあります読み取りおよび、関連するデータを表示: つまり、ナビゲーション プロパティに、Entity Framework が読み込まれるデータ。</span><span class="sxs-lookup"><span data-stu-id="99002-113">In this tutorial you'll read and display related data — that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="99002-114">次の図で作業をしているページを表示します。</span><span class="sxs-lookup"><span data-stu-id="99002-114">The following illustrations show the pages that you'll work with.</span></span>

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a><span data-ttu-id="99002-116">関連するデータの遅延、Eager、および明示的な読み込み</span><span class="sxs-lookup"><span data-stu-id="99002-116">Lazy, Eager, and Explicit Loading of Related Data</span></span>

<span data-ttu-id="99002-117">いくつかの方法が、Entity Framework が、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="99002-117">There are several ways that the Entity Framework can load related data into the navigation properties of an entity:</span></span>

- <span data-ttu-id="99002-118">*遅延読み込み*です。</span><span class="sxs-lookup"><span data-stu-id="99002-118">*Lazy loading*.</span></span> <span data-ttu-id="99002-119">エンティティが読み込まれると、関連データが取得されていません。</span><span class="sxs-lookup"><span data-stu-id="99002-119">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="99002-120">ただし、ナビゲーション プロパティにアクセスしようとすると、最初にそのナビゲーション プロパティに必要なデータが自動的に取得します。</span><span class="sxs-lookup"><span data-stu-id="99002-120">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="99002-121">これは、結果、データベースに送信される複数のクエリで — と 1 つ、エンティティ自体に関連したエンティティのデータを毎回を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="99002-121">This results in multiple queries sent to the database — one for the entity itself and one each time that related data for the entity must be retrieved.</span></span> 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- <span data-ttu-id="99002-123">*一括読み込み*です。</span><span class="sxs-lookup"><span data-stu-id="99002-123">*Eager loading*.</span></span> <span data-ttu-id="99002-124">エンティティが読み込まれると、それに伴う関連データを取得します。</span><span class="sxs-lookup"><span data-stu-id="99002-124">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="99002-125">通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。</span><span class="sxs-lookup"><span data-stu-id="99002-125">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="99002-126">一括読み込みを使用して指定する、`Include`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="99002-126">You specify eager loading by using the `Include` method.</span></span>

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- <span data-ttu-id="99002-128">*明示的な読み込み*です。</span><span class="sxs-lookup"><span data-stu-id="99002-128">*Explicit loading*.</span></span> <span data-ttu-id="99002-129">これは、似ていますが、遅延読み込みを明示的にデータを取得する、関連するコードです。ナビゲーション プロパティにアクセスするときに自動的に発生しません。</span><span class="sxs-lookup"><span data-stu-id="99002-129">This is similar to lazy loading, except that you explicitly retrieve the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="99002-130">エンティティと呼び出し元のオブジェクトの状態マネージャー エントリを取得することによって、関連するデータを手動で読み込む、`Collection.Load`のコレクションのメソッドまたは`Reference.Load`メソッドの 1 つのエンティティを保持するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="99002-130">You load related data manually by getting the object state manager entry for an entity and calling the `Collection.Load` method for collections or the `Reference.Load` method for properties that hold a single entity.</span></span> <span data-ttu-id="99002-131">(次の例では、管理者のナビゲーション プロパティをロードする場合は置き換えて`Collection(x => x.Courses)`で`Reference(x => x.Administrator)`)。</span><span class="sxs-lookup"><span data-stu-id="99002-131">(In the following example, if you wanted to load the Administrator navigation property, you'd replace `Collection(x => x.Courses)` with `Reference(x => x.Administrator)`.)</span></span>

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

<span data-ttu-id="99002-133">すぐに、プロパティの値を取得する、ので遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*です。</span><span class="sxs-lookup"><span data-stu-id="99002-133">Because they don't immediately retrieve the property values, lazy loading and explicit loading are also both known as *deferred loading*.</span></span>

<span data-ttu-id="99002-134">一般に、わかっている場合必要関連データのすべてのエンティティを取得した、一括読み込みでは、最高のパフォーマンスをデータベースに送信する 1 つのクエリが通常エンティティごとに取得される個別のクエリよりも効率的であるためです。</span><span class="sxs-lookup"><span data-stu-id="99002-134">In general, if you know you need related data for every entity retrieved, eager loading offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="99002-135">たとえば、前述の例では、各部門に 10 個の関連コースがあるとします。</span><span class="sxs-lookup"><span data-stu-id="99002-135">For example, in the above examples, suppose that each department has ten related courses.</span></span> <span data-ttu-id="99002-136">一括読み込みの例になる単なる (結合) を 1 つのクエリと 1 つのラウンド トリップをデータベースにします。</span><span class="sxs-lookup"><span data-stu-id="99002-136">The eager loading example would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="99002-137">遅延読み込みと明示的な読み込みの例が両方とも 11 個のクエリと 11 個のラウンド トリップでデータベースにします。</span><span class="sxs-lookup"><span data-stu-id="99002-137">The lazy loading and explicit loading examples would both result in eleven queries and eleven round trips to the database.</span></span> <span data-ttu-id="99002-138">データベースへの余分なラウンド トリップは、遅延が多場合に特にパフォーマンスを低下させます。</span><span class="sxs-lookup"><span data-stu-id="99002-138">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="99002-139">その一方で、一部のシナリオで遅延読み込みが効率的です。</span><span class="sxs-lookup"><span data-stu-id="99002-139">On the other hand, in some scenarios lazy loading is more efficient.</span></span> <span data-ttu-id="99002-140">一括読み込みには、非常に複雑な結合を生成するのには SQL Server を効率的に処理できない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="99002-140">Eager loading might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="99002-141">またはエンティティのセットのサブセットについてのみ、エンティティのナビゲーション プロパティにアクセスする必要がある場合を処理している場合、一括読み込みが必要以上のデータが取得されるため、遅延読み込みが向上します。</span><span class="sxs-lookup"><span data-stu-id="99002-141">Or if you need to access an entity's navigation properties only for a subset of a set of entities you're processing, lazy loading might perform better because eager loading would retrieve more data than you need.</span></span> <span data-ttu-id="99002-142">パフォーマンスが重要な場合は、パフォーマンスを最適な選択を行うために両方の方法をテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="99002-142">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

<span data-ttu-id="99002-143">通常、遅延読み込みを有効にした場合にのみ、明示的な読み込みを使用します。</span><span class="sxs-lookup"><span data-stu-id="99002-143">Typically you'd use explicit loading only when you've turned lazy loading off.</span></span> <span data-ttu-id="99002-144">遅延読み込みをオフにすると、1 つのシナリオでは、シリアル化中にします。</span><span class="sxs-lookup"><span data-stu-id="99002-144">One scenario when you should turn lazy loading off is during serialization.</span></span> <span data-ttu-id="99002-145">遅延読み込みとシリアル化しない、混在させるし、読み込みが有効になっていると遅延を意図したよりも大幅に多いデータを照会するが最終的に注意が必要でない場合。</span><span class="sxs-lookup"><span data-stu-id="99002-145">Lazy loading and serialization don't mix well, and if you aren't careful you can end up querying significantly more data than you intended when lazy loading is enabled.</span></span> <span data-ttu-id="99002-146">シリアル化は、型のインスタンス上の各プロパティにアクセスして、通常は動作します。</span><span class="sxs-lookup"><span data-stu-id="99002-146">Serialization generally works by accessing each property on an instance of a type.</span></span> <span data-ttu-id="99002-147">レイジー読み込まれたエンティティをシリアル化し、プロパティへのアクセスは、遅延読み込みをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="99002-147">Property access triggers lazy loading, and those lazy loaded entities are serialized.</span></span> <span data-ttu-id="99002-148">さらに多くの遅延読み込みとシリアル化が発生する可能性、遅延読み込みのエンティティの各プロパティがシリアル化プロセスにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="99002-148">The serialization process then accesses each property of the lazy-loaded entities, potentially causing even more lazy loading and serialization.</span></span> <span data-ttu-id="99002-149">この長時間応答しない連鎖反応を防ぐためには、レイジー エンティティをシリアル化する前の読み込みをオフにします。</span><span class="sxs-lookup"><span data-stu-id="99002-149">To prevent this run-away chain reaction, turn lazy loading off before you serialize an entity.</span></span>

<span data-ttu-id="99002-150">データベース コンテキスト クラスは、既定では、遅延読み込みを実行します。</span><span class="sxs-lookup"><span data-stu-id="99002-150">The database context class performs lazy loading by default.</span></span> <span data-ttu-id="99002-151">これには遅延読み込みを無効にする 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="99002-151">There are two ways to disable lazy loading:</span></span>

- <span data-ttu-id="99002-152">特定のナビゲーション プロパティに対して省略、`virtual`キーワード、プロパティを宣言するときにします。</span><span class="sxs-lookup"><span data-stu-id="99002-152">For specific navigation properties, omit the `virtual` keyword when you declare the property.</span></span>
- <span data-ttu-id="99002-153">すべてのナビゲーション プロパティでは、次のように設定します。`LazyLoadingEnabled`に`false`です。</span><span class="sxs-lookup"><span data-stu-id="99002-153">For all navigation properties, set `LazyLoadingEnabled` to `false`.</span></span> <span data-ttu-id="99002-154">たとえば、コンテキスト クラスのコンス トラクターで、次のコードを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="99002-154">For example, you can put the following code in the constructor of your context class:</span></span> 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="99002-155">遅延読み込みは、パフォーマンスの問題が発生するコードをマスクできます。</span><span class="sxs-lookup"><span data-stu-id="99002-155">Lazy loading can mask code that causes performance problems.</span></span> <span data-ttu-id="99002-156">たとえば、eager または明示的な読み込みが指定されていないが、大量のエンティティを処理し、各イテレーションでいくつかのナビゲーション プロパティを使用するコードできない可能性があります非常に効率的な (データベースに多くのラウンド トリップ) が原因です。</span><span class="sxs-lookup"><span data-stu-id="99002-156">For example, code that doesn't specify eager or explicit loading but processes a high volume of entities and uses several navigation properties in each iteration might be very inefficient (because of many round trips to the database).</span></span> <span data-ttu-id="99002-157">内部設置型 SQL server を使用した開発にもを実行するアプリケーションでは、待機時間の増加と遅延読み込みのための Azure SQL データベースに移動すると、パフォーマンスの問題があります。</span><span class="sxs-lookup"><span data-stu-id="99002-157">An application that performs well in development using an on premise SQL server might have performance problems when moved to Azure SQL Database due to the increased latency and lazy loading.</span></span> <span data-ttu-id="99002-158">プロファイルの負荷が現実的なテスト データベース クエリは遅延読み込みが適切なかどうかを判断するに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="99002-158">Profiling the database queries with a realistic test load will help you determine if lazy loading is appropriate.</span></span> <span data-ttu-id="99002-159">詳細については、次を参照してください。 [Demystifying Entity Framework 戦略: 関連するデータの読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と[SQL Azure へのネットワーク待機時間の削減に Entity Framework を使用して](https://msdn.microsoft.com/magazine/gg309181.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="99002-159">For more information see [Demystifying Entity Framework Strategies: Loading Related Data](https://msdn.microsoft.com/magazine/hh205756.aspx) and [Using the Entity Framework to Reduce Network Latency to SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).</span></span>

## <a name="create-a-courses-index-page-that-displays-department-name"></a><span data-ttu-id="99002-160">その表示部門名コース インデックス ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="99002-160">Create a Courses Index Page That Displays Department Name</span></span>

<span data-ttu-id="99002-161">`Course`エンティティには含むナビゲーション プロパティが含まれています、`Department`コースに割り当てられている部門のエンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-161">The `Course` entity includes a navigation property that contains the `Department` entity of the department that the course is assigned to.</span></span> <span data-ttu-id="99002-162">コースの一覧で、割り当てられている部門の名前を表示するのには、取得する必要があります、`Name`プロパティから、`Department`エンティティは、`Course.Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-162">To display the name of the assigned department in a list of courses, you need to get the `Name` property from the `Department` entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="99002-163">という名前のコント ローラーを作成する`CourseController`の`Course`の前に行った同じオプションを使用して、エンティティ型、`Student`コント ローラーは、次の図に示すように (を除く、イメージとは異なり、コンテキスト クラスは、DAL 名前空間、いないモデル名前空間):</span><span class="sxs-lookup"><span data-stu-id="99002-163">Create a controller named `CourseController` for the `Course` entity type, using the same options that you did earlier for the `Student` controller, as shown in the following illustration (except unlike the image, your context class is in the DAL namespace, not the Models namespace):</span></span>

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

<span data-ttu-id="99002-165">開いている*Controllers\CourseController.cs*見ると、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="99002-165">Open *Controllers\CourseController.cs* and look at the `Index` method:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="99002-166">自動のスキャフォールディングがの一括読み込みを指定、`Department`ナビゲーション プロパティを使用して、`Include`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="99002-166">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="99002-167">開いている*Views\Course\Index.cshtml*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="99002-167">Open *Views\Course\Index.cshtml* and replace the existing code with the following code.</span></span> <span data-ttu-id="99002-168">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-168">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

<span data-ttu-id="99002-169">スキャフォールディングのコードに、次の変更を加えた。</span><span class="sxs-lookup"><span data-stu-id="99002-169">You've made the following changes to the scaffolded code:</span></span>

- <span data-ttu-id="99002-170">見出しが変更**インデックス**に**コース**です。</span><span class="sxs-lookup"><span data-stu-id="99002-170">Changed the heading from **Index** to **Courses**.</span></span>
- <span data-ttu-id="99002-171">行へのリンクを左に移動します。</span><span class="sxs-lookup"><span data-stu-id="99002-171">Moved the row links to the left.</span></span>
- <span data-ttu-id="99002-172">見出しの下の列を追加**数**を示す、`CourseID`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="99002-172">Added a column under the heading **Number** that shows the `CourseID` property value.</span></span> <span data-ttu-id="99002-173">(既定では、主キーはスキャフォールディングされた通常はエンドユーザーにとって意味のないためです。</span><span class="sxs-lookup"><span data-stu-id="99002-173">(By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="99002-174">ただし、ここでは、主キーは意味のある、非表示にする。)</span><span class="sxs-lookup"><span data-stu-id="99002-174">However, in this case the primary key is meaningful and you want to show it.)</span></span>
- <span data-ttu-id="99002-175">最後の列見出しが変更**DepartmentID** (への外部キーの名前、`Department`エンティティ) に**部門**です。</span><span class="sxs-lookup"><span data-stu-id="99002-175">Changed the last column heading from **DepartmentID** (the name of the foreign key to the `Department` entity) to **Department**.</span></span>

<span data-ttu-id="99002-176">最後の列のスキャフォールディング コードが表示されることを確認、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-176">Notice that for the last column, the scaffolded code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

<span data-ttu-id="99002-177">ページを実行 (選択、**コース**Contoso 大学のホーム ページのタブ) リスト部署名を表示します。</span><span class="sxs-lookup"><span data-stu-id="99002-177">Run the page (select the **Courses** tab on the Contoso University home page) to see the list with department names.</span></span>

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="99002-179">コースおよびまで登録数を示す講習においてインストラクター インデックス ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="99002-179">Create an Instructors Index Page That Shows Courses and Enrollments</span></span>

<span data-ttu-id="99002-180">このセクションでをコント ローラーを作成し、表示、`Instructor`講習においてインストラクター インデックス ページを表示するためにエンティティ。</span><span class="sxs-lookup"><span data-stu-id="99002-180">In this section you'll create a controller and view for the `Instructor` entity in order to display the Instructors Index page:</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="99002-182">このページは、読み取りし、次のように関連するデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="99002-182">This page reads and displays related data in the following ways:</span></span>

- <span data-ttu-id="99002-183">講習においてインストラクターの一覧から関連するデータが表示されます、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-183">The list of instructors displays related data from the `OfficeAssignment` entity.</span></span> <span data-ttu-id="99002-184">`Instructor`と`OfficeAssignment`エンティティに 0 または 1 を 1 つの関係。</span><span class="sxs-lookup"><span data-stu-id="99002-184">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="99002-185">一括読み込みを使用して、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-185">You'll use eager loading for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="99002-186">前述のように、主テーブルの取得したすべての行に、関連するデータが必要なときに一括読み込みは通常より効率的です。</span><span class="sxs-lookup"><span data-stu-id="99002-186">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="99002-187">この場合、表示されているすべてのインストラクターのオフィスの割り当てを表示します。</span><span class="sxs-lookup"><span data-stu-id="99002-187">In this case, you want to display office assignments for all displayed instructors.</span></span>
- <span data-ttu-id="99002-188">ユーザーが関連する、インストラクターを選択すると`Course`エンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-188">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="99002-189">`Instructor`と`Course`エンティティに多対多の関係。</span><span class="sxs-lookup"><span data-stu-id="99002-189">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="99002-190">一括読み込みを使用して、`Course`エンティティとその関連`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-190">You'll use eager loading for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="99002-191">ここでは、選択した講師に対してのみコースする必要があるために、遅延読み込みがより効率的な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="99002-191">In this case, lazy loading might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="99002-192">ただし、この例は、ナビゲーション プロパティではエンティティ内でナビゲーション プロパティの一括読み込みを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="99002-192">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>
- <span data-ttu-id="99002-193">ユーザーは、コースを選択するときに関連するデータを`Enrollments`エンティティ セットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-193">When the user selects a course, related data from the `Enrollments` entity set is displayed.</span></span> <span data-ttu-id="99002-194">`Course`と`Enrollment`エンティティに一対多の関係。</span><span class="sxs-lookup"><span data-stu-id="99002-194">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span> <span data-ttu-id="99002-195">追加の明示的な読み込み`Enrollment`エンティティとその関連`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-195">You'll add explicit loading for `Enrollment` entities and their related `Student` entities.</span></span> <span data-ttu-id="99002-196">(明示的な読み込みは必要ありませんので、遅延読み込みが有効になっているが、この明示的な読み込みを行う方法を示しています。)</span><span class="sxs-lookup"><span data-stu-id="99002-196">(Explicit loading isn't necessary because lazy loading is enabled, but this shows how to do explicit loading.)</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="99002-197">講師インデックス ビューのビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="99002-197">Create a View Model for the Instructor Index View</span></span>

<span data-ttu-id="99002-198">講師インデックス ページには、次の 3 つの異なるテーブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-198">The Instructor Index page shows three different tables.</span></span> <span data-ttu-id="99002-199">そのため、3 つのプロパティを含むテーブルの 1 つのデータを保持している各ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="99002-199">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="99002-200">*ViewModels*フォルダー作成*InstructorIndexData.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="99002-200">In the *ViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a><span data-ttu-id="99002-201">選択した行のスタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="99002-201">Adding a Style for Selected Rows</span></span>

<span data-ttu-id="99002-202">選択した行をマークするには、別の背景色が必要です。</span><span class="sxs-lookup"><span data-stu-id="99002-202">To mark selected rows you need a different background color.</span></span> <span data-ttu-id="99002-203">この UI のスタイルを提供するには、セクションに次の強調表示されたコードを追加`/* info and errors */`で*Content\Site.css*、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="99002-203">To provide a style for this UI, add the following highlighted code to the section `/* info and errors */` in *Content\Site.css*, as shown below:</span></span>

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a><span data-ttu-id="99002-204">講師コント ローラーとビューの作成</span><span class="sxs-lookup"><span data-stu-id="99002-204">Creating the Instructor Controller and Views</span></span>

<span data-ttu-id="99002-205">作成、`InstructorController`次の図に示すように、コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="99002-205">Create an `InstructorController` controller as shown in the following illustration:</span></span>

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

<span data-ttu-id="99002-207">開いている*Controllers\InstructorController.cs*を追加し、`using`のステートメント、`ViewModels`名前空間。</span><span class="sxs-lookup"><span data-stu-id="99002-207">Open *Controllers\InstructorController.cs* and add a `using` statement for the `ViewModels` namespace:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="99002-208">スキャフォールディングのコード、`Index`メソッドに対してのみ、一括読み込みを指定する、`OfficeAssignment`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-208">The scaffolded code in the `Index` method specifies eager loading only for the `OfficeAssignment` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="99002-209">置換、`Index`追加を読み込むには、次のコードを持つメソッドに関連したデータとビュー モデルに格納します。</span><span class="sxs-lookup"><span data-stu-id="99002-209">Replace the `Index` method with the following code to load additional related data and put it in the view model:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="99002-210">メソッドは省略可能なルート データを受け入れます (`id`) およびクエリ文字列パラメーター (`courseID`) を選択したインストラクターと、選択したコースの ID 値を指定して、ビューにすべての必要なデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="99002-210">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course, and passes all of the required data to the view.</span></span> <span data-ttu-id="99002-211">パラメーターがによって提供される、**選択**ハイパーリンクをページ。</span><span class="sxs-lookup"><span data-stu-id="99002-211">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="99002-212">**ルート データ**</span><span class="sxs-lookup"><span data-stu-id="99002-212">**Route data**</span></span>
> 
> <span data-ttu-id="99002-213">ルート データは、ルーティング テーブルに指定された URL セグメント内のモデル バインダーにあるデータです。</span><span class="sxs-lookup"><span data-stu-id="99002-213">Route data is data that the model binder found in a URL segment specified in the routing table.</span></span> <span data-ttu-id="99002-214">たとえば、既定のルートを指定して`controller`、 `action`、および`id`セグメント。</span><span class="sxs-lookup"><span data-stu-id="99002-214">For example, the default route specifies `controller`, `action`, and `id` segments:</span></span>
> 
> <span data-ttu-id="99002-215">routes.MapRoute(</span><span class="sxs-lookup"><span data-stu-id="99002-215">routes.MapRoute(</span></span>  
>  <span data-ttu-id="99002-216">名前:"Default"、</span><span class="sxs-lookup"><span data-stu-id="99002-216">name: "Default",</span></span>  
>  <span data-ttu-id="99002-217">url: "{controller}/{action}/{id}",</span><span class="sxs-lookup"><span data-stu-id="99002-217">url: "{controller}/{action}/{id}",</span></span>  
>  <span data-ttu-id="99002-218">既定値: new {コント ローラー = アクションでは「ホーム」="Index"id = UrlParameter.Optional}</span><span class="sxs-lookup"><span data-stu-id="99002-218">defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }</span></span>  
> <span data-ttu-id="99002-219">);</span><span class="sxs-lookup"><span data-stu-id="99002-219">);</span></span>
> 
> <span data-ttu-id="99002-220">既定のルートでは、次の URL では、マップ`Instructor`として、 `controller`、`Index`として、 `action` 1 を`id`です。 これらは、ルート データの値。</span><span class="sxs-lookup"><span data-stu-id="99002-220">In the following URL, the default route maps `Instructor` as the `controller`, `Index` as the `action` and 1 as the `id`; these are route data values.</span></span>
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> <span data-ttu-id="99002-221">"? courseID 2021 ="クエリ文字列値です。</span><span class="sxs-lookup"><span data-stu-id="99002-221">"?courseID=2021" is a query string value.</span></span> <span data-ttu-id="99002-222">モデル バインダーは、合格した場合でも機能、`id`クエリ文字列値として。</span><span class="sxs-lookup"><span data-stu-id="99002-222">The model binder will also work if you pass the `id` as a query string value:</span></span>
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> <span data-ttu-id="99002-223">Url がによって作成された`ActionLink`Razor ビュー内のステートメント。</span><span class="sxs-lookup"><span data-stu-id="99002-223">The URLs are created by `ActionLink` statements in the Razor view.</span></span> <span data-ttu-id="99002-224">次のコードで、`id`ためパラメーターに既定のルートと一致する`id`ルート データを追加します。</span><span class="sxs-lookup"><span data-stu-id="99002-224">In the following code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> <span data-ttu-id="99002-225">次のコードに`courseID`クエリ文字列として追加されたために、既定のルートのパラメーターと一致しません。</span><span class="sxs-lookup"><span data-stu-id="99002-225">In the following code, `courseID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


<span data-ttu-id="99002-226">コードは、ビュー モデルのインスタンスを作成し、インストラクターの一覧に配置して開始します。</span><span class="sxs-lookup"><span data-stu-id="99002-226">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="99002-227">コードでの一括読み込みを指定、`Instructor.OfficeAssignment`と`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-227">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

<span data-ttu-id="99002-228">2 番目`Include`メソッド コース、読み込んでが読み込まれる各コースには一括読み込みの場合、`Course.Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-228">The second `Include` method loads Courses, and for each Course that is loaded it does eager loading for the `Course.Department` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="99002-229">前述のように、一括読み込みは必要ありませんが、パフォーマンスを向上させるために行われます。</span><span class="sxs-lookup"><span data-stu-id="99002-229">As mentioned previously, eager loading is not required but is done to improve performance.</span></span> <span data-ttu-id="99002-230">ビューが常に必要なため、`OfficeAssignment`エンティティを同じクエリでフェッチする方が効率的です。</span><span class="sxs-lookup"><span data-stu-id="99002-230">Since the view always requires the `OfficeAssignment` entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="99002-231">`Course`一括読み込みが、ページが表示されるより多くの場合、コースよりも選択されている場合にのみ、遅延読み込みよりも良いように web ページにあるインストラクターが選択されているときに、エンティティが必要です。</span><span class="sxs-lookup"><span data-stu-id="99002-231">`Course` entities are required when an instructor is selected in the web page, so eager loading is better than lazy loading only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="99002-232">講師 ID を選択した場合は、選択した講師が講習においてインストラクターにビューのモデルの一覧から取得されます。</span><span class="sxs-lookup"><span data-stu-id="99002-232">If an instructor ID was selected, the selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="99002-233">ビュー モデルの`Courses`でプロパティが読み込まれてから、`Course`そのインストラクターからエンティティ`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-233">The view model's `Courses` property is then loaded with the `Course` entities from that instructor's `Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="99002-234">`Where`メソッドのコレクションを返しますが、条件が 1 つだけでそのメソッドの結果に渡されるここでは`Instructor`返されるエンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-234">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single `Instructor` entity being returned.</span></span> <span data-ttu-id="99002-235">`Single`メソッドが、1 つに、コレクションを変換`Instructor`エンティティで、そのエンティティにアクセスできる`Courses`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="99002-235">The `Single` method converts the collection into a single `Instructor` entity, which gives you access to that entity's `Courses` property.</span></span>

<span data-ttu-id="99002-236">使用する、[単一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)メソッドのコレクションがわかっている場合にコレクションを 1 つの項目になります。</span><span class="sxs-lookup"><span data-stu-id="99002-236">You use the [Single](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="99002-237">`Single`に渡されるコレクションが空の場合、または複数の項目がある場合、メソッドが例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="99002-237">The `Single` method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="99002-238">代わりに[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)、既定値が返されます (`null`ここでは)、コレクションが空の場合。</span><span class="sxs-lookup"><span data-stu-id="99002-238">An alternative is [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), which returns a default value (`null` in this case) if the collection is empty.</span></span> <span data-ttu-id="99002-239">ただし、ここではするがまだ例外が発生 (から検索しようとしています、`Courses`プロパティを`null`参照)、例外メッセージは、問題の原因を示す小さい明確にします。</span><span class="sxs-lookup"><span data-stu-id="99002-239">However, in this case that would still result in an exception (from trying to find a `Courses` property on a `null` reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="99002-240">呼び出すと、`Single`メソッドに渡すことも、`Where`条件呼び出す代わりに、`Where`メソッドとは別に。</span><span class="sxs-lookup"><span data-stu-id="99002-240">When you call the `Single` method, you can also pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="99002-241">これは次のコードの代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="99002-241">Instead of:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="99002-242">次に、コースが選択されている場合は、選択したコースがビューのモデルでコースの一覧から取得されます。</span><span class="sxs-lookup"><span data-stu-id="99002-242">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="99002-243">モデルの表示の`Enrollments`プロパティが読み込まれ、`Enrollment`コースからエンティティ`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-243">Then the view model's `Enrollments` property is loaded with the `Enrollment` entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a><span data-ttu-id="99002-244">講師インデックス ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="99002-244">Modifying the Instructor Index View</span></span>

<span data-ttu-id="99002-245">*Views\Instructor\Index.cshtml*、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="99002-245">In *Views\Instructor\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="99002-246">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-246">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

<span data-ttu-id="99002-247">既存のコードに、次の変更を加えた。</span><span class="sxs-lookup"><span data-stu-id="99002-247">You've made the following changes to the existing code:</span></span>

- <span data-ttu-id="99002-248">モデル クラスを変更`InstructorIndexData`です。</span><span class="sxs-lookup"><span data-stu-id="99002-248">Changed the model class to `InstructorIndexData`.</span></span>
- <span data-ttu-id="99002-249">ページのタイトルを変更**インデックス**に**講習においてインストラクター**です。</span><span class="sxs-lookup"><span data-stu-id="99002-249">Changed the page title from **Index** to **Instructors**.</span></span>
- <span data-ttu-id="99002-250">行のリンクの列を左に移動します。</span><span class="sxs-lookup"><span data-stu-id="99002-250">Moved the row link columns to the left.</span></span>
- <span data-ttu-id="99002-251">削除、 **FullName**列です。</span><span class="sxs-lookup"><span data-stu-id="99002-251">Removed the **FullName** column.</span></span>
- <span data-ttu-id="99002-252">追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、`item.OfficeAssignment`が null でないです。</span><span class="sxs-lookup"><span data-stu-id="99002-252">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="99002-253">(これは、0 または 1 を 1 つのリレーションシップであるため、できない場合があります、関連する`OfficeAssignment`エンティティです)。</span><span class="sxs-lookup"><span data-stu-id="99002-253">(Because this is a one-to-zero-or-one relationship, there might not be a related `OfficeAssignment` entity.)</span></span> 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- <span data-ttu-id="99002-254">動的に追加するコードを追加しました`class="selectedrow"`を`tr`選択した講師の要素。</span><span class="sxs-lookup"><span data-stu-id="99002-254">Added code that will dynamically add `class="selectedrow"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="99002-255">これは、先ほど作成した CSS クラスを使用して、選択した行の背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="99002-255">This sets a background color for the selected row using the CSS class that you created earlier.</span></span> <span data-ttu-id="99002-256">(、`valign`属性が役に立ちます次のチュートリアルでは、テーブルに複数行の列を追加するとします)。</span><span class="sxs-lookup"><span data-stu-id="99002-256">(The `valign` attribute will be useful in the following tutorial when you add a multi-row column to the table.)</span></span> 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- <span data-ttu-id="99002-257">追加された新しい`ActionLink`というラベルの付いた**選択**各行の他のリンクの直前に送信される、選択した講師 ID が、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="99002-257">Added a new `ActionLink` labeled **Select** immediately before the other links in each row, which causes the selected instructor ID to be sent to the `Index` method.</span></span>

<span data-ttu-id="99002-258">アプリケーションの実行を選択して、**講習においてインストラクター**タブです。ページが表示されます、`Location`関連のプロパティ`OfficeAssignment`エンティティと空のテーブル セルがある場合に関連しない`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="99002-258">Run the application and select the **Instructors** tab. The page displays the `Location` property of related `OfficeAssignment` entities and an empty table cell when there's no related `OfficeAssignment` entity.</span></span>

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="99002-260">*Views\Instructor\Index.cshtml*の終了後にファイルを`table`(最後のファイル)、要素は、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="99002-260">In the *Views\Instructor\Index.cshtml* file, after the closing `table` element (at the end of the file), add the following highlighted code.</span></span> <span data-ttu-id="99002-261">これには、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-261">This displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

<span data-ttu-id="99002-262">このコードを読み取り、`Courses`コースの一覧を表示するビューのモデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="99002-262">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="99002-263">用意されています、`Select`ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="99002-263">It also provides a `Select` hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

> [!NOTE]
> <span data-ttu-id="99002-264">*.Css*ファイルがブラウザーによってキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="99002-264">The *.css* file is cached by browsers.</span></span> <span data-ttu-id="99002-265">アプリケーションを実行するときに、変更が見つからない場合は、ハード更新 (CTRL キーを押しながらクリックして、**更新** ボタン、または、ctrl キーを押しながら F5 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="99002-265">If you don't see the changes when you run the application, do a hard refresh (hold down the CTRL key while clicking the **Refresh** button, or press CTRL+F5).</span></span>


<span data-ttu-id="99002-266">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="99002-266">Run the page and select an instructor.</span></span> <span data-ttu-id="99002-267">これで、選択したインストラクターに割り当てられているコースを表示するグリッドが表示し、する各コースに割り当てられている部署の名前を参照してください。</span><span class="sxs-lookup"><span data-stu-id="99002-267">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="99002-269">追加したコード ブロックの後に、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="99002-269">After the code block you just added, add the following code.</span></span> <span data-ttu-id="99002-270">コースが選択されているときにコースに受講登録された者のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="99002-270">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

<span data-ttu-id="99002-271">このコードを読み取り、`Enrollments`コースに受講者の一覧を表示するために、ビュー モデルのプロパティを登録します。</span><span class="sxs-lookup"><span data-stu-id="99002-271">This code reads the `Enrollments` property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="99002-272">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="99002-272">Run the page and select an instructor.</span></span> <span data-ttu-id="99002-273">登録済みの受講者との成績の一覧を表示するコースを選択します。</span><span class="sxs-lookup"><span data-stu-id="99002-273">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a><span data-ttu-id="99002-275">明示的な読み込みの追加</span><span class="sxs-lookup"><span data-stu-id="99002-275">Adding Explicit Loading</span></span>

<span data-ttu-id="99002-276">開いている*InstructorController.cs*見ますが、どのように`Index`メソッド選択コースの「登録」リストを取得。</span><span class="sxs-lookup"><span data-stu-id="99002-276">Open *InstructorController.cs* and look at how the `Index` method gets the list of enrollments for a selected course:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="99002-277">一括読み込みを指定した講習においてインストラクターの一覧を取得するときに、`Courses`ナビゲーション プロパティと、`Department`コースごとのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="99002-277">When you retrieved the list of instructors, you specified eager loading for the `Courses` navigation property and for the `Department` property of each course.</span></span> <span data-ttu-id="99002-278">配置する、`Courses`コレクションにおいて、ビュー モデルにアクセスしているので、`Enrollments`そのコレクション内の 1 つのエンティティのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-278">Then you put the `Courses` collection in the view model, and now you're accessing the `Enrollments` navigation property from one entity in that collection.</span></span> <span data-ttu-id="99002-279">一括読み込みを指定していないため、`Course.Enrollments`ナビゲーション プロパティ、遅延読み込みの結果 ページでそのプロパティからデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-279">Because you didn't specify eager loading for the `Course.Enrollments` navigation property, the data from that property is appearing in the page as a result of lazy loading.</span></span>

<span data-ttu-id="99002-280">その他の方法でコードを変更することがなく遅延読み込みを無効にした場合、`Enrollments`プロパティはコースが実際にどのくらい登録に関係なく null になります。</span><span class="sxs-lookup"><span data-stu-id="99002-280">If you disabled lazy loading without changing the code in any other way, the `Enrollments` property would be null regardless of how many enrollments the course actually had.</span></span> <span data-ttu-id="99002-281">その場合、読み込みに、`Enrollments`プロパティ、一括読み込みまたは明示的な読み込みのいずれかを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="99002-281">In that case, to load the `Enrollments` property, you'd have to specify either eager loading or explicit loading.</span></span> <span data-ttu-id="99002-282">一括読み込みを行う方法は、既に説明しました。</span><span class="sxs-lookup"><span data-stu-id="99002-282">You've already seen how to do eager loading.</span></span> <span data-ttu-id="99002-283">明示的な読み込みの例を表示するのには、置換、`Index`メソッドを次のコードは、明示的に読み込む、`Enrollments`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="99002-283">In order to see an example of explicit loading, replace the `Index` method with the following code, which explicitly loads the `Enrollments` property.</span></span> <span data-ttu-id="99002-284">変更されたコードが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="99002-284">The code changed are highlighted.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

<span data-ttu-id="99002-285">選択したを取得したら`Course`エンティティ、新しいコードがそのコースを明示的に読み込みます`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="99002-285">After getting the selected `Course` entity, the new code explicitly loads that course's `Enrollments` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="99002-286">明示的に読み込む各し、`Enrollment`エンティティの関連`Student`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="99002-286">Then it explicitly loads each `Enrollment` entity's related `Student` entity:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="99002-287">使用することを確認、`Collection`コレクション プロパティを読み込みますが、使用する、1 つのエンティティを保持するプロパティの`Reference`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="99002-287">Notice that you use the `Collection` method to load a collection property, but for a property that holds just one entity, you use the `Reference` method.</span></span> <span data-ttu-id="99002-288">インストラクター インデックス ページを今すぐ実行することができ、わかりますなし ページで、表示される内容に違いが、データの取得方法を変更しました。</span><span class="sxs-lookup"><span data-stu-id="99002-288">You can run the Instructor Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="99002-289">まとめ</span><span class="sxs-lookup"><span data-stu-id="99002-289">Summary</span></span>

<span data-ttu-id="99002-290">ナビゲーション プロパティに関連するデータを読み込むすべての 3 つ方法 (レイジー、eager、および明示的な) を使用したようになりました。</span><span class="sxs-lookup"><span data-stu-id="99002-290">You've now used all three ways (lazy, eager, and explicit) to load related data into navigation properties.</span></span> <span data-ttu-id="99002-291">次のチュートリアルでは、関連するデータを更新する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="99002-291">In the next tutorial you'll learn how to update related data.</span></span>

<span data-ttu-id="99002-292">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="99002-292">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="99002-293">[前へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[次へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="99002-293">[Previous](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Next](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
