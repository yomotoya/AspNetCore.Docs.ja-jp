---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: モデルとコント ローラーの追加 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826798"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="9b90f-102">モデルとコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="9b90f-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9b90f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9b90f-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="9b90f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="9b90f-105">このセクションでは、データベースのエンティティを定義するモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="9b90f-106">これらのエンティティに対する CRUD 操作を実行する Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="9b90f-107">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-107">Add Model Classes</span></span>

<span data-ttu-id="9b90f-108">このチュートリアルで、データベース「Code First」アプローチを Entity Framework (EF) を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="9b90f-109">Code First でのデータベース テーブルに対応する c# クラスを作成して EF がデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="9b90f-110">(詳細については、次を参照してください[Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)。</span><span class="sxs-lookup"><span data-stu-id="9b90f-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="9b90f-111">まず、Poco (plain-old CLR object) としてのドメイン オブジェクトを定義します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="9b90f-112">次の Poco を作成します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="9b90f-113">作成者</span><span class="sxs-lookup"><span data-stu-id="9b90f-113">Author</span></span>
- <span data-ttu-id="9b90f-114">書籍</span><span class="sxs-lookup"><span data-stu-id="9b90f-114">Book</span></span>

<span data-ttu-id="9b90f-115">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="9b90f-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="9b90f-116">選択**追加**を選択し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="9b90f-117">クラスに `Author` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9b90f-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="9b90f-118">すべての Author.cs の定型コードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9b90f-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="9b90f-119">という名前の別のクラスを追加`Book`、次のコード。</span><span class="sxs-lookup"><span data-stu-id="9b90f-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="9b90f-120">Entity Framework では、これらのモデルを使用して、データベース テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="9b90f-121">各モデルの場合、`Id`プロパティは、データベース テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="9b90f-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="9b90f-122">Book クラスで、`AuthorId`への外部キーの定義、`Author`テーブル。</span><span class="sxs-lookup"><span data-stu-id="9b90f-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="9b90f-123">(わかりやすくするために、前提に話を各書籍には、1 つの作成者がいる。)Book クラスは、関連するナビゲーション プロパティも含まれています。`Author`します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="9b90f-124">ナビゲーション プロパティを使用するには、関連するへのアクセスに`Author`コード。</span><span class="sxs-lookup"><span data-stu-id="9b90f-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="9b90f-125">第 4 部でナビゲーション プロパティの詳細と言った[エンティティ関係の処理](part-4.md)します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="9b90f-126">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-126">Add Web API Controllers</span></span>

<span data-ttu-id="9b90f-127">このセクションでは、CRUD 操作をサポートする Web API コント ローラーを追加します (作成、読み取り、更新、および削除) します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="9b90f-128">コント ローラーでは、データベース層との通信に Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="9b90f-129">最初に、Controllers/ValuesController.cs ファイルを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="9b90f-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="9b90f-130">このファイルには、例の Web API コント ローラーが含まれていますが、このチュートリアルでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="9b90f-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="9b90f-131">次に、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="9b90f-131">Next, build the project.</span></span> <span data-ttu-id="9b90f-132">Web API のスキャフォールディングでは、リフレクションを使用して、コンパイル済みのアセンブリが必要があるため、モデル クラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="9b90f-133">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="9b90f-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9b90f-134">選択**追加**を選択し、**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="9b90f-135">**スキャフォールディングの追加**ダイアログ ボックスで、"Web API 2 コント ローラーとアクション、Entity Framework を使用"です。</span><span class="sxs-lookup"><span data-stu-id="9b90f-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="9b90f-136">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9b90f-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="9b90f-137">**コント ローラーの追加**ダイアログ ボックスで、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="9b90f-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="9b90f-138">**モデル クラス**] ドロップダウンで、[、`Author`クラス。</span><span class="sxs-lookup"><span data-stu-id="9b90f-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="9b90f-139">(ドロップダウンの一覧に表示されない場合、は、プロジェクトを作成することを確認します)。</span><span class="sxs-lookup"><span data-stu-id="9b90f-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="9b90f-140">「非同期コント ローラー アクションを使用する」を確認します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="9b90f-141">としてコント ローラーの名前をそのまま&quot;AuthorsController&quot;します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="9b90f-142">クリック プラス (+) ボタンを**データ コンテキスト クラス**します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="9b90f-143">**新しいデータ コンテキスト**ダイアログ ボックスで、既定の名前のままにし、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="9b90f-144">クリックして**追加**を完了する、**コント ローラーの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="9b90f-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="9b90f-145">ダイアログ ボックスでは、プロジェクトに 2 つのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="9b90f-146">`AuthorsController` Web API コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="9b90f-147">コント ローラーは、作成者の一覧に対する CRUD 操作を実行するクライアントを使用する REST API を実装します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="9b90f-148">`BookServiceContext` データベース、変更の追跡、およびデータの永続化から、データベースにデータを使用してオブジェクトの設定が含まれています。 ランタイム処理中にエンティティ オブジェクトを管理します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="9b90f-149">継承`DbContext`します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="9b90f-150">この時点では、プロジェクトを再度ビルドします。</span><span class="sxs-lookup"><span data-stu-id="9b90f-150">At this point, build the project again.</span></span> <span data-ttu-id="9b90f-151">API コント ローラーを追加する同じ手順に従って`Book`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="9b90f-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="9b90f-152">この時点で、`Book`既存を選択し、モデル クラスの`BookServiceContext`データ コンテキスト クラスのクラス。</span><span class="sxs-lookup"><span data-stu-id="9b90f-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="9b90f-153">(しないは新しいデータ コンテキストを作成します。)クリックして**追加**コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9b90f-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="9b90f-154">[前へ](part-1.md)
> [次へ](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="9b90f-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
