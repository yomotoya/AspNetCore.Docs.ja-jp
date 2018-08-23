---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: レポート データ (VB) のページングと並べ替え |Microsoft Docs
author: rick-anderson
description: ページングと並べ替えは、2 つの非常に一般的な機能は、オンライン アプリケーションでデータを表示する場合です。 このチュートリアルで見ていきますを最初に、並べ替えの追加としています.
ms.author: riande
ms.date: 08/15/2006
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2e1cc844122b0fdebbc0be09f88baa11a461ab8e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838658"
---
<a name="paging-and-sorting-report-data-vb"></a>レポート データ (VB) のページングと並べ替え
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe)または[PDF のダウンロード](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> ページングと並べ替えは、2 つの非常に一般的な機能は、オンライン アプリケーションでデータを表示する場合です。 このチュートリアルでは、並べ替えと、レポートでは、今後のチュートリアルの上に構築しページングの追加の紹介をみましょう。


## <a name="introduction"></a>はじめに

ページングと並べ替えは、2 つの非常に一般的な機能は、オンライン アプリケーションでデータを表示する場合です。 たとえば、何百ものこのようなブックがある可能性があります、オンライン書店で ASP.NET に関する書籍を検索するときが検索結果の一覧を表示するレポートには、1 ページあたり 10 個の一致項目が一覧表示されます。 さらに、タイトル、価格、ページ数、作成者名、して結果を並べ替えることができます。 23 のチュートリアルは、さまざまなデータの削除、追加、編集、およびインターフェイスを含め、レポートを構築する方法を確認した過去の中にいないデータとのみを並べ替える方法について見てきました例のページング文言は、DetailsView と FormView を使用されています。コントロール。

このチュートリアルでは、並べ替えとページング、レポートには、いくつかのチェック ボックスをチェックするだけで実現可能を追加する方法がわかります。 残念ながら、この簡単な実装が、並べ替えのインターフェイスが望ましい場合に少し短所の影響と、大きな結果セットを効率的にページング用ページング ルーチンではありません。 今後のチュートリアルは、-、-インボックス ページングと並べ替えのソリューションの制限を克服する方法を学びます。

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>手順 1: ページングを追加して、チュートリアルの Web ページの並べ替え

このチュートリアルを始める前に、最初に実行しなければならないし、このチュートリアルの 3 つの ASP.NET ページを追加するのには少し s を使用できます。 という名前のプロジェクトで新しいフォルダーを作成して開始`PagingAndSorting`します。 次に、次の 5 つの ASP.NET ページをこのフォルダーは、すべてのマスター ページを使用するように構成する追加`Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![PagingAndSorting フォルダーを作成し、チュートリアルの ASP.NET ページを追加します。](paging-and-sorting-report-data-vb/_static/image1.png)

**図 1**: PagingAndSorting フォルダーを作成し、チュートリアルの ASP.NET ページを追加します。


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン サーフェイスにフォルダー。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-vb.md)チュートリアルでは、サイト マップの列挙し、箇条書きリストに現在のセクションでこれらのチュートリアルを表示します。


![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](paging-and-sorting-report-data-vb/_static/image2.png)

**図 2**: Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加


箇条書きのページングと並べ替えのチュートリアルを作成しますを表示するのには、サイト マップに追加する必要があります。 開く、`Web.sitemap`ファイルを開き、編集、挿入、および削除すると、サイト マップ ノード マークアップの後、次のマークアップを追加します。


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![新しい ASP.NET ページは、サイト マップを更新します。](paging-and-sorting-report-data-vb/_static/image3.png)

**図 3**: 新しい ASP.NET ページは、サイト マップを更新


## <a name="step-2-displaying-product-information-in-a-gridview"></a>手順 2: GridView の製品情報を表示します。

ページングと並べ替え機能を実装実際には、前に、標準以外の srotable、製品情報を一覧表示する GridView が非ページングをまず作成 s を使用できます。 これは、タスクは作業が完了したら前に何度もこのチュートリアル シリーズ全体で、これらの手順を理解する必要があります。 開いて開始、`SimplePagingSorting.aspx`ページし、GridView コントロールをドラッグして、デザイナーの設定には、ツールボックスからその`ID`プロパティを`Products`します。 S ProductsBLL クラスを使用する新しい ObjectDataSource を次に、作成`GetProducts()`をすべての製品情報を返すメソッド。


![すべての GetProducts() メソッドを使用して製品に関する情報を取得します。](paging-and-sorting-report-data-vb/_static/image4.png)

**図 4**: GetProducts() メソッドを使用して製品のすべてに関する情報を取得します。


このレポートは読み取り専用のレポート、s が不要であるため、ObjectDataSource s をマップする必要があります`Insert()`、 `Update()`、または`Delete()`メソッドに対応する`ProductsBLL`メソッドです。 そのため、(なし) ドロップダウン リストから、更新、挿入、。タブを削除します。


![選択、挿入、更新プログラムの一覧でオプションを選択し、タブを削除する (なし)](paging-and-sorting-report-data-vb/_static/image5.png)

**図 5**: 選択では、UPDATE、INSERT、ドロップダウン リストでオプションを選択し、タブを削除する (なし)


次に、s のみ、製品名、仕入先、カテゴリ、価格、および提供が中止された状態が表示されるように、GridView のフィールドをカスタマイズすることができます。 さらに、自由に、フィールド レベルの書式設定を行うなどの変更を調整する、`HeaderText`プロパティまたは価格の通貨として書式設定します。 これらの変更後、GridView s の宣言型マークアップは次のようになります。


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。 ページが各製品の名前、カテゴリ、供給業者、価格を示す 1 つの画面では、製品のすべてを一覧表示、ステータスの提供が中止されたことに注意してください。


[![各製品の一覧表示されます。](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**図 6**: 各製品の一覧表示されます ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image8.png))。


## <a name="step-3-adding-paging-support"></a>手順 3: ページング サポートを追加します。

一覧表示する*すべて*の製品の 1 つの画面のデータを参照するためのユーザーの情報のオーバー ロードにつながることができます。 結果をより管理しやすくするためには、データのサイズの小さいページへのデータの分割を一度にデータの 1 つのページをステップにユーザーを許可できます。 実行するこのチェック ボックスをページングを有効にする GridView s のスマート タグから (GridView s 設定[`AllowPaging`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)に`true`)。


[![チェック ボックスを有効にするページング ページング サポートを追加するには](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**図 7**: チェック ボックスを有効にするページング ページング サポートを追加する ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image11.png))。


ページごとに表示されるレコードの数を制限し、追加ページングを有効にすると、*ページング インターフェイス*GridView にします。 図 7 に示すように、既定のページング インターフェイスは、一連のページ番号、ユーザーが別に 1 つのページのデータからすばやく移動できるようにします。 このページング見慣れたインターフェイス、私たちとして ve 過去のチュートリアルで、DetailsView コントロールと FormView コントロールにページング サポートを追加するときに見たこと。

FormView と DetailsView コントロールでは、1 ページあたり 1 つのレコードのみ表示されます。 ただし、GridView を参照してその[`PageSize`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx)1 ページに表示するレコードの数を決定する (このプロパティの値を 10 に既定値)。

この GridView、DetailsView、FormView のページング インターフェイスをカスタマイズするには、次のプロパティを使用します。

- `PagerStyle` ページング インターフェイスのスタイル情報を示しますなどの設定を指定できます`BackColor`、 `ForeColor`、 `CssClass`、`HorizontalAlign`など。
- `PagerSettings` ページング インターフェイスの機能をカスタマイズできるプロパティの多くが含まれています`PageButtonCount` (既定値は 10) ページング インターフェイスに表示される数値によるページ番号の最大数を示す、 [ `Mode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx)ページング インターフェイスの動作し、に設定することを示します。 

    - `NextPrevious` ユーザーが前後 1 つのページを一度にステップできるように、次へ および 前ボタンを示しています
    - `NextPreviousFirstLast` 次へ および 前のボタンだけでなく最初と最後のボタンが含まれています、ユーザーがデータの最初と最後のページにすばやく移動できるようにも
    - `Numeric` 一連のユーザーがすぐに任意のページに移動できるように、ページ番号を示しています。
    - `NumericFirstLast` ページ番号だけでなくユーザーがデータの最初と最後のページにすばやく移動できるように、最初と最後のボタンが含まれています。最初/最後のボタンは、すべての数値によるページ番号に格納できないかどうかのみ表示します。

