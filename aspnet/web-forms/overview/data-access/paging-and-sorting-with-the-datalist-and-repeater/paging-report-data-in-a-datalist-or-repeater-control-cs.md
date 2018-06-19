---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: Datalist コントロールまたは Repeater コントロール (c#) でのレポート データをページング |Microsoft ドキュメント
author: rick-anderson
description: DataList でも、リピータ オファーの自動ページングや並べ替えのサポート、中には、このチュートリアルは、DataList またはリピータ、ページング サポートを追加する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 43b1370e1411858cef02bca534d082a3c105e51e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887614"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>DataList または Repeater コントロール (c#) でレポート データのページング
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe)または[PDF のダウンロード](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> DataList でも、リピータ申し出ページングや並べ替えをサポート、このチュートリアルは、自動では、DataList またはリピータ、これによりより柔軟のページングとデータの表示のインターフェイスにページング サポートを追加する方法が表示されます。


## <a name="introduction"></a>はじめに

ページングや並べ替えは、2 つの非常に一般的な機能をオンライン アプリケーションでデータを表示するときにします。 たとえば、何百ものようなブックがある可能性があります、オンライン書店で本を ASP.NET 検索するときに検索結果を一覧表示するレポートには、1 ページあたり 10 個まで一致するが一覧表示します。 さらに、タイトル、価格、ページ数、作成者名、して結果を並べ替えることができます。 説明したよう、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-cs.md)チュートリアル、チェック ボックスのチェック マークを有効にできる組み込みのページング サポートを提供すべて GridView、DetailsView、およびフォームのコントロールです。 GridView には、並べ替えをサポートも含まれています。

残念ながら、DataList もリピータは自動ページングや並べ替えをサポートを提供します。 このチュートリアルでは、DataList またはリピータにページング サポートを追加する方法について確認します。 お必要があります手動でページング インターフェイスを作成、レコードの適切なページを表示してポストバック間でアクセスしたページに注意してください。 多くの時間とよりコードの GridView、DetailsView、またはフォーム ビューとそのためには、中より柔軟のページングとデータの DataList と Repeater を使用すると表示インターフェイスできます。

> [!NOTE]
> このチュートリアルでは、ページングした場合にのみ焦点を当てています。 次のチュートリアルでの並べ替え機能を追加することに注目有効にします。


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>手順 1: ページングを追加して、チュートリアルの Web ページの並べ替え

