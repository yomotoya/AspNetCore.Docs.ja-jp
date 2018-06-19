---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: 詳細 DataList (VB) のマスター レコードの箇条書きリストを使用してマスター/詳細 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでを前のチュートリアルの 2 ページ マスター/詳細レポートを単一のページに圧縮お t にカテゴリ名の箇条書きリストを表示しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d87dc7f4fb00e96d9eb2653e6fbc1efb8bb656c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889021"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>マスター/詳細詳細 DataList (VB) でマスター レコードの箇条書きリストの使い方
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe)または[PDF のダウンロード](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> このチュートリアルでを前のチュートリアルの 2 ページ マスター/詳細レポートを単一のページに圧縮お画面と、画面の右側に、選択したカテゴリの製品の左側にあるカテゴリ名の箇条書きリストを表示します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-acess-two-pages-datalist-vb.md)2 つのページ間でマスター/詳細レポートを分離する方法について説明しました。 マスター ページでは、Repeater コントロールを使用して、カテゴリの箇条書きリストを表示します。 各カテゴリ名が、ハイパーリンク、クリックするが実行の詳細ページで、2 つの列 DataList がこれらの製品を表示するユーザーに属している、選択したカテゴリ。

このチュートリアルでを 2 ページ チュートリアルを単一のページに圧縮お LinkButton としてレンダリングの各カテゴリ名と、画面の左側にあるカテゴリ名の箇条書きリストを表示します。 あるカテゴリ名のいずれかをクリックし、ポストバックを発生させる、選択したカテゴリの製品を画面の右側に 2 つの列 DataList にバインドします。 各カテゴリの名前を表示するだけでなく、左側にリピータを示しています合計製品数は、1 つのカテゴリ (図 1 を参照してください)。


