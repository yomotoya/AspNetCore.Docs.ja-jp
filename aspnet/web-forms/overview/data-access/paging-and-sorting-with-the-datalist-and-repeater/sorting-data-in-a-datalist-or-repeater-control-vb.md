---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: "DataList またはリピータ コントロール (VB) のデータの並べ替え |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルではあります、DataList およびリピータでのサポートの並べ替えをインクルードする方法とデータを含むことができます、DataList または Repeater を構築する方法を確認しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e3f505e525fd5e701bb40dc3e6467b880bf75447
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>DataList またはリピータ コントロール (VB) のデータの並べ替え
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe)または[PDF のダウンロード](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> このチュートリアルではあります、DataList およびリピータでのサポートの並べ替えをインクルードする方法と、DataList またはリピータがデータをページングされ並べ替えられることができますを構築する方法を確認します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](paging-report-data-in-a-datalist-or-repeater-control-vb.md)DataList にページング サポートを追加する方法について説明しました。 新しいメソッドを作成した、`ProductsBLL`クラス (`GetProductsAsPagedDataSource`) 返される、`PagedDataSource`オブジェクト。 DataList または Repeater をバインドするとき、DataList またはリピータ表示、要求されたページのデータだけです。 この手法は、組み込みの既定のページング機能を提供する GridView、DetailsView、FormView コントロールによって内部的にどのような使用に似ています。

ページング サポートを提供するだけでなく、GridView はその並べ替えをサポートするままの状態も含まれます。 DataList でも、リピータ; 組み込みの並べ替え機能を提供します。ただし、コードのビットを使用して並べ替え機能を追加することができます。 このチュートリアルではあります、DataList およびリピータでのサポートの並べ替えをインクルードする方法と、DataList またはリピータがデータをページングされ並べ替えられることができますを構築する方法を確認します。

## <a name="a-review-of-sorting"></a>並べ替えのレビュー

説明したとおり、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)チュートリアルでは、並べ替えをサポートするボックス外 GridView コントロールが用意されています。 各 GridView フィールドが関連付けられていることが`SortExpression`データの並べ替えに使用するデータ フィールドを示します。 ときに GridView s`AllowSorting`プロパティに設定されている`true`、定義されている GridView フィールド、`SortExpression`プロパティ値は、ヘッダー、LinkButton としてレンダリングされます。 ユーザーには、特定の GridView フィールドのヘッダーがクリックすると、ポストバックが発生し、データがクリックされたフィールド s に従って並び替え`SortExpression`です。

GridView コントロールが、`SortExpression`プロパティも、格納する、`SortExpression`によって GridView フィールドのデータが並べ替えられます。 さらに、`SortDirection`プロパティは、データが、昇順または降順 (場合連続して、並べ替え順序で 2 回特定 GridView フィールドのヘッダー リンクが切り替えられると、ユーザーのクリック) の順序で並べ替えるかどうかを示します。

渡す GridView がそのデータ ソース コントロールにバインドされると、その`SortExpression`と`SortDirection`プロパティ データをソース コントロール。 データ ソース コントロールのデータを取得し、指定されたに従って並べ替えます`SortExpression`と`SortDirection`プロパティです。 データの並べ替えの後に、データ ソース コントロールは、そのを GridView に返します。

DataList または Repeater コントロールでこの機能をレプリケートするには、必要があります。

- 並べ替えのインターフェイスを作成します。
- データ フィールドで並べ替えを行うと、昇順または降順で並べ替えるかを注意してください。
- 特定のデータ フィールドでデータを並べ替える ObjectDataSource を指示します。

これら 3 つのタスク手順 3 と 4 で取り上げるです。 次に、ページングとサポート DataList またはリピータ内の並べ替えの両方を含める方法について確認します。

## <a name="step-2-displaying-the-products-in-a-repeater"></a>手順 2: リピータで製品を表示します。

