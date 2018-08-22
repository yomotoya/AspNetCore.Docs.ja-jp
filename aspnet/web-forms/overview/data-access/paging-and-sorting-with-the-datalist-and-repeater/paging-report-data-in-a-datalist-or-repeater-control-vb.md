---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: DataList または Repeater コントロール (VB) でのレポート データをページング |Microsoft Docs
author: rick-anderson
description: DataList と Repeater のどちらもプランの自動ページングや並べ替えのサポートでは、中には、このチュートリアルは、DataList または Repeater、ページングのサポートを追加する方法を示します.
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 64615f126f87cec7a96f86385ee7a717fdcdd103
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826715"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>DataList または Repeater コントロール (VB) でのレポート データのページング
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe)または[PDF のダウンロード](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> DataList と Repeater のどちらもプラン ページングまたは並べ替えをサポート、このチュートリアルは、自動は、DataList または Repeater で、非常に柔軟性のページングとデータにより、表示のインターフェイスにページング サポートを追加する方法を示しています。


## <a name="introduction"></a>はじめに

ページングと並べ替えは、2 つの非常に一般的な機能は、オンライン アプリケーションでデータを表示する場合です。 たとえば、何百ものこのようなブックがある可能性があります、オンライン書店で ASP.NET に関する書籍を検索するときが検索結果の一覧を表示するレポートには、1 ページあたり 10 個の一致項目が一覧表示されます。 さらに、タイトル、価格、ページ数、作成者名、して結果を並べ替えることができます。 説明したように、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)チュートリアル、有効にできるチェック ボックスの目盛りに組み込みのページング サポートを提供すべて GridView、DetailsView と FormView コントロール。 GridView には、並べ替えをサポートも含まれています。

残念ながら、DataList でも、Repeater は自動ページングや並べ替えをサポートを提供します。 このチュートリアルでは DataList または Repeater にページング サポートを追加する方法を説明します。 必要があります手動でページング インターフェイスを作成、レコードの適切なページを表示おポストバック間でアクセスされているページに注意してください。 多くの時間と GridView、DetailsView、またはフォーム ビューでのコードよりもそのためには、中に、非常に柔軟性のページングとデータの DataList と Repeater を許可すると表示のインターフェイス。

> [!NOTE]
> このチュートリアルでは、ページングにのみ焦点を当てています。 次のチュートリアルでの並べ替え機能を追加することに注目有効にします。


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>手順 1: ページングを追加して、チュートリアルの Web ページの並べ替え

