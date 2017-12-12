---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "データ転送オブジェクト (Dto) を作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="8c48b-102">データ転送オブジェクト (Dto) を作成します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="8c48b-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8c48b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8c48b-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8c48b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8c48b-105">右側、web API、クライアントへのデータベース エンティティを公開します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="8c48b-106">クライアントは、データベース テーブルに直接マップされるデータを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8c48b-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="8c48b-107">ただし、れていない常にことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8c48b-107">However, that's not always a good idea.</span></span> <span data-ttu-id="8c48b-108">クライアントに送信されるデータの形状を変更する場合があります。</span><span class="sxs-lookup"><span data-stu-id="8c48b-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="8c48b-109">たとえば、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="8c48b-109">For example, you might want to:</span></span>

- <span data-ttu-id="8c48b-110">(前のセクションを参照してください) の循環参照を削除します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="8c48b-111">表示するのには、クライアントは想定されていない特定のプロパティを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="8c48b-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="8c48b-112">ペイロードのサイズを軽減するためにいくつかのプロパティを省略します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="8c48b-113">クライアントの方が便利にする、入れ子になったオブジェクトを格納するオブジェクト グラフを平坦化します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="8c48b-114">"過剰ポスティングの脆弱性を回避します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="8c48b-115">(を参照してください[モデルの検証](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過剰ポスティングのディスカッションのです)。</span><span class="sxs-lookup"><span data-stu-id="8c48b-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="8c48b-116">データベース層から、サービス層を分離します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="8c48b-117">これを実現することができますを定義する、*データ転送オブジェクト*(DTO)。</span><span class="sxs-lookup"><span data-stu-id="8c48b-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="8c48b-118">DTO は、ネットワーク経由でデータを送信する方法を定義するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="8c48b-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="8c48b-119">Book エンティティで動作する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="8c48b-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="8c48b-120">Models フォルダーには、2 つの DTO クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="8c48b-121">`BookDetailDTO`クラスには、すべての点を除いて、Book モデルからのプロパティが含まれます`AuthorName`作成者の名前を保持する文字列です。</span><span class="sxs-lookup"><span data-stu-id="8c48b-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="8c48b-122">`BookDTO`クラスからプロパティのサブセットに含まれる`BookDetailDTO`です。</span><span class="sxs-lookup"><span data-stu-id="8c48b-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="8c48b-123">2 つの GET メソッドを次に、置換、 `BooksController` dtos の使用を返すバージョンのクラスです。</span><span class="sxs-lookup"><span data-stu-id="8c48b-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="8c48b-124">LINQ を使用して**選択**ステートメントを Book エンティティからの dtos の使用に変換します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="8c48b-125">ここでは、新しいによって生成された SQL`GetBooks`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8c48b-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="8c48b-126">EF に LINQ が変換を参照してください**選択**SQL SELECT ステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="8c48b-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="8c48b-127">最後に、変更、 `PostBook` DTO を返します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="8c48b-128">このチュートリアルではお変換 dtos の使用を手動でのコードにします。</span><span class="sxs-lookup"><span data-stu-id="8c48b-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="8c48b-129">別のオプションはのようなライブラリを使用する[AutoMapper](http://automapper.org/)変換を自動的に処理します。</span><span class="sxs-lookup"><span data-stu-id="8c48b-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8c48b-130">[前へ](part-4.md)
[次へ](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="8c48b-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
