---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (9/10) で、リポジトリと Unit of Work パターンを実装する |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0ca67c67b763d003e5f395ddd4bac0c5ec28f1f1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366370"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a><span data-ttu-id="09503-103">ASP.NET MVC アプリケーション (9/10) で、リポジトリと Unit of Work パターンを実装します。</span><span class="sxs-lookup"><span data-stu-id="09503-103">Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)</span></span>
====================
<span data-ttu-id="09503-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="09503-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="09503-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="09503-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="09503-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09503-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="09503-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="09503-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="09503-108">チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。</span><span class="sxs-lookup"><span data-stu-id="09503-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="09503-109">を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="09503-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="09503-110">問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。</span><span class="sxs-lookup"><span data-stu-id="09503-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="09503-111">一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="09503-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="09503-112">前のチュートリアルでの冗長なコードを削減する継承を使用して、`Student`と`Instructor`エンティティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="09503-112">In the previous tutorial you used inheritance to reduce redundant code in the `Student` and `Instructor` entity classes.</span></span> <span data-ttu-id="09503-113">このチュートリアルでは、CRUD 操作のリポジトリと unit of work パターンを使用するいくつかの点を確認します。</span><span class="sxs-lookup"><span data-stu-id="09503-113">In this tutorial you'll see some ways to use the repository and unit of work patterns for CRUD operations.</span></span> <span data-ttu-id="09503-114">このいずれかで、前のチュートリアルでは、ように、コードの新しいページを作成するのではなく、既に作成したページの動作を変更します。</span><span class="sxs-lookup"><span data-stu-id="09503-114">As in the previous tutorial, in this one you'll change the way your code works with pages you already created rather than creating new pages.</span></span>

## <a name="the-repository-and-unit-of-work-patterns"></a><span data-ttu-id="09503-115">リポジトリと Unit of Work パターン</span><span class="sxs-lookup"><span data-stu-id="09503-115">The Repository and Unit of Work Patterns</span></span>

<span data-ttu-id="09503-116">リポジトリと unit of work パターンは、データ アクセス層とアプリケーションのビジネス ロジック層の間の抽象化レイヤーを作成する対象としています。</span><span class="sxs-lookup"><span data-stu-id="09503-116">The repository and unit of work patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="09503-117">これらのパターンを実装すると、データ ストアの変更からアプリケーションを隔離でき、自動化された単体テストやテスト駆動開発 (TDD) を円滑化できます。</span><span class="sxs-lookup"><span data-stu-id="09503-117">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span>

<span data-ttu-id="09503-118">このチュートリアルでは、各エンティティ型のリポジトリ クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="09503-118">In this tutorial you'll implement a repository class for each entity type.</span></span> <span data-ttu-id="09503-119">`Student`エンティティ型が、リポジトリ インターフェイスと、リポジトリ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09503-119">For the `Student` entity type you'll create a repository interface and a repository class.</span></span> <span data-ttu-id="09503-120">コント ローラーで、リポジトリをインスタンス化するときは、コント ローラーはリポジトリ インターフェイスを実装する任意のオブジェクトへの参照をそのまま使用できるように、インターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="09503-120">When you instantiate the repository in your controller, you'll use the interface so that the controller will accept a reference to any object that implements the repository interface.</span></span> <span data-ttu-id="09503-121">Web サーバー、コント ローラーを実行すると、Entity Framework で動作するリポジトリを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="09503-121">When the controller runs under a web server, it receives a repository that works with the Entity Framework.</span></span> <span data-ttu-id="09503-122">コント ローラーを単体テスト クラスの下で実行すると、メモリ内コレクションなど、テストを簡単に操作できる方法で格納されているデータで動作するリポジトリを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="09503-122">When the controller runs under a unit test class, it receives a repository that works with data stored in a way that you can easily manipulate for testing, such as an in-memory collection.</span></span>

<span data-ttu-id="09503-123">チュートリアルの後半では、複数のリポジトリと単位の作業のクラスを使用します、`Course`と`Department`エンティティ型、`Course`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="09503-123">Later in the tutorial you'll use multiple repositories and a unit of work class for the `Course` and `Department` entity types in the `Course` controller.</span></span> <span data-ttu-id="09503-124">クラスの作業の単位は、それらすべてによって共有される 1 つのデータベース コンテキスト クラスを作成して複数のリポジトリの作業を調整します。</span><span class="sxs-lookup"><span data-stu-id="09503-124">The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.</span></span> <span data-ttu-id="09503-125">自動化された単体テストを実行できる場合は、作成し、これらのクラスの場合と同様にインターフェイスを使用して、`Student`リポジトリ。</span><span class="sxs-lookup"><span data-stu-id="09503-125">If you wanted to be able to perform automated unit testing, you'd create and use interfaces for these classes in the same way you did for the `Student` repository.</span></span> <span data-ttu-id="09503-126">ただし、チュートリアルを簡潔にするを作成し、インターフェイスせずにこれらのクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="09503-126">However, to keep the tutorial simple, you'll create and use these classes without interfaces.</span></span>

