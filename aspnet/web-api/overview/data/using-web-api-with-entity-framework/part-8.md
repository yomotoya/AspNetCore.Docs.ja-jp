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
ms.locfileid: "30868088"
---
<a name="display-item-details"></a>項目の詳細を表示
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、各書籍の詳細を表示する機能を追加します。 App.js、ビュー モデルには、次のコードを追加します。

[!code-javascript[Main](part-8/samples/sample1.js)]

Views/Home/Index.cshtml では、[詳細] にデータ バインド要素を追加します。

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

クリック ハンドラーのバインド、 &lt;、&gt;要素を`getBookDetail`ビュー モデルの関数。

同じファイルでは、次のマークアップを置き換えます。

[!code-html[Main](part-8/samples/sample3.html)]

以下に置き換えます。

[!code-html[Main](part-8/samples/sample4.html)]

このマークアップがバインドされているデータのプロパティにテーブルを作成、`detail`ビュー モデルで監視可能な。

"&lt;!--Ko--&gt; &quot;構文では、DOM 要素の外側 Knockout バインドを追加できます。 この場合、`if`バインディングによって表示されるマークアップのこのセクションでされる場合にのみ`details`以外の場合します。

[!code-html[Main](part-8/samples/sample5.html)]

今すぐアプリを実行するのいずれかをクリックすると、&quot;詳細&quot;書籍の詳細をアプリのリンクが表示されます。

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [前へ](part-7.md)
> [次へ](part-9.md)
