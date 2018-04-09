---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: エンティティのリレーションシップの処理 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="handling-entity-relations"></a><span data-ttu-id="0b04c-102">処理のエンティティ関係</span><span class="sxs-lookup"><span data-stu-id="0b04c-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="0b04c-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0b04c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0b04c-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="0b04c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0b04c-105">このセクションでは、EF が、関連するエンティティを読み込む方法と、モデル クラスでの循環のナビゲーション プロパティを処理する方法の詳細の一部について説明します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="0b04c-106">(このセクションでは、背景知識を提供し、チュートリアルを完了する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0b04c-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="0b04c-107">場合は、スキップ[パート 5](part-5.md)。)。</span><span class="sxs-lookup"><span data-stu-id="0b04c-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="0b04c-108">Eager 遅延読み込みと読み込み</span><span class="sxs-lookup"><span data-stu-id="0b04c-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="0b04c-109">EF とリレーショナル データベースを使用する場合は、EF が関連するデータを読み込む方法を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0b04c-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="0b04c-110">EF を生成する SQL クエリを確認するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="0b04c-111">SQL をトレースするには、次のコードの行を追加、`BookServiceContext`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="0b04c-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="0b04c-112">/Api/books に GET 要求を送信する場合は、次のような JSON が返されます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="0b04c-113">ブックが有効な著者の Id を含んでいるにもかかわらず Author プロパティが null であることがわかります。</span><span class="sxs-lookup"><span data-stu-id="0b04c-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="0b04c-114">EF が関連の作成者エンティティを読み込んでいないためにです。</span><span class="sxs-lookup"><span data-stu-id="0b04c-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="0b04c-115">SQL クエリのトレース ログは、これを確認します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="0b04c-116">SELECT ステートメントでは、ブックのテーブルから受け取るし、作成者の表を参照していません。</span><span class="sxs-lookup"><span data-stu-id="0b04c-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="0b04c-117">リファレンスについては、メソッドでは、ここで、`BooksController`書籍の一覧を表すクラス。</span><span class="sxs-lookup"><span data-stu-id="0b04c-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="0b04c-118">方法の作成者は、JSON データの一部として返すことができますを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="0b04c-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="0b04c-119">Entity Framework に関連するデータを読み込む 3 つの方法: 一括読み込み、遅延読み込み、および明示的な読み込みします。</span><span class="sxs-lookup"><span data-stu-id="0b04c-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="0b04c-120">各手法でのトレードオフがあるため、これらの動作を理解することが重要です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="0b04c-121">一括読み込み</span><span class="sxs-lookup"><span data-stu-id="0b04c-121">Eager Loading</span></span>

<span data-ttu-id="0b04c-122">*一括読み込み*EF は、データベースの最初のクエリの一部として関連エンティティを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="0b04c-123">一括読み込みを実行するのには、使用、 **System.Data.Entity.Include**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="0b04c-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="0b04c-124">これは、クエリに含めるデータを作成、EF を示しています。</span><span class="sxs-lookup"><span data-stu-id="0b04c-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="0b04c-125">この変更を行うし、アプリを実行すると場合、JSON データは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0b04c-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="0b04c-126">トレース ログは、EF が Book と作成者のテーブルに結合を実行することを示します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="0b04c-127">遅延読み込み</span><span class="sxs-lookup"><span data-stu-id="0b04c-127">Lazy Loading</span></span>

