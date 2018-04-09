---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: ページングや並べ替えのレポートのデータ (c#) |Microsoft ドキュメント
author: rick-anderson
description: ページングや並べ替えは、2 つの非常に一般的な機能をオンライン アプリケーションでデータを表示するときにします。 このチュートリアルでは、並べ替えの追加の紹介を移動しますとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 06a907f2af0adb2eb8aef5a814c2d767b62db69a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="paging-and-sorting-report-data-c"></a>ページングや並べ替えのレポートのデータ (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe)または[PDF のダウンロード](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> ページングや並べ替えは、2 つの非常に一般的な機能をオンライン アプリケーションでデータを表示するときにします。 このチュートリアルでは、並べ替えと、レポートでは、それは、将来のチュートリアルに基づいて構築し、ページングを追加することで、最初に表示をみましょう。


## <a name="introduction"></a>はじめに

ページングや並べ替えは、2 つの非常に一般的な機能をオンライン アプリケーションでデータを表示するときにします。 たとえば、何百ものようなブックがある可能性があります、オンライン書店で本を ASP.NET 検索するときに検索結果を一覧表示するレポートには、1 ページあたり 10 個まで一致するが一覧表示します。 さらに、タイトル、価格、ページ数、作成者名、して結果を並べ替えることができます。 23 のチュートリアルは、さまざまなレポートでは、データの削除、追加、編集、およびインターフェイスを含むレポートを作成する方法を確認した過去の while おいないデータとのみを並べ替える方法について見てきました例のページングおを見てきましたが DetailsView FormView とされています。制御します。

このチュートリアルでは並べ替えとページング、これは、いくつかのチェック ボックスをチェックするだけで実現できる、ユーザーがレポートに追加する方法が表示されます。 残念ながら、この簡単な実装は並べ替えインターフェイスが望ましい場合にビットを離れるその欠点を持ち、ページング ルーチンが効率的に大きな結果セットのページングに設計されていません。 今後のチュートリアルは、-、-インボックス ページングや並べ替えソリューションの制限を克服する方法を説明します。

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>手順 1: ページングを追加して、チュートリアルの Web ページの並べ替え

このチュートリアルを始める前に、まずこのチュートリアルと、次の 3 つの必要があります ASP.NET ページを追加する少し時間を取って s を使用できます。 という名前のプロジェクトに新しいフォルダーを作成して開始`PagingAndSorting`です。 次に、すべてのマスター ページを使用するように構成して、このフォルダーに次の 5 つの ASP.NET ページを追加`Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![PagingAndSorting フォルダーを作成し、チュートリアルの ASP.NET ページを追加](paging-and-sorting-report-data-cs/_static/image1.png)

**図 1**: PagingAndSorting フォルダーを作成し、チュートリアルの ASP.NET ページを追加


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン画面上のフォルダーです。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップを列挙し、箇条書きリストの現在のセクションでこれらのチュートリアルが表示されます。


![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](paging-and-sorting-report-data-cs/_static/image2.png)

**図 2**: Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加


箇条書きのページングと並べ替えを作成してチュートリアルを表示するために、サイト マップに追加する必要があります。 開く、`Web.sitemap`ファイルし、編集、挿入、および削除すると、サイト マップ ノードのマークアップの後に、次のマークアップを追加します。


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![マップを更新するサイトに新しい ASP.NET ページを含める](paging-and-sorting-report-data-cs/_static/image3.png)

**図 3**: 新規の ASP.NET ページを含むサイト マップを更新


## <a name="step-2-displaying-product-information-in-a-gridview"></a>手順 2: GridView で製品情報を表示します。

実際にページングや並べ替えの機能を実装する前に、標準的な非-srotable、製品情報を一覧表示する GridView が非ページングをまず作成 s を使用できます。 これは、タスクは行った前に何度もこのチュートリアルの系列全体にわたって、これらの手順を理解する必要があります。 開いて開始、`SimplePagingSorting.aspx`ページし、デザイナーの設定には、ツールボックスから GridView コントロールをドラッグしてその`ID`プロパティを`Products`です。 S ProductsBLL クラスを使用する新しい ObjectDataSource を次に、作成`GetProducts()`をすべての製品情報を返すメソッド。


![すべての GetProducts() メソッドを使用して製品に関する情報を取得します。](paging-and-sorting-report-data-cs/_static/image4.png)

**図 4**: すべての GetProducts() メソッドを使用して製品に関する情報を取得


このレポートは、読み取り専用のレポート、s がないため、ObjectDataSource s をマップする必要があります`Insert()`、 `Update()`、または`Delete()`メソッドを対応する`ProductsBLL`メソッドです。 そのため、(なし) ドロップダウン リストから、更新、挿入、。DELETE タブです。


![選択、挿入、更新プログラムのドロップ ダウン リストでオプションおよびタブを削除する (なし)](paging-and-sorting-report-data-cs/_static/image5.png)

**図 5**: 選択、挿入、更新プログラムのドロップ ダウン リストでオプションおよびタブを削除する (なし)


次に、フィールドをカスタマイズする、GridView s のみ、製品名、仕入先、カテゴリ、価格、および提供が中止された状態が表示されるよう s を使用できます。 さらに、だと自由書式フィールド レベルに変更されると、調整など、`HeaderText`プロパティまたは価格を通貨として書式設定します。 これらの変更後 GridView s の宣言型マークアップを次のようになります。


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。 ページは、各製品の名前、カテゴリ、供給業者、価格を示す 1 つの画面では、製品のすべてを一覧表示し、提供が中止された状態に注意してください。


[![各製品の一覧表示されます。](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**図 6**: 各製品の一覧表示されます ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>手順 3: ページング サポートを追加します。

一覧表示する*すべて*の 1 つの画面で製品のデータを参照するためのユーザーの情報の過負荷につながります。 結果をより管理しやすくするためには、データのサイズの小さいページにデータを分割したり、データを 1 ページずつ処理するアクセス許可できます。 実行するこれだけでページングを有効にするチェック ボックスをオン GridView s のスマート タグから (GridView s 設定[`AllowPaging`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)に`true`)。


[![チェック ボックスを有効にするページング ページング サポートを追加するには](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**図 7**: チェック ボックスを有効にするページング ページング サポートを追加する ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image11.png))


ページごとに表示されるレコードの数を制限し、追加ページングを有効にすると、*ページング インターフェイス*GridView にします。 図 7 に示すように、既定のページング インターフェイスは、一連のページ番号、これにすばやく移動するデータの 1 つのページから別のユーザーです。 このページング、見慣れたインターフェイス、おとしてしたらそれを目に過去のチュートリアルで DetailsView およびフォームのコントロールへのページングのサポートを追加するときにします。

DetailsView とフォーム ビューの両方のコントロールでは、1 ページあたりのレコードを 1 つだけ表示されます。 ただし、GridView を参照してその[`PageSize`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx)1 ページに表示するレコードの数を決定する (このプロパティの値を 10 に既定値)。

この GridView、DetailsView、および FormView s のページングのインターフェイスをカスタマイズするには、次のプロパティを使用します。

- `PagerStyle` ページング インターフェイスのスタイル情報を示しますなどの設定を指定できます`BackColor`、 `ForeColor`、 `CssClass`、`HorizontalAlign`のようにします。
- `PagerSettings` 一連ページング インターフェイスの機能をカスタマイズ可能なプロパティにはが含まれています`PageButtonCount` (既定値は 10) ページング インターフェイスに表示される数値によるページ番号の最大数を示す; [ `Mode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx)ページング インターフェイスの動作し、に設定することを示します。 

    - `NextPrevious` 前後 1 つのページを一度にステップにユーザーを許可する、次へ および 戻るボタンを示しています
    - `NextPreviousFirstLast` 次へ 戻る ボタン、に加えて最初と最後のボタンも含まれています、ユーザーがデータの最初と最後のページにすばやく移動できるようにします。
    - `Numeric` 一連のユーザーがすぐに任意のページに移動できるように、ページ番号を示しています。
    - `NumericFirstLast` ページ番号だけでなくユーザーがデータの最初と最後のページにすばやく移動できるように、最初と最後のボタンが含まれています。最初/最後のボタンは、すべての数値によるページ番号が合わないかどうかのみ表示します。

さらに、GridView、DetailsView、および提供すべて FormView、`PageIndex`と`PageCount`プロパティで、現在のページが表示されていると、データのページの合計数をそれぞれ指定します。 `PageIndex`プロパティは、データの最初のページを表示するときに、つまり、0 から始まるインデックスが`PageIndex`0 になります。 `PageCount`、その一方で、開始 1 の位置、つまり`PageIndex`0 までの値に制限されておよび`PageCount - 1`です。

S の GridView のページング インターフェイスの既定の外観を向上させるためにはしばらく時間かかることができます。 具体的には、右揃えの明るい灰色の背景にページング インターフェイスを持っている s を使用できます。 GridView s によって直接これらのプロパティを設定するのではなく`PagerStyle`プロパティ内の CSS クラスを作成する let s`Styles.css`という名前`PagerRowStyle`し割り当てます、 `PagerStyle` s`CssClass`テーマを使用してプロパティです。 開いて開始`Styles.css`クラス定義を次の CSS を追加するとします。


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

次に、開く、`GridView.skin`ファイルで、`DataWebControls`内のフォルダー、`App_Themes`フォルダーです。 説明したよう、*マスター ページとサイト ナビゲーション*チュートリアル、スキン ファイルは、Web コントロールの既定のプロパティ値を指定するために使用できます。 そのため、設定を含めるには、既存の設定を増やして、 `PagerStyle` s`CssClass`プロパティを`PagerRowStyle`です。 Let s が多くてを使用して 5 つの数値のページ ボタンを表示するページング インターフェイスを構成するさらに、`NumericFirstLast`ページング インターフェイスです。


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>ページングのユーザー エクスペリエンス

図 8 は、ブラウザーからアクセスすると、GridView s ページングを有効にする チェック ボックスがチェックされた後、web ページを示しています。 および`PagerStyle`と`PagerSettings`を介して構成を行って、`GridView.skin`ファイル。 注のみを表示する 10 個のレコードが表示されているし、ページング インターフェイスは、データの最初のページを表示していることを示します。


[![ページングが有効なレコードのサブセットのみが、一度に表示されます。](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**図 8**: ページング有効で、一度にレコードのサブセットのみが表示されます ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image14.png))


ページング インターフェイスのページ番号のいずれかで、ユーザーがクリックするは、ポストバックが引き続き行われ、ページをリロードを要求したページのレコードを表示します。 図 9 では、データの最後のページを表示するオプトインの後、結果を示しています。 最後のページが 1 つのレコードを持ったことに注意してください。合計 8 ページ分の唯一のレコード ページと 1 ページあたり 10 個のレコードの結果として得られる 81 レコードがあるためにです。


[![ページ番号をクリックするとポストバックを発生させるし、レコードの適切なサブセットを示しています。](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**図 9**: ページ番号をクリックするとポストバックを発生させるし、適切なレコードのサブセットを示します ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>ページングのサーバー側ワークフロー

エンドユーザーは、ページング インターフェイスのボタンをクリックするは、ポストバックが引き続き行われ、次のサーバー側ワークフローの開始。

1. GridView s (または DetailsView または FormView)`PageIndexChanging`イベントの起動
2. ObjectDataSource を再要求*すべて*BLL; からのデータの GridView s`PageIndex`と`PageSize`GridView に表示される必要があります、BLL から返されるレコードを決定するプロパティ値が使用されます
3. GridView の`PageIndexChanged`イベントの起動

手順 2. で、ObjectDataSource を再要求のすべてのデータ ソースからデータ。 このスタイルのページングが一般に呼ば*既定ページング*、ので s ページング動作既定では設定時に使用、`AllowPaging`プロパティを`true`です。 レコードのサブセットのみ実際にレンダリングされる HTML に s、ブラウザーに送信される場合でも、既定値は、データの各ページのすべてのレコードを取得単純 Web コントロールのデータのページングです。 データベースのデータは、BLL または ObjectDataSource によってキャッシュは、しない限り、既定のページングが十分に大きな結果セットや多くの同時実行ユーザーによる web アプリケーションの実行可能です

次のチュートリアルで実装する方法を説明します*カスタム ページング*です。 カスタム ページングでのみ、要求されたページのデータに必要なレコードの正確なセットを取得する ObjectDataSource 具体的に指示できます。 想像できるとおり、カスタム ページングは、大きな結果セットのページングの効率を大幅に向上します。

> [!NOTE]
> 既定のページングを適切なは、多数の同時ユーザーに十分に大きな結果セットをまたはサイトをページングするときありません。 が、ことに注意して、カスタム ページング詳細の変更と労力を実装して (既定値は、チェック ボックスをチェックだけではありません。ページング)。 そのため、既定のページングがあります、トラフィックが少ない小規模の web サイトは、比較的小さい結果のページングが設定した場合の理想的な選択肢 s より簡単かつ迅速に実装します。


たとえば、あることも決して 100 を超える製品、データベースのことが分かって、カスタム ページングを楽しんで最小限のパフォーマンスの向上は可能性がありますそれを実装するために必要な作業にオフセット。 ただし、1 日必要に応じて何千もまたは何千もの製品、数万場合は、*いない*アプリケーションのスケーラビリティを大幅に妨害がカスタム ページングを実装します。

## <a name="step-4-customizing-the-paging-experience"></a>手順 4: ページング エクスペリエンスのカスタマイズ

Web コントロールのデータは、さまざまなユーザーのページング エクスペリエンスを強化するために使用できるプロパティを提供します。 `PageCount`プロパティ、たとえば、示します合計ページ数中、`PageIndex`プロパティが参照される現在のページを示す、特定のページにユーザーをすばやく移動設定することができます。 これらのプロパティを使用してユーザーのページング エクスペリエンスの改良、s のラベルを追加する方法を説明するために Web コントロールをページのページをユーザーに通知する、re DropDownList できるコントロールを特定のページに迅速にジャンプすると共に、現在アクセス.

最初に、ページに、ラベルの Web コントロールを追加、設定、`ID`プロパティを`PagingInformation`、クリアとその`Text`プロパティです。 GridView s のイベント ハンドラーを次に、作成`DataBound`イベントし、次のコードを追加します。


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

このイベント ハンドラーを割り当てます、`PagingInformation`ラベル s`Text`プロパティを現在のページは、ページをユーザーに通知メッセージに`Products.PageIndex + 1`数の合計ページ不足`Products.PageCount`(に 1 を追加、`Products.PageIndex`プロパティのため`PageIndex` 0 から始まるインデックスが作成されます)。 このラベルの割り当て を選択した`Text`プロパティに、`DataBound`イベント ハンドラーはなく、`PageIndexChanged`イベント ハンドラーのため、`DataBound`一方、データが GridView にバインドするたびにイベントが発生した、`PageIndexChanged`のみのイベント ハンドラーページ インデックスが変更されたときに発生します。 ときに、GridView は最初に最初のページにバインドされたデータを参照してください、`PageIndexChanging`イベントでは t 火災 (一方、`DataBound`イベントが)。

この追加すると、ユーザーはアクセス ページとデータの合計ページの数を示すメッセージを表示されています。


[![現在のページ番号と合計ページ数が表示されます。](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**図 10**: 現在のページ番号、および合計ページ数が表示されます ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image20.png))


Label コントロールだけでなくも選択されている表示されているページで、GridView のページ番号のリストの DropDownList コントロールを追加 s を使用できます。 ユーザーにすばやく移動できます、現在のページから別の DropDownList から新しいページのインデックスを選択するだけでという考え方です。 DropDownList をデザイナーの設定に追加することによって開始その`ID`プロパティを`PageList`とスマート タグを有効にする AutoPostBack オプションをチェックします。

戻り、`DataBound`イベント ハンドラーを次のコードを追加します。


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

このコードを内の項目をクリアする開始、 `PageList` DropDownList です。 これは、ように見えますが、不要な 1 つと t を変更するには、ページの数を予想するが、他のユーザーのシステムを同時に使用して、追加またはからレコードを削除することがあるので、`Products`テーブル。 このような挿入または削除は、データのページの数を変更可能性があります。

次に、いただくためには、ページ番号をもう一度作成し、現在の GridView にマップされる 1 つ`PageIndex`既定で選択します。 0 ~ ループが発生するために`PageCount - 1`、追加する新しい`ListItem`各イテレーションの設定でその`Selected`プロパティを現在の反復インデックス GridView s に等しい場合に true に`PageIndex`プロパティです。

最後に、DropDownList s のイベント ハンドラーを作成する必要があります`SelectedIndexChanged`イベントで、ユーザーが、一覧から別の項目を選択するたびに起動します。 このイベント ハンドラーを作成するには、デザイナーでの DropDownList をダブルクリックし、次のコードを追加します。


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

図 11 に示す GridView s を変更するだけで`PageIndex`プロパティにより、データを再 GridView にバインドします。 GridView s`DataBound`イベント ハンドラーで適切な DropDownList`ListItem`が選択されています。


[![ユーザーは、6 番目のページと、選択ページ 6 のドロップダウン リストの項目を自動的に実行されます。](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**図 11**: ユーザーは、6 番目のページと、選択ページ 6 のドロップダウン リストの項目を自動的に実行される ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>手順 5: 双方向の並べ替えのサポートを追加します。

GridView s のスマート タグは並べ替えを有効にする オプションを簡単にチェック ページング サポートを追加するだけで双方向の並べ替えのサポートの追加 (GridView s を設定する[`AllowSorting`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)に`true`)。 これの各 GridView のフィールドの見出しとしてレンダリングある、クリックされたときに、ポストバックを発生させるおよび戻り値でクリックした列を昇順で並べ替えられたデータ。 降順でデータを並べ替えます再同じヘッダー LinkButton を再度クリックするとします。

> [!NOTE]
> 型指定されたデータセットではなく、カスタムのデータ アクセス層を使用している場合は、GridView s のスマート タグの並べ替えを有効にするオプションをいないがあります。 ネイティブの並べ替えをサポートするデータ ソースにバインドされた Gridview だけでは、使用可能なこのチェック ボックスがあります。 ADO.NET DataTable が用意されているので、型指定されたデータセットが既定の並べ替えのサポートを提供、`Sort`メソッドを呼び出されると、DataTable s 条件で指定された Datarow を並べ替えます。


場合は、DAL では、DAL によってネイティブに並べ替えにデータを並べ替えるか、データがビジネス ロジック層、並べ替えの情報を渡す ObjectDataSource を構成する必要は並べ替えのサポート オブジェクトは返されません。 今後のチュートリアルでビジネス ロジックでのデータとデータ アクセス レイヤーを並べ替える方法について見ていきましょう。

並べ替えのあるは、ヘッダー行の背景色で衝突してが現在の色 (青、未表示リンクと、表示済みリンクの濃い赤)、HTML のハイパーリンクとしてレンダリングされます。 Let s がかどうかに関係なく、白色で表示されるすべてのヘッダー行のリンクがどのように定義する代わりに、これら ve されてアクセスしたか。 これには、次を追加することによって、`Styles.css`クラス。


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

この構文は、後のクラスを使用する要素内でハイパーリンクを表示するときに、白いテキストを使用することを示します。

この参照 CSS を追加した後、ブラウザーからページを訪問すると、画面のよう図 12 です。 具体的には、図 12 は Price フィールドのヘッダーのリンクがクリックしてされた後に結果を示します。


[![結果を昇順で UnitPrice によって並べ替えられています。](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**図 12**:、結果が並べ替えられている順序の昇順で UnitPrice によって ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>並べ替えのワークフローを確認します。

BoundField、CheckBoxField、TemplateField、フィールドのすべての GridView いてなど、`SortExpression`ヘッダーのリンクを並べ替えフィールド s がクリックされたときにデータの並べ替えに使用される式を示すプロパティです。 GridView があります、`SortExpression`プロパティです。 LinkButton がクリックされたヘッダーと並べ替え、GridView が割り当てられますフィールド s`SortExpression`値をその`SortExpression`プロパティです。 次に、データは、ObjectDataSource から再度取得され、GridView s に従って並び替え`SortExpression`プロパティです。 エンドユーザーが GridView にデータを並べ替えるには、一連の手順の詳細を次に示します。

1. GridView s [Sorting イベントを](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)発生
2. GridView s [ `SortExpression`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)に設定されている、`SortExpression`フィールドの LinkButton がクリックしてされた並べ替えのヘッダー
3. ObjectDataSource が再 BLL からデータをすべて取得し、GridView s を使用してデータを並べ替えます `SortExpression`
4. GridView の`PageIndex`プロパティを 0 にリセットすると、つまりユーザーを並べ替えるときに返されます (ページングのサポートが実装されていると仮定) データの最初のページ
5. GridView s [ `Sorted`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)発生

既定の並べ替えオプションと既定のページング取得再と同じように*すべて*BLL からレコードのです。 ページングせず並べ替えを使用する場合、または並べ替えを使用して既定のページング、s がこのパフォーマンスに影響 (データベースのデータをキャッシュ) が不足を回避する方法がありません。 ただし、今後のチュートリアルで後ほどお見せ、カスタム ページングを使用する場合にデータを効率的に並べ替えることが可能です。

GridView の各フィールドが自動的にバインドするとき、ObjectDataSource GridView s のスマート タグで、ドロップダウン リストを GridView に、その`SortExpression`内のデータ フィールドの名前に割り当てられているプロパティ、`ProductsRow`クラスです。 たとえば、 `ProductName` BoundField s`SortExpression`に設定されている`ProductName`宣言型マークアップを次に示すように、します。


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

フィールドを構成するように、s 並べ替えをクリアするその`SortExpression`(空の文字列を割り当てる) プロパティです。 これを示すためを想像してください。 おでしたたく並べ替える価格、製品、顧客に知らせます。 `UnitPrice` BoundField の`SortExpression`の宣言型マークアップ、または (される GridView s のスマート タグの列の編集リンクをクリックして) フィールド ダイアログ ボックスで、プロパティを削除することができます。


![結果を昇順で UnitPrice によって並べ替えられています。](paging-and-sorting-report-data-cs/_static/image27.png)

**図 13**: 結果を昇順で UnitPrice によって並べ替えられています。


1 回、`SortExpression`のプロパティが削除された、 `UnitPrice` BoundField、それによってユーザーの価格を指定して、データの並べ替え妨げ、リンクではなくテキストとして、ヘッダーがレンダリングされます。


[![SortExpression プロパティを削除すると、ユーザーは不要になった価格の製品を並べ替える](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**図 14**: SortExpression プロパティを削除すると、ユーザーは不要になった価格で製品を並べ替える ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>GridView のプログラムによる並べ替え

並べ替えることもできます GridView のコンテンツ プログラムによって GridView s を使用して、 [ `Sort`メソッド](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)です。 単に渡す、`SortExpression`値と共に並べ替えるには、 [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`または`Descending`)、GridView のデータが再度並べ替えられた、します。

並べ替えを無効にしました、理由を想像してください、`UnitPrice`でした。 お客様は、最低価格の製品のみに単純に購入がないかと心配ためでした。 ただし、d おような製品を最小にでは、価格が最も高い価格からのみ並べ替えできるように、最も負荷の高い製品を購入することをお勧めします。

実行するこの Button Web コントロールを追加 ページで、設定、`ID`プロパティを`SortPriceDescending`、およびその`Text`プロパティ価格で並べ替えをします。 S、ボタンのイベント ハンドラーを次に、作成`Click`イベントをデザイナーでボタン コントロールをダブルクリックします。 このイベント ハンドラーに次のコードを追加します。


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

このボタンをクリックすると製品の価格、最も安価な (参照図 15) に最も負荷の高いものから順に並べ替えて最初のページにユーザーを返します。


[![最も高価なから製品を注文 ボタンをクリックすると、最小](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**図 15**: 製品からの最も負荷の高い、最小注文 ボタンをクリックすると ([フルサイズのイメージを表示するをクリックして](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>まとめ

このチュートリアルでは、既定のページングや並べ替えの機能を実装する方法を説明しましたでどちらもが、チェック ボックスをオンと同じくらい簡単! 並べ替え時またはデータ ページ、類似のワークフローが展開します。

1. ポストバックに陥ります
2. Web コントロールのデータは、イベントの起動を事前レベル (`PageIndexChanging`または`Sorting`)
3. すべてのデータは、ObjectDataSource によって取得再
4. レベルのイベントの起動後データ Web コントロール s (`PageIndexChanged`または`Sorted`)

基本的なページングや並べ替えを実装することはとても簡単ですより効率的なカスタム ページングを使用する、またはページングや並べ替えのインターフェイスをさらに強化するために多くの労力が作用する必要があります。 今後のチュートリアルでは、これらのトピックを調査します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [次へ](efficiently-paging-through-large-amounts-of-data-cs.md)
