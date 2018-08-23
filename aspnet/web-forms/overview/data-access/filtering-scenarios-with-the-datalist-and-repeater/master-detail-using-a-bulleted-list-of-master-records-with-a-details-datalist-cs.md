---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: マスター/詳細のマスター レコードの箇条書きリストを使用すると詳細 DataList (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでを前のチュートリアルの 2 ページのマスター/詳細レポートを 1 つのページに圧縮 t のカテゴリ名の箇条書きリストを表示.
ms.author: riande
ms.date: 10/17/2006
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a7c7494a58fa7941924145805f32aa67164fac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827407"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>マスター/詳細のマスター レコードの箇条書きリストを使用すると詳細 DataList (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe)または[PDF のダウンロード](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> このチュートリアルではを圧縮します前のチュートリアルの 2 ページのマスター/詳細レポートを単一のページ、画面の右側で選択したカテゴリの製品、画面の左側にあるカテゴリ名の箇条書きリストを表示します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-acess-two-pages-datalist-cs.md)2 つのページ間でマスター/詳細レポートを分離する方法を説明しました。 マスター ページでは、カテゴリの箇条書きリストを表示するために、Repeater コントロールを使用しました。 各カテゴリ名が、ハイパーリンク、クリックするが実行の詳細ページで、2 つの列 DataList がこれらの製品を表示するユーザーに属している、選択したカテゴリ。

このチュートリアルでを 2 ページのチュートリアルを 1 つのページに圧縮 LinkButton としてレンダリングの各カテゴリ名と、画面の左側にあるカテゴリ名の箇条書きリストを表示します。 カテゴリ名は、Linkbutton をクリックすると、ポストバックを誘発しますし、選択したカテゴリ、製品を画面の右側に 2 つの列 DataList にバインドします。 に加えて、各カテゴリの名前を表示するには、左側の Repeater を示していますが合計製品の数が特定のカテゴリが (図 1 参照)。


[![左側のカテゴリ名と製品の合計数が表示されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**図 1**: 左側のカテゴリの名前と製品の合計数が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))。


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>手順 1: 画面の左側の部分に、Repeater を表示します。

このチュートリアルでは、選択したカテゴリ、製品の左側に表示されるカテゴリの箇条書きリストを用意する必要があります。 標準 HTML 要素段落タグ、改行なしスペースを使用して web ページ内のコンテンツを配置できる`<table>`と s、またはカスケード スタイル シート (CSS) の手法を使用します。 CSS の手法を配置の使用チュートリアルのすべてがきませんでした。 マスター ページで、ナビゲーション ユーザー インターフェイスを構築しました、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルを使用して*絶対配置*ナビゲーションの正確なピクセルのオフセットを示す一覧と、メイン コンテンツ。 CSS を使用して、右または左から別の 1 つの要素を配置する代わりに、*浮動*します。 カテゴリの箇条書きリスト、Repeater、DataList の左側の浮動小数点では、選択したカテゴリの製品の左側に表示があっても

開く、`CategoriesAndProducts.aspx`ページから、`DataListRepeaterFiltering`フォルダー Repeater および DataList は、ページに追加します。 Repeater s 設定`ID`に`Categories`およびに DataList の`CategoryProducts`。 ソース ビューに移動し、独自の Repeater および DataList コントロールを配置`<div>`要素。 内で Repeater を囲む、`<div>`要素最初および独自の DataList し`<div>`Repeater の直後後の要素。 マークアップにこの時点でようになります次。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

DataList の左側に Repeater を float を使用して必要があります、 `float` CSS スタイル属性では、次のように。


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

`float: left;`最初をフローティング`<div>`要素、2 つ目の左にします。 `width`と`padding-right`設定は、1 つ目を示すもの`<div>`s`width`間に余白を追加し、 `<div>` s の要素のコンテンツおよびその右余白。 CSS で要素をフロートの詳細については、チェック アウト、 [Floatutorial](http://css.maxdesign.com.au/floatutorial/)します。

最初から直接スタイル設定を指定するのではなく`<p>`要素 s`style`属性、s の代わりに新しい CSS クラスを作成できるように`Styles.css`という`FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

置き換えることができ、`<div>`で`<div class="FloatLeft">`します。

CSS クラスを追加して、マークアップでを構成したら、 `CategoriesAndProducts.aspx`  ページで、デザイナーに移動します。 (ただし、右両方だけに表示されます灰色のボックスからそのデータ ソースまたはテンプレートを構成するには、まだ ve) は、DataList の左側に浮動 Repeater が表示されます。


[![DataList の左にフロート リピータ](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**図 2**:、Repeater、DataList の左にフロート ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))。


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>手順 2: 各カテゴリの製品の数を決定します。

完全なマークアップを囲む Repeater および DataList 秒、Repeater にカテゴリ データをバインドする準備ができたら制御します。 ただし、図 1 内のカテゴリの箇条書きリストに示す各カテゴリの名前だけでなく必要もありますカテゴリに関連付けられている製品の数を表示します。 この情報にアクセスできますか。

- **ASP.NET ページの分離コード クラスからこの情報を決定します。** 特定の指定された*`categoryID`* 呼び出すことによって関連付けられている製品の数を決定しました、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。 このメソッドが戻る、`ProductsDataTable`オブジェクト`Count`プロパティを示します数`ProductsRow`%s に存在して、指定した製品の数は *`categoryID`* します。 作成できる、 `ItemDataBound` 、Repeater にバインドされている各カテゴリの呼び出しを Repeater のイベント ハンドラー、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドの出力にその数が含まれています。
- **更新プログラム、`CategoriesDataTable`に型指定されたデータセットで、`NumberOfProducts`列。** 今後更新できるし、`GetCategories()`メソッドで、`CategoriesDataTable`にこの情報が含まれますか、または、`GetCategories()`として-を新規作成は、`CategoriesDataTable`というメソッド`GetCategoriesAndNumberOfProducts()`。

これらの手法の両方のシナリオについて s を使用できます。 最初の方法は、データ アクセス レイヤーを更新する必要があるので、実装する方が簡単ただし、データベースと複数の通信が必要です。 呼び出し、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッドで、`ItemDataBound`イベント ハンドラーは、Repeater に表示されるカテゴリごとに、余分なデータベース呼び出しを追加します。 この手法では、 *N* +、1 つのデータベース呼び出し、 *N* Repeater に表示されるカテゴリの数です。 各カテゴリに関する情報を 2 つ目のアプローチでは、製品の数が返される、`CategoriesBLL`クラス s `GetCategories()` (または`GetCategoriesAndNumberOfProducts()`) メソッドを 1 つだけのトリップでデータベースになります。

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>ItemDataBound イベント ハンドラー内の製品の数を決定します。

Repeater s の各カテゴリの製品の数を決定する`ItemDataBound`イベント ハンドラーでは、既存のデータ アクセス層への変更は必要ありません。 内で直接すべての変更ができるように、`CategoriesAndProducts.aspx`ページ。 という名前の新しい ObjectDataSource に追加して開始`CategoriesDataSource`Repeater s のスマート タグを使用しています。 次に、構成、 `CategoriesDataSource` ObjectDataSource ことから、そのデータを取得するため、`CategoriesBLL`クラスの`GetCategories()`メソッド。


[![CategoriesBLL クラス GetCategories() メソッドを使用する ObjectDataSource を構成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**図 3**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))。


内の各項目、 `Categories` Repeater はクリック可能にして、クリックされたときに発生する必要があります、 `CategoryProducts` DataList、選択したカテゴリの製品を表示します。 これは、ハイパーリンクは、この同じページに戻るリンクの各カテゴリを作成して実行できます (`CategoriesAndProducts.aspx`) を渡して、`CategoryID`前のチュートリアルで使用したのと同様に、クエリ文字列をします。 このアプローチの利点は、特定のカテゴリの製品を表示するページのブックマークを追加し、検索エンジンによってインデックス付けされたことです。

またはを実行できます各カテゴリ LinkButton、これは、アプローチのこのチュートリアルを使用します。 LinkButton はハイパーリンクとして、ユーザーのブラウザーに表示されますが、クリックすると、ポストバックを誘発します。、ポストバックの DataList の ObjectDataSource する必要がある、選択したカテゴリに属しているこれらの製品を表示するように更新します。 このチュートリアルでは、ハイパーリンクを使用する方が合理的; LinkButton を使用するよりもただしより便利ですが、LinkButton を使用して他のシナリオがあります。 ハイパーリンクのアプローチがこの例では最適である場合は、s の代わりに LinkButton を試すことができます。 おわかりのとおり、LinkButton を使用してはいないそれ以外の場合に発生する、ハイパーリンクにいくつかの課題について説明します。 そのため、このチュートリアルでは、LinkButton を使用してこれらの課題を強調表示をハイパーリンクではなく LinkButton を使用することがあります、これらのシナリオのソリューションを提供します。

> [!NOTE]
> ハイパーリンク コントロールを使用して、このチュートリアルを繰り返すことが推奨されますまたは`<a>`代わりに LinkButton 要素。


次のマークアップでは、Repeater、ObjectDataSource の宣言の構文を示しています。 Repeater s のテンプレートが LinkButton として各項目に箇条書きリストをレンダリングすることに注意してください。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> このチュートリアルでは、Repeater のビュー ステートを有効になっている必要があります (の省略の指定に注意してください、 `EnableViewState="False"` Repeater s の宣言構文から)。 手順 3. で作成しますイベント ハンドラー Repeater s の`ItemCommand`が更新されます DataList の ObjectDataSource のイベント`SelectParameters`コレクション。 Repeater の`ItemCommand`、ただし、ビュー ステートが無効になっている場合は、t 火災を獲得します。 参照してください[、ASP.NET の質問の A Stumper](http://scottonwriting.net/sowblog/posts/1263.aspx)と[そのソリューション](http://scottonwriting.net/sowBlog/posts/1268.aspx)理由の詳細についてはビュー ステートを Repeater s を有効する必要があります`ItemCommand`イベントが発生します。


LinkButton を`ID`プロパティ値の`ViewCategory`がその`Text`プロパティ セット。 カテゴリ名を表示するたいだけですが場合、私たちがプロパティを設定、テキスト宣言によって、データ バインドの構文で次のようにします。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

ただし、カテゴリの名前を表示する*と*そのカテゴリに属する製品の数。 Repeater s からこの情報を取得することができます`ItemDataBound`イベント ハンドラーを呼び出すことによって、`ProductBLL`クラス s`GetCategoriesByProductID(categoryID)`メソッドと、その結果で返されるレコードの数を決定する`ProductsDataTable`、次のコードとして示しています。


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

されるため、まずデータ項目で作業しています (1 つ持つ`ItemType`は`Item`または`AlternatingItem`) し、参照、`CategoriesRow`だけ現在のバインドされたインスタンス`RepeaterItem`。 次のインスタンスを作成してこのカテゴリの製品の数を決定しました、`ProductsBLL`クラスを呼び出してその`GetCategoriesByProductID(categoryID)`メソッドをおよびを使用して返されるレコードの数を決定する、`Count`プロパティ。 最後に、 `ViewCategory` itemtemplate LinkButton が参照し、その`Text`プロパティに設定されて*CategoryName* (*NumberOfProductsInCategory*) ここで、 *NumberOfProductsInCategory*は 10 進数の桁数が 0 の数として書式設定します。

> [!NOTE]
> 代わりに、追加されましたが、*関数の書式設定*カテゴリ s を受け入れる ASP.NET ページの分離コード クラスに`CategoryName`と`CategoryID`値を返します、`CategoryName`の数と連結カテゴリの製品 (呼び出すことによって決定される、`GetCategoriesByProductID(categoryID)`メソッド)。 このような書式設定関数の結果を LinkButton s の必要性を置き換えるテキスト プロパティを宣言して割り当てることができます、`ItemDataBound`イベント ハンドラー。 参照してください、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)または[DataList と Repeater のデータと書式設定](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md)書式設定関数の使用の詳細についてはチュートリアル。


このイベント ハンドラーを追加した後、ブラウザーでページをテストするのには少しを実行します。 各カテゴリがカテゴリの名前とカテゴリに関連付けられている製品の数を表示する、箇条書きリストに表示されている方法に注意してください (図 4 参照)。


[![各カテゴリ名と製品の数が表示されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**図 4**: 各カテゴリ名と製品の数が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))。


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>更新、`CategoriesDataTable`と`CategoriesTableAdapter`に各カテゴリの製品の数を含める

として、各カテゴリの製品の数を決定するのではなく、s が、Repeater にバインドを調整してこのプロセスを合理化します、`CategoriesDataTable`と`CategoriesTableAdapter`データ アクセス層をネイティブでこの情報を含めます。 これを実現する新しい列を追加する必要があります`CategoriesDataTable`関連付けられている製品の数を保持するためにします。 新しい列を DataTable に追加するに型指定されたデータセットを開きます (`App_Code\DAL\Northwind.xsd`) を変更するデータ テーブルを右クリックし、追加の選択、/列。 新しい列を追加、 `CategoriesDataTable` (図 5 を参照してください)。


[![CategoriesDataSource に新しい列を追加します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**図 5**: 新しい列を追加、 `CategoriesDataSource` ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))。


これはという名前の新しい列を追加`Column1`、これは、別の名前を入力するだけで変更できます。 この新しい列の名前を変更`NumberOfProducts`します。 次に、この列のプロパティを構成する必要があります。 新しい列をクリックし、[プロパティ] ウィンドウに移動します。 S 列を変更する`DataType`プロパティから`System.String`に`System.Int32`設定と、`ReadOnly`プロパティを`True`図 6 に示すように、します。


![データ型と新しい列の読み取り専用プロパティを設定します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**図 6**: 設定、`DataType`と`ReadOnly`新しい列のプロパティ


中に、`CategoriesDataTable`ようになりましたが、`NumberOfProducts`列で、対応する TableAdapter クエリのいずれかでその値は設定されません。 今後更新できる、`GetCategories()`カテゴリ情報を取得するたびに、このような情報が必要である場合は、この情報を返すメソッドが返されます。 かどうか、ただし、のみ (このチュートリアルのためにだけなど)、まれに、カテゴリに関連付けられている製品の数を取得する必要がありますしそのままにできます`GetCategories()`としてでは、この情報を返す新しいメソッドを作成します。 という名前の新しいメソッドを作成するこの後者のアプローチを使用して、let s`GetCategoriesAndNumberOfProducts()`します。

これを新しい追加`GetCategoriesAndNumberOfProducts()`メソッドを右クリックし、`CategoriesTableAdapter`し、新しいクエリ を選択します。 TableAdapter クエリ構成ウィザードで、私たち何回も前のチュートリアルで使用するアップが表示されます。 このメソッドには、クエリが行を返す、アドホック SQL ステートメントを使用することを示すことにより、ウィザードを起動します。


[![アドホック SQL ステートメントを使用して、メソッドを作成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**図 7**: アドホック SQL ステートメントを使用してメソッドを作成 ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))。


[![SQL ステートメントには、行が返されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**図 8**:、行の SQL ステートメントを返します ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))。


次のウィザード画面では、使用するクエリの場合、us ように求められます。 各カテゴリの s を返す`CategoryID`、 `CategoryName`、および`Description`と共に、カテゴリに関連付けられている製品の数のフィールドは、次を使用して`SELECT`ステートメント。


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![使用するクエリを指定します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**図 9**: 使用するクエリを指定 ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))。


カテゴリに関連付けられている製品の数を計算するサブクエリは、別名として`NumberOfProducts`します。 この名前付けの一致が関連付けられるには、このサブクエリによって返される値をにより、 `CategoriesDataTable` s`NumberOfProducts`列。

このクエリを入力した後は、最後の手順は、新しいメソッドの名前を選択します。 使用`FillWithNumberOfProducts`と`GetCategoriesAndNumberOfProducts`塗りつぶし DataTable 戻って DataTable パターン、それぞれします。


[![名前の新しい TableAdapter のメソッド FillWithNumberOfProducts と GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**図 10**: 新しい tableadapter のメソッドの名前を付けます`FillWithNumberOfProducts`と`GetCategoriesAndNumberOfProducts`([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))。


この時点で、データ アクセス層がカテゴリごとに製品の番号を含める拡張されました。 対応するを追加する必要がありますので、すべてのプレゼンテーション層が別のビジネス ロジック層から DAL に対するすべての呼び出しをルーティング`GetCategoriesAndNumberOfProducts`メソッドを`CategoriesBLL`クラス。


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

このデータにバインドする準備 re DAL BLL 完了と、`Categories`で Repeater `CategoriesAndProducts.aspx`! Ve を決めるから Repeater を既に作成して、ObjectDataSource 場合の製品の数、`ItemDataBound`イベント ハンドラー セクションでは、この ObjectDataSource および s Repeater を削除`DataSourceID`プロパティ unwire; の設定も、Repeater s`ItemDataBound`イベント、イベント ハンドラーを削除してから、 `Handles Categories.OnItemDataBound` ASP.NET 分離コード クラスの構文。

元の状態に戻す Repeater、という名前の新しい ObjectDataSource を追加`CategoriesDataSource`Repeater s のスマート タグを使用しています。 構成を使用する ObjectDataSource、`CategoriesBLL`クラスが使用することではなく、`GetCategories()`メソッドが使用する必要が`GetCategoriesAndNumberOfProducts()`代わりに (図 11 を参照してください)。


[![ObjectDataSource GetCategoriesAndNumberOfProducts メソッドを使用して構成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**図 11**: 構成に使用する ObjectDataSource、`GetCategoriesAndNumberOfProducts`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))。


次に、更新、`ItemTemplate`ように LinkButton s`Text`プロパティがデータ バインディング構文を使用して宣言によって割り当てられ、両方が含まれる、`CategoryName`と`NumberOfProducts`データ フィールド。 リピータの完全な宣言型マークアップと`CategoriesDataSource`ObjectDataSource に従ってください。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

含める DAL を更新することによってレンダリングされる出力を`NumberOfProducts`列は使用する場合と同じ、`ItemDataBound`イベント ハンドラーのアプローチ (参照画面を表示する図 4 カテゴリ名と製品の数を示す Repeater のスクリーン ショット)。

## <a name="step-3-displaying-the-selected-category-s-products"></a>手順 3: 選択したカテゴリ、製品を表示します。

この時点である、 `Categories` Repeater の各カテゴリと製品の数のカテゴリの一覧を表示します。 リピータ LinkButton を使用して、クリックすると、位置、ポストバックの問題が発生ポイント各カテゴリで選択したカテゴリには、その製品を表示する必要があります、 `CategoryProducts` DataList します。

私たちが直面している課題の 1 つは、DataList、選択したカテゴリの製品だけを表示する方法です。 [マスター/詳細 DetailsView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)GridView の行を構築する方法を説明しましたチュートリアルを選択できませんでした、選択した行の詳細、DetailsView、同じページ上に表示されています。 GridView の ObjectDataSource が返される情報を使用してすべての製品について、 `ProductsBLL` s`GetProducts()`メソッド DetailsView の ObjectDataSource を使用して、選択した製品に関する情報は取得、`GetProductsByProductID(productID)`メソッド。 *`productID`* GridView s の値に関連付けることによってパラメーター値を宣言によって指定された`SelectedValue`プロパティ。 残念ながら、Repeater はありません、`SelectedValue`プロパティおよびパラメーターのソースとして使用できません。

> [!NOTE]
> これは、Repeater で LinkButton を使用するときに表示されるこれらの課題の 1 つです。 渡すためのハイパーリンクを使いましたが、`CategoryID`クエリ文字列を代わりにクエリ文字列フィールド ソースとして使用、パラメーターの値。


ないかと心配する前に、 `SelectedValue` 、Repeater のプロパティは、最初、ObjectDataSource、DataList にバインドし、指定の秒をできるようにその`ItemTemplate`します。

という名前の新しい ObjectDataSource を追加することを選択、DataList s のスマート タグから`CategoryProductsDataSource`を使用するように構成し、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。 このチュートリアルでは、DataList では、読み取り専用のインターフェイスを提供するので自由 INSERT、UPDATE でドロップダウン リストを設定し、(なし) タブを削除します。


[![ObjectDataSource ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**図 12**: 構成に使用する ObjectDataSource`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))。


