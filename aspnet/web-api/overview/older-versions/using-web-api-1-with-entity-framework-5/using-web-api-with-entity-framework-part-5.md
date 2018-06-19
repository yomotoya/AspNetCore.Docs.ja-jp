---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: '手順 5: Knockout.js にダイナミック UI を作成する |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873811"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>手順 5: Knockout.js にダイナミック UI を作成します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Knockout.js にダイナミック UI を作成します。

このセクションでは、管理ビューの機能を追加するのに Knockout.js 使用されます。

[Knockout.js](http://knockoutjs.com/) Javascript ライブラリの HTML コントロールをデータにバインドするが容易です。 Knockout.js は、モデル View-viewmodel (MVVM) パターンを使用します。

- *モデル*の場合、製品および orders) の「ビジネス ドメイン内のデータのサーバー側表現です。
- *ビュー*は、プレゼンテーション層 (HTML)。
- *ビュー モデル*Javascript オブジェクト モデルのデータを保持します。 ビュー モデルは、UI のコードの抽象化です。 HTML 形式の知識がないです。 代わりに、「アイテムの一覧」など、ビューの抽象機能を表します。

ビュー データにバインドされてビュー モデルです。 ビュー モデルへの更新は、ビューに自動的に反映されます。 ビュー モデルは、ボタンのクリックなど、ビューからイベントを取得し、注文を作成するなど、モデルでの操作を実行します。

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

最初のビュー モデルを定義します。 その後は、ビュー モデルに、HTML マークアップをバインドします。

Admin.cshtml に Razor の次のセクションを追加します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

このセクションでは、ファイルの任意の場所追加できます。 終了する前に、ビューが表示されると、セクションは、HTML ページの下部に表示されます。 右&lt;/body&gt;タグ。

このページのスクリプトのすべてがコメントで示されているスクリプト タグの内部参照してください。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

最初に、ビュー モデル クラスを定義します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** Knockout と呼ばれる内のオブジェクトの特殊なは、 *observable*です。 [Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html): 監視対象は、「を JavaScript オブジェクトの変更はサブスクライバーに通知できます」。 監視対象の内容を変更するときに一致するように、ビューが自動的に更新します。

設定する、`products`配列、web API への AJAX 要求を作成します。 ビュー バッグ内の API のベース URI を格納おことに注意してください (を参照してください[パート 4](using-web-api-with-entity-framework-part-4.md)チュートリアルの)。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

次に、関数を作成、更新、および製品を削除するビュー モデルに追加します。 これらの関数は、web API への AJAX 呼び出しを送信し、結果を使用して、ビュー モデルを更新します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

最も重要な部分ようになりました: と DOM が fulled ロードを呼び出し、 **ko.applyBindings**機能しの新しいインスタンスを渡す、 `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings**メソッド Knockout をアクティブにし、ビューをビュー モデルに接続します。

ビュー モデルがある、バインディングを作成できます。 Knockout.js でこれを行う追加することによって`data-bind`HTML 要素に属性します。 たとえば、HTML の一覧を配列にバインドするに使用して、`foreach`バインド。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach`バインディングが、配列を反復処理し、配列の各オブジェクトの要素の子を作成します。 子要素のバインディングは、配列のオブジェクトのプロパティを参照できます。

「製品更新プログラム」一覧に、以下のバインドを追加します。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>`のスコープ内の要素が発生した、 **foreach**バインドします。 手段 Knockout されますが、要素の各製品の 1 回をレンダリング、`products`配列。 すべてのバインド内で、`<li>`要素は、その製品のインスタンスを参照してください。 たとえば、`$data.Name`を指す、`Name`製品のプロパティです。

テキスト入力の値を設定するには、使用、`value`バインドします。 ボタンは、モデル ビューで関数にバインドされるを使用して、`click`バインドします。 製品のインスタンスは、各関数にパラメーターとして渡されます。 詳細については、 [Knockout.js ドキュメント](http://knockoutjs.com/documentation/observables.html)が各種のバインディングの適切な説明。

次に、追加のバインディングを**送信**製品の追加フォームでイベント。

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

このバインディングを呼び出す、`create`関数をビュー モデルに新しい製品を作成します。

管理ビューの完全なコードを次に示します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

アプリケーションを実行し、管理者アカウントでログイン"Admin"リンクをクリックします。 、製品の一覧を表示し、作成、更新、または製品を削除することができる必要があります。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-4.md)
> [次へ](using-web-api-with-entity-framework-part-6.md)
