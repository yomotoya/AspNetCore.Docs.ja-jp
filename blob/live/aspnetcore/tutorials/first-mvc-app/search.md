---
title: "検索の追加"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリに検索を追加する方法を紹介します"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: aaa00a9eedda73a2c66da30b5ace3b5b5a7760b2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="c00b7-103">**rename** コマンドで `searchString` パラメーターの名前を `id` に簡単に変更できます。</span><span class="sxs-lookup"><span data-stu-id="c00b7-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="c00b7-104">`searchString` を右クリックし、**[名前の変更]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c00b7-104">Right click on `searchString` **> Rename**.</span></span>

![コンテキスト メニュー](search/_static/rename.png)

<span data-ttu-id="c00b7-106">名前変更ターゲットが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="c00b7-106">The rename targets are highlighted.</span></span>

![コード エディター。Index ActionResult メソッド全体で変数が強調表示されています。](search/_static/rename2.png)

<span data-ttu-id="c00b7-108">パラメーターを `id` に変更します。`searchString` がすべて `id` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="c00b7-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![コード エディター。変数が ID に変更されています。](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="c00b7-110">intelliSense がマークアップの更新に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c00b7-110">Notice how intelliSense helps us update the markup.</span></span>

![Intellisense コンテキスト メニュー。フォーム要素の属性一覧でメソッドが選択されています。](search/_static/int_m.png)

![Intellisense コンテキスト メニュー。メソッド属性値の一覧で get が選択されています。](search/_static/int_get.png)

<span data-ttu-id="c00b7-113">`<form>` タグの特徴的なフォントにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="c00b7-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="c00b7-114">このタグが[タグ ヘルパー](../../mvc/views/tag-helpers/intro.md)対応であることを示しています。</span><span class="sxs-lookup"><span data-stu-id="c00b7-114">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![紫色のテキストを含む form タグ](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="c00b7-116">[前へ](controller-methods-views.md)
[次へ](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="c00b7-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