以降、`GetProductsByCategoryID(categoryID)`メソッドに入力パラメーターが必要ですが (*`categoryID`*)、データ ソース構成ウィザードでは、パラメーター %s のソースを指定できます。 コントロールを処理するパラメーターのソースのドロップダウン リストを設定カテゴリは、GridView、DataList に指定されていた、私たちの d、`ID`データ Web コントロールの。 ただし、Repeater の欠如以降、`SelectedValue`プロパティ パラメーターのソースとして使用できません。 チェックすると、見つかります ControlID のドロップダウン リストが 1 つのコントロールにはのみ含まれている`ID``CategoryProducts`、 `ID` DataList の。

ここでは、パラメーター ソースのドロップダウン リストを None に設定します。 カテゴリ、Repeater の LinkButton がクリックされたときにこのパラメーターの値をプログラムによって割り当てられるためです。


[![CategoryID パラメーターのパラメーターのソースは指定されていません](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**図 13**: パラメーターのソースを指定して、 *`categoryID`* パラメーター ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))。


データ ソース構成ウィザードを完了すると、Visual Studio 自動生成 DataList の`ItemTemplate`します。 この既定値を置き換える`ItemTemplate`テンプレートを使用しての 前のチュートリアルで使用されている; も、DataList s を設定しました`RepeatColumns`プロパティを 2。 これらの変更を行った後、DataList コントロールとその関連付けられている ObjectDataSource の宣言型マークアップは次のようになります。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

