---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: 詳細 DetailView (VB) で選択可能なマスター GridView を使用してマスター/詳細 |Microsoft Docs
author: rick-anderson
description: このチュートリアルは、GridView 行選択ボタンと共に各製品の価格と名前を含める必要があります。 Particu の Select ボタンをクリックしています.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 17885b2f4892011629e04596b24ca677de2fa8b7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820925"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>マスター/詳細の選択可能なマスター GridView を使用すると詳細 DetailView (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe)または[PDF のダウンロード](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> このチュートリアルは、GridView 行選択ボタンと共に各製品の価格と名前を含める必要があります。 特定の製品の選択 ボタンをクリックすると、同じページに DetailsView コントロールに表示される、完全な詳細情報が発生します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-across-two-pages-vb.md)2 つの web ページを使用してマスター/詳細レポートを作成する方法を説明しました"マスター"web ページ、サプライヤーの一覧を表示、および、選択したによって提供されるこれらの製品を一覧表示"詳細"web ページ。仕入先。 この 2 つのページのレポートの形式は、1 つのページに凝縮できます。 このチュートリアルは、GridView 行選択ボタンと共に各製品の価格と名前を含める必要があります。 特定の製品の選択 ボタンをクリックすると、同じページに DetailsView コントロールに表示される、完全な詳細情報が発生します。


[![[選択] ボタンをクリックしてには、製品の詳細が表示されます。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**図 1**: [選択] ボタンをクリックしてには、製品の詳細が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))。


## <a name="step-1-creating-a-selectable-gridview"></a>手順 1: 選択可能な GridView を作成します。

マスター/詳細の 2 ページで各マスター レコードにハイパーリンクが含まれていることを報告することを思い出してくださいをクリックすると、クリックされた行を渡すことの詳細ページにユーザーに送信`SupplierID`クエリ文字列値。 このようなハイパーリンクは、内を使用して GridView 行ごとに追加されました。 1 つのページ マスター/詳細レポートでは、私たちは必要があります、ボタンをクリックすると、各 GridView 行の詳細が表示されます。 GridView コントロールは、ポストバックが発生して、その行を GridView のとしてマークする行ごとに Select ボタンを含めるように構成できる[SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)します。

GridView コントロールを追加することで開始、`DetailsBySelecting.aspx`ページで、`Filtering`設定、フォルダー、`ID`プロパティを`ProductsGrid`します。 という名前の新しい ObjectDataSource を次に、追加`AllProductsDataSource`を呼び出す、`ProductsBLL`クラスの`GetProducts()`メソッド。


