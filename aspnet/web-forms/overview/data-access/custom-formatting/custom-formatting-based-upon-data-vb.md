---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: "カスタムの書式設定データ (VB) に基づいて |Microsoft ドキュメント"
author: rick-anderson
description: "GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは l します."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 43aed94fe5b1095af37abdae2cb4c9e67b7d7f6f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="custom-formatting-based-upon-data-vb"></a>データ (VB) に基づくカスタム書式設定
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)または[PDF のダウンロード](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは、データ バインドのデータ バインドおよび RowDataBound のイベント ハンドラーを使用して書式設定を実行する方法を紹介します。


## <a name="introduction"></a>はじめに

多数のスタイル関連プロパティの使用は、GridView、DetailsView、フォーム ビューの各コントロールの外観をカスタマイズできます。 ようなプロパティ`CssClass`、 `Font`、 `BorderWidth`、 `BorderStyle`、 `BorderColor`、 `Width`、および`Height`、他、レンダリングされたコントロールの外観を決定します。 プロパティを含む`HeaderStyle`、 `RowStyle`、 `AlternatingRowStyle`、特定のセクションに適用される同じこれらのスタイル設定を許可するものとします。 同様に、これらのスタイル設定は、フィールド レベルで適用できます。

多くのシナリオでは、書式の要件によって異なります表示されるデータの値。 たとえばを描画する注意の製品在庫として保管する製品情報を一覧表示するレポート可能性があります設定をこれらの製品の黄色の背景色`UnitsInStock`と`UnitsOnOrder`フィールドは、どちらも 0 に等しい。 高価な製品を強調表示するには、コスト太字のフォントで 75.00 ドル以上のこれらの製品の価格を表示することもできます。

GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでに紹介をデータ バインドを使用すると書式設定を実行する方法、`DataBound`と`RowDataBound`イベント ハンドラー。 次のチュートリアルでは、その他の方法を調べておみます。

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>DetailsView コントロールを使用して`DataBound`イベント ハンドラー

データが、DetailsView にバインドされているデータ ソース コントロールから、またはコントロールのプログラムでデータを割り当てることによりとき`DataSource`プロパティと呼び出し元の`DataBind()`メソッドでは、次の一連の手順が発生します。

1. データ Web コントロールの`DataBinding`イベントが発生します。
2. データは、Web コントロールのデータにバインドされます。
3. データ Web コントロールの`DataBound`イベントが発生します。

カスタム ロジックは、手順 1. と手順 3 でイベント ハンドラーの直後に挿入できます。 イベント ハンドラーを作成することで、`DataBound`イベントされているデータがデータ Web コントロールにバインドされ、必要に応じて書式設定を調整するプログラムで判断できます。 この説明を示すためがあるが、製品に関する一般的な情報を一覧表示されます DetailsView を作成、`UnitPrice`値で、***太字、斜体のフォント***75.00 ドルを超えた場合。

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>手順 1: DetailsView で製品情報を表示します。

開く、 `CustomColors.aspx`  ページで、`CustomFormatting`フォルダー、DetailsView コントロールをツールボックスからデザイナーにドラッグして、設定、`ID`プロパティの値を`ExpensiveProductsPriceInBoldItalic`を起動する新しい ObjectDataSource コントロールにバインドし、 `ProductsBLL`クラスの`GetProducts()`メソッドです。 これを実現する詳細な手順は、ここに詳細に調査前のチュートリアルで後簡略化のためここで省略されます。

DetailsView に、ObjectDataSource をバインドしたら、実行、フィールドの一覧を変更するのにはすぐになります。 削除する選択した、 `ProductID`、 `SupplierID`、 `CategoryID`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、および`Discontinued`BoundFields の名前を変更し、残りの BoundFields を再フォーマットします。 消去したも、`Width`と`Height`設定します。 DetailsView には、単一のレコードだけが表示されたら、ためすべての製品を表示するのには、エンドユーザーに許可するためにページングを有効にする必要があります。 DetailsView のスマート タグの表示でページングを有効にするチェック ボックスをオンようになります。


[![図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**図 1**: 図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image3.png))


これらの変更後にマークアップを DetailsView になります。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

すぐにこのページをブラウザーでテストをします。


[![DetailsView コントロールでは、一度に 1 つの製品が表示されます。](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**図 2**:「DetailsView コントロールが表示されます 1 つの製品、一度に ([フルサイズ イメージを表示するに、をクリックして](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>手順 2: プログラムによって、データ バインドされたイベント ハンドラー内のデータの値の決定

太字、斜体のフォントをこれらの製品の価格を表示するために持つ`UnitPrice`値 75.00 ドルを超えています、最初にプログラムで確認できる必要があります、`UnitPrice`値。 これは、detailsview で実行できます、`DataBound`イベント ハンドラー。 イベントを作成するには、ハンドラーはデザイナーで DetailsView をクリックし、[プロパティ] ウィンドウに移動します。 表示、または、[表示] メニューに移動ではない場合は、表示、f4 キーを押して、[プロパティ] ウィンドウのメニュー オプションを選択します。 [プロパティ] ウィンドウを DetailsView のイベントを一覧表示される稲妻アイコンをクリックします。 次をダブルクリックするか、`DataBound`イベントまたはイベント ハンドラーを作成する名前を入力します。


![データ バインドされたイベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image7.png)

**図 3**: イベント ハンドラーを作成、`DataBound`イベント


> [!NOTE]
> ASP.NET ページのコードの部分から、イベント ハンドラーを作成することもできます。 ページの上部にある 2 つのドロップダウン リストを入手できます。 左のドロップダウン リストからオブジェクトを選択し、右のドロップダウン リストと Visual Studio からのハンドラーを作成するイベントが自動的に適切なイベント ハンドラーを作成します。


これは自動的にイベント ハンドラーを作成に移動コード部分が追加されました。 この時点で表示されます。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

DetailsView にバインドされたデータは、経由でアクセスできる、`DataItem`プロパティです。 厳密に型指定された DataRow インスタンスのコレクションから成るは厳密に型指定された DataTable に、コントロールのバインドはおことに注意してください。 DetailsView の DataTable 内の最初の DataRow が割り当てられている DataTable が DetailsView にバインドされると、`DataItem`プロパティです。 具体的には、`DataItem`プロパティが割り当てられている、`DataRowView`オブジェクト。 使用して、`DataRowView`の`Row`これは、基になる DataRow オブジェクトへのアクセスを取得するプロパティが、実際に`ProductsRow`インスタンス。 これを取得したら`ProductsRow`インスタンス オブジェクトのプロパティの値を検査するだけで決定することできます。

次のコード例を確認するかどうか、 `UnitPrice` DetailsView コントロールにバインドされている値 75.00 ドルを超えています。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> `UnitPrice`持つことができます、`NULL`データベース内の値は、最初に確認で扱っていないかどうかを確認する、`NULL`値にアクセスする前に、`ProductsRow`の`UnitPrice`プロパティです。 このチェックは重要なためにアクセスしようとしている場合、`UnitPrice`プロパティにある場合に、`NULL`値、`ProductsRow`オブジェクトがスローされます、 [StrongTypingException 例外](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)です。


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>手順 3: DetailsView で UnitPrice の値の書式設定

この時点で判断できるかどうか、 `UnitPrice` DetailsView にバインドされる値を超える $75.00、値が DetailsView をプログラムで調整する方法についての形式に応じてまだ示しました。 DetailsView で行全体の書式設定を変更するプログラムでアクセスを使用して行`DetailsViewID.Rows(index)`へのアクセスを使用して、特定のセルを変更する;`DetailsViewID.Rows(index).Cells(index)`です。 行またはセルの外観のスタイル関連プロパティを設定し、調整できますへの参照を作成したらです。

行をプログラムでアクセスするには、0 から始まる行のインデックスがわかっている必要があります。 `UnitPrice`行が 4 のインデックスを付けることと、プログラムでアクセスできるように、DetailsView の 5 番目の行を使用して`ExpensiveProductsPriceInBoldItalic.Rows(4)`です。 この時点で、次のコードを使用して、太字、斜体のフォントで表示される行全体のコンテンツがあります。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

ただし、これにより、*両方*ラベル (価格) と太字と斜体の値。 値だけ太字と斜体いただくために設定する行で、これは、次を使用して実現できます 2 番目のセルを書式設定を確認します。 場合


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

これまでのチュートリアルは、レンダリングされるマークアップとスタイルに関連する情報の間で明確に分離を維持するためにスタイル シートを使用して、以降は、上記のようにしてみましょう代わりに特定のスタイル プロパティの設定ではなく、CSS クラスを使用します。 開く、 `Styles.css` stylesheet という名前の新しい CSS クラスを追加および`ExpensivePriceEmphasis`を次の定義。


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

次に、 `DataBound` 、イベント ハンドラーを設定、セルの`CssClass`プロパティを`ExpensivePriceEmphasis`です。 次のコードは、`DataBound`全体のイベント ハンドラー。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

通常フォントで価格が表示されます、Chai 75.00 ドル未満のコスト、これを表示するときに (図 4 を参照してください)。 ただし、97.00 ドルの価格を持つ、Mishi 日本の神戸 Niku を表示するときに、価格が表示されます、太字、斜体のフォントで (図 5 を参照してください)。


[![通常フォントで $75.00 より低い価格が表示されます。](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**図 4**: 標準フォントで $75.00 より低い価格が表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image10.png))


[![負荷の高い製品の価格が太字、斜体のフォントで表示されます。](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**図 5**: 高価な製品の価格が太字、斜体のフォントで表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>FormView のコントロールを使用して`DataBound`イベント ハンドラー

フォーム ビューにバインドされている基になるデータを決定する手順は、DetailsView を作成するものと同じ、 `DataBound` 、イベント ハンドラーのキャスト、`DataItem`プロパティを適切なオブジェクト型が、コントロールにバインドされ、続行する方法を決定します。 FormView DetailsView が違う、ただし、独自のユーザー インターフェイスの外観を更新する方法です。

FormView 任意 BoundFields が含まれていないとがないため、`Rows`コレクション。 静的な HTML の組み合わせを含めることができる、テンプレートのフォーム ビューを構成する代わりに、Web コントロール、およびデータ バインド構文です。 通常、フォーム ビューのスタイルを調整するには、Web コントロールをフォーム ビューのテンプレート内での 1 つ以上のスタイルを調整する必要があります。

これを示すためには、みましょうみましょうなど、前の例はこの時点で一覧の製品をフォーム ビューを使用して表示製品名とユニットのみ在庫商品の在庫が 10 以下である場合は、赤いフォントで表示される数にします。

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>手順 4: FormView で製品情報を表示します。

フォーム ビューの追加、 `CustomColors.aspx` DetailsView やセットの下のページ、`ID`プロパティを`LowStockedProductsInRed`です。 FormView を前の手順で作成された ObjectDataSource コントロールにバインドします。 これが作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView 用です。 削除、`EditItemTemplate`と`InsertItemTemplate`および簡略化、`ItemTemplate`に含めるだけ`ProductName`と`UnitsInStock`独自の適切な名前のラベル コントロールでは、それぞれの値します。 同様に、前の例から DetailsView も FormView のスマート タグの表示でページングを有効にするチェック ボックスを確認します。

これらの編集後に、フォーム ビューのマークアップを次のようになります。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

なお、`ItemTemplate`が含まれています。

- **静的な HTML**テキスト"製品:"と"在庫:"と共に、`<br />`と`<b>`要素。
- **Web コントロール**2 つのラベル コントロール`ProductNameLabel`と`UnitsInStockLabel`です。
- **データ バインド構文**、`<%# Bind("ProductName") %>`と`<%# Bind("UnitsInStock") %>`構文は、これらのフィールドからラベル コントロールの値を割り当てます`Text`プロパティです。

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>手順 5: プログラムによって、データ バインドされたイベント ハンドラー内のデータの値の決定

FormView のマークアップが完了したら、次の手順がどうかをプログラムで判断は、`UnitsInStock`値が 10 未満です。 DetailsView でが FormView でまったく同じ方法で行われます。 FormView のイベント ハンドラーを作成して開始`DataBound`イベント。


![データ バインドされたイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image14.png)

**図 6**: 作成、`DataBound`イベント ハンドラー


イベント ハンドラーにキャスト FormView の`DataItem`プロパティを`ProductsRow`インスタンス化し、確認するかどうか、`UnitsInPrice`値のある赤いフォントで表示する必要があります。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>手順 6: FormView の ItemTemplate の UnitsInStockLabel ラベル コントロールの書式設定

最後の手順は、表示されている書式設定を`UnitsInStock`赤いフォントで値の場合は、値は 10 個以下です。 プログラムにアクセスする必要があります。 これを実現する、`UnitsInStockLabel`内の制御、`ItemTemplate`し、そのテキストが赤で表示されるように、そのスタイル プロパティを設定します。 テンプレート内の Web コントロールにアクセスするには、使用、`FindControl("controlID")`次のようなメソッド。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

いるコントロールのラベルにアクセスする例`ID`値は`UnitsInStockLabel`のでを使用します。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Web コントロールへのプログラムによる参照を作成したら、必要に応じてそのスタイル関連プロパティ変更できます。 前の例では、内の CSS クラスを作成しました、`Styles.css`という`LowUnitsInStockEmphasis`です。 ラベルの Web コントロールには、このスタイルを適用するに次のように設定します。 その`CssClass`プロパティに応じて。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> プログラムによって、コントロールを使用して Web にアクセスするテンプレートを書式設定するための構文`FindControl("controlID")`し、そのプロパティを設定スタイルに関連することもできますを使用する場合と[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView または GridView制御します。 [次へ]、チュートリアルで TemplateFields について確認します。


製品を表示するときに、図 7 は FormView を示しています。 が`UnitsInStock`図 8 に製品が、値が 10 未満の値は 10 より大きくします。


[![製品と、十分に大きな Units In Stock、カスタム書式が適用されます。](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**図 7**: の製品では十分に大きな Units In Stock、カスタム書式の適用 ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image17.png))


[![在庫数の単位は、製品の値を 10 以下の赤で表示されます。](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**図 8**: 在庫数のユニットが、それらの製品での値が 10 以下赤で表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>GridView の書式が設定`RowDataBound`イベント

以前 DetailsView の手順の順序を調べたりお、FormView では、データ バインド中に使用して進行状況を制御します。 見てみましょうこれらの手順をもう一度リフレッシャーとして。

1. データ Web コントロールの`DataBinding`イベントが発生します。
2. データは、Web コントロールのデータにバインドされます。
3. データ Web コントロールの`DataBound`イベントが発生します。

次の 3 つの単純な手順は、単一のレコードだけを表示するので、DetailsView と FormView 十分です。 表示する GridView の*すべて*バインドされているレコードに (だけでなく、最初の)、手順 2 がもう少し複雑です。

手順 2 GridView は、データ ソースを列挙し、各レコード作成で、`GridViewRow`をインスタンス化し、現在のレコードをバインドします。 各`GridViewRow`GridView に追加された、2 つのイベントが発生します。

- **`RowCreated`**後に起動、`GridViewRow`が作成されました
- **`RowDataBound`**現在のレコードにバインドされた後に発生、`GridViewRow`です。

GridView し、データ バインディングがより正確に記載されている、次の一連の手順で。

1. GridView の`DataBinding`イベントが発生します。
2. データが GridView にバインドされます。   
  
 データ ソース内の各レコード 

    1. 作成、`GridViewRow`オブジェクト
    2. Fire、`RowCreated`イベント
    3. レコードにバインドします`GridViewRow`
    4. Fire、`RowDataBound`イベント
    5. 追加、`GridViewRow`を`Rows`コレクション
3. GridView の`DataBound`イベントが発生します。

GridView の個々 のレコードの形式をカスタマイズするには、次に、必要がありますのイベント ハンドラーを作成する、`RowDataBound`イベント。 これを示すためには、追加する GridView、`CustomColors.aspx`名前、カテゴリ、および各製品の価格がより小さい $10.00 を黄色の背景色では、これらの製品を強調表示価格を一覧するページ。

## <a name="step-7-displaying-product-information-in-a-gridview"></a>手順 7: GridView で製品情報を表示します。

前の例から、フォーム ビューの下にある GridView を追加し、設定、`ID`プロパティを`HighlightCheapProducts`です。 ページ上のすべての製品を返す ObjectDataSource が既にある、ために、GridView をバインドします。 最後に、製品の名前、カテゴリ、および価格だけを含める GridView の BoundFields を編集します。 これらの編集した後、GridView のマークアップをようになります。


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

図 9 では、ブラウザーで表示したときに、このポイントに作業の進行状況を示します。


[![GridView の名前、カテゴリ、および各製品の価格を一覧表示します。](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**図 9**: GridView リスト名、カテゴリ、および各製品の価格 ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>手順 8: プログラムによって、RowDataBound イベント ハンドラー内のデータの値の決定

ときに、 `ProductsDataTable` GridView にバインドされてその`ProductsRow`インスタンスが列挙され、各`ProductsRow`、`GridViewRow`を作成します。 `GridViewRow`の`DataItem`特定するプロパティが割り当てられている`ProductRow`、その後、GridView の`RowDataBound`イベント ハンドラーが呼び出されました。 決定する、`UnitPrice`製品ごとの値は、GridView にバインドし、gridview のイベント ハンドラーを作成する必要があります`RowDataBound`イベント。 このイベント ハンドラーでおを検査できる、`UnitPrice`現在の値`GridViewRow`その行の書式設定に決定します。

このイベント ハンドラーは、フォーム ビューと DetailsView で同じ一連の手順としてを使用して作成できます。


![GridView の RowDataBound イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image24.png)

**図 10**: gridview のイベント ハンドラーを作成する`RowDataBound`イベント


この方法でイベント ハンドラーを作成すると、ASP.NET ページのコードの部分に自動的に追加するのには、次のコードが発生します。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

ときに、`RowDataBound`イベントの起動、イベント ハンドラーとして渡される 2 番目のパラメーター型のオブジェクト`GridViewRowEventArgs`、という名前のプロパティを持つ`Row`します。 このプロパティへの参照を返します、`GridViewRow`バインドされたデータだけでした。 アクセスする、`ProductsRow`インスタンスにバインド、`GridViewRow`使用して、`DataItem`プロパティ次のようにします。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

使用する場合、`RowDataBound`イベント ハンドラーが GridView は、さまざまな種類の行で構成される、このイベントが発生したことに注意する重要*すべて*行の型。 A`GridViewRow`の型によって決定できる、`RowType`プロパティ、有効な値のいずれかを持つことができます。

- `DataRow`GridView からのレコードにバインドされている行`DataSource`
- `EmptyDataRow`表示される場合、行、GridView の`DataSource`が空です
- `Footer`フッター行です。表示されている場合、GridView の`ShowFooter`プロパティに設定`True`
- `Header`ヘッダー行です。GridView の ShowHeader プロパティが に設定されているかどうかを示す`True`(既定)
- `Pager`ページング インターフェイスを表示する行のページングを実装する gridview
- `Separator`GridView は使用されませんで使用される、 `RowType` DataList およびリピータのプロパティを制御する 2 つのデータについて説明します将来的にチュートリアルの Web コントロール

以降、 `EmptyDataRow`、 `Header`、`Footer`と`Pager`にない行が関連付けられている、`DataSource`レコード、これらが常に、値は`Nothing`の`DataItem`プロパティです。 このため、現在の作業を試みる前に`GridViewRow`の`DataItem`プロパティ、お最初を確認してくださいを扱っている、`DataRow`です。 これには、チェックして、`GridViewRow`の`RowType`プロパティ次のようにします。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>手順 9: が $10.00 未満には、行黄色と、UnitPrice の値を強調表示

最後の手順ではプログラム全体を強調表示を`GridViewRow`場合、`UnitPrice`行が $10.00 未満の値します。 GridView の行またはセルへのアクセスの構文が DetailsView の場合と同じ`GridViewID.Rows(index)`全体の行にアクセスする`GridViewID.Rows(index).Cells(index)`特定のセルにアクセスします。 ただし、ときに、`RowDataBound`イベント ハンドラーがデータ バインドを発生させる`GridViewRow`GridView に追加するのにはまだ`Rows`コレクション。 そのため、現在にアクセスできない`GridViewRow`インスタンスから、`RowDataBound`行のコレクションを使用してイベント ハンドラー。

代わりに`GridViewID.Rows(index)`、現在は参照できます`GridViewRow`インスタンス、`RowDataBound`イベント ハンドラーを使用して`e.Row`です。 現在強調表示するには、`GridViewRow`インスタンスから、`RowDataBound`イベント ハンドラーを使用します。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

設定ではなく、`GridViewRow`の`BackColor`プロパティを直接だけ作業 CSS クラスを使用するとします。 名前付き CSS クラスを作成した`AffordablePriceEmphasis`背景色を黄色に設定します。 完成した`RowDataBound`イベント ハンドラーがフォローします。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![最も低コスト製品が黄色が強調表示されます。](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**図 11**: 最も低コスト製品は黄色が強調表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>まとめ

このチュートリアルでは、GridView、DetailsView、およびコントロールにバインドされたデータに基づくフォーム ビューの書式を設定する方法を説明しました。 イベント ハンドラーを作成してこれを実現する、`DataBound`または`RowDataBound`イベント、必要な場合は書式設定の変更と共にに検査が基になるデータの場所。 DetailsView または FormView にバインドされたデータにアクセスするを使用、`DataItem`プロパティに、`DataBound`イベント ハンドラー以外の場合は、GridView の各`GridViewRow`インスタンスの`DataItem`プロパティには、で使用可能な行にバインドされたデータが含まれています。`RowDataBound`イベント ハンドラー。

プログラムによってデータ Web コントロールの書式設定を調整するための構文は、Web コントロールと書式設定するデータの表示方法によって異なります。 DetailsView および GridView には、コントロール、行およびセルを序数に基づくインデックスによってアクセスできることができます。 テンプレートを使用して、フォーム ビューの`FindControl("controlID")`メソッドは、通常、テンプレート内から Web コントロールの検索に使用します。

次のチュートリアルでは、GridView と DetailsView でテンプレートを使用する方法について見ていきます。 さらに、基になるデータに基づく書式をカスタマイズするためのもう 1 つの方法を会いしましょう。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者が E.R. Gilmore、Dennis Patterson と Dan Jagers です。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](displaying-summary-information-in-the-gridview-s-footer-cs.md)
[次へ](using-templatefields-in-the-gridview-control-vb.md)
