---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: "DataList コントロール (VB) を含む行ごとの複数のレコードを示す |Microsoft ドキュメント"
author: rick-anderson
description: "この短いチュートリアルでは、RepeatColumns と RepeatDirection プロパティを通じて DataList のレイアウトをカスタマイズする方法をについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 416178533f022f2a262799e6f042d6009bb9d999
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>DataList コントロール (VB) を含む行ごとの複数のレコードを表示
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe)または[PDF のダウンロード](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> この短いチュートリアルでは、RepeatColumns と RepeatDirection プロパティを通じて DataList のレイアウトをカスタマイズする方法をについて説明します。


## <a name="introduction"></a>はじめに

DataList の例は、お過去の 2 つのチュートリアルで説明しましたが、単一列 HTML 内の行とそのデータ ソースの各レコードを表示`<table>`です。 これは、DataList は、非常に簡単にデータ ソース アイテムは複数の列、複数行のテーブル間で分散されるように、DataList の表示をカスタマイズします。 さらに、そのソース DataList 単一行、複数の列に表示される項目の s のすべてのデータを設定することです。

を通じて DataList s のレイアウトをカスタマイズできますおその`RepeatColumns`と`RepeatDirection`それぞれ、列の数が表示され、それらの項目が配置されている垂直または水平方向にするかどうかを示すため、このプロパティは、します。 図 1 は、たとえば、3 つの列のテーブルに製品情報を表示する DataList を示します。


[![DataList は、1 行の 3 つの製品を示しています。](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**図 1**:、DataList 示します 3 つの製品の行ごと ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


行ごとに複数のデータ ソース アイテムを表示するには、DataList は水平画面スペースをより効果的に利用できます。 この短いチュートリアルでは、これら 2 つの DataList プロパティを調べておみます。

## <a name="step-1-displaying-product-information-in-a-datalist"></a>手順 1: DataList の製品情報を表示します。

について調べる前に、`RepeatColumns`と`RepeatDirection`プロパティ、let s まずを作成、DataList、標準的な単一列、複数行のテーブルのレイアウトを使用して製品情報を一覧表示するページ。 この例では、製品の名前、カテゴリ、および次のマークアップを使用して価格を表示する秒を使用できます。


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

お ve 迅速に以下の手順を移動しますので、前の例では、DataList にデータをバインドする方法について説明します。 開いて開始、 `RepeatColumnAndDirection.aspx`  ページで、`DataListRepeaterBasics`フォルダーと、ツールボックスからデザイナーにドラッグ、DataList です。 新しい ObjectDataSource を作成してからそのデータをプルするように構成することを選択、DataList s のスマート タグから、`ProductsBLL`クラスの`GetProducts`メソッド、() を選択するオプションをウィザードの INSERT、UPDATE、およびタブを削除します。

を作成し、DataList へのバインドのバインド後に Visual Studio が自動的に作成、`ItemTemplate`の各製品のデータ フィールドの名前と値を表示します。 調整、`ItemTemplate`交換上に示したマークアップを使用するようにオプション DataList s のスマート タグに適切な宣言型マークアップを使用して直接またはテンプレートの編集、 *Product Name*、*カテゴリ名*、および*価格*に値を代入するデータ バインドを適切な構文を使用するラベル コントロールでテキスト、`Text`プロパティです。 更新した後、`ItemTemplate`ページの宣言型のマークアップは、次のようになります。


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

通知です ve にで書式指定子が含まれている、`Eval`のデータ バインド構文、`UnitPrice`の通貨として返される値の書式設定`Eval("UnitPrice", "{0:C}").`

すぐをブラウザーでページを参照してください。 図 2 では、DataList は製品の複数行の単一列でテーブルとして表示されます。


[![既定では、複数行の単一列でテーブルとして DataList レンダリング](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**図 2**: 既定では、単一の列、複数の行のテーブルとして DataList レンダリング ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>手順 2: DataList のレイアウトの方向を変更します。

既定の動作中に、DataList という単一列、複数行のテーブルの縦方向にその項目をレイアウトするこの動作に簡単にで変更できます DataList s [ `RepeatDirection`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)です。 `RepeatDirection`プロパティは、2 つの値のいずれかを受け取ることができます:`Horizontal`または`Vertical`(既定)。

変更することによって、`RepeatDirection`プロパティから`Vertical`に`Horizontal`DataList はデータ ソース アイテムごとの 1 つの列を作成する 1 つの行でそのレコードを表示します。 この特殊効果を示すためには、デザイナーで DataList をクリックし、次に、[プロパティ] ウィンドウから次のように変更します。、`RepeatDirection`プロパティから`Vertical`に`Horiztonal`です。 すぐにこれを行うと、デザイナー レイアウトを調整 DataList s、単一行、複数の列のインターフェイスを作成する (図 3 を参照してください)。


[![RepeatDirection プロパティを指定する方法、方向 DataList s の項目がレイアウトには](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**図 3**:`RepeatDirection`プロパティでは、方向、DataList のアイテムがどのように配置を ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


少量のデータを単一行を表示するときに複数の列のテーブルが実際の画面を最大化するための望ましい方法にあります。 大量のデータは、ただし、1 つの行が必要になります多数の列、そのことができます t の右側にオフに画面に収まるアイテムをプッシュします。 図 4 は、単一行 DataList でレンダリングした場合、製品を示します。 多くの製品 (80) があるため、ユーザーがかなりの各製品に関する情報を表示する右にスクロールする必要があります。


[![十分な大きさのデータ ソースの場合は、1 つの列 DataList が必要になります水平方向にスクロール](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**図 4**: を十分に大きなデータ ソースを単一列 DataList は必要な水平方向にスクロール ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>手順 3: 複数の列、複数行のテーブルにデータを表示します。

複数の列、複数行 DataList を作成する必要がありますを設定する、 [ `RepeatColumns`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx)を表示する列の数。 既定では、`RepeatColumns`プロパティが 0、1 つの行または列にすべての項目を表示する DataList 原因となる (の値に応じて、`RepeatDirection`プロパティ)。

たとえば、テーブルの行ごとに 3 つの製品を表示する秒を使用できます。 このため、設定、`RepeatColumns`プロパティを 3 です。 この変更を行った後すぐをブラウザーで結果を表示します。 図 5 に示す、製品が 3 つの列、複数行のテーブルに表示されます。


[![行ごとに 3 つの製品が表示されます。](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**図 5**: 3 つの製品が行ごとに表示されます ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection`プロパティは、DataList 内の項目のレイアウトに影響します。図 5 は、結果を`RepeatDirection`プロパティに設定`Horizontal`です。 Chai、変更、および"ホワイトソルト"とは、最初の 3 つの製品が左から右、上下からにレイアウトされることに注意してください。 次の 3 つの製品 (Cajun Seasoning Chef Anton s 以降) は、最初の 3 つの下の行に表示されます。 変更、`RepeatDirection`プロパティ`Vertical`、ただし、上から下にこれらの製品のレイアウト、左から右、図 6 に示すようにします。


[![ここでは、製品が垂直方向にレイアウト](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**図 6**: ここでは、製品が垂直方向にレイアウト ([フルサイズのイメージを表示するをクリックして](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


結果のテーブルに表示される行の数は、DataList にバインドされているレコードの合計数によって異なります。 具体的には、そのデータ ソース アイテムの合計数の上限で割った s、`RepeatColumns`プロパティの値。 以降、 `Products` table 現在には 84 製品は、これは 3 で割り切れる、28 行が存在します。 場合、データ ソース内の項目数と`RepeatColumns`プロパティの値は割り切れる、ない場合、最後の行または列は空白セル。 場合、`RepeatDirection`に設定されている`Vertical`の場合、最後の列は空のセル場合`RepeatDirection`は`Horizontal`の場合、最後の行は空のセル。

## <a name="summary"></a>まとめ

DataList、既定では、単一 TemplateField GridView のレイアウトを模した、単一列、複数行のテーブル内の項目一覧表示します。 この既定のレイアウトは許容されるが、実際の画面を最大化の行ごとに複数のデータ ソース アイテムを表示することによっておできます。 DataList s を設定するだけではこれを実現する`RepeatColumns`プロパティを行ごとに表示する列の数。 さらに、DataList の`RepeatDirection`を示すかどうか、複数の列、複数行のテーブルの内容を配置する水平方向に左から右、上下からへ、または垂直方向に上から下、左右からにプロパティを使用できます。

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客は、John Suru をでした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
[次へ](nested-data-web-controls-vb.md)
