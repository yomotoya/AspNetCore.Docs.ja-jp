---
title: "検索の追加"
author: rick-anderson
description: "単純な ASP.NET Core MVC アプリに検索を追加する方法を紹介します"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

**rename** コマンドで `searchString` パラメーターの名前を `id` に簡単に変更できます。 `searchString` を右クリックし、**[名前の変更]** をクリックします。

![コンテキスト メニュー](search/_static/rename.png)

名前変更ターゲットが強調表示されます。

![コード エディター。Index ActionResult メソッド全体で変数が強調表示されています。](search/_static/rename2.png)

パラメーターを `id` に変更します。`searchString` がすべて `id` に変更されます。

![コード エディター。変数が ID に変更されています。](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

intelliSense がマークアップの更新に役立ちます。

![Intellisense コンテキスト メニュー。フォーム要素の属性一覧でメソッドが選択されています。](search/_static/int_m.png)

![Intellisense コンテキスト メニュー。メソッド属性値の一覧で get が選択されています。](search/_static/int_get.png)

`<form>` タグの特徴的なフォントにご注意ください。 このタグが[タグ ヘルパー](../../mvc/views/tag-helpers/intro.md)対応であることを示しています。

![紫色のテキストを含む form タグ](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[前へ](controller-methods-views.md)
[次へ](new-field.md)  