さらに、GridView、DetailsView、および、すべてのプラン FormView、`PageIndex`と`PageCount`プロパティは、現在のページが表示されていると、データのページの合計数をそれぞれ示します。 `PageIndex`プロパティは、データの最初のページを表示するときに、つまり、0 から始まるインデックスが`PageIndex`0 になります。 `PageCount`、その一方で、開始の 1 からカウント、つまり`PageIndex`は 0 までの値に制限されます、 `PageCount - 1`。

S GridView のページング インターフェイスの既定の外観を向上させるために少しのことができます。 具体的には、s の明るい灰色の背景の右側に整列ページング インターフェイスを持っていることができます。 GridView s から直接これらのプロパティ設定ではなく`PagerStyle`プロパティ内の CSS クラスを作成する let s`Styles.css`という名前の`PagerRowStyle`し割り当てます、 `PagerStyle` s`CssClass`テーマを使用してプロパティ。 開いて開始`Styles.css`クラス定義を次の CSS を追加するとします。


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

次に、開く、`GridView.skin`ファイル、`DataWebControls`内のフォルダー、`App_Themes`フォルダー。 説明したように、*マスター ページとサイト ナビゲーション*チュートリアル、スキン ファイルは、Web コントロールの既定のプロパティ値を指定するために使用できます。 そのため、設定に含める既存の設定を拡張、 `PagerStyle` s`CssClass`プロパティを`PagerRowStyle`します。 Let s が多くてを使用して 5 つの数値のページ ボタンを表示するページング インターフェイスを構成することも、`NumericFirstLast`ページング インターフェイス。


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>ページングのユーザー エクスペリエンス

