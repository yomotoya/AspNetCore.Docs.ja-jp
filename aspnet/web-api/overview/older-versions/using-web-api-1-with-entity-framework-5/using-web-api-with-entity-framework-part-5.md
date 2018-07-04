---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'パート 5: Knockout.js で動的 UI を作成する |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 1f645b853fba999ade11c420eceba1b310f80ca3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368429"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>パート 5: Knockout.js で動的 UI を作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Knockout.js で動的 UI の作成

このセクションで、管理ビューの機能を追加するのに Knockout.js 使用します。

[Knockout.js](http://knockoutjs.com/)をデータに HTML コントロールをバインドするが容易にする Javascript ライブラリです。 Knockout.js では、モデル-ビュー-ビューモデル (MVVM) パターンを使用します。

- *モデル*(の場合、製品および orders) で、ビジネス ドメイン内のデータのサーバー側の表現です。
- *ビュー*はプレゼンテーション層 (HTML) です。
- *ビュー モデル*はモデル データを保持する Javascript オブジェクトです。 ビュー モデルは、UI のコードの抽象化です。 HTML 形式の知識がありません。 代わりに、「アイテムの一覧」など、ビューの抽象の機能を表します。

ビューはデータ バインド ビュー モデルにします。 ビュー モデルに更新プログラムは、ビューで自動的に反映されます。 ビュー モデルは、ボタンのクリックなど、ビューからイベントを取得し、注文を作成するなど、モデルの操作を実行します。

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

最初に、ビュー モデルを定義します。 その後は、ビュー モデルに、HTML マークアップをバインドします。

Admin.cshtml には、次の Razor セクションを追加します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

任意の場所、ファイルのこのセクションを追加できます。 ときに、ビューが表示される、HTML ページの下部のセクションが表示されますが、終了する前に右&lt;/body&gt;タグ。

このページのスクリプトのすべてが、コメントで示されたスクリプト タグ内で参照してください。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

最初に、ビュー モデル クラスを定義します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray**オブジェクトと呼ばれる、Knockout での特殊なは、*オブザーバブル*します。 [Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html): オブザーバブルは「を JavaScript オブジェクトの変更についてサブスクライバーに通知できます」。 可能なオブジェクトの内容を変更するときに、ビューが一致するように自動的に更新します。

設定する、`products`配列、web API への AJAX 要求を作成します。 ビュー バッグで API のベース URI を格納することを思い出してください (を参照してください[パート 4](using-web-api-with-entity-framework-part-4.md)チュートリアルの)。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

次に、作成、更新、および製品を削除するビュー モデルに関数を追加します。 これらの関数は、web API への AJAX 呼び出しを送信し、結果を使用して、ビュー モデルを更新します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

最も重要な部分ようになりました: と DOM が fulled ロードを呼び出し、 **ko.applyBindings**関数し、の新しいインスタンスを渡す、 `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings**メソッドは、Knockout をアクティブにし、ビューをビュー モデルを作成します。

ビュー モデルをしたら、バインドを作成できます。 、Knockout.js でこれを行う追加して`data-bind`HTML 要素に属性します。 たとえば、HTML の一覧を配列にバインドするに使用して、`foreach`バインド。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach`バインディングは、配列を反復処理し、子が配列内の各オブジェクトの要素を作成します。 子要素のバインディングは、配列のオブジェクトのプロパティを参照できます。

「製品更新プログラム」一覧には、次のバインドを追加します。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>`のスコープ内の要素が、 **foreach**バインドします。 Knockout は意味がの製品ごとに 1 回要素を描画すること、`products`配列。 すべてのバインド内で、`<li>`要素は、その製品のインスタンスを参照してください。 たとえば、`$data.Name`を指す、`Name`製品のプロパティ。

テキスト入力値を設定するには、使用、`value`バインドします。 モデル-ビュー関数にバインドは、ボタンを使用して、`click`バインディング。 製品のインスタンスは、各関数にパラメーターとして渡されます。 詳細については、 [Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html)がさまざまなバインドの適切な説明。

バインドを次に、追加、**送信**イベントは成果物の追加フォーム。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

このバインディングの呼び出し、`create`新しい製品を作成するビュー モデルの関数。

管理ビューの完全なコードを次に示します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

アプリケーションを実行し、管理者アカウントでログイン"Admin"リンクをクリックします。 製品の一覧を参照してくださいし、作成、更新、または製品を削除することができる必要があります。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-4.md)
> [次へ](using-web-api-with-entity-framework-part-6.md)