現時点では、 `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* パラメーターは設定されません、ページを表示するときに製品が表示されないようにします。 このパラメーターの値に基づいて設定が行う必要があります、 `CategoryID` Repeater でクリックされたカテゴリの。 2 つのチャレンジが導入されます: 最初に、私たち特定方法と Repeater s で LinkButton`ItemTemplate`クリック;、第 2 にされていますがどのように確認する、`CategoryID`の LinkButton がクリックしてされた対応するカテゴリのでしょうか。

ボタンと ImageButton コントロールと同様に、LinkButton が、`Click`イベントと[`Command`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx)します。 `Click`イベントの目的は単に、LinkButton がクリックしてされたことに注意してください。 ただし、だけでなく、LinkButton がクリックしてされたことに注意してください。 必要もありますイベント ハンドラーに追加情報を渡す。 場合、LinkButton s [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx)と[ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx)プロパティがこの追加情報を割り当てられることができます。 次に、LinkButton がクリックされたときにその`Command`イベントが発生します (の代わりにその`Click`イベント) の値をイベント ハンドラーに渡されます、`CommandName`と`CommandArgument`プロパティ。

ときに、 `Command` Repeater、Repeater s でテンプレート内からイベントが発生[`ItemCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx)が起動し、渡される、`CommandName`と`CommandArgument`クリック LinkButton の値 (またはボタンまたはImageButton)。 そのため、カテゴリ、Repeater で LinkButton がクリックしてされたときを特定するには、次の必要があります。

