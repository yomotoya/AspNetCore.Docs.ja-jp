---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "モデルおよびコント ローラーの追加 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: b75eae11fd99b60864256f79d4770a3007487964
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="add-models-and-controllers"></a><span data-ttu-id="d11e3-102">モデルおよびコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="d11e3-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d11e3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d11e3-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d11e3-105">このセクションでは、データベースのエンティティを定義するモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="d11e3-106">それらのエンティティに対する CRUD 操作を実行する Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="d11e3-107">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-107">Add Model Classes</span></span>

<span data-ttu-id="d11e3-108">このチュートリアルでは、データベース"Code First"アプローチ Entity Framework (EF) を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="d11e3-109">Code First では、データベース テーブルに対応する c# クラスを作成して EF には、データベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d11e3-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="d11e3-110">(詳細については、次を参照してください[Entity Framework 開発方法](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)。</span><span class="sxs-lookup"><span data-stu-id="d11e3-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="d11e3-111">した poco から (従来の CLR オブジェクト) として、ドメイン オブジェクトを定義することで開始します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="d11e3-112">次のした poco からを作成されます。</span><span class="sxs-lookup"><span data-stu-id="d11e3-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="d11e3-113">作成者</span><span class="sxs-lookup"><span data-stu-id="d11e3-113">Author</span></span>
- <span data-ttu-id="d11e3-114">書籍</span><span class="sxs-lookup"><span data-stu-id="d11e3-114">Book</span></span>

<span data-ttu-id="d11e3-115">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="d11e3-116">選択**追加**選択してから、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="d11e3-117">クラスに `Author` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d11e3-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="d11e3-118">次のコードでは、すべての Author.cs に定型コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d11e3-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="d11e3-119">という名前の別のクラスを追加`Book`、次のコードにします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="d11e3-120">Entity Framework では、これらのモデルをデータベース テーブルの作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="d11e3-121">モデルごとに、`Id`プロパティは、データベース テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="d11e3-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="d11e3-122">Book クラスで、`AuthorId`への外部キーの定義、`Author`テーブル。</span><span class="sxs-lookup"><span data-stu-id="d11e3-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="d11e3-123">(わかりやすくするため、すればいると仮定すると書籍ごとに 1 つの作成者です。)Book クラスには、関連するナビゲーション プロパティも含まれています`Author`です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="d11e3-124">ナビゲーション プロパティを使用するには、関連するアクセス`Author`のコードにします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="d11e3-125">第 4 部でのナビゲーション プロパティの詳細音声[処理エンティティ関係](part-4.md)です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="d11e3-126">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-126">Add Web API Controllers</span></span>

<span data-ttu-id="d11e3-127">このセクションでは、CRUD 操作をサポートする Web API コント ローラーを追加します (作成、読み取り、更新、および削除) します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="d11e3-128">コント ローラーでは、Entity Framework をデータベース層との通信に使用します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="d11e3-129">最初に、Controllers/ValuesController.cs ファイルを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="d11e3-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="d11e3-130">このファイルには、例の Web API コント ローラーが含まれていますが、このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d11e3-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="d11e3-131">次に、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-131">Next, build the project.</span></span> <span data-ttu-id="d11e3-132">Web API のスキャフォールディングでは、リフレクションを使用して、モデル クラスを見つけるため、コンパイルされたアセンブリが必要があります。</span><span class="sxs-lookup"><span data-stu-id="d11e3-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="d11e3-133">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="d11e3-134">選択**追加**選択してから、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="d11e3-135">**追加 Scaffold**ダイアログで、"Web API 2 Entity Framework を使用してコント ローラーをアクションがある"です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="d11e3-136">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="d11e3-137">**コント ローラーの追加**ダイアログ ボックスで、次の操作します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="d11e3-138">**モデル クラス**ドロップダウンで、、`Author`クラスです。</span><span class="sxs-lookup"><span data-stu-id="d11e3-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="d11e3-139">(が、ドロップダウン リストに表示されることが表示されない場合は、プロジェクトを作成することを確認) します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="d11e3-140">「を使用して非同期コント ローラーのアクション」を確認してください。</span><span class="sxs-lookup"><span data-stu-id="d11e3-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="d11e3-141">としてコント ローラーの名前をそのまま&quot;AuthorsController&quot;です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="d11e3-142">クリック プラス (+) ボタンを**データ コンテキスト クラス**です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="d11e3-143">**新しいデータ コンテキスト**ダイアログ ボックスで、既定の名前のままにし、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="d11e3-144">をクリックして**追加**を完了する、**コント ローラーの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="d11e3-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="d11e3-145">ダイアログ ボックスでは、2 つのクラスをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="d11e3-146">`AuthorsController`Web API コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="d11e3-147">コント ローラーでは、クライアントの作成者の一覧での CRUD 操作を実行に使用する REST API を実装します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="d11e3-148">`BookServiceContext`実行中に、データベース、変更の追跡、およびデータの永続化から、データベースにデータを使用してオブジェクトの設定が含まれるエンティティ オブジェクトを管理します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="d11e3-149">継承`DbContext`です。</span><span class="sxs-lookup"><span data-stu-id="d11e3-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="d11e3-150">この時点では、プロジェクトを再度ビルドします。</span><span class="sxs-lookup"><span data-stu-id="d11e3-150">At this point, build the project again.</span></span> <span data-ttu-id="d11e3-151">ここでの API コント ローラーを追加する同じ手順を進める`Book`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="d11e3-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="d11e3-152">この時点で、`Book`モデル クラス、および既存の選択の`BookServiceContext`データ コンテキスト クラスのクラスです。</span><span class="sxs-lookup"><span data-stu-id="d11e3-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="d11e3-153">(しないは新しいデータ コンテキストを作成します。)をクリックして**追加**コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="d11e3-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
<span data-ttu-id="d11e3-154">[前へ](part-1.md)
[次へ](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="d11e3-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
