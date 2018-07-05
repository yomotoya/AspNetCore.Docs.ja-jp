---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: DataList または Repeater コントロール (VB) データを並べ替える |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは並べ替えを DataList と Repeater、サポートを含める方法と、データが含まれることができます、DataList または Repeater を構築する方法について説明します.
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fcbc1f83a00621ce0031cdcb775537992e3cb843
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828884"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>DataList または Repeater コントロール (VB) データを並べ替える
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe)または[PDF のダウンロード](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> このチュートリアルでは、ここに並べ替えを DataList と Repeater、サポートを含める方法と、DataList または Repeater データのページングし、並べ替えできますを構築する方法について説明します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](paging-report-data-in-a-datalist-or-repeater-control-vb.md)DataList にページング サポートを追加する方法について確認しました。 新しいメソッドを作成した、`ProductsBLL`クラス (`GetProductsAsPagedDataSource`) 返される、`PagedDataSource`オブジェクト。 DataList または Repeater にバインドするとき、DataList または Repeater は、データの要求されたページだけが表示されます。 この手法は、組み込みの既定のページング機能を提供する GridView、DetailsView、FormView コントロールによって内部的にどのような使用に似ています。

ページング サポートを提供するだけでなく、GridView は並べ替えをサポートするボックスも含まれます。 DataList と Repeater のどちらも、組み込みの並べ替え機能を提供します。ただし、少しのコードには、並べ替え機能を追加できます。 このチュートリアルでは、ここに並べ替えを DataList と Repeater、サポートを含める方法と、DataList または Repeater データのページングし、並べ替えできますを構築する方法について説明します。

## <a name="a-review-of-sorting"></a>並べ替えのレビュー

説明したように、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)チュートリアルでは、並べ替えをサポートなし、GridView コントロールを提供します。 GridView の各フィールドが関連付けられていることができますが`SortExpression`データの並べ替えに使用するデータ フィールドを示します。 ときに GridView s`AllowSorting`プロパティに設定されて`true`、各 GridView フィールドを持つ、`SortExpression`プロパティ値は、ヘッダー、LinkButton としてレンダリングされます。 ユーザーには、特定の GridView フィールドのヘッダーがクリックすると、ポストバックが発生し、データがクリックされたフィールド s に従って並び替え`SortExpression`します。

GridView コントロールは、`SortExpression`プロパティも、格納、`SortExpression`で GridView フィールドのデータが並べ替えられます。 さらに、`SortDirection`プロパティは、データが昇順または降順 (場合連続して、並べ替え順序で 2 回特定 GridView フィールドのヘッダー リンクが切り替えられたユーザーのクリック) の順序で並べ替えられるかどうかを示します。

渡す、GridView がそのデータ ソース コントロールにバインドされると、その`SortExpression`と`SortDirection`プロパティをデータ ソース コントロール。 データ ソース コントロールがデータを取得し、指定に従って並べ替えられます`SortExpression`と`SortDirection`プロパティ。 データの並べ替えの後、データ ソース コントロールは、それを GridView に返します。

DataList または Repeater コントロールでのこの機能をレプリケートするには、必要があります。

- 並べ替えのインターフェイスを作成します。
- データ フィールドを並べ替えるには、昇順または降順で並べ替えるかどうかに注意してください。
- 特定のデータ フィールドでデータを並べ替える ObjectDataSource を指示します。

手順 3 と 4 でこれらの 3 つのタスクに取り組むします。 次に、ページングと並べ替えを DataList または Repeater のサポートの両方を含める方法を説明します。

## <a name="step-2-displaying-the-products-in-a-repeater"></a>手順 2: Repeater で製品を表示します。

