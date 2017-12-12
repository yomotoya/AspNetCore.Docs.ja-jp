---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "項目の詳細の表示 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>項目の詳細を表示
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](https://github.com/MikeWasson/BookService)

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

>[!div class="step-by-step"]
[前へ](part-7.md)
[次へ](part-9.md)
