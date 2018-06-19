---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: データベースへの新しい項目の追加 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868374"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="ed42d-102">データベースに新しい項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed42d-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="ed42d-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ed42d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ed42d-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="ed42d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ed42d-105">このセクションでは、ユーザーが新しいブックを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed42d-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="ed42d-106">App.js では、ビュー モデルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed42d-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="ed42d-107">Index.cshtml、次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ed42d-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="ed42d-108">代入:</span><span class="sxs-lookup"><span data-stu-id="ed42d-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="ed42d-109">このマークアップでは、新しい著者を送信するためのフォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed42d-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="ed42d-110">作成者のドロップダウン リストの値は、データ バインドを`authors`ビュー モデルで監視可能な。</span><span class="sxs-lookup"><span data-stu-id="ed42d-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="ed42d-111">他のフォーム入力の値がデータ バインドを`newBook`ビュー モデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="ed42d-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="ed42d-112">フォームの送信ハンドラーにバインドされて、`addBook`関数。</span><span class="sxs-lookup"><span data-stu-id="ed42d-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="ed42d-113">`addBook`関数は、JSON オブジェクトを作成するデータ バインド フォームの入力の現在の値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="ed42d-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="ed42d-114">JSON オブジェクトをポストし、`/api/books`です。</span><span class="sxs-lookup"><span data-stu-id="ed42d-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed42d-115">[前へ](part-8.md)
> [次へ](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="ed42d-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