1. 設定、 `CommandName` Repeater s で LinkButton のプロパティ`ItemTemplate`になんらかの値 (は ListProducts も使用しました)。 これを設定して`CommandName`値、LinkButton の`Command`イベント、LinkButton がクリックされたときに発生します。
2. LinkButton s 設定`CommandArgument`プロパティの現在の項目の値を`CategoryID`します。
3. Repeater s のイベント ハンドラーを作成`ItemCommand`イベント。 イベント ハンドラーの設定、 `CategoryProductsDataSource` ObjectDataSource s`CategoryID`パラメーターを渡された内の値`CommandArgument`します。

次`ItemTemplate`カテゴリ Repeater のマークアップは、手順 1. および 2. を実装します。 注方法、`CommandArgument`値には、データ項目の s が割り当てられる`CategoryID`データ バインディング構文を使用して。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

作成されるたびに、`ItemCommand`イベント ハンドラーが常に、まず受信を確認することをお勧め`CommandName`ため、その値*任意*`Command`によってイベントが発生した*任意*ボタン、LinkButton、またはRepeater 内 ImageButton が発生、`ItemCommand`イベントが発生します。 現在のみがこのような 1 つの LinkButton これで、今後します (または、チームの他の開発者) 可能性があります ボタンが追加 Web コントロールを追加、Repeater をクリックすると、同じを発生させます。`ItemCommand`イベント ハンドラー。 そのため、常に確認することを確認するのベスト、`CommandName`プロパティと値を指定して、と一致する場合のみ、プログラム ロジックに進みます。

