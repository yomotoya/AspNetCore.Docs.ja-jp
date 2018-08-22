---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: 項目の詳細の表示 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 83f291326307a4964afdd5e8b50f2c375348ed0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833329"
---
<a name="display-item-details"></a>項目の詳細を表示
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、各書籍の詳細を表示する機能を追加します。 App.js では、次のコードをビュー モデルに追加します。

[!code-javascript[Main](part-8/samples/sample1.js)]

Views/Home/Index.cshtml で、[詳細] リンクをデータ バインド要素を追加します。

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

バインドの click ハンドラー、 &lt;、&gt;要素を`getBookDetail`ビュー モデルの関数。

同じファイルで次のマークアップに置き換えます。

[!code-html[Main](part-8/samples/sample3.html)]

以下に置き換えます。

[!code-html[Main](part-8/samples/sample4.html)]

このマークアップは、のプロパティにデータ バインドするテーブルを作成、`detail`ビュー モデルで監視可能な。

"&lt;!--Ko--&gt; &quot;構文は、DOM 要素の外部で Knockout バインディングを含めることができます。 ここで、`if`バインドによって表示されるマークアップのこのセクションで場合にのみ`details`以外の場合します。

[!code-html[Main](part-8/samples/sample5.html)]

ここで、アプリの実行の 1 つをクリックすると、&quot;詳細&quot;書籍の詳細をアプリのリンクが表示されます。

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [前へ](part-7.md)
> [次へ](part-9.md)