<span data-ttu-id="0b04c-128">遅延読み込みで、EF では、そのエンティティのナビゲーション プロパティが逆参照と関連するエンティティが自動的に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="0b04c-129">遅延読み込みを有効にするには、仮想ナビゲーション プロパティを確認します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="0b04c-130">たとえば、Book クラスで。</span><span class="sxs-lookup"><span data-stu-id="0b04c-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="0b04c-131">次のコードについて考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0b04c-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="0b04c-132">遅延読み込みを有効にするへのアクセス、`Author`プロパティ`books[0]`EF 作成者のデータベースを照会すると、します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="0b04c-133">遅延読み込みでは、EF は関連エンティティを取得するたびにクエリを送信するために、データベースの複数のトリップが必要です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="0b04c-134">一般に、遅延読み込みをシリアル化するオブジェクトを無効にします。</span><span class="sxs-lookup"><span data-stu-id="0b04c-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="0b04c-135">シリアライザーでは、すべてのプロパティを関連エンティティの読み込みをトリガーすると、モデルの読み取り。</span><span class="sxs-lookup"><span data-stu-id="0b04c-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="0b04c-136">EF 遅延読み込みで有効になっている書籍の一覧をシリアル化時に、SQL クエリをここではなどです。</span><span class="sxs-lookup"><span data-stu-id="0b04c-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="0b04c-137">EF が 3 つの作成者の 3 つの独立したクエリを行うことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="0b04c-138">遅延読み込みを使用する場合はまだあります.</span><span class="sxs-lookup"><span data-stu-id="0b04c-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="0b04c-139">一括読み込みでは、EF 非常に複雑な結合を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0b04c-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="0b04c-140">または、関連エンティティは、データの小さなサブセットにする必要があり、遅延読み込みを効率的になります。</span><span class="sxs-lookup"><span data-stu-id="0b04c-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="0b04c-141">シリアル化の問題を回避する方法の 1 つのエンティティ オブジェクトの代わりにデータ転送オブジェクト (Dto) をシリアル化を開始します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="0b04c-142">記事の後半でこの方法が紹介します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="0b04c-143">明示的な読み込み</span><span class="sxs-lookup"><span data-stu-id="0b04c-143">Explicit Loading</span></span>

<span data-ttu-id="0b04c-144">明示的な読み込み似ていますが、遅延読み込みをコードで明示的に、関連するデータを取得します。ナビゲーション プロパティにアクセスするときに自動的に発生しません。</span><span class="sxs-lookup"><span data-stu-id="0b04c-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="0b04c-145">明示的な読み込みでは、詳細な制御を関連するデータを読み込むときに利用できますが、余分なコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="0b04c-146">明示的な読み込みの詳細については、次を参照してください。[関連エンティティの読み込み](https://msdn.microsoft.com/data/jj574232#explicit)です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="0b04c-147">ナビゲーション プロパティと循環参照</span><span class="sxs-lookup"><span data-stu-id="0b04c-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="0b04c-148">ナビゲーション プロパティを定義した Book と作成者のモデルを定義したときに、`Book`ブックの作成者のリレーションシップのクラスが、逆方向にナビゲーション プロパティが定義されてはいなかった。</span><span class="sxs-lookup"><span data-stu-id="0b04c-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="0b04c-149">対応するナビゲーション プロパティを追加する場合の結果、`Author`クラスしますか?</span><span class="sxs-lookup"><span data-stu-id="0b04c-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="0b04c-150">残念ながら、モデルをシリアル化する際に問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="0b04c-151">関連するデータを読み込む場合は、オブジェクトの循環グラフが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="0b04c-152">JSON または XML フォーマッタは、グラフをシリアル化する際に、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="0b04c-153">2 つのフォーマッタは、別の例外のメッセージをスローします。</span><span class="sxs-lookup"><span data-stu-id="0b04c-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="0b04c-154">JSON のフォーマッタの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="0b04c-155">XML フォーマッタを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="0b04c-156">1 つのソリューションでは、次のセクションで説明する DTOs を使用します。</span><span class="sxs-lookup"><span data-stu-id="0b04c-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="0b04c-157">また、グラフのサイクルを処理する JSON および XML フォーマッタを構成できます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="0b04c-158">詳細については、次を参照してください。[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)です。</span><span class="sxs-lookup"><span data-stu-id="0b04c-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="0b04c-159">このチュートリアルでは、する必要はありません、`Author.Book`ナビゲーション プロパティ、ようにおくことができます。</span><span class="sxs-lookup"><span data-stu-id="0b04c-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b04c-160">[前へ](part-3.md)
> [次へ](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="0b04c-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