このチュートリアルを始める前に、まずこのチュートリアルと次の必要があります ASP.NET ページを追加する少し時間を取って s を使用できます。 という名前のプロジェクトに新しいフォルダーを作成して開始`PagingSortingDataListRepeater`です。 次に、すべてのマスター ページを使用するように構成して、このフォルダーに次の 5 つの ASP.NET ページを追加`Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![PagingSortingDataListRepeater フォルダーを作成し、チュートリアルの ASP.NET ページを追加](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**図 1**: 作成、`PagingSortingDataListRepeater`フォルダー チュートリアルの ASP.NET ページを追加


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン画面上のフォルダーです。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップを列挙し、箇条書きリストの現在のセクションでこれらのチュートリアルが表示されます。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


箇条書きのページングと並べ替えを作成してチュートリアルを表示するために、サイト マップに追加する必要があります。 開く、`Web.sitemap`ファイルし、DataList サイト マップ ノードのマークアップの後に編集および削除すると、次のマークアップを追加します。


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![マップを更新するサイトに新しい ASP.NET ページを含める](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**図 3**: 新規の ASP.NET ページを含むサイト マップを更新


## <a name="a-review-of-paging"></a>ページングのレビュー

前のチュートリアルでは、GridView、DetailsView、フォーム ビュー コントロール内のデータをページングする方法を説明しました。 これら 3 つのコントロールと呼ばれるページングの単純なフォームを提供する*既定ページング*コントロールのスマート タグでページングを有効にするオプションをチェックするだけでを実装することができます。 既定のページングとページのデータが要求されるたびに最初のページのいずれかを参照してくださいまたはときに、ユーザーがデータ GridView DetailsView の別のページに移動または FormView コントロールを再要求*すべて*からのデータのObjectDataSource です。 切り取り領域を表示するレコードの特定のセットを要求されたページ インデックスと 1 ページに表示するレコードの数を指定します。 既定のページングで詳しく説明した、[レポート データの並べ替えとページング](../paging-and-sorting/paging-and-sorting-report-data-cs.md)チュートリアルです。

既定のページングは、各ページのすべてのレコードを再要求、ので実用的ではありません十分に大量のデータをページングするときです。 たとえば、ページングとページ サイズが 10 のレコードが 50,000 個を想像してください。 ユーザーが新しいページに移動するたびにそれらの唯一の 10 個が表示される場合でも、すべてのレコードが 50,000 個は、データベースから取得する必要があります。

*カスタム ページング*要求されたページに表示するレコードの正確なサブセットのみを取得しておき、既定のページングのパフォーマンスの問題を解決します。 カスタム ページングを実装する場合を効率的に正しいレコードのセットだけを返す SQL クエリを記述お必要があります。 SQL Server 2005 の s を new を使用してクエリを作成する方法を説明しました[`ROW_NUMBER()`キーワード](http://www.4guysfromrolla.com/webtech/010406-1.shtml)に戻り、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)チュートリアルです。

DataList またはリピータ コントロール内の既定のページングを実装には、使用、 [ `PagedDataSource`クラス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx)をラップするラッパーとして、`ProductsDataTable`の内容が、ポケットベル通知を受け取ります。 `PagedDataSource`クラスには、`DataSource`任意の列挙可能なオブジェクトに割り当てることができるプロパティと[ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx)と[ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx)にレコードの数を示すプロパティ1 ページ、および現在のページ インデックスを表示します。 これらのプロパティを設定した後、 `PagedDataSource` Web コントロールのデータのデータ ソースとして使用できます。 `PagedDataSource`が列挙されるときに、その内部のレコードの適切なサブセットをのみを返すには`DataSource`に基づいて、`PageSize`と`CurrentPageIndex`プロパティです。 図 4 の機能を示しています、`PagedDataSource`クラスです。


![PagedDataSource ページング可能なインターフェイスと列挙可能なオブジェクトをラップします。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**図 4**:`PagedDataSource`ページング可能なインターフェイスと列挙可能なオブジェクトをラップ


`PagedDataSource`オブジェクトには作成されたビジネス ロジック層から直接構成し DataList または、ObjectDataSource を通じてリピータにバインドまたは作成、できる ASP.NET ページの分離コード クラス内で直接設定します。 後者のアプローチを使用する場合は、ObjectDataSource を使用せずして代わりに、ページングされたデータをバインド DataList またはリピータ プログラムで必要があります。

`PagedDataSource`オブジェクトにカスタム ページングをサポートするプロパティもあります。 ただし、ここをバイパスできますを使用して、`PagedDataSource`に BLL メソッドが既にあるため、カスタム ページング、`ProductsBLL`クラスを表示する正確なレコードを返すカスタム ページング用に設計されています。

このチュートリアルで見ていきます DataList の新しいメソッドを追加することで既定のページングを実装する、`ProductsBLL`を適切に構成された返すクラス`PagedDataSource`オブジェクト。 次のチュートリアルは、カスタム ページングを使用する方法が表示されます。

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>手順 2: ビジネス ロジック層での既定のページング メソッドの追加

`ProductsBLL`クラスにはすべての製品情報を返すためのメソッドは現在`GetProducts()`と開始インデックス位置の製品の特定のサブセットを返すための 1 つ`GetProductsPaged(startRowIndex, maximumRows)`です。 既定のページング、GridView、DetailsView、およびフォーム ビューのコントロールすべての使用、`GetProducts()`をすべての製品の取得を使用し、メソッド、`PagedDataSource`レコードの適切なサブセットのみを表示するには、内部的にします。 DataList とリピータ コントロールでこの機能を複製するには、この動作を模倣した BLL で新しいメソッドを作成できます。

メソッドを追加する、`ProductsBLL`という名前のクラス`GetProductsAsPagedDataSource`内の整数の 2 つの入力パラメーターを取得します。

- `pageIndex` 表示するには、ページのインデックス 0 でインデックスを作成し、
- `pageSize` ページごとに表示するレコードの数。

`GetProductsAsPagedDataSource` 取得することによって開始*すべて*からレコードの`GetProducts()`します。 これは、後、作成、`PagedDataSource`オブジェクト、設定、`CurrentPageIndex`と`PageSize`プロパティを渡された内の値に`pageIndex`と`pageSize`パラメーター。 メソッドが終了して返す未構成`PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>手順 3: 既定のページングを使用して、DataList で製品情報を表示します。