渡されたにすることを確認したら`CommandName`ListProducts と等しい値をイベント ハンドラーを割り当てます、 `CategoryProductsDataSource` ObjectDataSource s`CategoryID`パラメーターを渡された内の値`CommandArgument`します。 この変更を ObjectDataSource に`SelectParameters`自体を新しく選択したカテゴリの製品が表示されたデータ ソースに再バインドする DataList を自動的に発生します。


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

これらの項目を追加します。 このチュートリアルが完了しました。 ブラウザーでテストを実行する時間がかかります。 図 14 では、最初のページにアクセスしたときに、画面が表示されます。 選択するカテゴリがまだのための製品は表示されません。 2 つの列のビューで、製品カテゴリでこれらの製品が表示されます、生成などのカテゴリをクリックすると (図 15 を参照してください)。


[![表示されるときに最初にアクセスしてページを要求している製品はありません。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**図 14**: No 製品が表示されるときに最初にアクセスしてページ ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))。


[![生成カテゴリの一覧の右側に一致する製品のクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**図 15**: 生成カテゴリ をクリックして、右側に一致する製品を一覧表示されます ([フルサイズの画像を表示する をクリックします](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))。


## <a name="summary"></a>まとめ

このチュートリアルを上記で説明したように、マスター/詳細レポートは 2 つのページに分散させることができます。 または 1 つに統合します。 マスター/詳細レポートを表示する、1 つのページで、ただし、紹介方法のいくつかの課題最適なレイアウト、マスターと詳細レコード ページをします。 *マスター/詳細 DetailsView で選択可能なマスター GridView の使用について詳しく説明*マスター レコードの上に表示する詳細レコードがありましたこのチュートリアルでのマスター レコード float に CSS 手法を使用して、チュートリアル、。詳細の左端位置。

マスター/詳細レポートを表示するには、と共にもありました内からクリック LinkButton (またはボタンまたは ImageButton) ときに、サーバー側ロジックを実行する方法にも、各カテゴリに関連付けられた製品の数を取得する方法を調査する機会をRepeater します。

このチュートリアルでは、DataList と Repeater によるマスター/詳細レポート、調査を完了します。 次の一連のチュートリアルでは、編集と削除、DataList コントロールに機能を追加する方法について説明します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) CSS で CSS 要素をフロートに関するチュートリアル
- [CSS の配置](http://www.brainjar.com/css/positioning/)CSS を使用して要素の配置の詳細について
- [Html には、「コンテンツをレイアウトする](http://www.w3schools.com/html/html_layout.asp)を使用して`<table>`s と他の HTML 要素を配置する

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [次へ](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
