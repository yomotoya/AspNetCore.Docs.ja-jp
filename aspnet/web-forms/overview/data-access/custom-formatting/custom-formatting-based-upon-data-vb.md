---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: データ (VB) に基づくカスタム書式設定 |Microsoft Docs
author: rick-anderson
description: GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは、l をしましょう.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d55a9ece22805d7f5d248e81a01d059dbde0ee18
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809590"
---
<a name="custom-formatting-based-upon-data-vb"></a>データ (VB) に基づくカスタム書式設定
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)または[PDF のダウンロード](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは、データ バインドのデータ バインドと RowDataBound イベント ハンドラーを使用して書式設定を実現する方法を紹介します。


## <a name="introduction"></a>はじめに

無数のスタイル関連プロパティの使用は、GridView、DetailsView、FormView コントロールの外観をカスタマイズできます。 などのプロパティ`CssClass`、 `Font`、 `BorderWidth`、 `BorderStyle`、 `BorderColor`、 `Width`、および`Height`、他のユーザーの間でレンダリングされたコントロールの外観を決定します。 プロパティを含む`HeaderStyle`、 `RowStyle`、 `AlternatingRowStyle`、特定のセクションに適用されるこれらの同じスタイル設定を許可する他のユーザーとします。 同様に、これらのスタイル設定は、フィールド レベルで適用できます。

多くのシナリオで、書式設定の要件によって異なります、表示されるデータの値。 などを描画する注意の商品を在庫として保管する製品情報を一覧表示するレポート可能性があります設定を持つこれらの製品の黄色の背景色`UnitsInStock`と`UnitsOnOrder`フィールドは、どちらも 0 に等しい。 高価な製品を強調表示するには、太字のフォントで複数の 75.00 ドルのコストをこれらの製品の価格を表示することもします。

GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルではデータ バインドを使用すると書式設定を実行する方法に注目します、`DataBound`と`RowDataBound`イベント ハンドラー。 次のチュートリアルではその他の方法について説明します。

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>DetailsView コントロールの`DataBound`イベント ハンドラー

データは、DetailsView にバインドされるデータ ソース コントロールまたはコントロールのデータをプログラムで割り当てることでいつ`DataSource`プロパティと呼び出し元の`DataBind()`メソッドでは、次の一連の手順が発生します。

1. データ Web コントロールの`DataBinding`イベントが発生します。
2. データ Web コントロールにバインドされます。
3. データ Web コントロールの`DataBound`イベントが発生します。

カスタム ロジックは、イベント ハンドラーによって、手順 1 および 3 の直後に挿入できます。 イベント ハンドラーを作成して、`DataBound`イベントされたデータがデータ Web コントロールにバインドされ、必要に応じて、書式設定を調整プログラムで判断できます。 それではこれを説明するために、製品に関する一般的な情報が一覧表示が表示されます DetailsView を作成、`UnitPrice`値、***太字、斜体フォント***75.00 ドルを超える場合。

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>手順 1: DetailsView で製品情報を表示します。

開く、`CustomColors.aspx`ページで、`CustomFormatting`フォルダー、DetailsView コントロールをツールボックスからデザイナーにドラッグして、設定、`ID`プロパティの値を`ExpensiveProductsPriceInBoldItalic`を呼び出す新しい ObjectDataSource コントロールにバインドし、 `ProductsBLL`クラスの`GetProducts()`メソッド。 ここに詳細に調査前のチュートリアルであるためここでこれを実現する詳細な手順を省略し、簡潔にするためのされます。

ObjectDataSource は、DetailsView にバインドしたら後、は、フィールド リストを変更するには、少しを実行します。 削除することを選択する、 `ProductID`、 `SupplierID`、 `CategoryID`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、および`Discontinued`BoundFields の名前を変更して、残りの BoundFields を再フォーマットします。 私も取り除か、`Width`と`Height`設定します。 DetailsView では、1 つのレコードのみが表示されたら、以降のすべての製品を表示するエンドユーザーに許可するには、ページングを有効にする必要があります。 そのため、DetailsView のスマート タグでページングを有効にするチェック ボックスをオンします。


[![図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**図 1**図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ ([フルサイズの画像を表示する をクリックします。](custom-formatting-based-upon-data-vb/_static/image3.png))。


これらの変更後、DetailsView マークアップが表示されます。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

このページをブラウザーでテストする時間がかかります。


[![DetailsView コントロールは、一度に 1 つの製品を表示します](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**図 2**:、DetailsView コントロール表示製品の 1 つずつ ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image6.png))。


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>手順 2: データ バインド イベント ハンドラー内のデータの値をプログラムで判断します。

これらの製品の太字、斜体のフォントの価格を表示するために持つ`UnitPrice`値 75.00 ドルを超えていること、最初にプログラムで判断できる必要があります、`UnitPrice`値。 これは、DetailsView をで実行できます、`DataBound`イベント ハンドラー。 イベントを作成するには、は、ハンドラーは、DetailsView、デザイナーでクリックし、[プロパティ] ウィンドウに移動します。 表示、または [表示] メニューに移動ではない場合は、起動、f4 キーを押して、[プロパティ] ウィンドウのメニュー オプションを選択します。 [プロパティ] ウィンドウからは、DetailsView のイベントを一覧表示する稲妻のアイコンをクリックします。 次に、ダブルクリックするか、`DataBound`イベントまたはイベント ハンドラーを作成する名前を入力します。


![データ バインド イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image7.png)

**図 3**: イベント ハンドラーを作成、`DataBound`イベント


> [!NOTE]
> ASP.NET ページのコードの部分からイベント ハンドラーを作成することもできます。 あるページの上部にある 2 つのドロップダウン リストがあります。 左側のドロップダウン リストからオブジェクトを選択し、右のドロップダウン リストと Visual Studio からのハンドラーを作成するイベントが自動的に適切なイベント ハンドラーを作成します。


これは自動的にイベント ハンドラーを作成に移動コード部分が追加されました。 この時点で表示されます。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

DetailsView にバインドされたデータを使用してアクセスできる、`DataItem`プロパティ。 コントロールに厳密に型指定された DataRow インスタンスのコレクションから構成される厳密に型指定された DataTable をバインドしていることを思い出してください。 DetailsView に、DataTable 内の最初の DataRow が割り当てられている、DetailsView に DataTable をバインドするとと、`DataItem`プロパティ。 具体的には、`DataItem`プロパティが割り当てられている、`DataRowView`オブジェクト。 使用して、`DataRowView`の`Row`が基になる DataRow オブジェクトへのアクセスを取得するプロパティが、実際に`ProductsRow`インスタンス。 これを行って`ProductsRow`インスタンス オブジェクトのプロパティの値を検査するだけで、意思決定を行んだことができます。

次のコード例を確認するかどうか、 `UnitPrice` DetailsView コントロールにバインドされている値 75.00 ドルを超えています。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> `UnitPrice`できますが、`NULL`データベース内の値は、最初に確認で扱っていないかどうかを確認する、`NULL`値にアクセスする前に、`ProductsRow`の`UnitPrice`プロパティ。 このチェックは重要ですのでにアクセスする場合、`UnitPrice`プロパティが、`NULL`値、`ProductsRow`オブジェクトがスローされます、 [StrongTypingException 例外](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)。


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>手順 3: DetailsView で UnitPrice の値の書式設定

この時点で判断できるかどうか、 `UnitPrice` DetailsView にバインドされている値を超える $75.00、値を持つが、DetailsView をプログラムで調整する方法については適切に書式のまだ用意しています。 DetailsView で行全体の書式設定を変更するプログラムでアクセスを使用して行`DetailsViewID.Rows(index)`; を変更する特定のセルを使用するアクセス`DetailsViewID.Rows(index).Cells(index)`します。 行またはセルの外観のスタイル関連プロパティを設定し、調整できるへの参照を取得したら。

行をプログラムでアクセスするには、0 から始まる行のインデックスがわかっている必要があります。 `UnitPrice`行が 4 のインデックスを付けることと、プログラムでアクセスできるように、DetailsView の 5 番目の行を使用して`ExpensiveProductsPriceInBoldItalic.Rows(4)`します。 この時点で、次のコードを使用して、太字、斜体のフォントで表示される行全体のコンテンツがある可能性があります。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

ただし、これにより、*両方*ラベル (価格) と太字と斜体の値。 値を作成した太字と斜体書式設定を 2 番目のセル、行は、次を使用して実現可能でこれを適用する場合。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

ここまでチュートリアルは、レンダリングされたマークアップとスタイル関連の情報が明確に分離を維持するためにスタイル シートを使用して後、は、上記みましょう代わりに、特定のスタイル プロパティを設定するのではなく CSS クラスを使用します。 開く、`Styles.css`スタイル シートという名前の新しい CSS クラスを追加および`ExpensivePriceEmphasis`を次の定義。


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

次に、`DataBound`イベント ハンドラーでは、設定、セルの`CssClass`プロパティを`ExpensivePriceEmphasis`します。 次のコードは、`DataBound`全体のイベント ハンドラー。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

通常フォントで価格が表示されます、Chai 75.00 ドル未満のコスト、これを表示するときに (図 4 参照)。 ただし、97.00 ドルの価格を持つ、Mishi 日本の神戸 Niku を表示すると、価格が太字、斜体のフォントに表示されます (図 5 を参照してください)。


[![通常フォントで $75.00 より低い価格が表示されます。](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**図 4**: 標準フォントで $75.00 より低い価格が表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image10.png))。


[![高価な製品の価格は太字、斜体フォントで表示されます。](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**図 5**: 太字、斜体フォントで高価な製品の価格が表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image13.png))。


## <a name="using-the-formview-controlsdataboundevent-handler"></a>FormView コントロールの`DataBound`イベント ハンドラー

FormView にバインドされている基になるデータを決定するための手順はものと同じですが、DetailsView を作成、`DataBound`イベント ハンドラーでは、キャスト、`DataItem`プロパティを適切なオブジェクトの種類が、コントロールにバインドされ、続行する方法を決定します。 FormView と DetailsView が異なる場合、ただしでのユーザー インターフェイスの外観を更新する方法。

FormView 任意 BoundFields が含まれていないとがないため、`Rows`コレクション。 テンプレートは、静的な HTML の組み合わせを含めることができます、FormView を構成する代わりに、Web コントロール、およびデータ バインド構文。 通常、フォーム ビューのスタイルを調整するには、FormView のテンプレート内の Web コントロールの 1 つ以上のスタイルを調整する必要があります。

これを示すためには、みましょうみましょうなどが、前の例で、この時点で製品の一覧をフォーム ビューを表示、製品名と単位だけ在庫は 10 以下の場合は、赤いフォントで表示単位と在庫です。

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>手順 4: FormView で製品情報を表示します。

フォーム ビューの追加、 `CustomColors.aspx` DetailsView やセットの下のページ、`ID`プロパティを`LowStockedProductsInRed`します。 FormView を前の手順で作成された ObjectDataSource コントロールにバインドします。 これが作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView の。 削除、`EditItemTemplate`と`InsertItemTemplate`および簡略化、`ItemTemplate`を含めるだけ`ProductName`と`UnitsInStock`値、それぞれを適切に名前付きの独自のラベル コントロールにします。 ある DetailsView、前述の例でも FormView のスマート タグでページングを有効にするチェック ボックスをチェックします。

これらの編集後に、フォーム ビューのマークアップは、次のようなになります。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

なお、`ItemTemplate`が含まれています。

- **静的な HTML**テキスト"Product:"と"在庫数:"と共に、`<br />`と`<b>`要素。
- **Web コントロール**2 つのラベル コントロール`ProductNameLabel`と`UnitsInStockLabel`します。
- **データ バインディング構文**、`<%# Bind("ProductName") %>`と`<%# Bind("UnitsInStock") %>`構文は、これらのフィールドから値をラベル コントロールに割り当てます`Text`プロパティ。

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>手順 5: データ バインド イベント ハンドラー内のデータの値をプログラムで判断します。

次の手順はフォーム ビューのマークアップを完全なプログラムによる場合を決定する、`UnitsInStock`値が 10 未満。 これには、DetailsView であったため、フォーム ビューとまったく同じ方法で実現されます。 FormView のため、イベント ハンドラーを作成して開始`DataBound`イベント。


![データ バインド イベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image14.png)

**図 6**: 作成、`DataBound`イベント ハンドラー


イベント ハンドラーにキャスト FormView の`DataItem`プロパティを`ProductsRow`インスタンス化し、決定かどうか、`UnitsInPrice`値が赤いフォントで表示する必要がありますようにします。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>手順 6: FormView の ItemTemplate の UnitsInStockLabel ラベル コントロールの書式設定

最後の手順では、表示されている書式設定を`UnitsInStock`赤いフォントで値の場合は、値が 10 個以下。 プログラムでアクセスする必要があります。 これを実現する、`UnitsInStockLabel`を制御、`ItemTemplate`し、そのテキストが赤で表示されるように、そのスタイル プロパティを設定します。 テンプレートでの Web コントロールにアクセスするには、使用、`FindControl("controlID")`このようなメソッド。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

この例では、ラベルにアクセスする必要のあるを制御`ID`値は`UnitsInStockLabel`を使用しますので。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Web コントロールへの参照をプログラムによって取得したら、必要に応じて、そのスタイルに関連するプロパティ変更できます。 前の例では、CSS クラスを設けて、`Styles.css`という`LowUnitsInStockEmphasis`します。 ラベルの Web コントロールには、このスタイルを適用するに次のように設定します。 その`CssClass`プロパティに応じて。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> プログラムにアクセスする Web コントロール テンプレートの書式を設定するための構文`FindControl("controlID")`を使用する場合、そのスタイル関連プロパティを設定しを使用こともできます[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) GridView、DetailsView でコントロール。 TemplateFields で、次のチュートリアルについて説明します。


図 7 製品を表示するときに、フォーム ビューを示していますが`UnitsInStock`図 8 の製品があるその値が 10 未満の値が 10 より大きい。


[![カスタム書式の適用の製品で、十分に大きい Units In Stock、](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**図 7**: の製品で、十分に大きい Units In Stock"、"カスタム書式の適用 ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image17.png))。


[![在庫数の単位は、製品の値を 10 以下の赤で表示します。](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**図 8**: これら製品での値が 10 以下の在庫数単位が赤で表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image20.png))。


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>GridView の書式が設定`RowDataBound`イベント

それ以前一連の手順、DetailsView を調べるし、FormView がデータ バインド時の進行状況を制御します。 見て次の手順をもう一度リフレッシャーとして。

1. データ Web コントロールの`DataBinding`イベントが発生します。
2. データ Web コントロールにバインドされます。
3. データ Web コントロールの`DataBound`イベントが発生します。

1 つのレコードのみを表示するので、これら 3 つの簡単な手順は、DetailsView と FormView 十分です。 表示されます、GridView の*すべて*レコードにバインドされている手順 2 に (最初) だけでなく、少し複雑です。

GridView は、データ ソースを列挙し、各レコードでは、次のように作成します。 2 の手順で、`GridViewRow`インスタンスと、現在のレコードをバインドします。 各`GridViewRow`GridView に追加すると、2 つのイベントが発生します。

- **`RowCreated`** 後に起動、`GridViewRow`が作成されました
- **`RowDataBound`** 現在のレコードにバインドされた後に発生、`GridViewRow`します。

GridView、データ バインディングより正確に記載されている、次の一連の手順で。

1. GridView の`DataBinding`イベントが発生します。
2. データは、GridView にバインドされます。   
  
   データ ソース内の各レコード 

    1. 作成、`GridViewRow`オブジェクト
    2. 火災、`RowCreated`イベント
    3. レコードにバインドします `GridViewRow`
    4. 火災、`RowDataBound`イベント
    5. 追加、`GridViewRow`を`Rows`コレクション
3. GridView の`DataBound`イベントが発生します。

GridView の個々 のレコードの形式をカスタマイズするには、次に、必要がありますのイベント ハンドラーを作成する、`RowDataBound`イベント。 これを示すためには、追加を GridView、`CustomColors.aspx`名前、カテゴリ、およびこれらの製品価格がより小さい $10.00 黄色の背景色で強調表示し、製品ごとの価格を一覧表示されたページ。

## <a name="step-7-displaying-product-information-in-a-gridview"></a>手順 7: GridView の製品情報を表示します。

前の例から、GridView、FormView 下を追加し、設定、`ID`プロパティを`HighlightCheapProducts`します。 ページ上のすべての製品を返す、ObjectDataSource が既にある、ために、GridView をバインドします。 最後に、製品の名前、カテゴリ、および価格だけを含める GridView の BoundFields を編集します。 これらの編集後よう GridView のマークアップになります。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

図 9 は、ブラウザーで表示した場合は、このポイントに進行状況を示します。


[![GridView は、名前、カテゴリ、および各製品の価格を一覧表示されます。](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**図 9**: GridView には、名前、カテゴリ、および各製品の価格が一覧表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image23.png))。


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>手順 8: RowDataBound イベント ハンドラー内のデータの値をプログラムで判断します。

ときに、 `ProductsDataTable` GridView にバインドされてその`ProductsRow`インスタンスが列挙され、各`ProductsRow`、`GridViewRow`が作成されます。 `GridViewRow`の`DataItem`特定にプロパティが割り当てられている`ProductRow`、その後、GridView の`RowDataBound`イベント ハンドラーが発生します。 決定する、`UnitPrice`製品ごとの値は、GridView にバインドし、GridView のイベント ハンドラーを作成する必要があります`RowDataBound`イベント。 このイベント ハンドラーを検査できる、 `UnitPrice` 、現在の値`GridViewRow`し、その行の書式設定を決定します。

このイベント ハンドラーは、同じ一連の手順としてをフォーム ビューと DetailsView を使用して作成できます。


![GridView の RowDataBound イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image24.png)

**図 10**: の GridView のイベント ハンドラーを作成`RowDataBound`イベント


この方法でイベント ハンドラーを作成すると、ASP.NET ページのコード部分に自動的に追加するのには、次のコードが発生します。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

ときに、`RowDataBound`イベントが起動され、イベント ハンドラーとして渡される 2 番目のパラメーター型のオブジェクト`GridViewRowEventArgs`、という名前のプロパティを持つ`Row`します。 このプロパティへの参照を返します、`GridViewRow`単データ バインドでした。 アクセスする、`ProductsRow`インスタンスにバインドされる、`GridViewRow`使用して、`DataItem`プロパティようになります。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

使用する場合、`RowDataBound`イベント ハンドラーの注意のこのイベントが発生して GridView がさまざまな種類の行で構成することが重要*すべて*行型。 A`GridViewRow`の型によって決定できるその`RowType`プロパティ、使用可能な値のいずれかを指定できます。

- `DataRow` GridView からのレコードにバインドされている行 `DataSource`
- `EmptyDataRow` 行の場合に表示されます、GridView の`DataSource`が空です
- `Footer` フッター行。表示されている場合、GridView の`ShowFooter`プロパティに設定 `True`
- `Header` ヘッダー行です。GridView の ShowHeader のプロパティに設定されているかどうかを示す`True`(既定値)
- `Pager` ページング、ページング インターフェイスを表示する行を実装するは、GridView
- `Separator` GridView では使用されませんで使用される、 `RowType` DataList と Repeater のプロパティを制御する 2 つのデータ Web コントロールを紹介する今後のチュートリアル

以降、 `EmptyDataRow`、 `Header`、`Footer`と`Pager`行に関連付けられている、`DataSource`レコードは常に値があるの`Nothing`の`DataItem`プロパティ。 このため、現在の作業を試みる前に`GridViewRow`の`DataItem`プロパティでは、最初にする必要があることを確認してを扱っていること、`DataRow`します。 これをチェックして実現できます、`GridViewRow`の`RowType`プロパティようになります。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>手順 9: が $10.00 未満には、行黄色と、UnitPrice の値を強調表示

最後の手順ではプログラム全体を強調表示を`GridViewRow`場合、`UnitPrice`行が $10.00 未満の値します。 GridView の行またはセルにアクセスするための構文は、DetailsView と同じ`GridViewID.Rows(index)`、行全体にアクセスする`GridViewID.Rows(index).Cells(index)`特定のセルにアクセスします。 ただし、ときに、`RowDataBound`イベント ハンドラーは、データ バインドを発生させる`GridViewRow`GridView に追加するのにはまだ`Rows`コレクション。 そのため、現在にアクセスできない`GridViewRow`インスタンスから、`RowDataBound`行のコレクションを使用してイベント ハンドラー。

代わりに`GridViewID.Rows(index)`、現在から参照できます`GridViewRow`インスタンス、`RowDataBound`イベント ハンドラーを使用して`e.Row`します。 現在強調表示の順序は、`GridViewRow`インスタンスから、`RowDataBound`イベント ハンドラーを使用します。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

設定ではなく、`GridViewRow`の`BackColor`プロパティを直接ものだけ作業 CSS クラスを使用するとします。 という名前の CSS クラスを作成しました`AffordablePriceEmphasis`背景色を黄色に設定します。 完成した`RowDataBound`イベント ハンドラーに従います。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![最も低コスト製品は黄色の強調表示されています。](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**図 11**: 最も低コスト製品は黄色の強調表示されている ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image27.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、GridView、DetailsView と FormView コントロールにバインドされたデータに基づいて書式設定する方法を説明しました。 イベント ハンドラーを作成してこれを実現する、`DataBound`または`RowDataBound`イベント、必要な場合、書式設定の変更と共にに調査が基になるデータの場所。 使用して、DetailsView またはフォーム ビューにバインドされたデータにアクセスする、`DataItem`プロパティ、`DataBound`イベント ハンドラーは gridview が開きます各`GridViewRow`インスタンスの`DataItem`プロパティには、で使用可能な、その行にバインドされたデータが含まれています。`RowDataBound`イベント ハンドラー。

データ Web コントロールの書式設定をプログラムで調整するための構文は、Web コントロールと書式設定するデータの表示方法によって異なります。 DetailsView を GridView コントロールでは、行およびセルを序数インデックスによってアクセスすることができます。 テンプレートを使用して、フォーム ビューの`FindControl("controlID")`メソッドは、通常、テンプレート内から Web コントロールの検索に使用します。

次のチュートリアルでは、GridView と DetailsView をテンプレートを使用する方法を紹介します。 さらに、基になるデータに基づく書式をカスタマイズするための別の手法を見ていきます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が E.R. Gilmore、Dennis Patterson、および Dan Jagers します。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [次へ](using-templatefields-in-the-gridview-control-vb.md)