図 8 は GridView s のページングを有効にするチェック ボックスがチェックされた後、ブラウザーからアクセスしたときに、web ページと`PagerStyle`と`PagerSettings`を通じて構成を行って、`GridView.skin`ファイル。 注のみを表示する 10 個のレコードが表示され、ページング インターフェイスは、データの最初のページを表示することを示します。


[![ページングが有効なレコードのサブセットのみが、一度に表示されます。](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**図 8**: ページングが有効で、一度にレコードのサブセットのみが表示されます ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image14.png))。


ユーザーは、ページング インターフェイスのページ番号のいずれかをクリックすると、ポストバックに陥ります、ページが表示されたページのレコードを要求を再読み込みします。 図 9 では、データの最後のページを表示するオプトインの後、結果を示しています。 最後のページがのみ 1 つのレコードを持つことに注意してください。8 ページ分の唯一のレコードを含むページと 1 ページあたり 10 個のレコードの合計で 81 レコードがあるためにです。


[![ポストバックが発生するページ番号をクリックし、レコードの適切なサブセットを示しています](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**図 9**: ポストバックが発生するページ番号をクリックし、適切なレコードのサブセットを示しています ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image17.png))。


## <a name="paging-s-server-side-workflow"></a>ページングのサーバー側ワークフロー

ページング インターフェイスのボタンをクリックすると、エンドユーザー、ポストバック後し、次のサーバー側のワークフローが開始。

1. GridView s (または DetailsView または FormView)`PageIndexChanging`イベントが発生します
2. ObjectDataSource を再要求*すべて*BLL; からのデータの GridView s`PageIndex`と`PageSize`プロパティの値を使用して GridView に表示される必要があります、BLL から返されるレコードの決定
3. GridView の`PageIndexChanged`イベントが発生します

手順 2 で ObjectDataSource を再要求のすべてのデータ ソースからのデータ。 このスタイルのページングと呼ば*既定のページング*、s ページング動作使用される既定の設定時にほど、`AllowPaging`プロパティを`true`します。 レコードのサブセットのみ実際にレンダリングされる HTML に s、ブラウザーに送信された場合でも、既定値は、データの各ページのすべてのレコードを取得単純 Web コントロールのデータのページングします。 BLL または ObjectDataSource によって、データベースのデータがキャッシュされると、しない限り、既定のページングは十分に大きな結果セットまたは多数の同時ユーザーによる web アプリケーションの機能ではありません。

次のチュートリアルで実装する方法を説明します*カスタム ページング*します。 カスタム ページングを使用したのみ正確なデータの要求されたページに必要なレコード セットを取得する ObjectDataSource を具体的に指示できます。 ご想像のとおり、カスタム ページングが大きな結果セットのページングの効率が大幅に向上します。

