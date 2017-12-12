---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "Create View (UI) |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>ビュー (UI) を作成します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](https://github.com/MikeWasson/BookService)

このセクションで、アプリの HTML を定義し、HTML ビューとモデルとの間のデータ バインディングの追加を開始します。

Views/Home/Index.cshtml ファイルを開きます。 次のように、そのファイルの内容全体を置き換えます。

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

ほとんどの`div`の要素がある[ブートス トラップ](http://getbootstrap.com/)のスタイルを設定します。 重要な要素のものである`data-bind`属性。 この属性は、HTML をビュー モデルにリンクします。

例:

[!code-html[Main](part-7/samples/sample2.html)]

この例では、 &quot; `text` &quot;原因のバインド、`<p>`要素の値を表示する、`error`ビュー モデルからのプロパティです。 注意してください`error`として宣言されている、 `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

新しい値を代入するときに`error`、Knockout 内のテキストを更新する、`<p>`要素。

`foreach`バインディング通知の内容をループする Knockout、`books`配列。 Knockout、配列内の各項目の新規作成&lt;li&gt;要素。 コンテキスト内のバインド、`foreach`配列項目のプロパティを参照してください。 例:

[!code-html[Main](part-7/samples/sample4.html)]

ここで、`text`バインディングは、各書籍の著者プロパティを読み取ります。

今すぐアプリケーションを実行する場合は、次のようになります。

![](part-7/_static/image1.png)

書籍の一覧は、ページが読み込まれると、非同期的に読み込みます。 今すぐ、&quot;詳細&quot;リンクは機能しません。 次のセクションでこの機能を追加します。

>[!div class="step-by-step"]
[前へ](part-6.md)
[次へ](part-8.md)