並べ替え関連の機能のいずれかの実装について気にし、前に、s を Repeater コントロールでは、製品の一覧を表示して起動を使用できます。 開いて開始、`Sorting.aspx`ページで、`PagingSortingDataListRepeater`フォルダー。 Repeater コントロールを追加、web ページで、設定をその`ID`プロパティを`SortableProducts`します。 Repeater s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDataSource`からデータを取得するように構成し、`ProductsBLL`クラスの`GetProducts()`メソッド。 INSERT、UPDATE、および DELETE の各タブで、ドロップダウン リストから (なし) オプションを選択します。


[![ObjectDataSource を作成し、GetProductsAsPagedDataSource() メソッドを使用するように構成します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**図 1**: ObjectDataSource を作成し、使用するように構成、`GetProductsAsPagedDataSource()`メソッド ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))。


[![UPDATE、INSERT で、ドロップダウン リストを設定し、(なし) タブを削除します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**図 2**: UPDATE、INSERT で、ドロップダウン リストを設定し、[(なし) タブを削除する ([フルサイズの画像を表示する] をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))。


異なり、DataList で Visual Studio は自動的に作成されません、`ItemTemplate`データ ソースにバインドした後、Repeater コントロール。 さらに、これを追加します`ItemTemplate`宣言に、Repeater コントロールのスマート タグ DataList s であるテンプレートの編集オプションが不足しています。 Let s を使用して同じ`ItemTemplate`s の製品名、供給業者、およびカテゴリに表示される前のチュートリアルから。

追加した後、 `ItemTemplate`Repeater、ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

図 3 は、ブラウザーで表示した場合は、このページを示します。


[![各製品の名前、供給業者、およびカテゴリが表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**図 3**: 各製品の名前、供給業者、およびカテゴリが表示されます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))。


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>手順 3: データの並べ替えを ObjectDataSource に指示します。

Repeater で表示されるデータを並べ替えるには、データを並べ替える必要があります、並べ替え式の ObjectDataSource に通知する必要があります。 最初に発生させる、ObjectDataSource は、そのデータを取得する前にその[`Selecting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)並べ替え式を指定する機会を提供します。 `Selecting`イベント ハンドラーの型のオブジェクトが渡される[ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)、という名前のプロパティを持つ[ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx)型の[ `DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)します。 `DataSourceSelectArguments`クラスが、データ ソース コントロールにデータのコンシューマーからのデータ関連の要求を渡す設計されておりが含まれています、 [ `SortExpression`プロパティ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)します。

ObjectDataSource に、ASP.NET ページから並べ替え情報を渡すためのイベント ハンドラーを作成、`Selecting`イベントと、次のコードを使用します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression* (ProductName) など、データの並べ替えにデータ フィールドの名前の値を割り当てる必要があります。 並べ替えの方向に関連するプロパティがない、降順でデータを並べ替える場合が文字列 DESC を追加するために、 *sortExpression* (ProductName DESC) などの値。

使用して別のハード コーディングされた値をみましょう*sortExpression*し、ブラウザーで結果をテストします。 図 4 に示すとして ProductName DESC を使用する場合、 *sortExpression*製品がアルファベットの逆順での名前で並べ替えられます。


[![製品は名前がアルファベット順の逆順で順に並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**図 4**: The 製品はアルファベット順の逆で、名前で並べ替えられます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))。


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>手順 4: は、並べ替えのインターフェイスを作成し、並べ替え式と方向を記憶

LinkButton に各並べ替え可能なフィールドのヘッダー テキストを変換する並べ替えを gridview サポートの有効化をクリックすると、それに応じてデータを並べ替えます。 このような並べ替えのインターフェイスでは、そのデータが整然とレイアウトの列に、GridView の合理的です。 DataList と Repeater コントロールで、ただし、別の並べ替えインターフェイスが必要です。 (グリッド データ) ではなくデータの一覧については、共通の並べ替えインターフェイスは、データの並べ替えられて、フィールドを提供するドロップダウン リストです。 このチュートリアルでは、このようなインターフェイスを実装して使用できます。

上記の DropDownList Web コントロールを追加、 `SortableProducts` Repeater およびセットの`ID`プロパティを`SortBy`します。 [プロパティ] ウィンドウから、内の省略記号をクリックします。、 `Items` ListItem コレクション エディターを表示するプロパティ。 追加`ListItem`、データの並べ替えを`ProductName`、 `CategoryName`、および`SupplierName`フィールド。 追加も、`ListItem`製品の名前がアルファベットの逆順で並べ替える。

`ListItem` `Text` (名) などの任意の値にプロパティを設定することができますが、 `Value` (ProductName) などのデータ フィールドの名前にプロパティを設定する必要があります。 降順で結果の並べ替えには、ProductName DESC のように、データ フィールドの名前に文字列 DESC を追加します。


![ListItem を各並べ替え可能なデータ フィールドの追加します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**図 5**: 追加、`ListItem`各並べ替え可能なデータ フィールド


最後に、次のドロップダウン リストの右側にボタンの Web コントロールを追加します。 設定の`ID`に`RefreshRepeater`とその`Text`プロパティを更新します。

作成した後、`ListItem`の更新 ボタンを追加して、DropDownList とボタンの宣言構文を次のようななります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

完全な並べ替えの DropDownList で次に更新する必要が ObjectDataSource s ある`Selecting`イベント ハンドラーの使用、選択するように`SortBy``ListItem`s`Value`ハード コーディングされた並べ替え式ではなくプロパティ。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

最初のページにアクセスすると、この時点で、製品は初期状態で並べ替えられます、`ProductName`ほどのデータ フィールド s、 `SortBy` `ListItem`既定で選択されます (図 6 参照)。 さまざまな並べ替えカテゴリなどのオプションと [更新] を選択、ポストバックを発生させるし、再、図 7 に示すように、それらのカテゴリ名で、データを並べ替えます。


[![製品は、最初の名前で並べ替え](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**図 6**: The 製品は、最初に、名前で並べ替えられます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))。


[![製品がカテゴリで並べ替えられます](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**図 7**:、製品がカテゴリで並べ替えられます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))。


> [!NOTE]
> 更新 ボタンをクリックすると、データを自動的に再度並べ替えた Repeater s のビュー ステートが無効になっている、ため、原因となり、ポストバックのたびにそのデータ ソースにバインドする中継ぎ局です。 ドロップダウン リスト、並べ替えの変更が有効な Repeater のビュー ステートのままにしている場合、t が勝利したリストは並べ替え順序に、影響を与えます。 これを解決するには、[更新] ボタンのイベント ハンドラーを作成`Click`イベントとそのデータ ソースを Repeater を再バインド (s Repeater を呼び出すことによって`DataBind()`メソッド)。


## <a name="remembering-the-sort-expression-and-direction"></a>並べ替え式と方向を記憶

並べ替え以外に関係しているポストバックが発生すると、ページの並べ替え可能な DataList または Repeater を作成するときに、秒の命令型の並べ替え式と方向は、ポストバック間で記憶されます。 たとえば、各製品で [削除] ボタンを含めるには、このチュートリアルでは、Repeater を更新しました。 削除ボタンをクリックすると d は、選択した製品を削除し、Repeater にデータをバインドするコードを実行します。 並べ替えの詳細については、ポストバック間では保持されません、画面に表示されるデータの元の並べ替え順序に戻ります。

このチュートリアルでは、DropDownList 暗黙的を保存、並べ替え式と方向のビュー状態で私たちにとってします。 使用すると、さまざまな並べ替えオプションを指定すると答えると、ある 1 つ別の並べ替えインターフェイスを使用していた場合 d をポストバック間での並べ替え順序を覚えておくように注意する必要があります。 これは、クエリ文字列、またはその他の状態の永続化手法では、並べ替えパラメーターを含めることによって、ページ s のビュー ステートに並べ替えパラメーターを格納することで実現可能性があります。

このチュートリアルの今後の例では、並べ替えの詳細ページのビューステートを保存する方法について説明します。

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>手順 5: 追加の既定のページングを使用する DataList に並べ替えをサポート

[前のチュートリアル](paging-report-data-in-a-datalist-or-repeater-control-vb.md)DataList で既定のページングを実装する方法について確認しました。 S、ページングされたデータの並べ替え機能は、この前の例を拡張することができます。 開いて開始、`SortingWithDefaultPaging.aspx`と`Paging.aspx`ページで、`PagingSortingDataListRepeater`フォルダー。 `Paging.aspx`  ページで、ページの宣言型マークアップを表示する ソース ボタンをクリックします。 選択したテキストをコピー (図 8 参照) の宣言型マークアップを貼り付けます`SortingWithDefaultPaging.aspx`間、`<asp:Content>`タグ。


[![宣言型マークアップをレプリケート、 &lt;Asp:content&gt; SortingWithDefaultPaging.aspx Paging.aspx からタグ](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**図 8**: で宣言型マークアップをレプリケート、`<asp:Content>`からタグ`Paging.aspx`に`SortingWithDefaultPaging.aspx`([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))。


宣言型マークアップをコピーした後、メソッドとプロパティをコピー、`Paging.aspx`ページの分離コード クラスに分離コード クラスを s`SortingWithDefaultPaging.aspx`します。 表示する次に、少し、`SortingWithDefaultPaging.aspx`ブラウザーでページ。 同じ機能と外観を示す必要があります`Paging.aspx`します。

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>ページングと並べ替え方法には、既定値を含める ProductsBLL の強化

前のチュートリアルで作成した、`GetProductsAsPagedDataSource(pageIndex, pageSize)`メソッドで、`ProductsBLL`返されるクラス、`PagedDataSource`オブジェクト。 これは、`PagedDataSource`オブジェクトが設定されている*すべて*、製品の (BLL s を使用して`GetProducts()`メソッド)、指定したに対応するレコードのみを DataList にバインドされると、 *pageIndex**pageSize*入力パラメーターが表示されます。

このチュートリアルの前半で ObjectDataSource s から、並べ替え式を指定することで並べ替えのサポートを追加しました`Selecting`イベント ハンドラー。 これは、ObjectDataSource に並べ替えることができる、このようなオブジェクトが返される場合もと動作、`ProductsDataTable`によって返される、`GetProducts()`メソッド。 ただし、`PagedDataSource`によって返されるオブジェクト、`GetProductsAsPagedDataSource`メソッドは、その内部データ ソースの並べ替えをサポートしていません。 代わりから返される結果を並べ替える必要があります、`GetProducts()`メソッド*する前に*を入力しました、`PagedDataSource`します。

これを実現するで新しいメソッドを作成、`ProductsBLL`クラス、`GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`します。 並べ替え、`ProductsDataTable`によって返される、`GetProducts()`メソッドを指定、`Sort`プロパティの既定の`DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource`メソッドの違いから、`GetProductsAsPagedDataSource`前のチュートリアルで作成したメソッド。 具体的には、`GetProductsSortedAsPagedDataSource`追加の入力パラメーターを受け取り`sortExpression`にこの値が割り当てられます、`Sort`のプロパティ、 `ProductDataTable` s`DefaultView`します。 数行のコードを後で、`PagedDataSource`オブジェクト データ ソースが割り当てられている、 `ProductDataTable` s`DefaultView`します。

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>GetProductsSortedAsPagedDataSource メソッドを呼び出すと、SortExpression 入力パラメーターの値を指定します。

`GetProductsSortedAsPagedDataSource`このパラメーターの値を提供するメソッドの完全な次の手順は。 ObjectDataSource`SortingWithDefaultPaging.aspx`を呼び出すには、現在構成されている、`GetProductsAsPagedDataSource`メソッドを呼び出し、2 つを 2 つの入力パラメーターで`QueryStringParameters`で指定される、`SelectParameters`コレクション。 これらの 2 つ`QueryStringParameters`ことを示すためのソース、`GetProductsAsPagedDataSource`メソッド s *pageIndex*と*pageSize*パラメーターを取得するクエリ文字列フィールドから`pageIndex`と`pageSize`します。

ObjectDataSource %s 更新`SelectMethod`プロパティを呼び出す新しい`GetProductsSortedAsPagedDataSource`メソッド。 次に、新しい追加`QueryStringParameter`ように、 *sortExpression*入力パラメーターがクエリ文字列フィールドからアクセス`sortExpression`します。 設定、 `QueryStringParameter` s `DefaultValue` ProductName にします。

これらの変更後よう ObjectDataSource s の宣言型マークアップになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

この時点で、`SortingWithDefaultPaging.aspx`ページは、製品名、その結果をアルファベット順に並べ替えが (図 9 参照)。 既定では、商品名の値として渡されるため、これは、`GetProductsSortedAsPagedDataSource`メソッド s *sortExpression*パラメーター。


[![既定では、結果が商品名で並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**図 9**: 既定で、結果は並んで`ProductName`([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))。


手動で追加する場合、`sortExpression`などのクエリ文字列フィールド`SortingWithDefaultPaging.aspx?sortExpression=CategoryName`結果は並べ替え、指定した`sortExpression`します。 ただし、この`sortExpression`データの別のページに移動するときに、パラメーターは、クエリ文字列に含まれません。 米国にバックアップが実際には、ボタン、[次へ] または最後のページをクリックすると`Paging.aspx`! さらに、s が現在の並べ替えインターフェイスなし。 ユーザーは、ページングされたデータの並べ替え順序を変更できる唯一の方法は、クエリ文字列を直接操作することによってです。

## <a name="creating-the-sorting-interface"></a>並べ替えのインターフェイスを作成します。

最初に更新する必要があります、`RedirectUser`メソッドをユーザーに送信する`SortingWithDefaultPaging.aspx`(の代わりに`Paging.aspx`) を含めると、`sortExpression`クエリ文字列値。 読み取り専用ページ レベルという名前を付け加えておきます`SortExpression`プロパティ。 同様に、このプロパティ、`PageIndex`と`PageSize`前のチュートリアルで作成されたプロパティの値を返します、`sortExpression`存在する場合、クエリ文字列フィールドと、既定 (ProductName) をそれ以外の場合値します。

現在、`RedirectUser`メソッドは、のみ 1 つの入力パラメーターを表示するページのインデックスを受け取ります。 ただし、新機能、クエリ文字列で指定した以外の並べ替え式を使用してデータの特定のページにユーザーをリダイレクトする場合もあります。 すぐにこのページには、一連の指定した列でデータを並べ替えるためのボタンの Web コントロールが含まれる並べ替えインターフェイスを作成します。 これらのボタンのいずれかがクリックされたときに適切に並べ替える式の値を渡して、ユーザーをリダイレクトします。 この機能を提供する 2 つのバージョンを作成、`RedirectUser`メソッド。 1 つ目は、2 つ目は、ページのインデックスおよび並べ替え式を受け入れる中に、表示するページのインデックスだけを受け入れる必要があります。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

このチュートリアルでは最初の例では、DropDownList を使用して並べ替えインターフェイスを作成しました。 この例では、let s 使用によって並べ替え DataList いずれかの上に配置する 3 つのボタンの Web コントロール`ProductName`、1 つの`CategoryName`とに 1 つ`SupplierName`。 3 つのボタンの Web コントロールの設定を追加、`ID`と`Text`プロパティ適切に。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

次に、作成、`Click`ごとにイベント ハンドラー。 イベント ハンドラーを呼び出す必要があります、`RedirectUser`適切な並べ替え式を使用して最初のページにユーザーを返すメソッド。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

データがアルファベット順に並べ替え、製品名によって、ページを最初にアクセスして、ときに (戻るは図 9 を参照してください)。 データの 2 番目のページに進みカテゴリ ボタンによって、並べ替えをクリックして [次へ] ボタンをクリックします。 カテゴリ名で並べ替えられた、データの最初のページに戻ること (図 10 参照)。 同様に、サプライヤー ボタンによって、並べ替えをクリックすると、仕入先データの最初のページから開始して、データが並べ替えられます。 を通じて、データをページングと並べ替えの基準が記憶されます。 図 11 は、カテゴリでの並べ替えとデータの 13 番目のページに昇格し、後のページを示します。


[![製品はカテゴリ順に並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**図 10**: The 製品はカテゴリで並べ替えられます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))。


[![並べ替え式は記憶とページングを通じてデータ](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**図 11**: 並べ替え式が記憶とページングを通じて、データ ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))。


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Repeater 内のレコードを手順 6: カスタム ページング

DataList 例では、非効率的な既定のページングの手法を使用してそのデータを 5 つのページの手順で検査されます。 十分に大量のデータのページング、ときに、カスタム ページングを使用することが重要です。 戻り、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)と[カスタム ページングされたデータの並べ替え](../paging-and-sorting/sorting-custom-paged-data-vb.md)の BLL の既定およびカスタム ページングと作成されたメソッドの違いを調べてチュートリアル、カスタム ページングと並べ替えカスタム ページングされたデータを使用しています。 次の 3 つのメソッドが追加これら 2 つ前のチュートリアルで具体的には、`ProductsBLL`クラス。

- `GetProductsPaged(startRowIndex, maximumRows)` 開始位置としてレコードの特定のサブセットを返します*startRowIndex*を超過していないと*maximumRows*します。
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` 指定したで並べ替えたレコードの特定のサブセットを返します*sortExpression*入力パラメーター。
- `TotalNumberOfProducts()` 内のレコードの合計数を提供します、`Products`データベース テーブル。

効率的にページおよび DataList または Repeater コントロールを使用してデータを並べ替えるには、これらのメソッドを使用できます。 これを説明するには、ようにカスタム ページング サポートを Repeater コントロールを作成して開始 s並べ替え機能を追加します。

開く、`SortingWithCustomPaging.aspx`ページで、`PagingSortingDataListRepeater`フォルダー設定 ページで、Repeater に追加し、その`ID`プロパティを`Products`します。 Repeater s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDataSource`します。 データを選択するように構成、`ProductsBLL`クラスの`GetProductsPaged`メソッド。


[![ObjectDataSource ProductsBLL クラスの GetProductsPaged メソッドを使用して構成します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**図 12**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス s`GetProductsPaged`メソッド ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))。


UPDATE、INSERT でドロップダウン リストを設定 (None) にタブを削除して、[次へ] ボタンをクリックします。 データ ソースの構成ウィザードでのソースをようになりましたが、`GetProductsPaged`メソッド s *startRowIndex*と*maximumRows*パラメーターを入力します。 実際には、これらの入力パラメーターは無視されます。 代わりに、 *startRowIndex*と*maximumRows*経由で値が渡された、 `Arguments` ObjectDataSource s プロパティ`Selecting`イベント ハンドラーを指定した方法と同じように、*sortExpression*でこのチュートリアルの最初のデモです。 そのため、パラメーター ソースを None に設定ウィザードで、ドロップダウン リストままにします。


[![[なし] に、パラメーター ソースの設定をそのまま使用します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**図 13**: [なし] にパラメーターのソースの設定のままに ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))。


> [!NOTE]
> *いない*ObjectDataSource s 設定`EnablePaging`プロパティを`true`。 これにより、独自に自動的に含める ObjectDataSource *startRowIndex*と*maximumRows*パラメーターを`SelectMethod`s 既存のパラメーター リスト。 `EnablePaging`プロパティは、バインドのカスタム ページングされた GridView、DetailsView、FormView コントロールにデータがこれらのコントロールは、ObjectDataSource s から特定の動作を想定しているため場合に便利な場合にのみ使用できます`EnablePaging`プロパティが`true`します。 DataList と Repeater のページング サポートを手動で追加するがあるためこのプロパティに設定のままに`false`(既定)、ように、ASP.NET ページ内に直接必要な機能に焼き付けるします。


最後に、Repeater s を定義`ItemTemplate`s の製品名、カテゴリ、および仕入先が表示されるようにします。 これらの変更後 Repeater、ObjectDataSource s の宣言構文に、次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

ご協力にブラウザーを使用してページにアクセスし、レコードが返されないことに注意してください。 これはを指定するには、まだ ve、 *startRowIndex*と*maximumRows*パラメーターの値の両方でそのため、0 の値を渡されます。 これらの値を指定するには、ObjectDataSource s のイベント ハンドラーを作成`Selecting`イベントとこれらのパラメーター値をプログラムでの 0 から 5、ハード コーディングされた値にそれぞれ設定します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

この変更によりは、ブラウザーで表示したときに、ページには、最初の 5 つの製品が表示されます。


[![最初の 5 つのレコードが表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**図 14**: The の最初の 5 つのレコードが表示されます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))。


> [!NOTE]
> 図 14 に示される製品が発生するために、製品名で並べ替えられます、`GetProductsPaged`効率的なカスタム ページング クエリを実行するストアド プロシージャで結果を並べ替えます`ProductName`します。


ページをステップ ユーザーを許可するためには、開始行インデックスおよび行の最大数を追跡し、ポストバック間でこれらの値を保存する必要があります。 既定のページングの例ではこれらの値を保持するのにクエリ文字列フィールドを使用しましたこのデモでは、s が、ページ ビュー状態でこの情報を保持することができます。 次の 2 つのプロパティを作成します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

使用するように選択するとイベント ハンドラーのコードを次に、更新、`StartRowIndex`と`MaximumRows`0 および 5 のハード コーディングされた値ではなくプロパティ。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

この時点で、ページには、最初の 5 つのレコードだけが表示されます。 ただし、これらのプロパティの場所でページング インターフェイスを作成する準備が整いました。

## <a name="adding-the-paging-interface"></a>ページング インターフェイスを追加します。

Let 用途に同じ最初、Previous、次に、最後のページングのインターフェイスは、どのようなページのデータを表示するコントロールが表示されているラベル Web を含むの既定のページング例、および合計ページ数の存在で使用されます。 次の 4 つのボタンの Web コントロールと Repeater の下のラベルを追加します。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

次に、作成`Click`4 つのボタンのイベント ハンドラー。 これらのボタンがクリックされたときに更新する必要があります、`StartRowIndex`と Repeater にデータを再バインドします。 First、Previous、および [次へ] ボタンのコードが単純で、最後のボタンの操作を行います方針データの最後のページの開始行インデックスでしょうか このインデックスと経由の合計レコードの数がページングされているかを把握する必要があります、次へと最終ボタンを有効にするかどうかを判断できるを計算します。 これを呼び出すことによって確認できます、`ProductsBLL`クラスの`TotalNumberOfProducts()`メソッド。 Let s という名前の読み取り専用、ページ レベルのプロパティを作成する`TotalRowCount`の結果を返す、`TotalNumberOfProducts()`メソッド。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

このプロパティを持つ最後のページの開始行インデックスを判断できますようになりました。 具体的には、その結果の整数値の秒の`TotalRowCount`で割った値 1 を引いた`MaximumRows`を掛けた`MaximumRows`。 私たちが記述できるように、`Click`ページング インターフェイスの 4 つのボタンのイベント ハンドラー。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

最後に、最後のページを表示するときに、データと、次へと最終ボタンの最初のページを表示するときにページング インターフェイスで 1 つ目と前のボタンを無効にする必要があります。 これを実現するには、ObjectDataSource に次のコードを追加`Selecting`イベント ハンドラー。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

これらを追加した後`Click`イベント ハンドラーと、コードを有効にまたは現在の開始行インデックスに基づいて、ページング インターフェイス要素を無効にするには、ブラウザーでページをテストします。 図 15 に示した最初のページを最初にアクセスすると、[戻る] ボタンは無効です。 最後をクリックすると、最後のページを表示中には、[次へ] をクリックすると、データの 2 番目のページが表示されます (図 16、17 を参照してください)。 データの最後のページを表示するときに、次へと最後のボタンが無効です。


[![最初の製品ページを表示するときに、前と最後のボタンが無効にします。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**図 15**: 製品の最初のページを表示するときに、前と最後のボタンを無効に ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))。


[![2 つ目の製品ページがいないことを示す](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**図 16**: 製品、2 番目のページがいないことを示す ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))。


[![クリックすると過去のデータの最後のページを表示します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**図 17**: 最後のクリックしてには、データの最終的なページが表示されます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))。


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>手順 7: Repeater をページング サポートをカスタムの並べ替えを含む

これでカスタム ページングを実装すると、並べ替えを含めるには、準備ができ次第サポートされています。 `ProductsBLL`クラス s`GetProductsPagedAndSorted`メソッドには、同じ*startRowIndex*と*maximumRows*入力パラメーターとして`GetProductsPaged`、追加の許可が*sortExpression*入力パラメーター。 使用する、`GetProductsPagedAndSorted`メソッドから`SortingWithCustomPaging.aspx`、次の手順を実行する必要があります。

1. ObjectDataSource s 変更`SelectMethod`プロパティから`GetProductsPaged`に`GetProductsPagedAndSorted`します。
2. 追加、 *sortExpression* `Parameter` ObjectDataSource s オブジェクト`SelectParameters`コレクション。
3. プライベートであり、ページ レベルの作成`SortExpression`ページのビュー ステートをポストバック間でその値を保持するプロパティ。
4. ObjectDataSource %s 更新`Selecting`ObjectDataSource s を割り当てるには、イベント ハンドラー *sortExpression*ページ レベルの値をパラメーター`SortExpression`プロパティ。
5. 並べ替えのインターフェイスを作成します。

ObjectDataSource s を更新することで開始`SelectMethod`プロパティと追加、 *sortExpression* `Parameter`します。 確認、 *sortExpression* `Parameter` s`Type`プロパティに設定されて`String`します。 これら最初の 2 つのタスクを完了すると、ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

次に、ページ レベル必要があります`SortExpression`ビューステートにシリアル化する値を持つプロパティです。 並べ替え式の値が設定されていない場合は、既定値として ProductName を使用します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

ObjectDataSource を呼び出す前に、`GetProductsPagedAndSorted`メソッドを設定する必要があります、 *sortExpression* `Parameter`の値に、`SortExpression`プロパティ。 `Selecting`イベント ハンドラーでは、次のコード行を追加します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

残っているは、並べ替えのインターフェイスを実装します。 最後の例で行ったよう s 製品名、カテゴリ、または供給業者によって結果の並べ替えにユーザーを許可する 3 つのボタンの Web コントロールを使用して実装されている並べ替えインターフェイスを持っていることができます。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

作成`Click`これら 3 つのボタン コントロールのイベント ハンドラー。 イベント ハンドラーのリセット、`StartRowIndex`を 0 に設定、`SortExpression`適切な値、および Repeater にデータを再バインドします。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

すべてが s で終了です。 カスタム ページングと並べ替えを実装する手順の数値の中の手順が既定のページングの必要なものによく似ています。 図 18 では、カテゴリ別に並べ替えてときにデータの最後のページを表示するときに、製品が表示されます。


[![カテゴリ別に並べ替え、データの最後のページが表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**図 18**: データの最後のページ、カテゴリ別に並べ替えが表示されます ([フルサイズの画像を表示する をクリックします](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))。


> [!NOTE]
> 前の例、並べ替え式として使用された仕入業者によって並べ替えるときにします。 ただし、カスタムのページングの実装の CompanyName を使用する必要があります。 ため、これは、ストアド プロシージャをカスタム ページングを実装する責任を負います`GetProductsPagedAndSorted`に並べ替え式を渡します、`ROW_NUMBER()`キーワード、`ROW_NUMBER()`キーワードは、別名ではなく、実際の列名が必要です。 したがって、使用する必要があります`CompanyName`(内の列の名前、`Suppliers`テーブル) で使用される別名ではなく、`SELECT`クエリ (`SupplierName`) の並べ替え式。


## <a name="summary"></a>まとめ

どちらも、DataList と Repeater は、組み込みの並べ替えのサポートを提供が、少しのコードとカスタムの並べ替えインターフェイスでこのような機能を追加できます。 を通じて、並べ替え式を指定できます、並べ替え、ページングを実装する場合、 `DataSourceSelectArguments` ObjectDataSource s にオブジェクトが渡された`Select`メソッド。 これは、`DataSourceSelectArguments`オブジェクト`SortExpression`プロパティは、ObjectDataSource s で割り当てられる`Selecting`イベント ハンドラー。

DataList または Repeater 既にページング サポートを提供するには、並べ替え機能を追加するには、最も簡単な方法は、並べ替え式を受け取るメソッドを追加するビジネス ロジック レイヤーをカスタマイズしています。 この情報渡すことができますで ObjectDataSource s パラメーターを介して`SelectParameters`します。

このチュートリアルでは、DataList と Repeater コントロールによるページングと並べ替え、調査を完了します。 次へと最終チュートリアルでは、DataList と Repeater のテンプレートにアイテムごとにいくつか、ユーザーが開始したカスタムの機能を提供するために Button Web コントロールを追加する方法を説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、David Suru でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