> [!NOTE]
> 既定のページングを適切なは、多数の同時ユーザーを十分に大きな結果セット、またはサイトをページングするときありません。 が、実現カスタム ページングの詳細の変更とを実装するための労力が必要ですし、(既定値は、チェック ボックスをチェックするだけではありません。ページング)。 したがって、既定のページングは、低トラフィックの小規模の web サイトまたはので、比較的小規模の結果のページングが設定した場合の理想的な選択肢可能性があります s 簡単かつ迅速に実装します。


などの場合は、データベースに 100 を超える製品があるをしないことがわかっていますカスタム ページングを最小限のパフォーマンスの向上はそれを実装するための労力でオフセット可能性があります。 、ただし、が 1 日がある場合数十万または数万の製品、*いない*、アプリケーションのスケーラビリティを大幅に接続的はカスタム ページングを実装します。

## <a name="step-4-customizing-the-paging-experience"></a>手順 4: ページング操作のカスタマイズ

データ Web コントロールでは、さまざまなページングのユーザーのエクスペリエンスを強化するために使用できるプロパティを提供します。 `PageCount`プロパティなどを示す数の合計ページがありますが中、`PageIndex`プロパティがアクセスされている現在のページを示すし、特定のページにユーザーをすばやく移動に設定することができます。 これらのプロパティを使用して、ユーザーのページングのエクスペリエンスの改良、s のラベルを追加する方法を説明するために Web コントロールをページのページをユーザーに通知する、現在にアクセスして、すぐに、特定のページにジャンプすることを許可する DropDownList コントロールと共に再.

最初に、ページに、ラベルの Web コントロールを追加、設定、`ID`プロパティを`PagingInformation`、クリアとその`Text`プロパティ。 次に、GridView s のイベント ハンドラーを作成`DataBound`イベントし、次のコードを追加します。


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

このイベント ハンドラーを割り当てます、`PagingInformation`ラベル s`Text`プロパティが現在アクセスして、ページをユーザーに通知メッセージに`Products.PageIndex + 1`合計ページ数から`Products.PageCount`(に 1 を追加、`Products.PageIndex`プロパティのため`PageIndex` 0 から始まるインデックスが作成されます)。 このラベルの割り当て を選択した`Text`プロパティ、`DataBound`イベント ハンドラーではなく、`PageIndexChanged`イベント ハンドラーのため、`DataBound`イベントが発生する一方、データが GridView にバインドされるたびに、`PageIndexChanged`のみのイベント ハンドラーページ インデックスが変更されたときに発生します。 ときに GridView が最初に最初のページにバインドされたデータを参照してください、`PageIndexChanging`イベントが t fire (一方、`DataBound`イベント)。

これにより、ユーザーは、アクセスしているどのようなページとデータの合計数のページは、ことを示すメッセージを表示されています。


[![現在のページ番号と合計ページ数が表示されます。](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**図 10**:、現在のページ番号と合計ページ数が表示されます ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image20.png))。


に加えて、ラベル コントロールも選択されている、現在表示されているページで、GridView でページ番号のリストの DropDownList コントロールを追加して使用できます。 ユーザーにすばやくジャンプできます現在のページから別、DropDownList から新しいページのインデックスを選択するだけでという考え方です。 DropDownList をデザイナーの設定に追加して、開始、`ID`プロパティを`PageList`のスマート タグから AutoPostBack を有効にするオプションをオンにするとします。

戻り、`DataBound`イベント ハンドラーを次のコードを追加します。


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

内の項目を消去するでこのコードは、まず、 `PageList` DropDownList します。 1 つと t を変更するページの数を予想されるが、他のユーザーのシステムを同時に使用して、追加またはからレコードを削除する場合があるため、余分なこの見えます、`Products`テーブル。 このような挿入または削除は、データのページ数を変更可能性があります。

次に、ページ番号をもう一度作成して、現在 GridView にマップする必要があります`PageIndex`既定で選択します。 0 ~ ループが発生するために`PageCount - 1`、新しい追加`ListItem`各イテレーションの設定でその`Selected`プロパティを現在の反復インデックス GridView 秒に等しい場合に true に`PageIndex`プロパティ。

最後に、DropDownList s のイベント ハンドラーを作成する必要があります`SelectedIndexChanged`イベントで、ユーザーの一覧から別の項目を選択するたびに発生します。 このイベント ハンドラーを作成するには、デザイナーで DropDownList をダブルクリックし、次のコードを追加します。


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

