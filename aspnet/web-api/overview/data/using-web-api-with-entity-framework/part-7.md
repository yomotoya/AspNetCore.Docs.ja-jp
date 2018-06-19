---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Create View (UI) |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878803"
---
<a name="create-the-view-ui"></a><span data-ttu-id="f1606-102">ビュー (UI) を作成します。</span><span class="sxs-lookup"><span data-stu-id="f1606-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="f1606-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f1606-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f1606-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="f1606-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="f1606-105">このセクションで、アプリの HTML を定義し、HTML ビューとモデルとの間のデータ バインディングの追加を開始します。</span><span class="sxs-lookup"><span data-stu-id="f1606-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="f1606-106">Views/Home/Index.cshtml ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f1606-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="f1606-107">次のように、そのファイルの内容全体を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f1606-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="f1606-108">ほとんどの`div`の要素がある[ブートス トラップ](http://getbootstrap.com/)のスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="f1606-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="f1606-109">重要な要素のものである`data-bind`属性。</span><span class="sxs-lookup"><span data-stu-id="f1606-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="f1606-110">この属性は、HTML をビュー モデルにリンクします。</span><span class="sxs-lookup"><span data-stu-id="f1606-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="f1606-111">例えば:</span><span class="sxs-lookup"><span data-stu-id="f1606-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="f1606-112">この例では、 &quot; `text` &quot;原因のバインド、`<p>`要素の値を表示する、`error`ビュー モデルからのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="f1606-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="f1606-113">注意してください`error`として宣言されている、 `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="f1606-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="f1606-114">新しい値を代入するときに`error`、Knockout 内のテキストを更新する、`<p>`要素。</span><span class="sxs-lookup"><span data-stu-id="f1606-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="f1606-115">`foreach`バインディング通知の内容をループする Knockout、`books`配列。</span><span class="sxs-lookup"><span data-stu-id="f1606-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="f1606-116">Knockout、配列内の各項目の新規作成&lt;li&gt;要素。</span><span class="sxs-lookup"><span data-stu-id="f1606-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="f1606-117">コンテキスト内のバインド、`foreach`配列項目のプロパティを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1606-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="f1606-118">例えば:</span><span class="sxs-lookup"><span data-stu-id="f1606-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="f1606-119">ここで、`text`バインディングは、各書籍の著者プロパティを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="f1606-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="f1606-120">今すぐアプリケーションを実行する場合は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f1606-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="f1606-121">書籍の一覧は、ページが読み込まれると、非同期的に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="f1606-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="f1606-122">今すぐ、&quot;詳細&quot;リンクは機能しません。</span><span class="sxs-lookup"><span data-stu-id="f1606-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="f1606-123">次のセクションでこの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="f1606-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1606-124">[前へ](part-6.md)
> [次へ](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="f1606-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