並べ替えに関連する機能を実装する心配し、前にリピータ コントロール内の製品を一覧表示して開始 s を使用できます。 開いて開始、 `Sorting.aspx`  ページで、`PagingSortingDataListRepeater`フォルダーです。 リピータ コントロールの設定、web ページを追加、`ID`プロパティを`SortableProducts`です。 リピータ s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDataSource`からデータを取得するように構成し、`ProductsBLL`クラスの`GetProducts()`メソッドです。 INSERT、UPDATE、および DELETE の各タブでドロップダウン リストからオプション (なし) を選択します。


[![ObjectDataSource を作成および GetProductsAsPagedDataSource() メソッドを使用するように構成します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**図 1**: ObjectDataSource を作成し、使用するように構成、`GetProductsAsPagedDataSource()`メソッド ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**図 2**: 更新、挿入のドロップダウン リストを設定し、(なし) タブを削除する ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


異なり、DataList で Visual Studio は自動作成されません、 `ItemTemplate` Repeater コントロール、データ ソースにバインドした後にします。 さらに、これを追加します`ItemTemplate`宣言、リピータ コントロールのスマート タグには、テンプレートの編集 オプションで DataList s がないです。 Let s を使用して同じ`ItemTemplate`前のチュートリアルからは、s の製品名、供給業者、およびカテゴリが表示されます。

追加した後、`ItemTemplate`リピータと ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

図 3 は、ブラウザーで表示したときに、このページを示します。


[![各製品の名前、供給業者、およびカテゴリが表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**図 3**: 各製品名、供給業者、およびカテゴリが表示されます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>手順 3: データの並べ替えに ObjectDataSource を指示します。

リピータに表示されるデータを並べ替えるには、データの並べ替えられた、並べ替え式の ObjectDataSource を通知する必要があります。 最初に発生させる、ObjectDataSource は、そのデータを取得する前にその[`Selecting`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)並べ替え式を指定する機会を提供します。 `Selecting`イベント ハンドラーは型のオブジェクトに渡されます[ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)、という名前のプロパティを持つ[ `Arguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx)型の[ `DataSourceSelectArguments`](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx)です。 `DataSourceSelectArguments`クラスは、データ ソース コントロールにデータのコンシューマーからのデータ関連の要求を渡すように設計され、が含まれています、 [ `SortExpression`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)です。

ASP.NET ページから、ObjectDataSource に並べ替え情報を渡すのイベント ハンドラーを作成、`Selecting`イベントと、次のコードを使用します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression*値に (ProductName) など、データの並べ替えにデータ フィールドの名前を割り当てる必要があります。 並べ替えの方向に関連するプロパティが存在しない、降順でデータを並べ替える場合が DESC 文字列を追加するために、 *sortExpression* (ProductName DESC) などの値。

いくつか異なるハード コーディングされた値を再試行してください*sortExpression*およびブラウザーで結果をテストします。 図 4 に示すとして ProductName DESC を使用する場合、 *sortExpression*製品はアルファベットの逆順で名前によって並べ替えられます。