図 11 に示す GridView s を変更するだけで`PageIndex`プロパティにより、データを GridView に再バインドできます。 GridView s`DataBound`イベント ハンドラーでは、適切な DropDownList`ListItem`が選択されています。


[![ユーザーは、6 番目のページと、選択ページ 6 のドロップダウン リストの項目を自動的に実行](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**図 11**: ユーザーは、6 番目のページと、選択ページ 6 のドロップダウン リストの項目を自動的に実行 ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image23.png))。


## <a name="step-5-adding-bi-directional-sorting-support"></a>手順 5: 双方向の並べ替えのサポートを追加します。

GridView のスマート タグから並べ替えを有効にするオプションをオンに双方向の並べ替えのサポートがページング サポートを追加するだけを追加するだけです (GridView s 設定[`AllowSorting`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)に`true`)。 これは、各 GridView のフィールドのヘッダーとしてレンダリング Linkbutton、クリックされたときに、ポストバックを発生させるおよびによってクリックされた列を昇順で並べ替えられたデータを返します。 降順でデータを並べ替えます再もう一度同じヘッダー LinkButton をクリックします。

> [!NOTE]
> 型指定されたデータセットではなく、カスタム データ アクセス層を使用している場合は、GridView s のスマート タグの並べ替えを有効にするオプションをしないがあります。 Gridview の並べ替えをネイティブでサポートするデータ ソースにバインドされているだけでは、このチェック ボックスを使用可能ながあります。 ADO.NET DataTable が用意されているので、型指定されたデータセットがボックスの並べ替えのサポートを提供します、`Sort`メソッドを呼び出されると、DataTable の Datarow を指定した条件を使用してを並べ替えます。


場合は、DAL では、DAL によってネイティブにサポートの並べ替えには、データを並べ替えるか、またはデータがビジネス ロジック層、並べ替えの情報を渡す ObjectDataSource を構成する必要が並べ替えられているオブジェクトは返されません。 今後のチュートリアルでは、ビジネス ロジックでデータおよびデータ アクセス層を並べ替える方法について説明します。

並べ替えの Linkbutton は、ヘッダー行の背景色と衝突している現在の色 (青、未表示リンクにアクセスしたリンクの濃い赤)、HTML ハイパーリンクとしてレンダリングされます。 代わりに、白、かどうかに関係なくすべてのヘッダー行のリンクのある let s、ve されているかどうかを表示しました。 これには、次を追加することで実現できます、`Styles.css`クラス。


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

この構文は、後のクラスを使用する要素内でハイパーリンクを表示するときに、白いテキストを使用することを示します。

この CSS 追加した後、ブラウザーでページにアクセスして、画面よう図 12 になる必要があります。 具体的には、図 12 は、価格フィールドのヘッダーのリンクがクリックしてされた後、結果を示します。


[![結果を昇順で UnitPrice によって並べ替えられています。](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**図 12**: The 結果並べ替えられた順序の昇順で UnitPrice によって ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image26.png))。


## <a name="examining-the-sorting-workflow"></a>並べ替えのワークフローを調べる

GridView のすべてのフィールド、BoundField、CheckBoxField、TemplateField などが、`SortExpression`フィールド s 並べ替えのヘッダーのリンクがクリックされたときに、データの並べ替えに使用する式を示すプロパティです。 GridView があります、`SortExpression`プロパティ。 LinkButton がクリックされたヘッダーと並べ替え、GridView ではそのフィールドの s が割り当てられます`SortExpression`値をその`SortExpression`プロパティ。 データが次に、ObjectDataSource から再取得し、GridView s に従って並び替え`SortExpression`プロパティ。 エンドユーザーを GridView にデータの並べ替え時に、一連の手順の詳細を次に示します。

1. GridView s [Sorting イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)起動
2. GridView s [ `SortExpression`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)に設定されている、`SortExpression`フィールドの並べ替え LinkButton がクリックしてされたヘッダー
3. ObjectDataSource は再 BLL からデータをすべてを取得し、GridView s を使用してデータを並べ替える `SortExpression`
4. GridView の`PageIndex`(ページングのサポートが実装されていると仮定) のデータの最初のページに返されるプロパティが 0 にリセット、ユーザーを並べ替えるときに、つまり
5. GridView s [ `Sorted`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)起動