[![ObjectDataSource AllProductsDataSource という名前の作成します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**図 2**: 名前付き、ObjectDataSource 作成`AllProductsDataSource`([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))。


[![ProductsBLL クラスを使用して、](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**図 3**: 使用して、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))。


[![ObjectDataSource GetProducts() メソッドを呼び出すための構成します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**図 4**: invoke ObjectDataSource を構成、`GetProducts()`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))。


GridView のフィールドを削除する編集はすべて、`ProductName`と`UnitPrice`BoundFields。 また、自由に書式設定など、必要に応じてこれら BoundFields をカスタマイズする、`UnitPrice`を通貨として BoundField を変更して、 `HeaderText` BoundFields のプロパティ。 次の手順は、GridView のスマート タグからの列の編集リンクをクリックして、または宣言の構文を手動で構成することで、グラフィカルに実現できます。


[![ProductName および UnitPrice BoundFields 以外はすべて削除します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**図 5**: 削除が、`ProductName`と`UnitPrice`BoundFields ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))。


GridView の最終的なマークアップを示します。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

次に、行ごとに Select ボタンが追加されますとして選択可能な GridView をマークする必要があります。 これを実現するには、GridView のスマート タグの選択を有効にするチェック ボックスにチェックします。


[![GridView の行を選択できるように](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**図 6**: GridView の行を選択できるように ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))。


選択を有効にするオプションをオンにする [commandfield] を追加、`ProductsGrid`を含む GridView その`ShowSelectButton`プロパティを True に設定します。 これは、結果に Select ボタン、GridView の行ごとの図 6 に示すようにします。 既定では、Select ボタンがある、としてレンダリングされますが、ボタンまたは ImageButtons 代わりに [commandfield] の使用`ButtonType`プロパティ。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

ポストバックに陥ります GridView 行の Select ボタンがクリックされたときに、GridView の`SelectedRow`プロパティが更新されます。 加え、`SelectedRow`プロパティ、GridView の提供、 [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx)、 [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)、および[SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx)プロパティ。 `SelectedIndex`プロパティが選択した行のインデックスを返しますが、`SelectedValue`と`SelectedDataKey`プロパティ GridView のに基づいて値を返します[DataKeyNames プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)します。

`DataKeyNames`プロパティを使用して、1 つを関連付ける、または複数のデータ フィールドの各行と値および GridView の各行と基になるデータから一意に識別する情報を属性に一般的に使用します。 `SelectedValue`プロパティは、最初の値を返します`DataKeyNames`選択した行のデータ フィールドが該当します、`SelectedDataKey`プロパティは、選択した行を返します`DataKey`オブジェクトで、すべての指定したデータのキー フィールドの値が含まれます。その行。

`DataKeyNames`プロパティは GridView、DetailsView、またはフォーム ビュー、デザイナーを使用するデータ ソースをバインドすると、自動的に一意に識別するデータ フィールドを設定します。 中に、このプロパティが設定されてを自動的に、上記のチュートリアルで、例では、ならば、なし、`DataKeyNames`プロパティを指定します。 ただし、このチュートリアルで選択可能な GridView およびを私たちを調べて、検証挿入、更新、および削除するには、今後のチュートリアルについては、`DataKeyNames`プロパティを適切に設定する必要があります。 GridView のことを確認する少し`DataKeyNames`プロパティに設定されて`ProductID`します。

ブラウザーできませんでしたが、進行状況を表示してみましょう。 GridView には、すべての選択の LinkButton と製品の価格と名前が一覧表示されるに注意してください。 [選択] ボタンをクリックすると、ポストバックが発生します。 手順 2 では、選択した製品の詳細を表示することによってこのポストバックに DetailsView 応答する方法を表示されます。


[![各製品の行には、Select LinkButton が含まれています。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**図 7**: 各製品の行には選択 LinkButton が含まれています ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))。


## <a name="highlighting-the-selected-row"></a>選択した行を強調表示

`ProductsGrid` GridView が、`SelectedRowStyle`プロパティが選択した行の visual スタイルをディクテーションを使用できます。 適切に使用して詳細を明確に示す GridView の行が現在選択されているユーザーのエクスペリエンスの向上このことができます。 このチュートリアルでは、背景が黄色で強調表示は選択した行があてみましょう。

同様に、以前のチュートリアルでは、CSS クラスとして定義されている美学上に関連する設定を保持するよう努力しましょう。 そのため、新しい CSS クラスを作成`Styles.css`という`SelectedRowStyle`します。


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

この CSS クラスを適用する、`SelectedRowStyle`プロパティの*すべて*チュートリアル シリーズで Gridview 編集、`GridView.skin`にスキンを適用、`DataWebControls`に含めるテーマ、`SelectedRowStyle`次に示すように設定。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

このさらに、GridView に選択された行は今すぐ黄色の背景色で強調表示されます。


[![GridView の SelectedRowStyle プロパティを使用して、選択した行の外観をカスタマイズします。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**図 8**: GridView のカスタマイズ [選択した行の外観を使用して`SelectedRowStyle`プロパティ ([フルサイズの画像を表示する] をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))。


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>手順 2: DetailsView で選択した製品の詳細を表示します。

`ProductsGrid` GridView が完了残っては選択されている特定の製品についての情報を表示する DetailsView を追加することだけです。 上、GridView、DetailsView コントロールを追加しという名前の新しい ObjectDataSource を作成して`ProductDetailsDataSource`します。 選択した製品に関する特定の情報を表示するには、この DetailsView ので、構成、`ProductDetailsDataSource`を使用する、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッド。


[![ProductsBLL クラスの GetProductByProductID(productID) メソッドを呼び出す](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**図 9**: 呼び出す、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))。


*`productID`* パラメーターの値を GridView コントロールから取得した`SelectedValue`プロパティ。 前に、GridView に説明したよう`SelectedValue`プロパティは、最初のデータ キーの選択した行の値を返します。 そのため、命令型を GridView の`DataKeyNames`プロパティに設定されて`ProductID`いるため、選択した行の`ProductID`によって値が返される`SelectedValue`します。


[![GridView の SelectedValue プロパティに productID パラメーターを設定します。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**図 10**: 設定、 *`productID`* パラメーターを GridView の`SelectedValue`プロパティ ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))。


1 回、 `productDetailsDataSource` ObjectDataSource が正しく構成されていて、DetailsView にバインドされている、このチュートリアルは終了です。 ページが初めてアクセスしたときに行が選択されていないため、GridView の`SelectedValue`プロパティが返す`Nothing`します。 製品がないので、 `NULL` `ProductID`値、レコードは返されませんが、`GetProductByProductID(productID)`メソッド DetailsView が表示されていないことを意味 (図 11 を参照してください)。 GridView の行の選択ボタンをクリックするとポストバックに陥ります、DetailsView を更新します。 GridView の今回`SelectedValue`プロパティが返す、`ProductID`選択した行の`GetProductByProductID(productID)`メソッドを返します。 を`ProductsDataTable`その特定の製品と DetailsView についてこれらの詳細を示しています (図 12 を参照してください)。


[![最初のアクセス、GridView のみが表示される場合](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**図 11**: 最初にアクセスすると、GridView のみが表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))。


[![行を選択すると、製品の詳細が表示されます。](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**図 12**: 行を選択すると、製品の詳細が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))。


## <a name="summary"></a>まとめ

これで、上記の 3 つのチュートリアルのさまざまなマスター/詳細レポートを表示するための手法を説明しました。 調べるこのチュートリアルでは選択可能な GridView を使用して、マスター レコードと同じページで選択したマスター レコードの詳細を表示する、DetailsView を格納します。 前のチュートリアルには、Dropdownlist を使用して、1 つの web ページと詳細レコードを別のマスター レコードを表示するマスター/詳細レポートを表示する方法を説明しました。

このチュートリアルでは、マスター/詳細レポートのマイクロソフトの調査で終了します。 以降、次のチュートリアルではまず、探索の GridView、DetailsView、FormView とカスタマイズされた書式します。 ここにバインドされたデータに基づくこれらのコントロールの外観をカスタマイズする方法、GridView のフッターにデータを集計する方法、およびテンプレートを使用してより詳細なレイアウトの制御を取得する方法が表示されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-across-two-pages-vb.md)