`GetProductsAsPagedDataSource`メソッドに追加された、`ProductsBLL`クラス、お今すぐ作成できる DataList または既定のページングを提供するリピータします。 開いて開始、 `Paging.aspx`  ページで、`PagingSortingDataListRepeater`フォルダーと、デザイナーは、DataList s を設定するのには、ツールボックスからドラッグ DataList`ID`プロパティを`ProductsDefaultPaging`です。 DataList s のスマート タグからの作成という名前の新しい ObjectDataSource`ProductsDefaultPagingDataSource`しを使用してデータを取得するため、構成、`GetProductsAsPagedDataSource`メソッドです。


[![ObjectDataSource を作成および GetProductsAsPagedDataSource () メソッドを使用するように構成します。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**図 5**: ObjectDataSource を作成し、使用するように構成、 `GetProductsAsPagedDataSource` `()`メソッド ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**図 6**: 更新、挿入のドロップダウン リストを設定し、(なし) タブを削除する ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


以降、`GetProductsAsPagedDataSource`メソッドに次の 2 つの入力パラメーターが必要ですが、ウィザードの指示に従って us これらのパラメーター値のソース。

ページのインデックスとページ サイズの値は、ポストバック間で記憶する必要があります。 ビュー ステートに格納できる、クエリ文字列に永続化される、セッション変数に格納されているまたは別の手法を使用して記憶します。 このチュートリアルでは、クエリ文字列は、ブックマークへのデータの特定のページをできるという利点が使用されます。

具体的には、クエリ文字列フィールド pageIndex および pageSize を使用して、`pageIndex`と`pageSize`パラメーター、それぞれ (図 7 を参照してください)。 T が勝利したクエリ文字列の値はユーザーが最初にこのページを訪問したときに存在するとすぐ、これらのパラメーターの既定値を設定します。 `pageIndex`、(データの最初のページが表示されます) を 0 に既定値を設定し、 `pageSize` 4 s 既定値です。


[![PageIndex および pageSize のパラメーターのソースとして、クエリ文字列を使用します。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**図 7**: のソースとして、クエリ文字列を使用して、`pageIndex`と`pageSize`パラメーター ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


ObjectDataSource を構成すると、Visual Studio が自動的に作成、 `ItemTemplate` DataList のです。 カスタマイズ、 `ItemTemplate` s の製品名、カテゴリ、および仕入先のみが表示されるようにします。 DataList s の設定も`RepeatColumns`プロパティを 2、その`Width`を 100% とその`ItemStyle`s `Width` 50% にします。 これらの幅の設定では、2 つの列の等しい間隔を提供します。

これらの変更を加えたら、DataList と ObjectDataSource のマークアップを次のようになります。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> 更新プログラムを実行していないか、またはこのチュートリアルでは機能を削除してから、レンダリングされるページ サイズを小さく DataList のビュー ステートを無効にすることがあります。


最初にも、ブラウザーからこのページにアクセスしたとき、`pageIndex`も`pageSize`querystring パラメーターを指定します。 そのため、0 から 4 の既定値が使用されます。 図 8 に示す最初の 4 つの製品が表示される DataList でこの結果します。


[![最初の 4 つの製品の一覧が表示されます。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**図 8**: の最初の 4 つの製品の一覧が表示されます ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


なし、ページング インターフェイスが現在簡単な s は、データの 2 番目のページに移動するユーザーに意味します。 手順 4. でページング インターフェイスを作成します。 ここでは、ただし、ページングするしかない直接条件を指定して、ページング、クエリ文字列の。 たとえば、2 番目のページを表示する、ブラウザーのアドレス バーからの URL を変更`Paging.aspx`に`Paging.aspx?pageIndex=2`し、enter キーを押してです。 これが原因で表示されるデータの 2 ページ目 (図 9 を参照してください)。


[![2 番目のページ データが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**図 9**: 2 番目のページのデータが表示されます ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>手順 4: ページング インターフェイスの作成

これは、さまざまな異なるページング インターフェイスを実装することができます。 GridView、DetailsView、フォーム ビューのコントロールを選択する 4 つの異なるインターフェイスを提供します。

- **次に、以前**ユーザーは、[次へ] またはそれ以前のいずれかに、一度に 1 つのページを移動できます。
- **次に、Previous、最初、最後**だけでなく、前のボタンでは、このインターフェイスに非常に最初と最後のページに移動するための最初と最後のボタンが含まれています。
- **数値**ページング インターフェイスで、特定のページに迅速にジャンプするユーザーを許可するページ番号が一覧表示します。
- **数値、最初に、最終**数値によるページ数字だけでなく、非常に最初と最後のページに移動するためのボタンが含まれています。

DataList とリピータ、ページング インターフェイス決定し、実装することを担当しています。 ページで必要な Web コントロールを作成する特定のページング インターフェイス ボタンがクリックされたときに、要求されたページを表示する必要があります。 さらに、特定のページング インターフェイス コントロールは、無効にする必要があります。 たとえば、次を使用してデータの最初のページを表示すると、Previous、最初に、最後インターフェイスでは、最初と前の両方のボタンが無効になります。

このチュートリアルで使用すると、s を使用して次に、Previous、最初に、最後のインターフェイスです。 4 つのボタンの Web コントロールをページに追加し、設定、 `ID` s `FirstPage`、 `PrevPage`、 `NextPage`、および`LastPage`です。 設定、`Text`プロパティ&lt;&lt;最初、 &lt; 前へ、次へ&gt;、姓と&gt;&gt;です。


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

次に、作成、`Click`これらのボタンのイベント ハンドラー。 すぐには、要求されたページを表示するために必要なコードを追加します。

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>記憶を介してページングされるレコードの合計数

選択されているページング インターフェイスに関係なくコンピューティングをを介してページングされるレコードの合計数を覚えておく必要があります。 (ページ サイズと組み合わせて) 合計行の数は、データの合計ページ数がページングされるを通じて、どのようなインターフェイスのページング コントロールの追加、または有効になってを決定するを決定します。 次へ、Previous、最初、最後のインターフェイスを構築しているページ数は 2 つの方法で使用されます。

- かどうか表示している最後のページでは、次へと最後のボタンが無効になっている場合を決定します。
- 最後 whisk にする必要がありますの最後のボタンをクリックした場合、ページより小さい ページで、インデックスが 1 つをカウントします。

ページ サイズで除算し、合計行の数の上限として、ページ数が計算されます。 たとえば、79 レコードと 1 ページあたり 4 つのレコードをページングお場合、ページ数は 20 (79 の ceiling/4)。 使用していない場合、数値のページング インターフェイス数値によるページ ボタンの数に関するこの情報が示さを表示します。ページングのインターフェイスには、次へ、または最後のボタンが含まれている 次へ、または最後のボタンを無効にするタイミングを決定するページ数が使用されます。

ページング インターフェイスには、最後のボタンが含まれている場合、レコードの合計数からページングされる記憶しておくことポストバック間での最後のボタンがクリックされたときに、最後のページ インデックスを確認おできるようにが不可欠です。 この作業を容易に作成、`TotalRowCount`状態を表示するには、その値が引き続き発生する ASP.NET ページの分離コード クラスのプロパティ。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

加え`TotalRowCount` ページのインデックス、ページ サイズを簡単にアクセスするための読み取り専用ページ レベルのプロパティを作成するには、1 分を行い、ページ数。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>を通じてページングされるレコードの合計数を決定します。

`PagedDataSource` ObjectDataSource s から返されたオブジェクト`Select()`メソッドは、その中に*すべて*製品レコードの場合でも、それらのサブセットのみが表示されます、DataList でします。 `PagedDataSource` S [ `Count`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx); DataList で表示される項目の数のみを返します、 [ `DataSourceCount`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx)内のアイテムの合計数を返します、`PagedDataSource`. そのため、ASP.NET ページを割り当てる必要があります`TotalRowCount`プロパティ値の`PagedDataSource`s`DataSourceCount`プロパティです。

これを実現するには、ObjectDataSource s のイベント ハンドラーを作成`Selected`イベント。 `Selected` ObjectDataSource s の戻り値へのアクセスがあるイベント ハンドラー`Select()`この場合、メソッド、`PagedDataSource`です。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>データの要求されたページを表示します。

ユーザーをクリックすると、ページング インターフェイスでボタンのいずれかが、要求されたデータのページを表示する必要があります。 ページング パラメーターは、データの使用の要求されたページを表示する、クエリ文字列を使用して指定ため`Response.Redirect(url)`、ユーザーのブラウザー再要求して、`Paging.aspx`ページングの適切なパラメーターを含むページです。 たとえば、表示するには、2 番目のページのデータおはユーザーをリダイレクトして`Paging.aspx?pageIndex=1`です。

この作業を容易に作成、`RedirectUser(sendUserToPageIndex)`メソッドにユーザーをリダイレクトする`Paging.aspx?pageIndex=sendUserToPageIndex`です。 このメソッドは、次の 4 つのボタンをクリックし、`Click`イベント ハンドラー。 `FirstPage` `Click`イベント ハンドラーを呼び出します`RedirectUser(0)`、; で、最初のページに送信する、 `PrevPage` `Click`イベント ハンドラーを使用して`PageIndex - 1`ページ インデックスとなどです。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

`Click`イベント ハンドラーが完了したら、DataList のレコードは、ボタンをクリックしてからページングされることができます。 試しに少しの時間を取ってください。

## <a name="disabling-paging-interface-controls"></a>インターフェイスのコントロールのページングを無効にします。

現時点では、次の 4 つのすべてのボタンが表示されているページに関係なく有効です。 ただし、最後のページを表示するときに、データ、および 次へと最後のボタンの最初のページを表示する場合、まず 戻る ボタンを無効にします。 `PagedDataSource` ObjectDataSource s によって返されるオブジェクト`Select()`メソッドがプロパティ[ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx)と[ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx)お表示しているかどうかを決定することを検証できます。データの最初と最後のページです。

ObjectDataSource s に、以下を追加`Selected`イベントのハンドラー。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

この追加すると、最後のページを表示するときに、次へと最後のボタンは無効になります中に、最初のページを表示するときに最初 戻る ボタンを使用できなくなります。

Let s 完了ページング インターフェイス ユーザーに通知してどのようなページが現在表示するいると、合計ページ数が存在します。 Label Web コントロールをページに追加し、設定、`ID`プロパティを`CurrentPageNumber`です。 設定の`Text`ObjectDataSource s 選択されたイベント ハンドラーでこのようなプロパティが表示されている現在のページが含まれている (`PageIndex + 1`) と合計ページ数 (`PageCount`)。


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

図 10 示します`Paging.aspx`初めてアクセスしたときにします。 DataList の; 最初の 4 つの製品が表示された既定値は、クエリ文字列が空のため、まず [戻る] ボタンは無効です。 [次へ] をクリックすると次の 4 つレコードを表示します (図 11 を参照してください)。まず [戻る] ボタンが有効になりました。


[![データの最初のページが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**図 10**: データの最初のページが表示されます ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![2 番目のページ データが表示されます。](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**図 11**: 2 番目のページのデータが表示されます ([フルサイズのイメージを表示するをクリックして](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> ページング インターフェイスは、1 ページを表示するページ数を指定するユーザーを許可することで、さらに拡張できます。 たとえば、DropDownList に 5、10、25、50、およびすべてのようなリスト ページ サイズのオプションを追加できませんでした。 ページ サイズを選択すると、ユーザーが にリダイレクトする必要があります`Paging.aspx?pageIndex=0&pageSize=selectedPageSize`です。 演習としてこの拡張機能を実装するリーダーのままにします。


## <a name="using-custom-paging"></a>カスタム ページングを使用します。

非効率的な既定ページング技法を使用してそのデータを DataList ページ。 十分に大量のデータのページング、ときに、カスタム ページングを使用することが不可欠です。 実装の詳細が若干異なります、DataList でカスタム ページングを実装する概念は既定のページングと同じです。 カスタムのページングを使用して、`ProductBLL`クラス s`GetProductsPaged`メソッド (の代わりに`GetProductsAsPagedDataSource`)。 説明したように、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)チュートリアルでは、`GetProductsPaged`を返す行の開始行インデックスと最大数を渡す必要があります。 これらのパラメーターは、クエリ文字列と同様を介して維持することができます、`pageIndex`と`pageSize`既定のページングに使用されるパラメーター。

S 以降ありません`PagedDataSource`かどうかおよびを介してページングされるレコードの合計数を決定するカスタム ページングは、代替手法を使用する必要がありますお re データの最初と最後のページを表示します。 `TotalNumberOfProducts()`メソッドで、`ProductsBLL`クラスを介してページングされる製品の合計数を返します。 データの最初のページが表示されているかどうか、調べます開始行インデックス場合は 0、最初のページが表示されているし、します。 開始行のインデックスの合計を返す最大行数がより大きいかを介してページングされるレコードの合計数に等しい場合、最後のページが表示されています。

次のチュートリアルより詳細にカスタム ページングを実装することを検討しましょう。

## <a name="summary"></a>まとめ

DataList でも、リピータの出力を提供しています。 DetailsView、GridView でボックス ページングのサポートが検出された、FormView を制御し最小限の労力でこのような機能を追加することができます。 内の製品のセット全体をラップする既定のページングを実装する最も簡単な方法は、`PagedDataSource`し、バインド、 `PagedDataSource` DataList またはリピータにします。 このチュートリアルでは追加、`GetProductsAsPagedDataSource`メソッドを`ProductsBLL`を返すために、`PagedDataSource`です。 `ProductsBLL`クラスには、カスタム ページングのために必要なメソッドが既に`GetProductsPaged`と`TotalNumberOfProducts`です。

カスタム ページングを表示するレコードの正確な設定またはすべてのレコードのいずれかを取得すると共に、`PagedDataSource`既定ページングも必要があります、ページング インターフェイスを手動で追加します。 このチュートリアルで作成した、次に、Previous、最初に、最終インターフェイスの 4 つのボタンの Web コントロールを使用します。 また、現在のページ番号とページの合計数を表示するラベル コントロールが追加されました。

次のチュートリアルでは、DataList と Repeater を並べ替えのサポートを追加する方法が表示されます。 両方ページおよびできる (例と共に既定およびカスタム ページングを使用して並べ替えられて DataList を作成する方法を会いしましょう。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Liz Shulok、Ken Pespisa、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](sorting-data-in-a-datalist-or-repeater-control-cs.md)
