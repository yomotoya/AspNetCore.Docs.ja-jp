---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: DataList コントロール (VB) での行ごとに複数のレコードの表示 |Microsoft Docs
author: rick-anderson
description: この短いチュートリアルでは、RepeatColumns と RepeatDirection プロパティを通して、DataList のレイアウトをカスタマイズする方法を見ていきます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cad9967d35ed870ae18579e65eb866b794866aad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393935"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>DataList コントロール (VB) での行ごとの複数のレコードを表示
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe)または[PDF のダウンロード](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> この短いチュートリアルでは、RepeatColumns と RepeatDirection プロパティを通して、DataList のレイアウトをカスタマイズする方法を見ていきます。


## <a name="introduction"></a>はじめに

例では、DataList 過去の 2 つのチュートリアルで確認したところには、単一列 HTML 内の行としてそのデータ ソースの各レコードがレンダリング`<table>`します。 DataList の既定の動作は、非常に簡単にデータ ソースの項目が複数の列、複数行のテーブルに分散されるように DataList 表示をカスタマイズできます。 さらに、そのことができますが、すべてのデータ ソース DataList 単一行、複数の列に表示される項目。

を通して、DataList のレイアウトをカスタマイズすることができます、`RepeatColumns`と`RepeatDirection`プロパティがそれぞれに、列の数が表示され、それらの項目がへとレイアウトされる垂直方向または水平方向にするかどうかを示します。 図 1 には、たとえば、次の 3 つの列を含むテーブルで製品情報を表示する DataList が表示されます。


[![DataList は、行ごとに 3 つの製品を示しています。](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**図 1**:、DataList 示します 3 つの製品の行ごと ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))。


行ごとに複数のデータ ソース アイテムを表示するには、DataList は水平方向の画面スペースをより効果的に利用できます。 この短いチュートリアルではこれら 2 つの DataList プロパティについて説明します。

## <a name="step-1-displaying-product-information-in-a-datalist"></a>手順 1: DataList で製品情報を表示します。

調べる前に、`RepeatColumns`と`RepeatDirection`プロパティ、let s まずを作成、DataList、標準の単一列、複数行のテーブルのレイアウトを使用して製品情報を一覧表示するページ。 この例では、s、製品の名前、カテゴリ、および次のマークアップを使用して価格を表示できます。


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

私たち ve 迅速にこれらの手順を移動しますので、前の例では、DataList にデータをバインドする方法について説明します。 開いて開始、`RepeatColumnAndDirection.aspx`ページで、`DataListRepeaterBasics`フォルダーと、ツールボックスからデザイナーにドラッグ、DataList します。 新しい ObjectDataSource を作成してから、そのデータをプルするように構成することを選択、DataList s のスマート タグから、`ProductsBLL`クラスの`GetProducts`() を選択するメソッド オプションをウィザードの INSERT、UPDATE、およびタブを削除します。

作成し、新しい ObjectDataSource を DataList にバインドする、Visual Studio が自動的に作成、`ItemTemplate`の各製品のデータ フィールドの名前と値を表示します。 調整、 `ItemTemplate` DataList s のスマート タグで置き換えて、上記のようにマークアップを使用するようにオプション宣言型マークアップを直接またはテンプレートの編集、*製品名*、*カテゴリ名*、および*価格*テキスト ラベル コントロールに値を割り当てる適切なデータ バインディング構文を使用すると、`Text`プロパティ。 更新した後、 `ItemTemplate`、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

通知が ve にで書式指定子が含まれている、`Eval`のデータ バインディング構文、 `UnitPrice`、- 通貨型として返される値の書式設定 `Eval("UnitPrice", "{0:C}").`

ご協力をブラウザーでページを参照してください。 図 2 に示すよう、DataList は、製品の単一列、複数行のテーブルとしてレンダリングします。


[![既定では、単一列、複数行のテーブルとして DataList レンダリング](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**図 2**: 既定では、単一列、複数行のテーブルとして DataList レンダリング ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))。


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>手順 2: DataList のレイアウトの方向を変更します。

既定の動作中に、DataList で単一列、複数行のテーブルの縦方向には、そのアイテムのレイアウトにはこの動作に簡単にで変更できます DataList s [ `RepeatDirection`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)します。 `RepeatDirection`プロパティは、2 つの値のいずれかを受け入れることができます:`Horizontal`または`Vertical`(既定)。

変更することで、`RepeatDirection`プロパティから`Vertical`に`Horizontal`DataList はデータ ソースの項目ごとに 1 つの列を作成する 1 つの行では、そのレコードをレンダリングします。 この効果を示すためには、DataList、デザイナーでをクリックし、次に、[プロパティ] ウィンドウから次のように変更します。、`RepeatDirection`プロパティから`Vertical`に`Horiztonal`します。 すぐにそうと、デザイナー レイアウトを調整します DataList s、単一行、複数列のインターフェイスを作成する (図 3 を参照してください)。


[![RepeatDirection プロパティを指定する方法、方向 DataList の項目は配置には](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**図 3**:`RepeatDirection`プロパティでは、方向、DataList のアイテムがどのように配置を ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))。


少量のデータを単一行を表示するときに、複数列テーブルには理想的な方法を実際の画面を最大化する可能性があります。 大量のデータは、ただし、1 つの行が必要になりますそのは t の右側には、オフ、画面上に収まるアイテムがプッシュの多数の列。 図 4 には、単一行 DataList でレンダリングした場合、製品が表示されます。 多くの製品 (80) があるので、ユーザーは、かなりの各製品に関する情報を表示する右にスクロールする必要があります。


[![十分な大きさのデータ ソースの場合は、1 つの列 DataList が必要になります水平方向にスクロール](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**図 4**: を十分に大きなデータ ソース、単一列 DataList は必要な水平方向にスクロール ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))。


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>手順 3: 複数の列、複数行のテーブルにデータを表示します。

複数の列、複数行の DataList を作成する必要がありますを設定する、 [ `RepeatColumns`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx)を表示する列の数。 既定で、`RepeatColumns`プロパティが 0 で、すべての項目を 1 つの行または列に表示する DataList になります (の値に応じて、`RepeatDirection`プロパティ)。

たとえば、s のテーブルの行ごとに 3 つの製品を表示することができます。 そのため、設定、`RepeatColumns`プロパティを 3 にします。 この変更を加えたら、少しブラウザーで結果を表示します。 図 5 に示す、製品が 3 つの列、複数行の表に表示されます。


[![行ごとに 3 つの製品が表示されます。](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**図 5**: 行ごとに 3 つの製品が表示されます ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))。


`RepeatDirection`プロパティは、DataList 内の項目のレイアウトに影響します。図 5 で結果を示しています、`RepeatDirection`プロパティに設定`Horizontal`します。 Chai、変更、およびアニス シロップの最初の 3 つの製品が左から右、上から下にレイアウトされることに注意してください。 次の 3 つの製品 (Cajun Seasoning Chef Anton s で始まる) は、最初の 3 つの下の行に表示されます。 変更、`RepeatDirection`プロパティ`Vertical`、ただし、上から下にこれらの製品レイアウト、左から右、図 6 に示すようにします。


[![ここでは、製品が、垂直方向にレイアウト](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**図 6**: ここでは、製品が垂直方向に配置されますが ([フルサイズの画像を表示する をクリックします](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))。


結果のテーブルに表示される行の数は、DataList にバインドされているレコードの合計数によって異なります。 正確には、そのデータ ソース アイテムの合計数の上限が除算 s、`RepeatColumns`プロパティの値。 以降、`Products`テーブル現在 84 製品は、これは 3 で割り切れる、28 行があります。 場合、データ ソース内の項目の数と`RepeatColumns`プロパティの値がで割り切れる場合、最後の行または列は空白のセル。 場合、`RepeatDirection`に設定されている`Vertical`場合、最後の列は空のセルを持つなります`RepeatDirection`は`Horizontal`、最後の行が空のセルを持つなります。

## <a name="summary"></a>まとめ

DataList、既定では、単一 TemplateField を GridView のレイアウトを模倣する、単一列、複数行のテーブルでは、その項目を一覧表示されます。 この既定のレイアウトは、受け入れられますが、実際の画面を最大化の行ごとに複数のデータ ソース アイテムを表示することによってできます。 DataList s を設定するだけではこれを実現`RepeatColumns`プロパティに行ごとに表示する列の数。 さらに、DataList の`RepeatDirection`を示すかどうか、複数の列、複数行のテーブルの内容をレイアウトする水平方向または垂直方向に左から右、上下からに、上から下、左右からにプロパティを使用できます。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、John Suru でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [次へ](nested-data-web-controls-vb.md)