このチュートリアルを始める前に、このチュートリアルと、次にある必要があります、ASP.NET ページを追加する最初に少し s を使用できます。 という名前のプロジェクトで新しいフォルダーを作成して開始`PagingSortingDataListRepeater`します。 次に、次の 5 つの ASP.NET ページをこのフォルダーは、すべてのマスター ページを使用するように構成する追加`Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![PagingSortingDataListRepeater フォルダーを作成し、チュートリアルの ASP.NET ページを追加します。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**図 1**: 作成、`PagingSortingDataListRepeater`フォルダー チュートリアル ASP.NET ページを追加


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン サーフェイスにフォルダー。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-vb.md)チュートリアルでは、サイト マップの列挙し、箇条書きリストに現在のセクションでこれらのチュートリアルを表示します。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))。


箇条書きのページングと並べ替えのチュートリアルを作成しますを表示するのには、サイト マップに追加する必要があります。 開く、`Web.sitemap`ファイルを開き DataList サイト マップ ノードのマークアップを編集および削除した後、次のマークアップを追加します。


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![新しい ASP.NET ページは、サイト マップを更新します。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**図 3**: 新しい ASP.NET ページは、サイト マップを更新


## <a name="a-review-of-paging"></a>ページングのレビュー

前のチュートリアルでは、GridView、DetailsView、FormView コントロール内のデータをページングする方法を説明しました。 これら 3 つのコントロールのページングと呼ばれる単純なフォームを提供する*既定のページング*をコントロールのスマート タグでページングを有効にするオプションをチェックするだけで実装できます。 既定のページングを使用したデータのページが要求されるたびに最初のページのいずれかを参照してください、ユーザーがデータ GridView、DetailsView の別のページに移動またはと FormView コントロールを再要求*すべて*からのデータのObjectDataSource します。 切り取り領域を表示するレコードの特定のセットを要求されたページ インデックスと 1 ページに表示するレコードの数を指定します。 既定のページングで詳しく説明した、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)チュートリアル。

既定のページングが各ページのすべてのレコードを再要求するため実用的ではない十分に大量のデータをページングするときです。 たとえば、ページ サイズが 10 の 50,000 個のレコードを使用するページングがあるとします。 たびに、ユーザーが新しいページに移動 10 それらのみが表示される場合でも、50,000 個のレコードをすべては、データベースから取得する必要があります。

*カスタム ページング*要求されたページに表示するレコードの正確なサブセットのみを取得しておき、既定のページングのパフォーマンスの問題を解決します。 カスタム ページングを実装する場合を効率的に正しいレコードのセットだけを返す SQL クエリを記述する必要があります。 新しい SQL Server 2005 の s を使用してクエリを作成する方法を説明しました[`ROW_NUMBER()`キーワード](http://www.4guysfromrolla.com/webtech/010406-1.shtml)に戻り、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)チュートリアル。

DataList または Repeater コントロールで既定のページングを実装するために使用できる、 [ `PagedDataSource`クラス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx)のラッパーとして、`ProductsDataTable`内容がページングされています。 `PagedDataSource`クラスには、`DataSource`列挙可能なオブジェクトに割り当てることができるプロパティと[ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx)と[ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx)レコードの数を示すプロパティ1 つのページ、現在のページ インデックスを表示します。 これらのプロパティを設定すると、`PagedDataSource`データ Web コントロールのデータ ソースとして使用できます。 `PagedDataSource`列挙されると、その内部のレコードの適切なサブセットのみを返すは`DataSource`に基づいて、`PageSize`と`CurrentPageIndex`プロパティ。 図 4 の機能を示しています、`PagedDataSource`クラス。


![PagedDataSource ページング可能なインターフェイスを持つ列挙可能なオブジェクトをラップします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**図 4**:`PagedDataSource`ページング可能なインターフェイスを持つ列挙可能なオブジェクトをラップします。


`PagedDataSource`オブジェクトは作成し、ビジネス ロジック層から直接構成し DataList または Repeater、ObjectDataSource を通じてにバインドされているまたは作成および構成できる ASP.NET ページの分離コード クラスに直接します。 後者のアプローチを使用する場合は、ObjectDataSource を使用せず必要があり、DataList または Repeater に、ページングされたデータをプログラムで代わりにバインドします。

`PagedDataSource`オブジェクトにカスタム ページングをサポートするプロパティもあります。 使用してをバイパスしましたただし、`PagedDataSource`に BLL メソッドが既にあるため、カスタム ページング、`ProductsBLL`クラスを表示する正確なレコードを返すカスタム ページング用に設計されています。

このチュートリアルで紹介する新しいメソッドを追加することで、DataList で既定のページングを実装する、`ProductsBLL`クラスを返す適切に構成された`PagedDataSource`オブジェクト。 次のチュートリアルでは、カスタム ページングを使用する方法がわかります。

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>手順 2: ビジネス ロジック層での既定のページング メソッドの追加

`ProductsBLL`クラスが現在のすべての製品情報を返すメソッドを持つ`GetProducts()`と開始インデックス位置にある製品の特定のサブセットを返す 1 つずつ`GetProductsPaged(startRowIndex, maximumRows)`します。 既定のページングを使用した GridView、DetailsView と FormView コントロールすべての使用、`GetProducts()`メソッドを使用して、すべての製品を取得、`PagedDataSource`レコードの適切なサブセットのみを表示するには、内部的にします。 DataList と Repeater コントロールでのこの機能をレプリケートするには、この動作を模倣した BLL で新しいメソッドを作成できます。

メソッドを追加、`ProductsBLL`という名前のクラス`GetProductsAsPagedDataSource`内の整数の 2 つの入力パラメーターを取得します。

- `pageIndex` 表示するには、ページのインデックス 0 からインデックスを作成し、
- `pageSize` 1 ページに表示するレコードの数。

`GetProductsAsPagedDataSource` 取得することによって開始*すべて*からのレコードの`GetProducts()`します。 これは、後、作成、`PagedDataSource`オブジェクト、設定、`CurrentPageIndex`と`PageSize`プロパティを渡された内の値に`pageIndex`と`pageSize`パラメーター。 メソッドは最後に構成することを返すことによって`PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>手順 3: 既定のページングを使用して、DataList で製品情報を表示します。

