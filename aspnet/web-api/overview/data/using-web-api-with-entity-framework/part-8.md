---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: 項目の詳細の表示 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="display-item-details"></a><span data-ttu-id="0301f-102">項目の詳細を表示</span><span class="sxs-lookup"><span data-stu-id="0301f-102">Display Item Details</span></span>
====================
<span data-ttu-id="0301f-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0301f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0301f-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="0301f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0301f-105">このセクションでは、各書籍の詳細を表示する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="0301f-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="0301f-106">App.js、ビュー モデルには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0301f-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="0301f-107">Views/Home/Index.cshtml では、[詳細] にデータ バインド要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="0301f-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="0301f-108">クリック ハンドラーのバインド、 &lt;、&gt;要素を`getBookDetail`ビュー モデルの関数。</span><span class="sxs-lookup"><span data-stu-id="0301f-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="0301f-109">同じファイルでは、次のマークアップを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0301f-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="0301f-110">以下に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0301f-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="0301f-111">このマークアップがバインドされているデータのプロパティにテーブルを作成、`detail`ビュー モデルで監視可能な。</span><span class="sxs-lookup"><span data-stu-id="0301f-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="0301f-112">"&lt;!--Ko--&gt; &quot;構文では、DOM 要素の外側 Knockout バインドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0301f-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="0301f-113">この場合、`if`バインディングによって表示されるマークアップのこのセクションでされる場合にのみ`details`以外の場合します。</span><span class="sxs-lookup"><span data-stu-id="0301f-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="0301f-114">今すぐアプリを実行するのいずれかをクリックすると、&quot;詳細&quot;書籍の詳細をアプリのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0301f-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="0301f-115">[前へ](part-7.md)
> [次へ](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="0301f-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
