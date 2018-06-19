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
<a name="add-a-new-item-to-the-database"></a>データベースに新しい項目を追加します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、ユーザーが新しいブックを作成する機能を追加します。 App.js では、ビュー モデルに次のコードを追加します。

[!code-javascript[Main](part-9/samples/sample1.js)]

Index.cshtml、次のマークアップに置き換えます。

[!code-html[Main](part-9/samples/sample2.html)]

代入:

[!code-html[Main](part-9/samples/sample3.html)]

このマークアップでは、新しい著者を送信するためのフォームを作成します。 作成者のドロップダウン リストの値は、データ バインドを`authors`ビュー モデルで監視可能な。 他のフォーム入力の値がデータ バインドを`newBook`ビュー モデルのプロパティです。

フォームの送信ハンドラーにバインドされて、`addBook`関数。

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`関数は、JSON オブジェクトを作成するデータ バインド フォームの入力の現在の値を読み取ります。 JSON オブジェクトをポストし、`/api/books`です。

> [!div class="step-by-step"]
> [前へ](part-8.md)
> [次へ](part-10.md)