[![製品はアルファベット順の逆に、名前順に並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**図 4**: この製品はアルファベット順の逆に、名前によって並べ替えられます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>手順 4: 並べ替えのインターフェイスを作成して、並べ替え式および方向を記憶

並べ替えをサポート GridView でオンにする各並べ替え可能なフィールドのヘッダー テキストに変換 LinkButton をクリックすると、それに応じてデータを並べ替えます。 このような並べ替えインターフェイスは、ここでそのデータが整然とレイアウトの列に、GridView にかなっています。 DataList リピータ コントロール、ただし、別の並べ替えインターフェイスが必要です。 共通の並べ替えインターフェイスではなく、データのグリッド)、データの一覧については、ドロップダウン リストで、データの並べ替えに使用できるフィールドを提供します。 このチュートリアルのようなインターフェイスを実装 s を使用できます。

上記の DropDownList の Web コントロールを追加、`SortableProducts`リピータとその`ID`プロパティを`SortBy`です。 [プロパティ] ウィンドウ内の省略記号をクリックして、 `Items` ListItem コレクション エディターを表示するプロパティです。 追加`ListItem`、データを並べ替えるには、 `ProductName`、 `CategoryName`、および`SupplierName`フィールドです。 追加も、`ListItem`アルファベットの逆順で名前によって、製品の並べ替えにします。

`ListItem` `Text` (名前など)、任意の値にプロパティを設定することができますが、 `Value` (ProductName) などのデータ フィールドの名前にプロパティを設定する必要があります。 降順で結果を並べ替えるには、するには、ProductName DESC と同様に、データ フィールド名に文字列の説明を追加します。


![並べ替え可能なデータ フィールドのそれぞれに対して、ListItem を追加します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**図 5**: 追加、`ListItem`並べ替え可能なデータ フィールドの各


最後に、次のドロップダウン リストの右側にボタン Web コントロールを追加します。 設定の`ID`に`RefreshRepeater`とその`Text`プロパティを更新します。

作成した後、 `ListItem` s、[更新] ボタンを追加して、DropDownList とボタン s の宣言構文を次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

完全な並べ替え DropDownList で次に必要があります、ObjectDataSource s を更新する`Selecting`イベント ハンドラーを使用する、選択したので`SortBy``ListItem`s`Value`ハード コーディングされた並べ替え式ではなくプロパティです。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

最初のページにアクセスすると、この時点で、製品は最初に並べ替えられますが、`ProductName`データ フィールドをその s、 `SortBy` `ListItem`既定で選択されている (図 6 を参照してください)。 異なる並べ替えカテゴリなどのオプションと [更新] を選択、ポストバックを発生させるし、再、図 7 に示すように、それらのカテゴリ名で、データを並べ替えます。


[![製品は、最初にその名前で並べ替え](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**図 6**:、製品は、最初にその名前で並べ替えられます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![製品がカテゴリで並べ替えられます](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**図 7**: この製品がカテゴリで並べ替えられます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> データを自動的に再度並べ替えたリピータのビュー ステートが無効になっているために原因となり、ポストバックごとにそのデータ ソースにバインドする中継ぎ局は、更新ボタンをクリックします。 ドロップダウン並べ替えを変更する有効な場合、リピータのビュー状態のままにしている場合、並べ替え順序に影響が t が勝利した一覧にあります。 この問題を解決するには、[更新] ボタン s のイベント ハンドラーを作成`Click`イベントおよび再バインドのデータ ソースにリピータ (リピータ s を呼び出すことによって`DataBind()`メソッド)。


## <a name="remembering-the-sort-expression-and-direction"></a>並べ替え式と方向を記憶

非並べ替えに関係しているポストバックが行われ、ページの並べ替え可能な DataList または Repeater を作成するときに、s の命令型の並べ替え式と方向をポストバック間で記憶されます。 たとえば、各商品の削除 ボタンを含めるには、このチュートリアルでは、リピータに更新されたことを想像してください。 ユーザーが [削除] ボタンをクリックしたときに d おリピータにデータをバインドし、直す、選択した製品を削除するコードを実行します。 並べ替えの詳細についてはポストバック間で永続化されない場合は、画面に表示されるデータは元の並べ替え順序に戻ります。

このチュートリアルでは、DropDownList 暗黙的に保存、並べ替え式と方向のビュー状態でご利用の米国します。 さまざまな並べ替えオプションを指定すると、あるいずれかの異なる並べ替えインターフェイスを使用していた場合は、ポストバック間での並べ替え順序を保存するように注意 d お必要があります。 これは、または別の状態の永続化手法で、クエリ文字列の並べ替えのパラメーターを含めることによってページ s のビュー ステートに並べ替え、パラメーターを格納することにより実現可能性があります。

このチュートリアルで将来の例では、ページのビュー状態で並べ替えの詳細を永続化する方法について説明します。

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>手順 5: 既定のページングを使用する DataList に並べ替えをサポートを追加します。

[前のチュートリアル](paging-report-data-in-a-datalist-or-repeater-control-vb.md)DataList で既定のページングを実装する方法について説明しました。 S をページングされたデータを並べ替えることは、この前の例の拡張を使用できます。 開いて開始、`SortingWithDefaultPaging.aspx`と`Paging.aspx`内のページ、`PagingSortingDataListRepeater`フォルダーです。 `Paging.aspx`  ページで、ページの宣言型マークアップを表示する ソース ボタンをクリックします。 選択したテキストをコピー (図 8 を参照してください) の宣言型マークアップを貼り付けます`SortingWithDefaultPaging.aspx`間、`<asp:Content>`タグ。


[![宣言型マークアップを複製、 &lt;Asp:content&gt; SortingWithDefaultPaging.aspx Paging.aspx からタグ](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**図 8**: 内の宣言型マークアップを複製、`<asp:Content>`からタグ`Paging.aspx`に`SortingWithDefaultPaging.aspx`([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


宣言型マークアップをコピーするには、その後メソッドとプロパティに、コピー、`Paging.aspx`ページの分離コード クラスを分離コード クラスを s`SortingWithDefaultPaging.aspx`です。 次に、すぐを表示、`SortingWithDefaultPaging.aspx`ブラウザーのページです。 同じ機能ととしての外観を示す必要があります`Paging.aspx`です。

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>ページングや並べ替えメソッドには、既定値を含める ProductsBLL の強化

前のチュートリアルで作成した、`GetProductsAsPagedDataSource(pageIndex, pageSize)`メソッドで、`ProductsBLL`返されるクラス、`PagedDataSource`オブジェクト。 これは、`PagedDataSource`オブジェクトが設定されている*すべて*の製品の (BLL s を介して`GetProducts()`メソッド) にバインドするとき、DataList を指定された対応するレコードだけが、 *pageIndex*および*pageSize*入力パラメーターが表示されます。

このチュートリアルで先ほど ObjectDataSource s から並べ替え式を指定して並べ替えのサポートを追加しました`Selecting`イベント ハンドラー。 これは、動作は問題、ObjectDataSource に並べ替えができるように、オブジェクトが返される場合、`ProductsDataTable`によって返される、`GetProducts()`メソッドです。 ただし、`PagedDataSource`によって返されるオブジェクト、`GetProductsAsPagedDataSource`メソッドがその内部データ ソースの並べ替えをサポートしていません。 代わりから返される結果を並べ替える必要があります、`GetProducts()`メソッド*する前に*で挿入しました、`PagedDataSource`です。

これを実現するで新しいメソッドを作成、`ProductsBLL`クラス、`GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`です。 並べ替えに、`ProductsDataTable`によって返される、`GetProducts()`メソッドを指定して、`Sort`プロパティの既定の`DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource`メソッドの違いから、`GetProductsAsPagedDataSource`メソッドの前のチュートリアルで作成します。 具体的には、`GetProductsSortedAsPagedDataSource`追加の入力パラメーターを受け取り`sortExpression`にこの値を代入し、`Sort`のプロパティ、 `ProductDataTable` s`DefaultView`です。 数行のコードを後で、`PagedDataSource`オブジェクト データ ソースが割り当てられている、 `ProductDataTable` s`DefaultView`です。

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>GetProductsSortedAsPagedDataSource メソッドを呼び出すと、SortExpression 入力パラメーターの値を指定します。

`GetProductsSortedAsPagedDataSource`メソッドが完了したら、次の手順は、このパラメーターの値を提供します。 ObjectDataSource`SortingWithDefaultPaging.aspx`を呼び出して現在構成されている、`GetProductsAsPagedDataSource`メソッドを 2 つから 2 つの入力パラメーターで渡します`QueryStringParameters`で指定されている、`SelectParameters`コレクション。 これらの 2 つ`QueryStringParameters`ことを示すための基になる、`GetProductsAsPagedDataSource`メソッド s *pageIndex*と*pageSize*パラメーターを取得するクエリ文字列フィールドから`pageIndex`と`pageSize`です。

ObjectDataSource %s 更新`SelectMethod`プロパティが新しいことを呼び出すように`GetProductsSortedAsPagedDataSource`メソッドです。 次に、追加、新しい`QueryStringParameter`できるように、 *sortExpression* querystring フィールドからの入力パラメーターにアクセス`sortExpression`です。 設定、 `QueryStringParameter` s `DefaultValue` ProductName にします。

これらの変更後に、ObjectDataSource s の宣言型マークアップようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

この時点で、`SortingWithDefaultPaging.aspx`ページは、製品名、その結果をアルファベット順に並べ替えが (図 9 を参照してください)。 これは、既定では、商品の値として渡されるため、`GetProductsSortedAsPagedDataSource`メソッド s *sortExpression*パラメーター。


[![既定では、結果が ProductName によって並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**図 9**: 既定では、結果が並べ替えられます`ProductName`([フルサイズのイメージを表示するには、をクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))。


手動で追加する場合、`sortExpression`などのクエリ文字列フィールド`SortingWithDefaultPaging.aspx?sortExpression=CategoryName`結果を並べ替えると指定`sortExpression`です。 ただし、この`sortExpression`データの別のページに移動するときに、パラメーターは、クエリ文字列に含まれていません。 実際には、次へ、または最後のページをクリックするとボタンが配置に戻すことは`Paging.aspx`! さらに、s が現在の並べ替えインターフェイスなし。 ユーザーは、ページングされたデータの並べ替え順序を変更できますのみ方法は、クエリ文字列を直接操作することによってです。

## <a name="creating-the-sorting-interface"></a>並べ替えのインターフェイスを作成します。

最初に更新する必要があります、`RedirectUser`メソッドをユーザーに送信する`SortingWithDefaultPaging.aspx`(の代わりに`Paging.aspx`) を含めると、`sortExpression`クエリ文字列の値。 読み取り専用ページ レベルの名前を追加する必要がありますもお`SortExpression`プロパティです。 このプロパティは、のような`PageIndex`と`PageSize`前のチュートリアルで作成されたプロパティの値を返します、 `sortExpression` querystring フィールドが存在する場合、既定値 (ProductName) それ以外の場合。

現在、`RedirectUser`メソッドは 1 つ入力パラメーターを表示するページのインデックスのみを受け取ります。 ただし、クエリ文字列で指定されたどの s 以外の並べ替え式を使用してデータの特定のページにユーザーをリダイレクトする場合もあります。 すぐにこのページは、一連の指定した列によってデータを並べ替えるためのボタンの Web コントロールを含めるの並べ替えのインターフェイスを作成します。 これらのボタンのいずれかがクリックされたときに適切な並べ替え式の値を渡して、ユーザーをリダイレクトするとします。 この機能を提供する 2 つのバージョンを作成、`RedirectUser`メソッドです。 1 つ目は、表示するには、2 つ目がページのインデックスおよび並べ替え式を受け取るときにページのインデックスだけを受け入れる必要があります。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

このチュートリアルでは最初の例では、DropDownList を使用して、並べ替えのインターフェイスを作成しました。 Let s をこの例では、3 つのボタン Web コントロールでの並べ替えのいずれかの DataList 上に配置を使用して`ProductName`,、1 つの`CategoryName`とに 1 つ`SupplierName`です。 設定、3 つのボタンの Web コントロールを追加、`ID`と`Text`プロパティ適切に。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

次に、作成、`Click`ごとのイベント ハンドラー。 イベント ハンドラーを呼び出す必要があります、`RedirectUser`適切な並べ替え式を使用して最初のページにユーザーを返すメソッド。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

最初のページへのアクセス、ときに、データはアルファベット順、製品名 (戻るは図 9 を参照してください)。 データの 2 番目のページまで進むし、カテゴリ ボタンで並べ替え をクリックするには、次へ ボタンをクリックします。 これは、カテゴリの名前で並べ替えられたデータの最初のページに戻ります us (図 10 参照)。 同様に、業者ボタンをクリックして、並べ替えをクリックするとデータの最初のページから業者によってデータを並べ替えます。 を通じて、データをページングと並べ替えの選択肢が記憶されます。 図 11 では、カテゴリでの並べ替えとデータの 13 番目のページに移動し、後のページが表示されます。


[![製品はカテゴリ順に並べ替えられます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**図 10**: この製品はカテゴリで並べ替えられます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![並べ替え式は記憶とページング経由のデータ](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**図 11**:、並べ替え式が記憶ときのページングを Data ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>手順 6: カスタム ページングとリピータ内のレコード

DataList の例では、非効率的な既定ページング技法を使用してそのデータを 5 つのページの手順に検査されます。 十分に大量のデータのページング、ときに、カスタム ページングを使用することが不可欠です。 戻り、[効率的にから大規模な量のデータのページング](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)と[カスタム ページングされたデータの並べ替え](../paging-and-sorting/sorting-custom-paged-data-vb.md)の BLL の既定およびカスタム ページングと作成されたメソッドの違いを調べるおチュートリアルについては、カスタム ページングとカスタム ページングされたデータの並べ替えを使用しています。 具体的には、これら 2 つの前のチュートリアルが追加されましたに次の 3 つのメソッド、`ProductsBLL`クラス。

- `GetProductsPaged(startRowIndex, maximumRows)`開始位置としてレコードの特定のサブセットを返します*startRowIndex*を超えないと*maximumRows*です。
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`特定の順に並べ替えて、指定されたレコードのサブセットを返します*sortExpression*入力パラメーターです。
- `TotalNumberOfProducts()`内のレコードの合計数を提供、`Products`データベース テーブルです。

ページを効率的におよび、DataList またはリピータ コントロールを使用してデータを並べ替えるには、これらのメソッドを使用できます。 これを示すためには、カスタム ページング サポート; Repeater コントロールを作成して開始 s を使用できます。並べ替え機能を追加します。

開く、 `SortingWithCustomPaging.aspx`  ページで、`PagingSortingDataListRepeater`フォルダー設定 ページにリピータを追加し、その`ID`プロパティを`Products`です。 リピータ s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDataSource`です。 データを選択するように構成、`ProductsBLL`クラスの`GetProductsPaged`メソッドです。


[![ObjectDataSource ProductsBLL クラスの GetProductsPaged メソッドを使用して構成します。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**図 12**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス s`GetProductsPaged`メソッド ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


UPDATE、INSERT でのドロップダウン リストの設定を (なし) タブを削除して、次へ ボタンをクリックします。 ソースでデータ ソースの構成ウィザードの指示に従って今すぐ、`GetProductsPaged`メソッド s *startRowIndex*と*maximumRows*パラメーターを入力します。 実際には、これらの入力パラメーターは無視されます。 代わりに、 *startRowIndex*と*maximumRows*経由で値が渡される、 `Arguments` ObjectDataSource s プロパティ`Selecting`お指定する方法と同じように、イベント ハンドラー、*sortExpression*このチュートリアルの最初のデモでします。 そのため、パラメーターのソースを None に設定ウィザードでのドロップダウン リストままにします。


[![パラメーターのソースになし](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**図 13**: なし にパラメーターのソースの設定のままにして ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> 操作を行います*されません*集合 ObjectDataSource s`EnablePaging`プロパティを`true`です。 これにより、自動的に含まれる、独自に ObjectDataSource *startRowIndex*と*maximumRows*パラメーターを`SelectMethod`s 既存のパラメーター リストです。 `EnablePaging`プロパティは、バインディング カスタム ページングされた DetailsView、GridView、フォーム ビューのコントロールにデータがこれらのコントロールは、ObjectDataSource s から特定の動作を想定しているためときに役立つ場合にのみ使用できます`EnablePaging`プロパティは`true`します。 DataList およびリピータ ページング サポートを手動で追加するがある、のでこのプロパティ設定のままにして`false`(既定)、おを組み込むことが必要な機能で、ASP.NET ページ内で直接にします。


リピータ s を最後に、定義`ItemTemplate`s の製品名、カテゴリ、および仕入先が表示されるようにします。 これらの変更後にリピータと ObjectDataSource s の宣言構文は次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

すぐをブラウザーでページにアクセスし、レコードが返されなかったことに注意してください。 これは、ためおを指定するには、まだ ve、 *startRowIndex*と*maximumRows*パラメーターの値の両方でそのため、値は 0 を渡されます。 これらの値を指定するには、ObjectDataSource s のイベント ハンドラーを作成`Selecting`イベント、これらのパラメーター値をプログラムでが 0 で、5、ハードコード値にそれぞれ設定します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

この変更によりは、ブラウザーを介して表示したときに、ページは、最初の 5 つの製品を表示します。


[![最初の 5 つのレコードが表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**図 14**: の最初の 5 つのレコードが表示されます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> 図 14 に示される製品が製品名によって並べ替えするために発生する、`GetProductsPaged`効率的なカスタム ページング クエリを実行するストアド プロシージャで結果を順序付けます`ProductName`です。


手順、ページを実行するユーザーをできるようにするのには、開始行のインデックスと行の最大数を追跡し、ポストバック間でこれらの値を保存する必要があります。 ページングの既定の例で使用して、クエリ文字列フィールドにこれらの値を永続化このデモでは、s がページのビュー ステートにこの情報を永続化を使用できます。 次の 2 つのプロパティを作成します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

使用するように選択するとイベント ハンドラーでコードを次に、更新、`StartRowIndex`と`MaximumRows`0 および 5 のハードコード値ではなくプロパティ。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

この時点で、ページは、最初の 5 つのレコードだけを引き続き表示されます。 ただし、これらのプロパティを同じ位置でのページング インターフェイスを作成する準備ができたらします。

## <a name="adding-the-paging-interface"></a>ページング インターフェイスの追加

使用すると、用途に、同じ最初、前に、次に、最後のページングのインターフェイスは、既定ページング例では、どのようなページのデータを表示するコントロールが表示されているラベル Web を含むと合計ページ数の存在で使用されます。 次の 4 つのボタン Web コントロールおよびリピータ下にあるラベルを追加します。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

次に、作成`Click`4 つのボタンのイベント ハンドラー。 これらのボタンがクリックされたときに更新する必要があります、`StartRowIndex`リピータにデータを再バインドとします。 最初、Previous、および [次へ] ボタンのコードは単純なのでが最後のボタンの方法はおを確認するデータの最後のページの開始行のインデックスしますか。 このインデックスだけでなくを介してページングされている合計レコードの数を知っておくべき次へ および最後のボタンを有効にするかどうかを判断できるを計算します。 これを呼び出すことによって判断できます、`ProductsBLL`クラスの`TotalNumberOfProducts()`メソッドです。 Let s という名前の読み取り専用で、ページ レベルのプロパティを作成する`TotalRowCount`の結果を返す、`TotalNumberOfProducts()`メソッド。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

このプロパティを持つようになりました最後のページの開始行のインデックスを判断できます。 具体的には、その結果の整数値の秒の`TotalRowCount`で割った値 1 を引いた`MaximumRows`を掛けた`MaximumRows`です。 お記述できるよう、`Click`ページング インターフェイスの 4 つのボタンのイベント ハンドラー。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

最後に、最後のページを表示するときに、データと、次へと最後のボタンの最初のページを表示するときに、まず 戻る ボタン、ページング インターフェイスでを無効にする必要があります。 これを実現する次のコードを追加、ObjectDataSource の`Selecting`イベントのハンドラー。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

これらの追加後に`Click`イベント ハンドラーとコードが有効にするにまたは現在の開始行のインデックスに基づくページング インターフェイス要素を無効にするには、ブラウザーでページをテストします。 図 15 示すように、まず最初のページにアクセスすると、[戻る] ボタンは無効になります。 最後をクリックすると最後のページが表示されますが、データの 2 ページ目 [次へ] をクリックすると表示 (図 16、17 を参照してください)。 データの最後のページを表示するときに、次へと最後のボタンは無効です。


[![製品の最初のページを表示するときに、前と最後のボタンが無効にします。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**図 15**: 製品の最初のページを表示するときに、前と最後のボタンが無効に ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![2 番目のページの製品がいないことを示す](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**図 16**: 2 番目のページの製品がいないことを示す ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![最後の表示データの最後のページをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**図 17**: 最後のデータ ページの最後をクリックすると表示 ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>手順 7: Repeater をページング サポートをカスタムの並べ替えを含む

これで、カスタム ページングを実装すると、並べ替えを含める準備ができたらがサポートされることです。 `ProductsBLL`クラス s`GetProductsPagedAndSorted`メソッドには、同じ*startRowIndex*と*maximumRows*入力パラメーターとして`GetProductsPaged`、追加で許可されるが、 *sortExpression*入力パラメーターです。 使用する、`GetProductsPagedAndSorted`メソッドから`SortingWithCustomPaging.aspx`、次の手順を実行する必要があります。

1. ObjectDataSource s を変更する`SelectMethod`プロパティから`GetProductsPaged`に`GetProductsPagedAndSorted`です。
2. 追加、 *sortExpression* `Parameter`オブジェクトに、ObjectDataSource の`SelectParameters`コレクション。
3. プライベート、ページ レベルの作成`SortExpression`ページ ビュー ステートをポストバック間でその値が引き続き発生するプロパティです。
4. ObjectDataSource %s 更新`Selecting`イベント ハンドラーに、ObjectDataSource s を割り当てる*sortExpression*パラメーター ページ レベルの値`SortExpression`プロパティです。
5. 並べ替えのインターフェイスを作成します。

ObjectDataSource s を更新することによって開始`SelectMethod`プロパティと追加する、 *sortExpression* `Parameter`です。 確認して、 *sortExpression* `Parameter` s`Type`プロパティに設定されている`String`です。 これらの最初の 2 つのタスクを完了すると、ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

次に、ページ レベル必要があります`SortExpression`ビューステートにシリアル化する値を持つプロパティです。 並べ替え式の値が設定されていない場合は、既定値として ProductName を使用します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

ObjectDataSource を呼び出す前に、`GetProductsPagedAndSorted`メソッドを設定する必要があります、 *sortExpression* `Parameter`の値に、`SortExpression`プロパティです。 `Selecting`イベント ハンドラーでは、次のコード行を追加します。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

並べ替えのインターフェイスを実装します。 最後の例で行ったように、製品名、カテゴリ、または供給業者によって結果の並べ替えをユーザーに許可する 3 つのボタンの Web コントロールを使用して実装されている並べ替えのインターフェイスを持っている s を使用できます。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

作成`Click`これら 3 つのボタン コントロールのイベント ハンドラー。 イベント ハンドラー、リセット、`StartRowIndex`を 0 に設定、`SortExpression`へ rebind リピータするデータを適切な値。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

すべてが s です! カスタム ページングと実装の並べ替えを取得する手順の数が、手順を実行しました既定のページングの必要なアクセス許可によく似ています。 図 18 では、カテゴリで並べ替えられたデータの最後のページを表示するときに、製品が表示されます。


[![最後のページのデータ、カテゴリ別、Sorted が表示されます。](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**図 18**: 最後のページのデータ、カテゴリ別、Sorted が表示されます ([フルサイズのイメージを表示するをクリックして](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> 前の例で、並べ替え式として使用されました。 選択は業者によって並べ替えるときにします。 ただし、カスタム ページングの実装の CompanyName を使用する必要があります。 これはカスタム ページングの実装を担当するストアド プロシージャ`GetProductsPagedAndSorted`に並べ替え式を渡します、`ROW_NUMBER()`キーワード、`ROW_NUMBER()`キーワードは、別名ではなく、実際の列名が必要です。 したがって、使用する必要があります`CompanyName`(内の列の名前、`Suppliers`テーブル) で使用される別名ではなく、`SELECT`クエリ (`SupplierName`) の並べ替え式。


## <a name="summary"></a>概要

どちらも DataList もリピータが組み込みの並べ替えのサポートを提供してがほんの少しのコードとカスタムの並べ替えインターフェイスのこのような機能を追加できます。 を介して、並べ替え式を指定することができます、並べ替え、ページングを実装する場合、 `DataSourceSelectArguments` ObjectDataSource s にオブジェクトが渡された`Select`メソッドです。 これは、`DataSourceSelectArguments`オブジェクト s `SortExpression` ObjectDataSource s でプロパティを割り当てることができます`Selecting`イベント ハンドラー。

DataList またはリピータ既にページング サポートを提供するには、並べ替え機能を追加するには、最も簡単な方法は、並べ替え式を受け取るメソッドを追加するビジネス ロジック層をカスタマイズするのにです。 この情報できますし、渡されるで ObjectDataSource s パラメーター`SelectParameters`です。

このチュートリアルでは、ページングと、DataList およびリピータ コントロールの並べ替え弊社の調査を完了します。 [次へ] および最後のチュートリアルでは、アイテムごとにいくつか、ユーザーによるカスタムの機能を提供するために、DataList およびリピータのテンプレートにボタンの Web コントロールを追加する方法を確認します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客は、David Suru をでした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