[![カテゴリ名と製品の合計数が、左側に表示されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**図 1**: カテゴリ名と製品の合計数が、左側に表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>手順 1: 画面の左側の部分にリピータを表示します。

このチュートリアルでは、選択したカテゴリの製品の左側に表示されるカテゴリの行頭文字付き一覧を持つ必要があります。 標準の HTML 要素の段落タグ、改行しないスペースを使用して web ページ内のコンテンツを配置することができます`<table>`秒、およびなまたはカスケード スタイル シート (CSS) 手法を使用します。 すべてチュートリアルのこれまでが使用 CSS 手法の位置を決定します。 マスター ページで、ナビゲーションのユーザー インターフェイスを構築お、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-vb.md)を使用したチュートリアル*絶対配置*ナビゲーションのピクセル単位のオフセットを示すリストとメイン コンテンツ。 CSS を使用して、右または左を介して別の 1 つの要素を配置する代わりに、*浮動*です。 DataList の左側にリピータを浮動小数点では、選択したカテゴリ製品の左側に表示カテゴリの箇条書きリストがあっても

開く、`CategoriesAndProducts.aspx`ページから、`DataListRepeaterFiltering`フォルダー リピータと DataList のページに追加します。 リピータ s を設定`ID`に`Categories`およびを DataList の`CategoryProducts`です。 ソース ビューに移動し、独自にリピータおよび DataList コントロールを配置`<div>`要素。 つまりにリピータを囲む、`<div>`要素最初およびそれ自体の DataList し`<div>`リピータのすぐ後に要素。 マークアップにこの時点でなります次のような。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

DataList の左側にリピータを float、する必要がありますを使用する、 `float` CSS スタイル属性では、次のようにします。


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

`float: left;`最初を寄せて配置`<div>`要素、2 つ目の左側にします。 `width`と`padding-right`設定による最初`<div>`s`width`の間に余白を追加し、`<div>`要素 %s のコンテンツとその右余白。 CSS で要素をフローティング状態の詳細については、チェック アウト、 [Floatutorial](http://css.maxdesign.com.au/floatutorial/)です。

最初から直接スタイル設定を指定するのではなく`<p>`要素 s`style`属性、s の代わりに、新しい CSS クラスを作成できるように`Styles.css`という`FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

置き換えることができ、`<div>`で`<div class="FloatLeft">`です。

CSS クラスを追加して、内のマークアップを構成した後、 `CategoriesAndProducts.aspx`  ページで、デザイナーに移動します。 (右両方だけに表示されます灰色のボックスお以降、データ ソースまたはテンプレートを構成するには、まだ ve) には、DataList の左側に浮動小数点リピータが表示されます。


[![リピータは、DataList の左にフロートします。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**図 2**:「リピータは、DataList 左にフロート ([フルサイズのイメージを表示するには、をクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>手順 2: 各カテゴリの製品の数を決定します。

完全なマークアップを囲むリピータおよび DataList s、おリピータにカテゴリ データをバインドする準備ができたら、次の操作を制御します。 ただし、図 1 内のカテゴリの箇条書きリストでは、各カテゴリの名前だけでなくも必要がありますをカテゴリに関連付けられている製品の数を表示します。 この情報にアクセスできますか。

- **ASP.NET ページの分離コード クラスのこの情報を決定します。** 指定された特定の*`categoryID`* ところを呼び出して関連付けられている製品の数を判断することができます、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。 このメソッドが戻る、`ProductsDataTable`オブジェクト`Count`プロパティを示します数`ProductsRow`%s に存在して、指定した製品の数は *`categoryID`* です。 作成できるよう、 `ItemDataBound` Repeater を呼び出し、リピータにバインドされている各カテゴリのイベント ハンドラー、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドの出力に、カウントが含まれています。
- **更新プログラム、`CategoriesDataTable`に含める型指定されたデータセットで、`NumberOfProducts`列です。** 更新することが、`GetCategories()`メソッドで、`CategoriesDataTable`を含めることがこの情報、または、`GetCategories()`として-を新規作成は、`CategoriesDataTable`呼び出されるメソッド`GetCategoriesAndNumberOfProducts()`です。

S、これらの手法の両方のシナリオを使用できます。 最初の方法は、データ アクセス層; を更新する必要ありませんので、実装に簡単です。ただし、データベースと複数の通信が必要です。 呼び出し、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッドで、`ItemDataBound`リピータに表示されるカテゴリごとに、追加のデータベース呼び出しをイベント ハンドラーに追加します。 この手法では*N* + 1 つのデータベース呼び出し、ここで*N*リピータに表示されるカテゴリの数です。 各カテゴリに関する情報を 2 番目の方法で製品の数が返される、`CategoriesBLL`クラス s `GetCategories()` (または`GetCategoriesAndNumberOfProducts()`) メソッドを招き、1 つだけのトリップで結果としてデータベースにします。

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>ItemDataBound イベント ハンドラー内の製品の数を決定します。

リピータ s 内の各カテゴリの製品の数を決定する`ItemDataBound`イベント ハンドラーは、既存のデータ アクセス層に変更を加える必要ありません。 直下にあるすべての変更ができるように、`CategoriesAndProducts.aspx`ページ。 という名前の新しい ObjectDataSource を追加することによって開始`CategoriesDataSource`リピータ s のスマート タグを使用しています。 次に、構成、 `CategoriesDataSource` ObjectDataSource it からのデータの取得のため、`CategoriesBLL`クラスの`GetCategories()`メソッドです。


[![構成、ObjectDataSource CategoriesBLL クラスの GetCategories() メソッドを使用するには](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**図 3**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


内の各項目、`Categories`リピータに必要がありますクリック可能なクリックすると、原因、 `CategoryProducts` DataList を選択したカテゴリの製品を表示します。 これには、各カテゴリをハイパーリンクは、このページにリンクすることで (`CategoriesAndProducts.aspx`) を渡して、`CategoryID`前のチュートリアルで説明したのと同様に、クエリ文字列を通じてします。 このアプローチの利点は、特定のカテゴリの製品を表示するページのブックマークが付けられたおよび検索エンジンによってインデックス設定されることです。

また、お寄せください各カテゴリ LinkButton、これは、このチュートリアルの使用方法です。 LinkButton がハイパーリンクとしてユーザーのブラウザーで表示されますが、クリックされたときに即して、ポストバックポストバックで DataList の ObjectDataSource 更新する必要が、選択したカテゴリに属する製品を表示します。 このチュートリアルでは、ハイパーリンクを使用する方が有意義 LinkButton; を使用するよりもただし、LinkButton を使用して複数のメリットがあるその他のシナリオがあります。 この例の理想的ながあるは、ハイパーリンク方法、中に s の LinkButton を使用する代わりに探索を使用できます。 おわかりのよう LinkButton を使用していないそれ以外の場合に発生する、ハイパーリンクと課題がいくつか導入されています。 そのため、このチュートリアルでは、LinkButton を使用してこれらの課題を強調表示されハイパーリンクではなく、LinkButton を使用して必要のあるシナリオのソリューションを実現するのに役立つされます。

> [!NOTE]
> ハイパーリンク コントロールを使用してこのチュートリアルを繰り返すことをお勧めまたは`<a>`代わりに、LinkButton 要素。


次のマークアップでは、リピータと、ObjectDataSource 宣言の構文を示します。 リピータのテンプレートが LinkButton として各項目に行頭文字付きリストをレンダリングすることに注意してください。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> このチュートリアルでは、リピータのビュー ステートを有効になっている必要があります (の省略を注意してください、`EnableViewState="False"`リピータ s 宣言構文から)。 手順 3. で作成するイベント ハンドラー リピータ s の`ItemCommand`イベントで更新 DataList の ObjectDataSource の`SelectParameters`コレクション。 リピータの`ItemCommand`、ただし、ビュー ステートが無効になっている場合、t 火災を優先します。 参照してください[ASP.NET 質問の A Stumper](http://scottonwriting.net/sowblog/posts/1263.aspx)と[そのソリューション](http://scottonwriting.net/sowBlog/posts/1268.aspx)理由についての詳細ビュー ステートを有効にするリピータの`ItemCommand`イベントが発生します。


LinkButton を`ID`のプロパティの値`ViewCategory`がその`Text`プロパティ セットです。 だけにする場合、カテゴリ名を表示、おが Text プロパティを設定宣言によって、データ バインドの構文を次のようにします。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

ただし、カテゴリの名前を表示する*と*そのカテゴリに属している製品の数。 リピータ s からこの情報を取得することができます`ItemDataBound`イベント ハンドラーを呼び出すことによって、`ProductBLL`クラス s`GetCategoriesByProductID(categoryID)`メソッドと、その結果で返されるレコードの数を決定する`ProductsDataTable`、次のコードとして示しています。


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

確認することから始めましょうデータ項目で作業して (1 つが`ItemType`は`Item`または`AlternatingItem`) し、参照、`CategoriesRow`だけが、現在にバインドされているインスタンス`RepeaterItem`です。 次のインスタンスを作成することでこのカテゴリの製品の数を決定したら、`ProductsBLL`クラス、呼び出し、`GetCategoriesByProductID(categoryID)`メソッド、および使用して返されるレコードの数を決定する、`Count`プロパティです。 最後に、 `ViewCategory` ItemTemplate の LinkButton が参照とその`Text`プロパティに設定されている*CategoryName* (*NumberOfProductsInCategory*) ここで、 *NumberOfProductsInCategory*がゼロの小数点以下桁数を表す数値として書式設定します。

> [!NOTE]
> または、でしたが追加されました、*関数の書式設定*ASP.NET ページの分離コード クラスをカテゴリ s を受け付ける`CategoryName`と`CategoryID`、値を返します、`CategoryName`の数を連結しました。カテゴリの製品 (呼び出しによって決定される、`GetCategoriesByProductID(categoryID)`メソッド)。 このような書式設定関数の結果は LinkButton s の必要性を置き換えるテキスト プロパティを宣言によって割り当て可能、`ItemDataBound`イベント ハンドラー。 参照してください、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)または[DataList とリピータ ベース時のデータの書式設定](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md)書式設定関数を使用する方法についてのチュートリアルです。


このイベント ハンドラーを追加すると、すぐをブラウザーでページをテストします。 各カテゴリ、カテゴリの名前とカテゴリに関連付けられている製品の数を表示する、箇条書きリストに表示する方法 (図 4 を参照してください)。


[![各カテゴリ名と製品の数が表示されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**図 4**: 各カテゴリ名と製品の数が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>更新、`CategoriesDataTable`と`CategoriesTableAdapter`に各カテゴリの製品の数を含める

リピータには、各カテゴリの製品の数を決定するのではなく s がバインドされている調整することでこのプロセスを合理化お、`CategoriesDataTable`と`CategoriesTableAdapter`情報が含まれるこのネイティブ データ アクセス層にします。 これを実現するの新しい列を追加します`CategoriesDataTable`関連付けられている製品の数を保持します。 DataTable に新しい列を追加するに型指定されたデータセットを開きます (`App_Code\DAL\Northwind.xsd`) を変更するデータ テーブルを右クリックし、追加/列です。 新しい列を追加、 `CategoriesDataTable` (図 5 を参照してください)。


[![CategoriesDataSource に新しい列を追加します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**図 5**: 新しい列を追加、 `CategoriesDataSource` ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


これによりという名前の新しい列が追加されます`Column1`、これは、別の名前を入力するだけで変更できます。 この新しい列の名前を変更`NumberOfProducts`です。 次に、この列のプロパティを構成する必要があります。 新しい列をクリックし、[プロパティ] ウィンドウに移動します。 S 列を変更する`DataType`プロパティから`System.String`に`System.Int32`設定と、`ReadOnly`プロパティを`True`図 6 に示すように、します。


![データ型と新しい列の ReadOnly プロパティを設定します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**図 6**: 設定、`DataType`と`ReadOnly`新しい列のプロパティ


中に、`CategoriesDataTable`ようになりました、`NumberOfProducts`列で、その値が設定されていない対応する TableAdapter のクエリのいずれか。 更新することが、`GetCategories()`カテゴリ情報を取得するたびなどの情報をたい場合は、この情報を返すメソッドが返されます。 、ただし、まれに (このチュートリアル用になど)、カテゴリに関連付けられている製品の数を取得するだけで済む場合しなくてもかまいません`GetCategories()`としてでは、この情報を返す新しいメソッドを作成します。 Let s という名前の新しいメソッドを作成するこの後者のアプローチを使用して`GetCategoriesAndNumberOfProducts()`です。

これを新規に追加する`GetCategoriesAndNumberOfProducts()`メソッドを右クリックし、`CategoriesTableAdapter`新しいクエリを選択します。 これにより、TableAdapter クエリ構成ウィザードで、どのお何回前のチュートリアルで使用するアップされます。 このメソッドでクエリが行を返す、アドホック SQL ステートメントを使用していることを示すウィザードを起動します。


[![アドホック SQL ステートメントを使用するメソッドを作成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**図 7**: アドホック SQL ステートメントを使用してメソッドを作成 ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![SQL ステートメントには、行が返されます。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**図 8**:、行の SQL ステートメントを返します ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


次のウィザード画面では、使用するクエリの場合、us ように求められます。 各カテゴリの s を返す`CategoryID`、 `CategoryName`、および`Description`、カテゴリに関連付けられている製品の数と共に、フィールドは、次を使用して`SELECT`ステートメント。


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![使用するクエリを指定します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**図 9**: 使用するクエリを指定 ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


カテゴリに関連付けられている製品の数を計算するサブクエリは、別名として`NumberOfProducts`です。 この名前付けの一致とを関連付けるには、このサブクエリによって返される値、 `CategoriesDataTable` s`NumberOfProducts`列です。

このクエリを入力するは、最後の手順は、新しいメソッドの名前を選択します。 使用して`FillWithNumberOfProducts`と`GetCategoriesAndNumberOfProducts`塗りつぶしの DataTable と戻り値、データ テーブルのパターンをそれぞれします。


[![名前の新しい TableAdapter のメソッド FillWithNumberOfProducts と GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**図 10**: 新しい tableadapter のメソッドの名前を付けます`FillWithNumberOfProducts`と`GetCategoriesAndNumberOfProducts`([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


この時点で、データ アクセス層が拡張されてカテゴリごとに製品の数が含まれます。 対応するを追加する必要がありますので、すべてのプレゼンテーション層が、個別のビジネス ロジック層を介して、DAL にすべての呼び出しをルーティング`GetCategoriesAndNumberOfProducts`メソッドを`CategoriesBLL`クラス。


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

このデータをバインドする準備 re お DAL BLL 完了と、`Categories`でリピータ`CategoriesAndProducts.aspx`! いる、決定から Repeater をまだ作成して、ObjectDataSource 場合の製品の数、`ItemDataBound`イベント ハンドラー セクションで、この ObjectDataSource およびリピータ s を削除`DataSourceID`プロパティ unwire; の設定も、リピータ s`ItemDataBound`イベント、イベント ハンドラーを削除してから、 `Handles Categories.OnItemDataBound` ASP.NET 分離コード クラスの構文。

元の状態に戻りますリピータ、追加という名前の新しい ObjectDataSource`CategoriesDataSource`リピータ s のスマート タグを使用しています。 構成を使用する ObjectDataSource、`CategoriesBLL`クラスが使用するのではなく、`GetCategories()`メソッドが使用する必要が`GetCategoriesAndNumberOfProducts()`代わりに (図 11 を参照してください)。


[![ObjectDataSource GetCategoriesAndNumberOfProducts メソッドを使用して構成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**図 11**: 構成を使用する ObjectDataSource、`GetCategoriesAndNumberOfProducts`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


次に、更新、`ItemTemplate`ように LinkButton s`Text`プロパティが、データ バインドの構文を使用してを宣言によって割り当てられ、両方が含まれる、`CategoryName`と`NumberOfProducts`データ フィールドです。 リピータの完全な宣言型マークアップと`CategoriesDataSource`ObjectDataSource に従ってください。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

含める、DAL を更新することによって表示される出力、`NumberOfProducts`列は、同じを使用して、`ItemDataBound`イベント ハンドラーのアプローチ (画面を表示する図 4 に戻ってカテゴリ名と製品の数を示す中継ぎ局のショット)。

## <a name="step-3-displaying-the-selected-category-s-products"></a>手順 3: 選択したカテゴリの製品を表示します。

この時点である、`Categories`リピータ各カテゴリの製品の数と共にカテゴリの一覧を表示します。 リピータ、クリックすると、問題が発生する、ポストバック ポイントは、各カテゴリの LinkButton を使用して、選択したカテゴリでのそれらの製品を表示する必要があります、 `CategoryProducts` DataList です。

私たちが直面する 1 つの課題は、DataList、選択したカテゴリの製品だけを表示する方法を示します。 [マスター/詳細 DetailsView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)行が GridView を構築する方法を説明しましたチュートリアルを選択することが、選択した行 s 詳細は同じページ上の DetailsView に表示されています。 GridView の ObjectDataSource が返される情報を使用してすべての製品について、 `ProductsBLL` s `GetProducts()` DetailsView の ObjectDataSource 中にメソッドを使用して、選択した製品に関する情報を取得する、`GetProductsByProductID(productID)`メソッドです。 *`productID`* GridView 秒の値に関連付けることにより宣言によって指定されたがパラメーター値`SelectedValue`プロパティです。 残念ながら、リピータはありません、`SelectedValue`プロパティおよびパラメーターのソースとして使用できません。

> [!NOTE]
> これは、リピータ内の LinkButton を使用するときに表示されるこれらの課題の 1 つです。 使用して、ハイパーリンクに渡す、`CategoryID`クエリ文字列を通じて代わりに、お QueryString フィールド ソースとして使用、パラメーターの値。


欠如に関する心配する前に、`SelectedValue`リピータのプロパティは、使用できます s 最初、ObjectDataSource を DataList をバインドし、指定の`ItemTemplate`します。

という名前の新しい ObjectDataSource を追加することを選択、DataList s のスマート タグから`CategoryProductsDataSource`を使用するように構成し、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。 このチュートリアルでは、DataList は読み取り専用のインターフェイスを提供するため自由、insert、UPDATE、ドロップダウン リストを設定し、(なし) タブを削除してください。


[![ObjectDataSource ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**図 12**: 構成を使用する ObjectDataSource`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


以降、`GetProductsByCategoryID(categoryID)`メソッドが入力パラメーターが必要です (*`categoryID`*)、データ ソース構成ウィザードにより、パラメーターのソースを指定します。 カテゴリは、GridView、DataList に指定されていた、d 設定パラメーターのソースのドロップダウン リスト コントロールを処理する、 `ID` Web コントロールのデータのです。 ただし、リピータ欠如以降、`SelectedValue`プロパティ パラメーターのソースとして使用することはできません。 チェックする場合、できたら、ControlID ドロップダウン リストに 1 つのコントロールにはのみが含まれている`ID``CategoryProducts`、 `ID` DataList のです。

ここでは、パラメーターのソースのドロップダウン リストを None に設定します。 プログラムでこのカテゴリでリピータ LinkButton がクリックされたときに、パラメーターの値を割り当てることになります。


[![CategoryID パラメーターのパラメーターのソースを指定しないをしないでください。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**図 13**: パラメーターのソースを指定しない、 *`categoryID`* パラメーター ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


データ ソース構成ウィザードを完了すると、Visual Studio 自動生成 DataList の`ItemTemplate`です。 この既定値を置き換える`ItemTemplate`テンプレートを使用して前のチュートリアルで使用されている以外の場合は DataList s を設定しても、`RepeatColumns`プロパティを 2 です。 これらの変更を行った後は、DataList および関連付けられている、ObjectDataSource の宣言型マークアップは、次のようになります。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

現時点では、 `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* パラメーターは設定されません、ページを表示するときに製品が表示されないようにします。 このパラメーター値が設定されてに基づいて行う必要がありますが、`CategoryID`リピータでクリックしたカテゴリのです。 これにより、2 つの課題: 最初は方針ときリピータ s の LinkButton`ItemTemplate`が行われてクリックした 2 番目、どのように確認する、`CategoryID`の LinkButton がクリックされた、対応するカテゴリのですか?。

ボタンおよび ImageButton コントロールと同様の LinkButton が、`Click`イベントおよび[`Command`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx)です。 `Click`イベントの目的だけの LinkButton がクリックしてされたことに注意してください。 ときに、ただし、に加えて、LinkButton がクリックしてされたことに注意してくださいもが必要な追加情報をイベント ハンドラーに渡します。 このような LinkButton s 場合[ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx)と[ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx)プロパティにこの追加情報を割り当てることができます。 LinkButton がクリックされたときにし、その`Command`イベントの起動 (の代わりにその`Click`イベント) でイベント ハンドラーには、値が渡され、`CommandName`と`CommandArgument`プロパティ。

ときに、`Command`リピータ、リピータ s でテンプレート内のイベントは[`ItemCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx)が起動し、渡される、`CommandName`と`CommandArgument`クリックされた LinkButton の値 (またはボタンまたはImageButton)。 そのため、カテゴリ、リピータ LinkButton がクリックしてされたときを特定するのには、次のお必要があります。

1. 設定、`CommandName`リピータ s の LinkButton のプロパティ`ItemTemplate`になんらかの値 (すれば ListProducts も使用しました)。 これを設定して`CommandName`値、LinkButton の`Command`の LinkButton がクリックされたときにイベントが発生します。
2. 集合 LinkButton s`CommandArgument`プロパティを現在のアイテムの秒の値に`CategoryID`です。
3. リピータ s のイベント ハンドラーを作成する`ItemCommand`イベント。 イベント ハンドラー、セットに、 `CategoryProductsDataSource` ObjectDataSource s`CategoryID`パラメーターの値が渡されたの`CommandArgument`します。

次`ItemTemplate`カテゴリ リピータのマークアップを手順 1. と 2. を実装します。 注方法、`CommandArgument`値には、データ項目の s が割り当てられる`CategoryID`databinding 構文を使用します。


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

作成されるたびに、`ItemCommand`イベント ハンドラーが常に、まず受信を確認することは賢明`CommandName`ので値*任意*`Command`によってイベントが発生した*任意*ボタン、LinkButton、またはリピータ内の ImageButton がなります、`ItemCommand`イベントが発生します。 お現在のみがこのような 1 つの LinkButton これで、今後お (または、チームの他の開発者) 可能性があります ボタンが追加 Web コントロールを追加、リピータをクリックすると、同じを発生させます。`ItemCommand`イベント ハンドラー。 そのため、常に確認してくださいことをお勧め、`CommandName`プロパティのみ想定される値に一致した場合、プログラム ロジックを続行します。

渡されたことを確認した後は`CommandName`ListProducts の値が、イベント ハンドラーを割り当てます、 `CategoryProductsDataSource` ObjectDataSource s`CategoryID`パラメーターの値が渡されたの`CommandArgument`します。 この変更に、ObjectDataSource の`SelectParameters`に自体を新しく選択したカテゴリの製品を表示、データ ソースにバインドし直す DataList を自動的に発生します。


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

これらの項目を追加します。 このチュートリアルが完了しました。 すぐをブラウザーでテストしてみましょう。 図 14 は、最初のページにアクセスしたときの画面を示しています。 カテゴリにまだへを選択するための製品は表示されません。 2 つの列ビューで、製品カテゴリでこれらの製品生成などのカテゴリをクリックすると表示されます (図 15 を参照してください)。


[![表示されるときに最初にアクセス ページを要求している製品がありません。](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**図 14**: いいえの製品が表示されるときに最初にアクセス ページ ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![生成カテゴリの一覧を右に一致する製品をクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**図 15**: 右側に一致する製品を一覧表示生成カテゴリをクリックすると ([フルサイズのイメージを表示するをクリックして](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>まとめ

このチュートリアルを上記で説明したとおりは、1 つに統合もまたはマスター/詳細レポートは 2 つのページに分散させることができます。 マスター/詳細レポートを表示する、1 つのページで、ただしが導入されて 方法のいくつかの課題最適なマスターのレイアウトやページの詳細レコード。 *マスター/詳細 DetailsView で選択可能なマスター GridView の使用について詳しく説明*が発生しました詳細レコードがマスター レコード上に表示されます。 このチュートリアルでは、マスター レコードする浮動小数点数に CSS 手法を使用おチュートリアル、。詳細の左側です。

マスター/詳細レポートを表示するには、と共にもありました内からクリック LinkButton (またはボタンまたは ImageButton) ときに、サーバー側ロジックを実行する方法と同様に、各カテゴリに関連付けられている製品の数を取得する方法を調査する機会をリピータします。

このチュートリアルでは、DataList リピータとマスター/詳細レポートのマイクロソフトの調査を完了します。 次の一連のチュートリアルでは、編集、および削除 DataList コントロールする機能を追加する方法を示します。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/)フローティング CSS と CSS 要素に関するチュートリアル
- [CSS 配置](http://www.brainjar.com/css/positioning/)CSS を使用して要素の配置の詳細について
- [Html には、「コンテンツをレイアウトに使用する](http://www.w3schools.com/html/html_layout.asp)を使用して`<table>`s およびその他の HTML 要素位置を決定

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Zack Jones しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-acess-two-pages-datalist-vb.md)
