---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: データベースに新しい項目の追加 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818509"
---
<a name="add-a-new-item-to-the-database"></a>データベースに新しい項目を追加します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、ユーザーが新しいブックを作成する機能を追加します。 App.js では、ビュー モデルに次のコードを追加します。

[!code-javascript[Main](part-9/samples/sample1.js)]

Index.cshtml、次のマークアップに置き換えます。

[!code-html[Main](part-9/samples/sample2.html)]

代入:

[!code-html[Main](part-9/samples/sample3.html)]

このマークアップは、新しい執筆者に送信するためのフォームを作成します。 作成者のドロップダウン リストの値は、データ バインドを`authors`ビュー モデルで監視可能な。 その他のフォーム入力では、値はデータ バインドを`newBook`ビュー モデルのプロパティ。

フォームの送信ハンドラーにバインドされて、`addBook`関数。

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`関数は JSON オブジェクトを作成するデータ バインド フォームの入力の現在の値を読み取ります。 JSON オブジェクトを投稿し、`/api/books`します。

> [!div class="step-by-step"]
> [前へ](part-8.md)
> [次へ](part-10.md)
