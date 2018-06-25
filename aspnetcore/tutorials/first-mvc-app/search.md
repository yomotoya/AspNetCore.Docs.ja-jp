---
title: 検索の追加
author: rick-anderson
description: 単純な ASP.NET Core MVC アプリに検索を追加する方法を紹介します
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: fb93d9688c9abf76ad0057c646c4b7662d003108
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274841"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="79002-103">**rename** コマンドで `searchString` パラメーターの名前を `id` に簡単に変更できます。</span><span class="sxs-lookup"><span data-stu-id="79002-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="79002-104">`searchString` を右クリックし、**[名前の変更]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79002-104">Right click on `searchString` **> Rename**.</span></span>

![コンテキスト メニュー](search/_static/rename.png)

<span data-ttu-id="79002-106">名前変更ターゲットが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="79002-106">The rename targets are highlighted.</span></span>

![コード エディター。Index ActionResult メソッド全体で変数が強調表示されています。](search/_static/rename2.png)

<span data-ttu-id="79002-108">パラメーターを `id` に変更します。`searchString` がすべて `id` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="79002-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![コード エディター。変数が ID に変更されています。](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="79002-110">intelliSense がマークアップの更新に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="79002-110">Notice how intelliSense helps us update the markup.</span></span>

![Intellisense コンテキスト メニュー。フォーム要素の属性一覧でメソッドが選択されています。](search/_static/int_m.png)

![Intellisense コンテキスト メニュー。メソッド属性値の一覧で get が選択されています。](search/_static/int_get.png)

<span data-ttu-id="79002-113">`<form>` タグの特徴的なフォントにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="79002-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="79002-114">このタグが[タグ ヘルパー](~/mvc/views/tag-helpers/intro.md)対応であることを示しています。</span><span class="sxs-lookup"><span data-stu-id="79002-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![紫色のテキストを含む form タグ](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="79002-116">[前へ](controller-methods-views.md)
> [次へ](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="79002-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