`GetProductsAsPagedDataSource`メソッドに追加、`ProductsBLL`クラスを作成できます DataList または Repeater を既定のページングを提供します。 開いて開始、`Paging.aspx`ページで、`PagingSortingDataListRepeater`フォルダーと、デザイナーは、DataList s を設定するのには、ツールボックスからドラッグ DataList`ID`プロパティを`ProductsDefaultPaging`します。 DataList s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDefaultPagingDataSource`を使用してデータを取得するように構成し、`GetProductsAsPagedDataSource`メソッド。


[![ObjectDataSource を作成し、GetProductsAsPagedDataSource () メソッドを使用するように構成](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**図 5**: ObjectDataSource を作成し、使用するように構成、 `GetProductsAsPagedDataSource` `()`メソッド ([フルサイズの画像を表示する をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))。


UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。


[![UPDATE、INSERT で、ドロップダウン リストを設定し、(なし) タブを削除します。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**図 6**: UPDATE、INSERT で、ドロップダウン リストを設定し、[(なし) タブを削除する ([フルサイズの画像を表示する] をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))。


以降、`GetProductsAsPagedDataSource`メソッドに 2 つの入力パラメーターが必要ですが、これらのパラメーター値のソースの私たち、ウィザードで入力します。

ページのインデックスとページ サイズの値は、ポストバック間で記憶する必要があります。 ビュー ステートに格納できる、クエリ文字列に永続化、セッション変数に格納されているまたはその他のいくつかの手法を使用して記憶します。 このチュートリアルでは、ブックマークを設定するデータの特定のページを許可という利点がありますが、クエリ文字列を使用します。

具体的には、クエリ文字列フィールド pageIndex との pageSize を使用して、`pageIndex`と`pageSize`パラメーターをそれぞれ (図 7 を参照してください)。 T が勝利したクエリ文字列値は、ユーザーが最初にこのページにアクセスする場合に存在するようこれらのユーザーには、より低い権限のロールや日常的なニーズに適したロールを割り当てることをお勧めします現在の割り当てを確認し、 ここに提案した変更をご検討ください、これらのパラメーターの既定値を設定します。 `pageIndex`、既定値を 0 (これは、データの最初のページが表示されます) に設定し、`pageSize`を 4 秒の既定値。


[![PageIndex および pageSize パラメーターのソースとして、クエリ文字列を使用します。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**図 7**: のソースとして、クエリ文字列を使用して、`pageIndex`と`pageSize`パラメーター ([フルサイズの画像を表示する をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))。


ObjectDataSource を構成すると、Visual Studio が自動的に作成されます、 `ItemTemplate` DataList にします。 カスタマイズ、 `ItemTemplate` s の製品名、カテゴリ、および仕入先のみが表示されるようにします。 DataList s の設定も`RepeatColumns`プロパティを 2、その`Width`100% に、およびその`ItemStyle`s`Width`を 50% にします。 これらの幅の設定では、2 つの列に等しい間隔を提供します。

これらの変更を加えたら、DataList コントロールと ObjectDataSource のマークアップを次のようになります。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> 更新プログラムを実行していないか、またはこのチュートリアルでは機能を削除しましたから、レンダリングされたページ サイズを小さくには、DataList のビュー状態を無効にすることがあります。


最初に、ブラウザーからこのページにもアクセスしたとき、`pageIndex`も`pageSize`クエリ文字列パラメーターを指定します。 そのため、0 から 4 の既定値が使用されます。 図 8 に示す最初の 4 つの製品が表示される DataList でこの結果します。


[![最初の 4 つの製品の一覧が表示されます。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**図 8**: The の最初の 4 つの製品の一覧が表示されます ([フルサイズの画像を表示する をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))。


なし、ページングのインターフェイスが現在なく簡単には、データの 2 番目のページに移動するユーザーを意味します。 手順 4. でページング インターフェイスを作成します。 ここでは、ただし、ページングのみ実現できますで直接ページング条件を指定する、クエリ文字列。 たとえば、2 番目のページを表示するからブラウザーのアドレス バーに URL を変更`Paging.aspx`に`Paging.aspx?pageIndex=2`し、Enter キーを押します。 これが原因で表示されるデータの 2 番目のページ (図 9 参照)。


[![2 番目のページ データが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**図 9**: [2 番目のページのデータが表示されます ([フルサイズの画像を表示する] をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))。


## <a name="step-4-creating-the-paging-interface"></a>手順 4: ページング インターフェイスの作成

これは、さまざまな実装できるさまざまなページング インターフェイスです。 GridView、DetailsView、FormView コントロールを選択する 4 つの異なるインターフェイスを提供します。

- **次に、以前**ユーザーは、次または前のいずれかのものを一度に 1 つのページを移動できます。
- **次に、Previous、First、Last**このインターフェイスにはに加えて、前のボタンには、最初または最後のページに移動するための最初と最後のボタンが含まれています。
- **数値**ページング インターフェイスで、ユーザーに特定のページにすばやく移動できるようにするページ番号を一覧表示します。
- **数値、最初に、最終**数値によるページの数値だけでなく最初または最後のページに移動するためのボタンが含まれています。

DataList と Repeater では、私たちは、決定ページング インターフェイスを実装する責任を負います。 これは、ページで、必要な Web コントロールを作成し、特定のページング インターフェイス ボタンがクリックされたときに、要求されたページを表示する必要があります。 さらに、特定のページング インターフェイス コントロールは、無効にする必要があります。 たとえば、次を使用してデータの最初のページを表示すると、Previous、最初に、最後インターフェイスを最初と前の両方のボタンが無効になります。

このチュートリアルでは、let s 使用して、[次へ]、Previous、最初に、最後のインターフェイスです。 4 つのボタンの Web コントロールをページに追加し、設定、 `ID` s `FirstPage`、 `PrevPage`、 `NextPage`、および`LastPage`します。 設定、`Text`プロパティを&lt;&lt;最初、 &lt; [前へ]、[次へ] &gt;、姓と&gt;&gt;します。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

次に、作成、`Click`これらのボタンのイベント ハンドラー。 すぐには、要求されたページを表示するために必要なコードを追加します。

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>記憶がページングされるレコードの合計数

選択されているページング インターフェイスに関係なくは、ポケットベル通知でレコードの合計数に注意してくださいを計算する必要があります。 データの合計数のページはページされて、どのようなページング インターフェイス コントロールの追加、または有効になってを決定する (ページ サイズと組み合わせて) 行の合計数を決定します。 次へ、前、最初、最後のインターフェイスは、作成するページ数は、2 つの方法で使用されます。

- 最後のページを場合に表示するかどうかを判断するには、次へと最終ボタンは無効です。
- 最後 whisk にする必要があります。 最後のボタンをクリックした場合、ページより小さい ページで、インデックスが 1 つをカウントします。

行の合計数の上限は、ページ サイズで割った値には、ページ数が計算されます。 たとえば、79 レコードと 1 ページあたりの 4 つのレコードをページングしますし、ページ数は 20 (79 の ceiling/4)。 この情報私たちの通知を表示する数値によるページ ボタンの数に関する場合は、数値のページング インターフェイスを使用していることページング インターフェイスには、[次へ] または最後のボタンが含まれている場合、ページ数は、[次へ]、または最後にボタンを無効にする場合の判別に使用されます。

ページング インターフェイスには、最後のボタンが含まれている場合は、ポケットベル通知でレコードの合計数は、最後のボタンがクリックされたときに、最後のページ インデックスを確認しましたできるようにポストバック間で記憶されます必要があります。 そのため、作成、`TotalRowCount`状態を表示するには、その値を保持する ASP.NET ページの分離コード クラスのプロパティ。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

加え`TotalRowCount`、ページ インデックス、ページのサイズを簡単にアクセスするための読み取り専用ページ レベルのプロパティを作成する時間がかかるし、ページ数。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>ポケットベル通知でレコードの合計数を決定します。

`PagedDataSource` ObjectDataSource s から返されたオブジェクト`Select()`メソッドは、その中に*すべて*、製品レコードの場合でも、それらのサブセットのみが表示されます、DataList でします。 `PagedDataSource` S [ `Count`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx); DataList で表示される項目の数のみを返します、 [ `DataSourceCount`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx)内の項目の合計数を返します、`PagedDataSource`. そのため、ASP.NET ページを割り当てる必要があります`TotalRowCount`プロパティ値の`PagedDataSource`s`DataSourceCount`プロパティ。

これを実現するには、ObjectDataSource s のイベント ハンドラーを作成`Selected`イベント。 `Selected` ObjectDataSource s の戻り値へのアクセスがあるイベント ハンドラー`Select()`この場合、メソッド、`PagedDataSource`します。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>データの要求されたページを表示します。

ユーザーには、いずれかのページング インターフェイスでボタンがクリックすると、データの要求されたページを表示する必要があります。 ページングのパラメーターが要求されたデータの使用のページを表示する、クエリ文字列を使用して指定されているため`Response.Redirect(url)`、ユーザーのブラウザー再要求して、`Paging.aspx`ページングの適切なパラメーターを含むページ。 たとえば、データの 2 番目のページを表示するは、ユーザーをリダイレクト`Paging.aspx?pageIndex=1`します。

そのため、作成、`RedirectUser(sendUserToPageIndex)`メソッドにユーザーをリダイレクトする`Paging.aspx?pageIndex=sendUserToPageIndex`します。 このメソッドは、次の 4 つのボタンをクリックし、`Click`イベント ハンドラー。 `FirstPage` `Click`イベント ハンドラーを呼び出します`RedirectUser(0)`、最初のページに送信する、 `PrevPage` `Click`イベント ハンドラーを使用して`PageIndex - 1`ページ インデックスとこれにします。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

`Click`イベント ハンドラーが完了すると、ボタンをクリックして DataList のレコードがページングされることができます。 すぐお試しください!

## <a name="disabling-paging-interface-controls"></a>インターフェイスのコントロールのページングを無効にします。

現時点では、すべての 4 つのボタンが表示されているページに関係なく有効です。 ただし、最後のページを表示するときに、データ、および 次へ と 最後のボタンの最初のページを表示するときに、最初および戻るボタンを無効にします。 `PagedDataSource` ObjectDataSource s によって返されるオブジェクト`Select()`メソッドがプロパティ[ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx)と[ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx)表示しているかどうかを判断することを検証できます。データの最初と最後のページ。

ObjectDataSource s に、次の追加`Selected`イベント ハンドラー。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

これにより、最後のページを表示するときに、次へと最終ボタンは無効にするときに、最初のページを表示するときに、最初および戻るボタンを無効化されます。

Let s ページング インターフェイスの完了をユーザーに通知してどのようなページが現在表示するいると、合計ページ数が存在します。 ラベルの Web コントロールをページに追加し、設定、`ID`プロパティを`CurrentPageNumber`します。 設定の`Text`ObjectDataSource s 選択されたイベント ハンドラーでこのようなプロパティが表示されている現在のページが含まれている (`PageIndex + 1`) と合計ページ数 (`PageCount`)。


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

図 10 に示します`Paging.aspx`初めてアクセスしたときにします。 DataList の; 最初の 4 つの製品が表示された既定値は、クエリ文字列は空であるため1 つ目と前のボタンが無効です。 [次へ] をクリックすると次の 4 つレコードを表示します (図 11 を参照してください)。1 つ目と前のボタンが有効になりました。


[![最初のページ データが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**図 10**: データの最初のページが表示されます ([フルサイズの画像を表示する をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))。


[![2 番目のページ データが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**図 11**: [2 番目のページのデータが表示されます ([フルサイズの画像を表示する] をクリックします](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))。


> [!NOTE]
> ページング インターフェイスは、1 ページを表示するページ数を指定するユーザーを許可することで、さらに拡張できます。 たとえば、DropDownList には、5、10、25、50、およびすべてのようなリスト ページ サイズのオプションを追加でした。 ページ サイズを選択すると、ユーザーが にリダイレクトする必要があります`Paging.aspx?pageIndex=0&pageSize=selectedPageSize`します。 練習としてこの拡張機能を実装するリーダーのままにします。


## <a name="using-custom-paging"></a>カスタム ページングを使用します。

既定の非効率的なページングの手法を使用してそのデータを DataList ページ。 十分に大量のデータのページング、ときに、カスタム ページングを使用することが重要です。 実装の詳細が若干異なりますが、DataList でカスタム ページングを実装する概念は既定のページングと同じになります。 カスタム ページングを使用して、`ProductBLL`クラス s`GetProductsPaged`メソッド (の代わりに`GetProductsAsPagedDataSource`)。 説明したように、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)チュートリアルでは、`GetProductsPaged`返す行の開始行インデックスと最大数を渡す必要があります。 これらのパラメーターは、クエリ文字列と同様で維持できる、`pageIndex`と`pageSize`既定のページングに使用されるパラメーター。

S 以降ありません`PagedDataSource`かどうかによって、およびページングされるレコードの合計数を決定するカスタムのページングを使用した手法を使用する必要があります re データの最初と最後のページを表示します。 `TotalNumberOfProducts()`メソッドで、`ProductsBLL`クラスを介してページングされる製品の合計数を返します。 特定するデータの最初のページが表示されているかどうか、調査開始行インデックス、0 の場合は、最初のページが表示されています。 最後のページが、開始行インデックスの合計を返す最大行数がを介してページングされるレコードの総数以上場合に表示されています。

次のチュートリアルでさらに詳しくカスタム ページングを実装するについて説明します。

## <a name="summary"></a>まとめ

DataList と Repeater のどちらも帯を提供しています。 GridView、DetailsView にあるボックス、ページングをサポートし、コントロールと FormView コントロール、最小限の労力でこのような機能を追加することができます。 既定のページングを実装する最も簡単な方法に含まれる製品のセット全体をラップする、`PagedDataSource`し、バインド、 `PagedDataSource` DataList または Repeater をします。 このチュートリアルでは追加されました、`GetProductsAsPagedDataSource`メソッドを`ProductsBLL`を返すために、`PagedDataSource`します。 `ProductsBLL`クラスが既にカスタム ページングのために必要なメソッドを含む`GetProductsPaged`と`TotalNumberOfProducts`します。

カスタム ページングを表示するレコードの正確なセット、またはすべてのレコードのいずれかを取得すると共に、`PagedDataSource`既定のページングも必要があります、ページング インターフェイスを手動で追加します。 このチュートリアルで作成した、次に、Previous、First、最終インターフェイスを 4 つのボタンの Web コントロール。 また、現在のページ番号とページの合計数を表示するラベル コントロールが追加されました。

次のチュートリアルでは、DataList と Repeater を並べ替えのサポートを追加する方法がわかります。 ページおよびできる (既定およびカスタム ページングを使用して例) で並べ替えを DataList を作成する方法についても説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Liz Shulok、Ken Pespisa、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [次へ](sorting-data-in-a-datalist-or-repeater-control-vb.md)