既定のページングを使用した既定の並べ替えオプションが再度を取得するよう*すべて*BLL からのレコードの。 ページングなしの並べ替えを使用する場合、または並べ替えを使用する場合は、このパフォーマンスに (場合を除き、データベースのデータをキャッシュする) 影響を回避する方法はありませんが s、ページングを既定します。 ただし、今後のチュートリアルに表示されるようにカスタム ページングを使用する場合に効率的にデータを並べ替えることができます。

GridView の各フィールドが自動的には、ObjectDataSource を GridView s のスマート タグで、ドロップダウン リストを GridView にバインドするとき、`SortExpression`内のデータ フィールドの名前に割り当てられているプロパティ、`ProductsRow`クラス。 たとえば、 `ProductName` BoundField s`SortExpression`に設定されている`ProductName`の次の宣言型マークアップに示しますように。


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

フィールドを構成するように、s の並べ替えをクリアするその`SortExpression`プロパティ (空の文字列に割り当てる)。 これを示すためを想像してください。 私たちでしたたくはありません、お客様の価格を、マイクロソフトの製品を並べ替えできるようにします。 `UnitPrice` BoundField の`SortExpression`宣言型マークアップまたは (つまりアクセス GridView s のスマート タグで列の編集リンクをクリックして) フィールド ダイアログ ボックスで、プロパティを削除できます。


![結果を昇順で UnitPrice によって並べ替えられています。](paging-and-sorting-report-data-vb/_static/image27.png)

**図 13**: 結果を昇順で UnitPrice によって並べ替えられています。


1 回、`SortExpression`のプロパティが削除された、 `UnitPrice` BoundField、それによってユーザーの価格で、データの並べ替え妨げをリンクとしてではなくテキストとして、ヘッダーが表示されます。


[![SortExpression プロパティを削除すると、ユーザーは不要になった価格で製品を並べ替える](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**図 14**: SortExpression プロパティを削除すると、ユーザーは不要になった製品で価格を並べ替える ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image30.png))。


## <a name="programmatically-sorting-the-gridview"></a>GridView をプログラムで並べ替え

並べ替えることも、GridView の内容プログラムで GridView s を使用して[`Sort`メソッド](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)します。 渡すだけです、`SortExpression`と共に並べ替える値、 [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`または`Descending`)、GridView のデータが再度並べ替えたされます。

並べ替えを無効にしました、理由、imagine、`UnitPrice`のため、お客様が最も低価格の製品のみを単純に購入がないかと心配でしたが。 ただし、d ように並べ替えるには、製品、価格が最も高い価格からのみが最も低いことができるように、最も負荷の高い製品を購入することをお勧めします。

ページにボタンの Web コントロールを追加これを実現する設定、`ID`プロパティを`SortPriceDescending`とその`Text`価格による並べ替えのプロパティ。 S ボタンのイベント ハンドラーを次に、作成`Click`イベントで、デザイナーでボタン コントロールをダブルクリックします。 このイベント ハンドラーに次のコードを追加します。


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

このボタンをクリックすると、製品の価格は、最もコストのかからない (図 15 参照) に最も負荷の高いものから順に並べ替えて最初のページにユーザーを返します。


[![最も負荷の高い製品の注文 ボタンをクリックすると、最小](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**図 15**: 製品から、最も負荷の高い、最小の注文 ボタンをクリックして ([フルサイズの画像を表示する をクリックします](paging-and-sorting-report-data-vb/_static/image33.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、既定のページングと並べ替え機能を実装する方法を説明しましたでどちらもが簡単にチェック ボックスをチェックできます! ユーザーは、並べ替え、データをページまたは、ときに、色付けされていく様子のようなワークフロー。

1. ポストバック後します。
2. データ Web コントロール s 事前レベルのイベントが発生します (`PageIndexChanging`または`Sorting`)
3. ObjectDataSource してすべてのデータを再取得します。
4. データ Web コントロール s 後レベルのイベントが発生します (`PageIndexChanged`または`Sorted`)

基本的なページングと並べ替えの実装は簡単ですが、多くの労力はより効率的なカスタム ページングを使用する、またはさらに、ページングまたは並べ替えのインターフェイスを強化するために用いる必要があります。 今後のチュートリアルでは、これらのトピックを紹介します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](creating-a-customized-sorting-user-interface-cs.md)
> [次へ](efficiently-paging-through-large-amounts-of-data-vb.md)
