---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: マスター/詳細詳細 DetailView (c#) で選択可能なマスター GridView の使用 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、行が選択ボタンと共に、各製品の価格と名前を含む GridView があります。 A particu の選択 ボタンをクリックするとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d39786cb17449b93e6f728a0a3c920e1be089be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>マスター/詳細詳細 DetailView (c#) で選択可能なマスター GridView の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe)または[PDF のダウンロード](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> このチュートリアルでは、行が選択ボタンと共に、各製品の価格と名前を含む GridView があります。 特定の製品を選択 ボタンをクリックすると、同じページ上の DetailsView コントロールに表示される、完全な詳細情報が発生します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-across-two-pages-cs.md)2 つの web ページを使用してマスター/詳細レポートを作成する方法を説明しました"マスター"web ページが、suppliers; の一覧を表示お元となると、選択したによって提供されるこれらの製品を一覧表示"details"web ページ。供給業者です。 この 2 つのページのレポート形式は、1 つのページに凝縮ことができます。 このチュートリアルでは、行が選択ボタンと共に、各製品の価格と名前を含む GridView があります。 特定の製品を選択 ボタンをクリックすると、同じページ上の DetailsView コントロールに表示される、完全な詳細情報が発生します。


[![[選択] ボタンをクリックするとには、製品の詳細が表示されます。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**図 1**: 製品の詳細を表示する [選択] ボタンをクリックすると ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>手順 1: 選択可能な GridView の作成

マスター/詳細の 2 ページにハイパーリンクが master の各レコードに含まれていることを報告する取り消しをクリックすると、ユーザーをクリックした行を渡すことの詳細 ページに送信される`SupplierID`クエリ文字列の値。 このようなハイパーリンクは、内を使用して GridView 行ごとに追加されました。 1 つのページのマスター/詳細レポートは必要があります、ボタンをクリックすると、各 GridView 行の詳細が表示されます。 GridView コントロールは、ポストバックを発生させるし、その行を GridView のとしてマークする各行の選択 ボタンを含めるように構成できる[SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)です。

GridView コントロールを追加することで開始、 `DetailsBySelecting.aspx`  ページで、`Filtering`設定、フォルダー、`ID`プロパティを`ProductsGrid`です。 次に、追加という名前の新しい ObjectDataSource`AllProductsDataSource`を呼び出す、`ProductsBLL`クラスの`GetProducts()`メソッドです。


[![ObjectDataSource AllProductsDataSource をという名前の作成します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**図 2**: 名前付き、ObjectDataSource 作成`AllProductsDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![ProductsBLL クラスを使用します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**図 3**: を使用して、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![ObjectDataSource GetProducts() メソッドを呼び出すための構成します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**図 4**: 構成 invoke ObjectDataSource、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


削除する GridView のフィールドを編集以外のすべての`ProductName`と`UnitPrice`BoundFields です。 また、自由に書式設定など、必要に応じてこれら BoundFields をカスタマイズする、`UnitPrice`通貨として BoundField を変更して、 `HeaderText` BoundFields のプロパティです。 次の手順は、GridView のスマート タグからの列の編集リンクをクリックするか、宣言の構文を手動で構成することによって、視覚的に、実現できます。


[![ProductName および UnitPrice BoundFields 以外はすべて削除します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**図 5**: 削除が、`ProductName`と`UnitPrice`BoundFields ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


GridView の最終的なマークアップは次のとおりです。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

次に、それぞれの行に [選択] ボタンを追加、選択可能として GridView をマークする必要があります。 これを実現するには、GridView のスマート タグの選択を有効にするチェック ボックスを簡単にチェックします。


[![GridView の行を選択できるように](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**図 6**: GridView の行を選択できるように ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


CommandField を追加、選択範囲を有効にする チェック、 `ProductsGrid` GridView その`ShowSelectButton`プロパティを True に設定します。 これは、結果 [選択] ボタン、GridView の行ごとに図 6 に示すようになります。 既定では、選択ボタンがある、としてレンダリングされますが使用できるボタンまたは ImageButtons 代わりに CommandField のを通じて`ButtonType`プロパティです。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

ポストバックに陥ります GridView 行の選択 ボタンがクリックされたときに、GridView の`SelectedRow`プロパティを更新します。 加え、`SelectedRow`プロパティ、GridView のでは、 [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx)、 [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)、および[SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx)プロパティです。 `SelectedIndex`一方、プロパティが選択した行のインデックスを返します、`SelectedValue`と`SelectedDataKey`プロパティ GridView のに基づいて値を返します[DataKeyNames プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)です。

`DataKeyNames`に関連付けるプロパティを使用または複数のデータ フィールド、各行の値を GridView の各行と基になるデータから一意に識別する情報を属性に一般的に使用します。 `SelectedValue`プロパティは、最初の値を返します`DataKeyNames`選択した行のデータ フィールドが該当します、`SelectedDataKey`プロパティは、選択した行を返します`DataKey`オブジェクトで、すべての指定されたデータのキー フィールドの値が含まれます。該当する行。

`DataKeyNames`プロパティは、GridView、DetailsView、または、デザイナーによる FormView をデータ ソースをバインドすると、自動的に一意に識別するデータ フィールドに設定します。 このプロパティが設定されてうえで自動的に前のチュートリアルで中の例が操作していたせず、`DataKeyNames`指定されたプロパティ。 ただし、このチュートリアルでは選択可能な GridView のだけでなくをおを確認する挿入、更新、および削除すると、今後のチュートリアルについては、`DataKeyNames`プロパティを適切に設定する必要があります。 GridView を確実に少し時間を取って`DataKeyNames`プロパティに設定されている`ProductID`です。

ブラウザーを使ってこれまで、進行状況を表示します。 GridView には、すべての選択の LinkButton と共に製品の価格と名前が一覧表示されるに注意してください。 [選択] ボタンをクリックすると、ポストバックが発生します。 手順 2. で選択した製品の詳細を表示することによってこのポストバックに DetailsView 応答する方法を会いしましょう。


[![各製品の行には、Select LinkButton が含まれています。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**図 7**: 各製品の行には、選択の LinkButton が含まれています ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>選択した行の強調表示

`ProductsGrid` GridView は、`SelectedRowStyle`プロパティが選択した行の visual スタイルをディクテーションを使用できます。 正しく使用すると、詳細を明確に示す GridView の行が現在選択されているで、ユーザーのエクスペリエンスを向上このできます。 このチュートリアルでは、黄色の背景色で強調表示する、選択した行があてみましょう。

同様に、前のチュートリアルは、CSS クラスとして定義されている美しさに関連する設定をそのまま目指していましてみましょう。 そのため、クラスを作成、新しい CSS`Styles.css`という`SelectedRowStyle`です。


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

この CSS クラスを適用する、`SelectedRowStyle`プロパティの*すべて*シリーズでは、チュートリアル、Gridview の編集、`GridView.skin`のスキン、`DataWebControls`に含めるテーマ、`SelectedRowStyle`次に示すように設定。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

このさらに、選択された GridView 行は今すぐ黄色の背景色で強調表示されます。


[![GridView の SelectedRowStyle プロパティを使用して、選択した行の外観をカスタマイズします。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**図 8**: GridView のカスタマイズ、選択した行の外観を使用して`SelectedRowStyle`プロパティ ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>手順 2: DetailsView で選択した製品の詳細を表示します。

`ProductsGrid` GridView すべてを完了、選択した特定の製品に関する情報を表示する DetailsView を追加しています。 GridView 上 DetailsView コントロールを追加し、作成という名前の新しい ObjectDataSource`ProductDetailsDataSource`です。 選択した製品に関する特定の情報を表示するには、この DetailsView ので、構成、`ProductDetailsDataSource`を使用する、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッドです。


[![ProductsBLL クラスの GetProductByProductID(productID) メソッドを呼び出す](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**図 9**: 呼び出し、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


*`productID`* GridView コントロールから得られたパラメーターの値`SelectedValue`プロパティです。 以前、GridView に説明したよう`SelectedValue`プロパティは、最初のデータ キーの選択した行の値を返します。 そのため、命令型を GridView の`DataKeyNames`プロパティに設定されている`ProductID`いるため、選択した行の`ProductID`によって値が返される`SelectedValue`です。


[![GridView の SelectedValue プロパティに、productID パラメーターを設定します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**図 10**: 設定、 *`productID`*パラメーターを GridView の`SelectedValue`プロパティ ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


1 回、 `productDetailsDataSource` ObjectDataSource を正しく構成されていて、DetailsView にバインドされている、このチュートリアルは終了です。 ページが初めてアクセスしたときに行が選択されていないため、GridView の`SelectedValue`プロパティから返される`null`です。 製品ではないため、 `NULL` `ProductID`値、レコードは返されませんでしたが、 `GetProductByProductID(productID)` DetailsView が表示されていないことを意味しているメソッド (図 11 を参照してください)。 GridView の行の選択 ボタンをクリックすると、ポストバックに陥ります、DetailsView は更新されます。 今回は、GridView の`SelectedValue`プロパティから返される、`ProductID`選択した行の`GetProductByProductID(productID)`メソッドを返します。、`ProductsDataTable`に関する情報を特定の製品、および DetailsView には、これらの詳細を示しています (図 12 を参照してください)。


[![最初の閲覧、GridView のみが表示される場合](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**図 11**: 最初にアクセスされる GridView のみが表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![行を選択すると、製品の詳細が表示されます。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**図 12**: 行を選択すると、製品の詳細が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>まとめ

この例と前述の 3 つのチュートリアルは、マスター/詳細レポートを表示するための手法の数を見たです。 検査はこのチュートリアルでは選択可能な GridView を使用して、マスター レコードと同じページに、選択したマスター レコードに関する詳細を表示する DetailsView を格納します。 前のチュートリアルでは、DropDownLists を使用して、および 1 つの web ページと詳細レコードを別のマスター レコードを表示するマスター/詳細レポートを表示する方法を説明しました。

このチュートリアルでは、マスター/詳細レポートの検査で終了します。 [次へ] tutorialwe で始まる最初の GridView、DetailsView、フォーム ビューとカスタマイズされた書式設定、探索します。 ここにバインドされたデータに基づくこれらのコントロールの外観をカスタマイズする方法、GridView のフッター内のデータを集計する方法、およびテンプレートを使用してより多くのレイアウトを制御を取得する方法が表示されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-across-two-pages-cs.md)
> [次へ](master-detail-filtering-with-a-dropdownlist-vb.md)