<span data-ttu-id="09503-127">次の図は、コント ローラーとまったく使用しないリポジトリまたは unit of work パターンと比較するコンテキスト クラス間の関係を理解する 1 つの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09503-127">The following illustration shows one way to conceptualize the relationships between the controller and context classes compared to not using the repository or unit of work pattern at all.</span></span>

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

<span data-ttu-id="09503-129">このチュートリアル シリーズでは、単体テストを作成しません。</span><span class="sxs-lookup"><span data-stu-id="09503-129">You won't create unit tests in this tutorial series.</span></span> <span data-ttu-id="09503-130">概要については TDD リポジトリ パターンを使用して MVC アプリケーションで、次を参照してください。[チュートリアル: ASP.NET MVC で TDD を使用して](https://msdn.microsoft.com/library/ff847525.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="09503-130">For an introduction to TDD with an MVC application that uses the repository pattern, see [Walkthrough: Using TDD with ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx).</span></span> <span data-ttu-id="09503-131">リポジトリ パターンの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="09503-131">For more information about the repository pattern, see the following resources:</span></span>

- <span data-ttu-id="09503-132">[リポジトリ パターン](https://msdn.microsoft.com/library/ff649690.aspx)msdn です。</span><span class="sxs-lookup"><span data-stu-id="09503-132">[The Repository Pattern](https://msdn.microsoft.com/library/ff649690.aspx) on MSDN.</span></span>
- <span data-ttu-id="09503-133">[Entity Framework 4.0 でリポジトリと Unit of Work パターンを使用して](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework チームのブログ。</span><span class="sxs-lookup"><span data-stu-id="09503-133">[Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) on the Entity Framework team blog.</span></span>
- <span data-ttu-id="09503-134">[アジャイルの Entity Framework 4 リポジトリ](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman のブログ投稿のシリーズです。</span><span class="sxs-lookup"><span data-stu-id="09503-134">[Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) series of posts on Julie Lerman's blog.</span></span>
- <span data-ttu-id="09503-135">[ビルド概要の HTML5 と jQuery アプリケーションでアカウント](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin のブログ。</span><span class="sxs-lookup"><span data-stu-id="09503-135">[Building the Account at a Glance HTML5/jQuery Application](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) on Dan Wahlin's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="09503-136">リポジトリと unit of work パターンを実装する方法はたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="09503-136">There are many ways to implement the repository and unit of work patterns.</span></span> <span data-ttu-id="09503-137">クラスの作業の単位の有無は、リポジトリ クラスを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="09503-137">You can use repository classes with or without a unit of work class.</span></span> <span data-ttu-id="09503-138">すべてのエンティティ型、または種類ごとに 1 つ 1 つのリポジトリを実装することができます。</span><span class="sxs-lookup"><span data-stu-id="09503-138">You can implement a single repository for all entity types, or one for each type.</span></span> <span data-ttu-id="09503-139">種類ごとに 1 つを実装する場合は、別のクラス、ジェネリック基底クラスと派生クラスまたは抽象基本クラスおよび派生クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="09503-139">If you implement one for each type, you can use separate classes, a generic base class and derived classes, or an abstract base class and derived classes.</span></span> <span data-ttu-id="09503-140">リポジトリでビジネス ロジックを含むまたはデータ アクセス ロジックに制限できます。</span><span class="sxs-lookup"><span data-stu-id="09503-140">You can include business logic in your repository or restrict it to data access logic.</span></span> <span data-ttu-id="09503-141">使用して、データベース コンテキスト クラスに抽象化レイヤーを構築することもできます[IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)の代わりにインターフェイスがあります[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)種類、エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="09503-141">You can also build an abstraction layer into your database context class by using [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfaces there instead of [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) types for your entity sets.</span></span> <span data-ttu-id="09503-142">このチュートリアルで示すように抽象化レイヤーを実装する方法は、いないすべてのシナリオおよび環境の推奨事項を考慮するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="09503-142">The approach to implementing an abstraction layer shown in this tutorial is one option for you to consider, not a recommendation for all scenarios and environments.</span></span>


## <a name="creating-the-student-repository-class"></a><span data-ttu-id="09503-143">学生のリポジトリ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09503-143">Creating the Student Repository Class</span></span>

<span data-ttu-id="09503-144">*DAL*フォルダー、という名前のクラス ファイルを作成する*IStudentRepository.cs*既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="09503-144">In the *DAL* folder, create a class file named *IStudentRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="09503-145">このコードは 2 つの読み取りメソッドを含む、CRUD メソッドの標準的なセットを宣言します-1 つをすべて返す`Student`エンティティ、および 1 つを検索する 1 つ`Student`ID によってエンティティ</span><span class="sxs-lookup"><span data-stu-id="09503-145">This code declares a typical set of CRUD methods, including two read methods — one that returns all `Student` entities, and one that finds a single `Student` entity by ID.</span></span>

<span data-ttu-id="09503-146">*DAL*フォルダー、という名前のクラス ファイルを作成する*StudentRepository.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="09503-146">In the *DAL* folder, create a class file named *StudentRepository.cs* file.</span></span> <span data-ttu-id="09503-147">実装する次のコードで、既存のコードを置き換える、`IStudentRepository`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="09503-147">Replace the existing code with the following code, which implements the `IStudentRepository` interface:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="09503-148">データベース コンテキストはクラスの変数で定義され、コンス トラクターは、コンテキストのインスタンスを渡すには、呼び出し元のオブジェクトが必要ですが。</span><span class="sxs-lookup"><span data-stu-id="09503-148">The database context is defined in a class variable, and the constructor expects the calling object to pass in an instance of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="09503-149">リポジトリで、新しいコンテキストをインスタンス化する可能性がありますが、1 つのコント ローラーで複数のリポジトリを使用した場合の各は別のコンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="09503-149">You could instantiate a new context in the repository, but then if you used multiple repositories in one controller, each would end up with a separate context.</span></span> <span data-ttu-id="09503-150">後で複数のリポジトリを使用します、`Course`コント ローラー、わかる方法クラスの作業の単位がすべてのリポジトリが、同じコンテキストを使用することを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="09503-150">Later you'll use multiple repositories in the `Course` controller, and you'll see how a unit of work class can ensure that all repositories use the same context.</span></span>

<span data-ttu-id="09503-151">リポジトリを実装して[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)しコント ローラーは、前述のように、CRUD メソッドは、前述と同じ方法でデータベースのコンテキストに呼び出しを行う、データベース コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="09503-151">The repository implements [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) and disposes the database context as you saw earlier in the controller, and its CRUD methods make calls to the database context in the same way that you saw earlier.</span></span>

## <a name="change-the-student-controller-to-use-the-repository"></a><span data-ttu-id="09503-152">リポジトリを使用する学生コント ローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="09503-152">Change the Student Controller to Use the Repository</span></span>

<span data-ttu-id="09503-153">*StudentController.cs*クラスの現在のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="09503-153">In *StudentController.cs*, replace the code currently in the class with the following code.</span></span> <span data-ttu-id="09503-154">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="09503-154">The changes are highlighted.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

<span data-ttu-id="09503-155">コント ローラーは今すぐを実装するオブジェクトのクラス変数を宣言、`IStudentRepository`コンテキスト クラスではなくインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="09503-155">The controller now declares a class variable for an object that implements the `IStudentRepository` interface instead of the context class:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="09503-156">既定の (パラメーターなしの) コンス トラクターが新しいコンテキスト インスタンスを作成し、省略可能なコンス トラクターにより、呼び出し元コンテキスト インスタンスを渡します。</span><span class="sxs-lookup"><span data-stu-id="09503-156">The default (parameterless) constructor creates a new context instance, and an optional constructor allows the caller to pass in a context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="09503-157">(使用していた場合*依存関係の注入*DI、DI ソフトウェアは、適切なリポジトリ オブジェクトは常に指定することを確認するため、既定のコンス トラクターを必要はありませんか)。</span><span class="sxs-lookup"><span data-stu-id="09503-157">(If you were using *dependency injection*, or DI, you wouldn't need the default constructor because the DI software would ensure that the correct repository object would always be provided.)</span></span>

<span data-ttu-id="09503-158">CRUD のメソッドでコンテキストの代わりに、リポジトリと呼ばれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="09503-158">In the CRUD methods, the repository is now called instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="09503-159">および`Dispose`メソッドは、コンテキストではなく、リポジトリを破棄するようになりました。</span><span class="sxs-lookup"><span data-stu-id="09503-159">And the `Dispose` method now disposes the repository instead of the context:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="09503-160">サイトを実行し、をクリックして、**学生**タブ。</span><span class="sxs-lookup"><span data-stu-id="09503-160">Run the site and click the **Students** tab.</span></span>

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="09503-162">ページは、外観し、リポジトリを使用するコードを変更して、その他の学生のページも同じ動作前に、と同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="09503-162">The page looks and works the same as it did before you changed the code to use the repository, and the other Student pages also work the same.</span></span> <span data-ttu-id="09503-163">ただし、方法で重要な違いがある、`Index`コント ローラーのメソッドはフィルター処理と並べ替え。</span><span class="sxs-lookup"><span data-stu-id="09503-163">However, there's an important difference in the way the `Index` method of the controller does filtering and ordering.</span></span> <span data-ttu-id="09503-164">このメソッドの元のバージョンには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="09503-164">The original version of this method contained the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

<span data-ttu-id="09503-165">更新された`Index`メソッドには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="09503-165">The updated `Index` method contains the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

<span data-ttu-id="09503-166">強調表示されたコードのみが変更されました。</span><span class="sxs-lookup"><span data-stu-id="09503-166">Only the highlighted code has changed.</span></span>

<span data-ttu-id="09503-167">コードの元のバージョンで`students`として型指定された、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="09503-167">In the original version of the code, `students` is typed as an `IQueryable` object.</span></span> <span data-ttu-id="09503-168">などのメソッドを使用してコレクションに変換されるまで、データベースにクエリが送信されない`ToList`、student モデルが、インデックス ビューにアクセスするまで発生しません。</span><span class="sxs-lookup"><span data-stu-id="09503-168">The query isn't sent to the database until it's converted into a collection using a method such as `ToList`, which doesn't occur until the Index view accesses the student model.</span></span> <span data-ttu-id="09503-169">`Where`上の元のコードでメソッドになります、`WHERE`データベースに送信される SQL クエリ内の句。</span><span class="sxs-lookup"><span data-stu-id="09503-169">The `Where` method in the original code above becomes a `WHERE` clause in the SQL query that is sent to the database.</span></span> <span data-ttu-id="09503-170">さらに、選択したエンティティのみがデータベースによって返されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="09503-170">That in turn means that only the selected entities are returned by the database.</span></span> <span data-ttu-id="09503-171">変更した結果として、ただし、`context.Students`に`studentRepository.GetStudents()`、`students`後、このステートメントは、変数、`IEnumerable`で、データベース内のすべての学生を含むコレクション。</span><span class="sxs-lookup"><span data-stu-id="09503-171">However, as a result of changing `context.Students` to `studentRepository.GetStudents()`, the `students` variable after this statement is an `IEnumerable` collection that includes all students in the database.</span></span> <span data-ttu-id="09503-172">適用した結果、`Where`メソッドには、web サーバーとデータベースではなく、メモリ内処理を実行するようになりました。</span><span class="sxs-lookup"><span data-stu-id="09503-172">The end result of applying the `Where` method is the same, but now the work is done in memory on the web server and not by the database.</span></span> <span data-ttu-id="09503-173">大量のデータを返すクエリでは、これはできない効率的です。</span><span class="sxs-lookup"><span data-stu-id="09503-173">For queries that return large volumes of data, this can be inefficient.</span></span>

> [!TIP]
> 
> <span data-ttu-id="09503-174">**Vs の IQueryable。IEnumerable**</span><span class="sxs-lookup"><span data-stu-id="09503-174">**IQueryable vs. IEnumerable**</span></span>
> 
> <span data-ttu-id="09503-175">実装した後、リポジトリ、次のように値を入力する場合でも、**検索**ボックス SQL Server に送信されるクエリでは、検索条件は含まれないため、すべての学生行が返されます。</span><span class="sxs-lookup"><span data-stu-id="09503-175">After you implement the repository as shown here, even if you enter something in the **Search** box the query sent to SQL Server returns all Student rows because it doesn't include your search criteria:</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> <span data-ttu-id="09503-176">このクエリはリポジトリ検索条件の詳細について知ることがなく、クエリを実行するためには、学生データのすべてを返します。</span><span class="sxs-lookup"><span data-stu-id="09503-176">This query returns all of the student data because the repository executed the query without knowing about the search criteria.</span></span> <span data-ttu-id="09503-177">プロセスは、並べ替え、検索条件を適用すること、およびメモリ内で行われます (この場合のみに 3 つの行を表示) ページング、データのサブセットを選択するときに後で、`ToPagedList`メソッドが、`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="09503-177">The process of sorting, applying search criteria, and selecting a subset of the data for paging (showing only 3 rows in this case) is done in memory later when the `ToPagedList` method is called on the `IEnumerable` collection.</span></span>
> 
> <span data-ttu-id="09503-178">(前に、リポジトリを実装する) コードの以前のバージョンで、クエリは送信されませんまでデータベースに、検索条件を適用した後ときに`ToPagedList`で呼び出される、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="09503-178">In the previous version of the code (before you implemented the repository), the query is not sent to the database until after you apply the search criteria, when `ToPagedList` is called on the `IQueryable` object.</span></span>
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> <span data-ttu-id="09503-179">ToPagedList が呼び出された場合、`IQueryable`オブジェクト、SQL Server に送信されるクエリは、検索文字列を指定し、検索条件を満たす行のみが返された結果として、およびフィルター処理する必要はありませんが、メモリ内で実行します。</span><span class="sxs-lookup"><span data-stu-id="09503-179">When ToPagedList is called on an `IQueryable` object, the query sent to SQL Server specifies the search string, and as a result only rows that meet the search criteria are returned, and no filtering needs to be done in memory.</span></span>
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> <span data-ttu-id="09503-180">(次のチュートリアルでは、SQL Server に送信されるクエリを調査する方法を説明します)。</span><span class="sxs-lookup"><span data-stu-id="09503-180">(The following tutorial explains how to examine queries sent to SQL Server.)</span></span>


<span data-ttu-id="09503-181">次のセクションでは、データベースでこの作業を行う必要がありますを指定するためのリポジトリ メソッドを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="09503-181">The following section shows how to implement repository methods that enable you to specify that this work should be done by the database.</span></span>

<span data-ttu-id="09503-182">コント ローラーと Entity Framework データベース コンテキストの間の抽象化レイヤーが作成されました。</span><span class="sxs-lookup"><span data-stu-id="09503-182">You've now created an abstraction layer between the controller and the Entity Framework database context.</span></span> <span data-ttu-id="09503-183">実装する単体テスト プロジェクトで別のリポジトリ クラスを作成することが自動化された単体テストでこのアプリケーションを実行する場合は、 `IStudentRepository`*します。*</span><span class="sxs-lookup"><span data-stu-id="09503-183">If you were going to perform automated unit testing with this application, you could create an alternative repository class in a unit test project that implements `IStudentRepository`*.*</span></span> <span data-ttu-id="09503-184">データを読み書きするコンテキストを呼び出す代わりにこのモック リポジトリ クラスは、コント ローラーの機能をテストするには、インメモリ コレクションを操作する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09503-184">Instead of calling the context to read and write data, this mock repository class could manipulate in-memory collections in order to test controller functions.</span></span>

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a><span data-ttu-id="09503-185">汎用リポジトリと単位作業クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="09503-185">Implement a Generic Repository and a Unit of Work Class</span></span>

<span data-ttu-id="09503-186">多くの冗長なコードは、結果でした各エンティティ型のリポジトリ クラスを作成したり、部分的な更新プログラムになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09503-186">Creating a repository class for each entity type could result in a lot of redundant code, and it could result in partial updates.</span></span> <span data-ttu-id="09503-187">たとえば、同じトランザクションの一部として 2 つの異なるエンティティ タイプを更新する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="09503-187">For example, suppose you have to update two different entity types as part of the same transaction.</span></span> <span data-ttu-id="09503-188">それぞれには、別のデータベース コンテキストのインスタンスが使用されている場合が成功した 1 つと、もう一方が失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09503-188">If each uses a separate database context instance, one might succeed and the other might fail.</span></span> <span data-ttu-id="09503-189">冗長なコードを最小限に抑える方法の 1 つは汎用的なリポジトリでは、および確認する方法を使用することのすべてのリポジトリと同じデータベース コンテキストを使用して (およびしたがってすべての更新プログラムを調整) はクラスの作業の単位を使用します。</span><span class="sxs-lookup"><span data-stu-id="09503-189">One way to minimize redundant code is to use a generic repository, and one way to ensure that all repositories use the same database context (and thus coordinate all updates) is to use a unit of work class.</span></span>

<span data-ttu-id="09503-190">チュートリアルのこのセクションで作成します、`GenericRepository`クラスと`UnitOfWork`クラス、およびそれらを使用して、`Course`両方にアクセスするコント ローラー、 `Department` 、`Course`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="09503-190">In this section of the tutorial, you'll create a `GenericRepository` class and a `UnitOfWork` class, and use them in the `Course` controller to access both the `Department` and the `Course` entity sets.</span></span> <span data-ttu-id="09503-191">前述のようをチュートリアルのこの部分をシンプルにするは作成して、これらのクラスのインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="09503-191">As explained earlier, to keep this part of the tutorial simple, you aren't creating interfaces for these classes.</span></span> <span data-ttu-id="09503-192">使用して TDD を容易にする場合は通常実装するインターフェイスを持つ、同じ方法が、`Student`リポジトリ。</span><span class="sxs-lookup"><span data-stu-id="09503-192">But if you were going to use them to facilitate TDD, you'd typically implement them with interfaces the same way you did the `Student` repository.</span></span>

### <a name="create-a-generic-repository"></a><span data-ttu-id="09503-193">汎用的なリポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="09503-193">Create a Generic Repository</span></span>

<span data-ttu-id="09503-194">*DAL*フォルダー作成*GenericRepository.cs*既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="09503-194">In the *DAL* folder, create *GenericRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="09503-195">データベース コンテキストとエンティティ セットのリポジトリがインスタンス化される、クラス変数が宣言されます。</span><span class="sxs-lookup"><span data-stu-id="09503-195">Class variables are declared for the database context and for the entity set that the repository is instantiated for:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="09503-196">コンス トラクターは、データベース コンテキスト インスタンスを受け取り、エンティティ セットの変数を初期化します。</span><span class="sxs-lookup"><span data-stu-id="09503-196">The constructor accepts a database context instance and initializes the entity set variable:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="09503-197">`Get`メソッドでは、ラムダ式を使用して、フィルター条件と列で、結果の順序を指定するには、呼び出し元のコードを許可して、文字列パラメーターにより、一括読み込みでナビゲーション プロパティのコンマ区切りのリストは、呼び出し元。</span><span class="sxs-lookup"><span data-stu-id="09503-197">The `Get` method uses lambda expressions to allow the calling code to specify a filter condition and a column to order the results by, and a string parameter lets the caller provide a comma-delimited list of navigation properties for eager loading:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="09503-198">コード`Expression<Func<TEntity, bool>> filter`呼び出し元がラムダ式に基づくを提供することを意味、`TEntity`型、およびこの式はブール値を返します。</span><span class="sxs-lookup"><span data-stu-id="09503-198">The code `Expression<Func<TEntity, bool>> filter` means the caller will provide a lambda expression based on the `TEntity` type, and this expression will return a Boolean value.</span></span> <span data-ttu-id="09503-199">リポジトリがのインスタンス化される場合など、`Student`エンティティ型の場合は、呼び出し元のメソッドのコードを指定`student => student.LastName == "Smith`&quot;の`filter`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="09503-199">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `student => student.LastName == "Smith`&quot; for the `filter` parameter.</span></span>

<span data-ttu-id="09503-200">コード`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`呼び出し元がラムダ式を渡すことも意味します。</span><span class="sxs-lookup"><span data-stu-id="09503-200">The code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` also means the caller will provide a lambda expression.</span></span> <span data-ttu-id="09503-201">式への入力は、ここでは、`IQueryable`オブジェクト、`TEntity`型。</span><span class="sxs-lookup"><span data-stu-id="09503-201">But in this case, the input to the expression is an `IQueryable` object for the `TEntity` type.</span></span> <span data-ttu-id="09503-202">式は、順序付けられたバージョンを返します`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="09503-202">The expression will return an ordered version of that `IQueryable` object.</span></span> <span data-ttu-id="09503-203">リポジトリがのインスタンス化される場合など、`Student`エンティティ型の場合は、呼び出し元のメソッドのコードを指定`q => q.OrderBy(s => s.LastName)`の`orderBy`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="09503-203">For example, if the repository is instantiated for the `Student` entity type, the code in the calling method might specify `q => q.OrderBy(s => s.LastName)` for the `orderBy` parameter.</span></span>

<span data-ttu-id="09503-204">コードでは、`Get`メソッドを作成、`IQueryable`オブジェクトし、1 つを使用する必要がある場合、フィルター式を適用します。</span><span class="sxs-lookup"><span data-stu-id="09503-204">The code in the `Get` method creates an `IQueryable` object and then applies the filter expression if there is one:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="09503-205">次に、コンマ区切りの一覧を解析した後、一括読み込みの式が適用されます。</span><span class="sxs-lookup"><span data-stu-id="09503-205">Next it applies the eager-loading expressions after parsing the comma-delimited list:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

<span data-ttu-id="09503-206">最後に、適用、`orderBy`式のいずれかを使用する必要がある場合の結果を返しますそれ以外の場合、順序なしのクエリから結果を返します。</span><span class="sxs-lookup"><span data-stu-id="09503-206">Finally, it applies the `orderBy` expression if there is one and returns the results; otherwise it returns the results from the unordered query:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="09503-207">呼び出すと、`Get`メソッド、フィルター処理とでの並べ替えを実行できます、`IEnumerable`これらの関数のパラメーターを提供するのではなく、メソッドによって返されるコレクション。</span><span class="sxs-lookup"><span data-stu-id="09503-207">When you call the `Get` method, you could do filtering and sorting on the `IEnumerable` collection returned by the method instead of providing parameters for these functions.</span></span> <span data-ttu-id="09503-208">ただし、web サーバー上のメモリで並べ替えおよびフィルター処理を行うことが。</span><span class="sxs-lookup"><span data-stu-id="09503-208">But the sorting and filtering work would then be done in memory on the web server.</span></span> <span data-ttu-id="09503-209">これらのパラメーターを使用すると、web サーバーではなく、データベースでの作業が実行されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="09503-209">By using these parameters, you ensure that the work is done by the database rather than the web server.</span></span> <span data-ttu-id="09503-210">特定のエンティティ型の派生クラスの作成を追加する特殊な代替策は`Get`メソッドなど`GetStudentsInNameOrder`または`GetStudentsByName`します。</span><span class="sxs-lookup"><span data-stu-id="09503-210">An alternative is to create derived classes for specific entity types and add specialized `Get` methods, such as `GetStudentsInNameOrder` or `GetStudentsByName`.</span></span> <span data-ttu-id="09503-211">ただし、複雑なアプリケーションの場合は、これにより、多数のような派生クラスと特殊なメソッド、維持するためにより多くの作業である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="09503-211">However, in a complex application, this can result in a large number of such derived classes and specialized methods, which could be more work to maintain.</span></span>

<span data-ttu-id="09503-212">内のコード、 `GetByID`、 `Insert`、および`Update`メソッドは、非ジェネリック リポジトリでの表示内容に似ています。</span><span class="sxs-lookup"><span data-stu-id="09503-212">The code in the `GetByID`, `Insert`, and `Update` methods is similar to what you saw in the non-generic repository.</span></span> <span data-ttu-id="09503-213">(一括読み込みパラメーターを提供することはありません、`GetByID`のシグネチャで一括読み込みを行うことはできませんので、`Find`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="09503-213">(You aren't providing an eager loading parameter in the `GetByID` signature, because you can't do eager loading with the `Find` method.)</span></span>

<span data-ttu-id="09503-214">2 つのオーバー ロードが用意されて、`Delete`メソッド。</span><span class="sxs-lookup"><span data-stu-id="09503-214">Two overloads are provided for the `Delete` method:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

<span data-ttu-id="09503-215">削除するエンティティの ID のみによりこれらのいずれかを渡すし、エンティティ インスタンスは、1 つ。</span><span class="sxs-lookup"><span data-stu-id="09503-215">One of these lets you pass in just the ID of the entity to be deleted, and one takes an entity instance.</span></span> <span data-ttu-id="09503-216">見たよう、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)同時実行処理する必要があるは、チュートリアル、`Delete`エンティティ インスタンスを受け取るメソッドには、追跡プロパティの元の値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="09503-216">As you saw in the [Handling Concurrency](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial, for concurrency handling you need a `Delete` method that takes an entity instance that includes the original value of a tracking property.</span></span>

<span data-ttu-id="09503-217">この汎用リポジトリでは、通常の CRUD 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="09503-217">This generic repository will handle typical CRUD requirements.</span></span> <span data-ttu-id="09503-218">特定のエンティティ型により複雑なフィルター処理や、順序付けなどの特別な要件がある場合は、派生クラスであり、その型の追加のメソッドを作成できます。</span><span class="sxs-lookup"><span data-stu-id="09503-218">When a particular entity type has special requirements, such as more complex filtering or ordering, you can create a derived class that has additional methods for that type.</span></span>

## <a name="creating-the-unit-of-work-class"></a><span data-ttu-id="09503-219">クラスの作業の単位を作成します。</span><span class="sxs-lookup"><span data-stu-id="09503-219">Creating the Unit of Work Class</span></span>

<span data-ttu-id="09503-220">クラスの作業の単位は 1 つの目的を果たします。 ように複数のリポジトリを使用すると、1 つのデータベース コンテキストを共有します。</span><span class="sxs-lookup"><span data-stu-id="09503-220">The unit of work class serves one purpose: to make sure that when you use multiple repositories, they share a single database context.</span></span> <span data-ttu-id="09503-221">これにより、作業単位が完了すると呼び出すことができます、`SaveChanges`コンテキストのインスタンスに対してメソッドし関連するすべての変更内容が調整することを保証します。</span><span class="sxs-lookup"><span data-stu-id="09503-221">That way, when a unit of work is complete you can call the `SaveChanges` method on that instance of the context and be assured that all related changes will be coordinated.</span></span> <span data-ttu-id="09503-222">クラス必要なものはすべて、`Save`メソッドと各リポジトリのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="09503-222">All that the class needs is a `Save` method and a property for each repository.</span></span> <span data-ttu-id="09503-223">リポジトリの各プロパティは、他のリポジトリ インスタンスとして同じデータベース コンテキスト インスタンスを使用してインスタンス化されているリポジトリ インスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="09503-223">Each repository property returns a repository instance that has been instantiated using the same database context instance as the other repository instances.</span></span>

<span data-ttu-id="09503-224">*DAL*フォルダー、という名前のクラス ファイルを作成する*UnitOfWork.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="09503-224">In the *DAL* folder, create a class file named *UnitOfWork.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="09503-225">コードは、データベース コンテキストと各リポジトリ クラス変数を作成します。</span><span class="sxs-lookup"><span data-stu-id="09503-225">The code creates class variables for the database context and each repository.</span></span> <span data-ttu-id="09503-226">`context`変数、新しいコンテキストをインスタンス化されます。</span><span class="sxs-lookup"><span data-stu-id="09503-226">For the `context` variable, a new context is instantiated:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="09503-227">リポジトリの各プロパティは、リポジトリが既に存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="09503-227">Each repository property checks whether the repository already exists.</span></span> <span data-ttu-id="09503-228">それ以外の場合は、コンテキスト インスタンスを渡して、リポジトリがインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="09503-228">If not, it instantiates the repository, passing in the context instance.</span></span> <span data-ttu-id="09503-229">その結果、すべてのリポジトリは、同じコンテキスト インスタンスを共有します。</span><span class="sxs-lookup"><span data-stu-id="09503-229">As a result, all repositories share the same context instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="09503-230">`Save`メソッド呼び出し`SaveChanges`データベース コンテキストでします。</span><span class="sxs-lookup"><span data-stu-id="09503-230">The `Save` method calls `SaveChanges` on the database context.</span></span>

<span data-ttu-id="09503-231">などの任意のクラスの変数では、データベース コンテキストをインスタンス化するクラス、`UnitOfWork`クラスが実装する`IDisposable`とコンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="09503-231">Like any class that instantiates a database context in a class variable, the `UnitOfWork` class implements `IDisposable` and disposes the context.</span></span>

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a><span data-ttu-id="09503-232">リポジトリおよび UnitOfWork クラスを使用するコースのコント ローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="09503-232">Changing the Course Controller to use the UnitOfWork Class and Repositories</span></span>

<span data-ttu-id="09503-233">現在のあるコードに置き換えます*CourseController.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="09503-233">Replace the code you currently have in *CourseController.cs* with the following code:</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

<span data-ttu-id="09503-234">このコードは追加のクラスの変数、`UnitOfWork`クラス。</span><span class="sxs-lookup"><span data-stu-id="09503-234">This code adds a class variable for the `UnitOfWork` class.</span></span> <span data-ttu-id="09503-235">(ここで、インターフェイスを使用していた場合、は、ここで変数を初期化するでしょう以外の場合と同様にコンス トラクターの 2 つのパターンを実装するは代わりに、、`Student`リポジトリです。)。</span><span class="sxs-lookup"><span data-stu-id="09503-235">(If you were using interfaces here, you wouldn't initialize the variable here; instead, you'd implement a pattern of two constructors just as you did for the `Student` repository.)</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

<span data-ttu-id="09503-236">クラスの残りの部分で、データベース コンテキストに対するすべての参照は置き換えられます、適切なリポジトリへの参照を使用して`UnitOfWork`リポジトリにアクセスするプロパティ。</span><span class="sxs-lookup"><span data-stu-id="09503-236">In the rest of the class, all references to the database context are replaced by references to the appropriate repository, using `UnitOfWork` properties to access the repository.</span></span> <span data-ttu-id="09503-237">`Dispose`メソッドを破棄します、`UnitOfWork`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="09503-237">The `Dispose` method disposes the `UnitOfWork` instance.</span></span>

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

<span data-ttu-id="09503-238">サイトを実行し、をクリックして、**コース**タブ。</span><span class="sxs-lookup"><span data-stu-id="09503-238">Run the site and click the **Courses** tab.</span></span>

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="09503-240">ページは、外観し、変更内容は、先ほどし、他のページのコースと同じ動作も同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="09503-240">The page looks and works the same as it did before your changes, and the other Course pages also work the same.</span></span>

## <a name="summary"></a><span data-ttu-id="09503-241">まとめ</span><span class="sxs-lookup"><span data-stu-id="09503-241">Summary</span></span>

<span data-ttu-id="09503-242">リポジトリと unit of work パターンの両方が実装されました。</span><span class="sxs-lookup"><span data-stu-id="09503-242">You have now implemented both the repository and unit of work patterns.</span></span> <span data-ttu-id="09503-243">汎用リポジトリ内のメソッド パラメーターとしてラムダ式を使用しています。</span><span class="sxs-lookup"><span data-stu-id="09503-243">You have used lambda expressions as method parameters in the generic repository.</span></span> <span data-ttu-id="09503-244">これらの式を使用する方法について、`IQueryable`オブジェクトを参照してください[IQueryable(T) インターフェイス (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="09503-244">For more information about how to use these expressions with an `IQueryable` object, see [IQueryable(T) Interface (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) in the MSDN Library.</span></span> <span data-ttu-id="09503-245">次のチュートリアルの一部を処理する方法について説明しますには、シナリオが高度な。</span><span class="sxs-lookup"><span data-stu-id="09503-245">In the next tutorial you'll learn how to handle some advanced scenarios.</span></span>

<span data-ttu-id="09503-246">その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="09503-246">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="09503-247">[前へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="09503-247">[Previous](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
