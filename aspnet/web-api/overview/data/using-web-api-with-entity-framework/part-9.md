---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: データベースに新しい項目の追加 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: b1f7935c70efcc3ee486e76fc356ff43716632dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368878"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="529d8-102">データベースに新しい項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="529d8-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="529d8-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="529d8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="529d8-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="529d8-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="529d8-105">このセクションでは、ユーザーが新しいブックを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="529d8-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="529d8-106">App.js では、ビュー モデルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="529d8-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="529d8-107">Index.cshtml、次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="529d8-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="529d8-108">代入:</span><span class="sxs-lookup"><span data-stu-id="529d8-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="529d8-109">このマークアップは、新しい執筆者に送信するためのフォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="529d8-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="529d8-110">作成者のドロップダウン リストの値は、データ バインドを`authors`ビュー モデルで監視可能な。</span><span class="sxs-lookup"><span data-stu-id="529d8-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="529d8-111">その他のフォーム入力では、値はデータ バインドを`newBook`ビュー モデルのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="529d8-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="529d8-112">フォームの送信ハンドラーにバインドされて、`addBook`関数。</span><span class="sxs-lookup"><span data-stu-id="529d8-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="529d8-113">`addBook`関数は JSON オブジェクトを作成するデータ バインド フォームの入力の現在の値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="529d8-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="529d8-114">JSON オブジェクトを投稿し、`/api/books`します。</span><span class="sxs-lookup"><span data-stu-id="529d8-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="529d8-115">[前へ](part-8.md)
> [次へ](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="529d8-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
