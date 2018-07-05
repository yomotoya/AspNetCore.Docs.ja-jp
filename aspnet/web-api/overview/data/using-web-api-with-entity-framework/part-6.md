---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: JavaScript クライアントの作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b0b8ef9bd44bbce5102f2b12717e330f72a9e0c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400935"
---
<a name="create-the-javascript-client"></a>JavaScript クライアントを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、HTML、JavaScript を使用して、アプリケーションのクライアントを作成して、 [Knockout.js](http://knockoutjs.com/)ライブラリ。 段階的に、クライアント アプリを作成します。

- 書籍の一覧を表示しています。
- 書籍の詳細を表示しています。
- 新しい書籍を追加します。

Knockout ライブラリは、モデル-ビュー-ビューモデル (MVVM) パターンを使用します。

- **モデル**(当社の場合は、書籍と著者) で、ビジネス ドメイン内のデータのサーバー側の表現です。
- **ビュー**はプレゼンテーション層 (HTML) です。
- **ビュー モデル**は、モデルを保持する JavaScript オブジェクトです。 ビュー モデルは、UI のコードの抽象化です。 HTML 形式の知識がありません。 など、ビューの抽象の機能を表す、代わりに、&quot;書籍の一覧&quot;します。

ビューはデータ バインドはビュー モデルにします。 ビュー モデルに更新プログラムは、ビューで自動的に反映されます。 ビュー モデルは、ボタンのクリックしてなどもイベントと、ビューから取得します。

![](part-6/_static/image1.png)

このアプローチでは、任意のコードを書き直すことがなく、バインドを変更できるため、簡単にレイアウトし、アプリの UI を変更できます。 たとえば、として項目の一覧を表示する場合があります、 `<ul>`、後で、テーブルに変更します。

## <a name="add-the-knockout-library"></a>Knockout ライブラリを追加します。

Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**します。 選び**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](part-6/samples/sample1.cmd)]

このコマンドは、Scripts フォルダーに Knockout ファイルを追加します。

## <a name="create-the-view-model"></a>ビュー モデルを作成します。

Scripts フォルダーに app.js をという名前の JavaScript ファイルを追加します。 (ソリューション エクスプ ローラーで、Scripts フォルダーを右クリックして**追加**を選択し、 **JavaScript ファイル**)。次のコードを貼り付けます。

[!code-javascript[Main](part-6/samples/sample2.js)]

Knockout で、`observable`クラスは、データ バインディングを使用できます。 可能なオブジェクトの内容を変更するときに、観測可能なオブジェクトに通知のすべてのデータ バインド コントロール自体を更新できるようにします。 (、`observableArray`クラスは、配列のバージョンの*オブザーバブル*)。最初に、このビュー モデルでは、2 つの観測可能なオブジェクトがあります。

- `books` 書籍の一覧を保持します。
- `error` AJAX 呼び出しが失敗した場合は、エラー メッセージが含まれています。

`getAllBooks`メソッドは、AJAX 呼び出しをブックの一覧を取得します。 結果をプッシュし、`books`配列。

`ko.applyBindings`メソッドは、Knockout ライブラリの一部です。 パラメーターとしてビュー モデルを受け取るし、データ バインディングを設定します。

## <a name="add-a-script-bundle"></a>スクリプト バンドルを追加します。

バンドルは、ASP.NET 4.5 を結合または複数のファイルを 1 つのファイルをバンドルするが容易にする機能です。 バンドル化と、ページ読み込み時間を改善できると、サーバーに要求の数が減少します。

アプリのファイルを開く\_Start/BundleConfig.cs します。 RegisterBundles メソッドに次のコードを追加します。

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [前へ](part-5.md)
> [次へ](part-7.md)
