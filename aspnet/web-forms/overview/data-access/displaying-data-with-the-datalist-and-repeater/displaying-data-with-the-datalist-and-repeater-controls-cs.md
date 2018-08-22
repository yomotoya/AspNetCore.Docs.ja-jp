---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: DataList と Repeater コントロール (c#) によるデータの表示 |Microsoft Docs
author: rick-anderson
description: 上記のチュートリアルでは、データを表示する GridView コントロールを使いましたが。 以降このチュートリアルでは、一般的なレポートを使用してパターンを構築を見ています.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: a58a9501a546a437b44e078c628d7db010700b5c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827182"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>DataList と Repeater コントロール (c#) によるデータの表示
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe)または[PDF のダウンロード](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> 上記のチュートリアルでは、データを表示する GridView コントロールを使いましたが。 以降このチュートリアルでは、説明、DataList と Repeater コントロールで共通のレポート パターンの構築これらのコントロールでデータの表示の基礎を開始します。


## <a name="introduction"></a>はじめに

すべての全体にわたって過去の例で 28 チュートリアルでは、GridView コントロールをここになっているデータ ソースから複数のレコードを表示する必要がある場合。 GridView は、列に、レコードのデータ フィールドを表示する行をデータ ソース内の各レコードを表示します。 GridView により、表示、並べ替え、編集、およびデータの削除をページに、外観が少し角張ったです。 さらに、担当のマークアップ GridView s 構造体が容量固定を含む、HTML`<table>`テーブル行を持つ (`<tr>`) の各レコードは、表のセル (`<td>`) の各フィールド。

複数のレコードを表示するときに、高いレベルで出力されるマークアップと外観のカスタマイズを提供するには、ASP.NET 2.0 は、DataList と Repeater コントロールを提供しています (どちらも ASP.NET のバージョンで使用できたも 1.x)。 DataList と Repeater コントロールでは、BoundFields CheckBoxFields、ButtonFields ではなく、テンプレートを使用して、コンテンツをレンダリングし、します。 HTML としてレンダリングを DataList、GridView のような`<table>`、一方を複数のデータ ソースのレコードをテーブルの行ごとに表示します。 その一方で、Repeater が何を明示的に指定、および最適な候補は、出力されるマークアップを正確に制御する必要がある場合よりもマークアップを追加レンダリングせずします。

次に数十個のチュートリアルを見て、DataList と Repeater コントロールで共通のレポート パターンの構築以降では、これらのコントロール テンプレートでデータの表示の基本です。 これらのコントロールを書式設定方法を見ていきますを編集し、データを削除するには、DataList、マスター/詳細の一般的なシナリオ、方法でデータ ソースのレコードのレイアウトを変更する方法、レコードを閲覧する方法。

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>手順 1: DataList と Repeater チュートリアル Web ページの追加

このチュートリアルを始める前に、このチュートリアルと DataList と Repeater を使用してデータの表示を処理する次のいくつかのチュートリアルの必要があります、ASP.NET ページを追加する最初に少し s を使用できます。 という名前のプロジェクトで新しいフォルダーを作成して開始`DataListRepeaterBasics`します。 次に、次の 5 つの ASP.NET ページをこのフォルダーは、すべてのマスター ページを使用するように構成する追加`Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![DataListRepeaterBasics フォルダーを作成し、チュートリアルの ASP.NET ページを追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**図 1**: 作成、`DataListRepeaterBasics`フォルダー チュートリアル ASP.NET ページを追加


開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン サーフェイスにフォルダー。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップの列挙し、箇条書きリストに現在のセクションから、チュートリアルを表示します。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))。


箇条書きリストに表示するために作成します、DataList と Repeater チュートリアル必要がありますサイト マップに追加します。 開く、`Web.sitemap`ファイルを開き、カスタム ボタンの追加サイト マップ ノードのマークアップの後に、次のマークアップを追加します。


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![新しい ASP.NET ページは、サイト マップを更新します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**図 3**: 新しい ASP.NET ページは、サイト マップを更新


## <a name="step-2-displaying-product-information-with-the-datalist"></a>手順 2: DataList で製品情報を表示します。

テンプレートではなく BoundFields、CheckBoxFields、具合に依存 DataList コントロールの出力にレンダリングされますをフォーム ビューと同様に、します。 FormView とは異なり、DataList は単独の 1 つではなく、レコードのセットを表示する設計されています。 S で製品情報を DataList にバインドを参照してください、このチュートリアルを開始することができます。 開いて開始、`Basics.aspx`ページで、`DataListRepeaterBasics`フォルダー。 次に、DataList をツールボックスからデザイナーにドラッグします。 DataList s のテンプレートを指定する前に、図 4 に示すように、デザイナーで、灰色のボックスとして表示にします。


[![DataList をツールボックスからデザイナーにドラッグします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**図 4**: DataList から、ツールボックスに、デザイナーをドラッグします ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))。


スマート タグの DataList s から、新しい ObjectDataSource を追加しを使用するように構成、`ProductsBLL`クラスの`GetProducts`メソッド。 ウィザード s INSERT (なし) をドロップダウン リストを設定では、このチュートリアルでは、読み取り専用 DataList を作成しますので、更新、およびタブを削除します。


[![新しい ObjectDataSource を作成することを選択します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**図 5**: 新しい ObjectDataSource を作成すること ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))。


[![ProductsBLL クラスを使用する ObjectDataSource を構成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**図 6**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))。


[![すべての GetProducts メソッドを使用して製品に関する情報を取得します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**図 7**: 取得情報に関するすべての製品を使用して、`GetProducts`メソッド ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))。


ObjectDataSource を構成すると、スマート タグを DataList に関連付ける、Visual Studio が自動的に作成、`ItemTemplate`名前とデータ ソースによって返される各データ フィールドの値を表示する DataList で (を参照してください、次のマークアップ)。 この既定`ItemTemplate`外観が自動的に作成、FormView、デザイナーを使用するデータ ソースをバインドするときにテンプレートの場合と同じです。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> FormView s のスマート タグを FormView コントロールをデータ ソースをバインドするときに Visual Studio によって作成されたことを思い出してください、 `ItemTemplate`、 `InsertItemTemplate`、および`EditItemTemplate`します。 ただし、DataList でのみ、`ItemTemplate`が作成されます。 これは、DataList に同じ組み込み編集と、フォーム ビューで提供されるサポートの挿入があるないためにです。 DataList には編集および削除に関連のイベントにが含まれて編集および削除のサポートでく少しコードが存在 s のない単純なボックスのサポートを追加として、FormView で。 含める編集および今後のチュートリアルでは、DataList でサポートを削除する方法を見ていきます。


このテンプレートの外観を向上させるために少し s を使用できます。 すべてのデータ フィールドを表示するのではなくのみ製品の名前、仕入先、カテゴリ、単位、および単価ごとの数を表示する秒を使用できます。 Let s がさらに、内の名前を表示、`<h4>`見出しし、その他のフィールドを使用して、レイアウト、`<table>`見出しの下にします。

DataList タグは、テンプレートの編集リンクをクリックしてまたはページの宣言構文から手動でテンプレートを変更するにはスマート s から、デザイナーの機能を編集するテンプレートを使用するかを変更することができます。 デザイナーでテンプレートの編集オプションを使用する場合、結果として得られるマークアップで、次のマークアップを完全に一致しない可能性がありますが、ブラウザーのスクリーン ショット、図 8 に示すとよく似ていますはずで表示した場合。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> 使用して上記の例のラベル Web コントロールが`Text`プロパティには、データ バインディング構文の値が割り当てられます。 または、でしたを省略したラベル、データ バインド構文だけを入力します。 つまり、使用する代わりに`<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`宣言の構文を代わりに使用`<%# Eval("CategoryName") %>`します。


ラベルの Web コントロールのままにして、ただし、2 つの利点を提供します。 最初に、次のチュートリアルで表示されるように、データに基づくデータを書式設定するための簡単な手段を提供します。 次に、デザイナーは t 表示宣言型データ バインディング構文で、テンプレートの編集オプション表示される一部の Web コントロールの外部でします。 代わりに、テンプレートの編集インターフェイスは作業 static のマークアップを容易に設計されていて、Web を制御し、任意のデータ バインディングを Web コントロールのスマート タグからアクセス可能である DataBindings の編集 ダイアログ ボックスで実行することを前提としています。

そのため、デザイナーを使用して、テンプレートの編集のオプションを提供すると、DataList を使用する場合は、コンテンツは、テンプレートの編集インターフェイスを通じてアクセスできるように、ラベルの Web コントロールを使用する優先します。 間もなく表示されるよう、Repeater は、ソース ビューから、テンプレート コンテンツを編集することが必要です。 そのため、書式設定する必要がありますがわからないコントロールを Label Web 多くの場合は省略します Repeater のテンプレートを作成する際に、データの外観はプログラム ロジックに基づくテキストにバインドされます。


[![各製品の出力は、DataList の ItemTemplate を使用してレンダリング](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**図 8**: 各製品の出力は、DataList s を使用してレンダリング`ItemTemplate`([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))。


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>手順 3: DataList の外観を向上させる

、GridView のような、DataList は数多くのスタイル関連のプロパティをなど`Font`、 `ForeColor`、 `BackColor`、 `CssClass`、 `ItemStyle`、 `AlternatingItemStyle`、`SelectedItemStyle`など。 内のスキン ファイルを作成した GridView および DetailsView コントロールを使用する場合、`DataWebControls`の定義済みテーマ、`CssClass`これら 2 つのコントロールのプロパティと`CssClass`そのサブプロパティのいくつかのプロパティ (`RowStyle`、`HeaderStyle`など)。 S、DataList に同じ処理を行うことができます。

説明したように、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)スキン ファイルを Web コントロールの既定の外観に関連するプロパティを指定しますテーマを定義する、スキン、CSS、イメージ、および JavaScript ファイルのコレクションは、チュートリアル。web サイトの特定ルック アンド フィールです。 *、ObjectDataSource でデータを表示する*チュートリアルでは、作成した、`DataWebControls`テーマ (は内のフォルダーとして実装されている、`App_Themes`フォルダー) を持つ、現時点では、2 つのスキン ファイル`GridView.skin`と`DetailsView.skin`. S サード DataList の事前定義されたスタイルの設定を指定する、スキン ファイルを追加することができます。

スキン ファイルを追加するを右クリックし、`App_Themes/DataWebControls`フォルダーが、新しい項目の追加を選択し、一覧からスキン ファイル オプションを選択します。 そのファイルに `DataList.skin` という名前を付けます。


[![DataList.skin をという名前の新しいスキン ファイルを作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**図 9**: 新しいスキン ファイルの名前付き作成`DataList.skin`([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))。


次のマークアップを使用して、`DataList.skin`ファイル。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

これらの設定は、GridView および DetailsView コントロールで使用されていた同じ CSS クラスを適切なプロパティに割り当てます。 ここで使用される CSS クラス`DataWebControlStyle`、 `AlternatingRowStyle`、`RowStyle`などで定義されて、`Styles.css`ファイルし、前のチュートリアルが追加されました。

このスキン ファイルの追加により、(; [表示] メニューから新しいスキン ファイルの効果を確認、更新を選択するデザイナー ビューを更新する必要があります)、デザイナー、DataList 外観が更新されます。 図 10 に示すよう各代替製品が薄いピンク色の背景色にします。


[![DataList.skin をという名前の新しいスキン ファイルを作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**図 10**: 新しいスキン ファイルの名前付き作成`DataList.skin`([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))。


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>手順 4: DataList s 他のテンプレートの表示

加え、 `ItemTemplate`DataList は、その他の省略可能な 6 つのテンプレートをサポートしています。

- `HeaderTemplate` 指定した場合、出力にヘッダー行を追加し、この行を表示するために使用
- `AlternatingItemTemplate` 交互の項目を表示するために使用
- `SelectedItemTemplate` 選択された項目を表示するために使用選択した項目は、DataList s にインデックスが対応項目[`SelectedIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` 編集中の項目を表示するために使用
- `SeparatorTemplate` 指定した場合、各項目間の区切り記号を追加し、この区切り記号を表示するために使用
- `FooterTemplate` -指定した場合、出力にフッター行を追加し、この行を表示するために使用

指定するときに、`HeaderTemplate`または`FooterTemplate`DataList、表示される出力に追加のヘッダーまたはフッター行を追加します。 ように GridView のヘッダーとフッターを行、ヘッダーおよびフッター datalist にバインドされていないデータ。 そのため、データ バインド構文で、`HeaderTemplate`または`FooterTemplate`にバインドされたデータにアクセスしようとは、空の文字列を返します。

> [!NOTE]
> 説明したように、 [GridView のフッターに概要情報を表示する](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)チュートリアルでは、ヘッダーとフッター行のない t サポート データ バインディング構文をデータに固有の情報からこれらの行に直接挿入することができます、GridView の`RowDataBound`イベント ハンドラー。 この手法に使用できる実行中の合計を計算する両方またはその他の情報、データから、コントロールにバインドされているだけでなく、フッターにその情報を割り当てます。 これと同じ考え方は、DataList と Repeater コントロールに適用できます。DataList と Repeater のイベント ハンドラーを作成する唯一の違いは、`ItemDataBound`イベント (の代わりの`RowDataBound`イベント)。


この例では let s したタイトルが DataList の結果の上部に表示される製品情報、`<h3>`見出し。 これを行うには、追加、`HeaderTemplate`を適切なマークアップ。 これは、デザイナーでは、DataList s のスマート タグのテンプレートの編集リンクをクリックすると、ドロップダウン リストからヘッダーのテンプレートを選択するスタイルのドロップダウン リストから 3 の見出しのオプションを選択した後のテキストを入力して実行できます (図 11 を参照してください) を一覧表示します。


[![テキストの製品情報を使って HeaderTemplate を追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**図 11**: 追加、`HeaderTemplate`テキストの製品情報 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))。


また、これは追加できます宣言内で次のマークアップを入力して、`<asp:DataList>`タグ。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

各製品の一覧の間にスペースを追加するには、s を追加できるように、`SeparatorTemplate`各セクションの間に行が含まれます。 水平線タグ (`<hr>`)、このような区分線を追加します。 作成、`SeparatorTemplate`次のマークアップがあるようにします。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> ように、`HeaderTemplate`と`FooterTemplates`、`SeparatorTemplate`データ ソースから任意のレコードにバインドされていないと、そのため、データ ソース、DataList にバインドされているレコードへのアクセスを直接ことはできません。


この参照を追加したら、ブラウザーを使用してページを表示するときに図 12 ようなります。 ヘッダー行と各製品の一覧の間の線に注意してください。


[![DataList には、ヘッダー行と各製品の一覧の間で水平方向の規則が含まれます。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**図 12**: DataList には、ヘッダー行と、水平方向の規則の間で各 Product Listing が含まれています ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))。


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>手順 5: Repeater コントロールで特定のマークアップのレンダリング

図 12 から DataList 例にアクセスしたときに、お使いのブラウザーから/ソースの表示を行う場合、DataList が HTML を出力が表示されます`<table>`テーブルの行を格納している (`<tr>`) に 1 つのテーブル セル (`<td>`) にバインドされた各項目に対して、DataList します。 実際には、この出力は、単一 TemplateField に GridView からどのような出力と同じです。 今後のチュートリアルに表示されるよう、DataList でカスタマイズを許可さらに、出力のテーブルの行ごとの複数のデータ ソースのレコードを表示できるようになりました。

HTML を生成したくない場合`<table>`できたでしょうか。 データ Web コントロールによって生成されたマークアップの合計で、完全な制御は、Repeater コントロールを使用して必要があります。 DataList のように、テンプレートに基づいて、Repeater が構築されます。 Repeater、ただし、のみには次の 5 つのテンプレート。

- `HeaderTemplate` 指定した場合、項目の前に、指定したマークアップを追加します。
- `ItemTemplate` 項目を表示するために使用
- `AlternatingItemTemplate` 交互の項目を表示するために使用される指定した場合、
- `SeparatorTemplate` 指定した場合、各項目の間、指定したマークアップを追加します。
- `FooterTemplate` -指定した場合、項目の後、指定したマークアップを追加します。

Asp.net 1.x では、Repeater コントロールでは、いくつかのデータ ソースからデータが付属して箇条書きリストを表示するために使用が一般的です。 このような場合は、`HeaderTemplate`と`FooterTemplates`開始タグと終了が含まれます`<ul>`タグはそれぞれ、while、`ItemTemplate`が含まれます`<li>`データ バインディング構文を持つ要素。 2 つの例で説明したように、この方法を ASP.NET 2.0 の使用ができます、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアル。

- `Site.master`マスター ページで、Repeater を使いました箇条書き (基本的なレポート、レポートのフィルター処理、カスタマイズされた書式設定、およびなど) は、最上位サイト マップの内容の一覧を表示する; 別、入れ子になった Repeater の子セクションでは、表示に使用された、最上位レベルのセクションでは
- `SectionLevelTutorialListing.ascx`Repeater がの現在のサイト マップ セクションの子セクションでは、箇条書きリストを表示するために使用

> [!NOTE]
> ASP.NET 2.0 が導入されていますが、新しい[BulletedList コントロール](https://msdn.microsoft.com/library/ms228101.aspx)、する単純な箇条書きリストを表示するためにデータ ソース コントロールにバインドできます。 BulletedList コントロールで必要はありません。 リストに関連する HTML のいずれかを指定するには代わりに、各リスト項目のテキストとして表示するデータ フィールド指定だけです。


Repeater では、すべてのデータ Web コントロール catch として機能します。 必要なマークアップを生成する既存のコントロールがない場合は、Repeater コントロールを使用できます。 Repeater を使用して示すためには、s は手順 2. で作成した製品情報 DataList 上に表示されるカテゴリの一覧があることができます。 具体的には、let s が単一行の HTML で表示されるカテゴリをある`<table>`各カテゴリが、テーブル内の列として表示されます。

これを実現するには、Repeater コントロールをツールボックスから、デザイナーの 製品情報 DataList 上にドラッグして開始します。 DataList と Repeater 最初に表示されます灰色のボックスとしてそのテンプレートが定義されるまでです。


[![Repeater をデザイナーに追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**図 13**: Repeater をデザイナーに追加 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))。


Repeater s のスマート タグのオプションを 1 つだけある s: データ ソースを選択します。 新しい ObjectDataSource を作成および使用するように構成することを選択、`CategoriesBLL`クラスの`GetCategories`メソッド。


[![新しい ObjectDataSource を作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**図 14**: 新しい ObjectDataSource を作成 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))。


[![CategoriesBLL クラスを使用する ObjectDataSource を構成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**図 15**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))。


[![すべてのメソッドを使用してカテゴリに関する情報を取得します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**図 16**: 取得情報に関するすべてのカテゴリを使用して、`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))。


DataList とは異なり Visual Studio は自動的に作成されません、ItemTemplate、Repeater のデータ ソースにバインドした後。 さらに、Repeater のテンプレートは、デザイナーでは構成できず、宣言によって指定する必要があります。

単一行として、カテゴリを表示する`<table>`の各カテゴリの列に、Repeater、次のようなマークアップを生成する必要があります。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

以降、`<td>Category X</td>`テキストはこの Repeater の ItemTemplate に表示されます、繰り返される部分です。 その前に表示されるマークアップ`<table><tr>`-に配置されます、`HeaderTemplate`終了のマークアップの中に`</tr></table>`-に配置されますが、`FooterTemplate`します。 これらのテンプレート設定を入力してには、、次の構文型の左下隅の [ソース] ボタンでクリックして、ASP.NET ページの宣言型の部分に移動します。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Repeater では、そのテンプレート、それ以上、何も以下で指定された正確なマークアップを出力します。 図 17 では、ブラウザーで表示したときに、Repeater s の出力を示します。


[![単一行の HTML&lt;テーブル&gt;別の列に各カテゴリを一覧表示](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**図 17**: A 単一行の HTML`<table>`別の列の各カテゴリの一覧表示されます ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))。


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>手順 6: Repeater の外観を向上させる

Repeater では、そのテンプレートで指定されたマークアップを正確には出力、ために、信じない取得するように、Repeater のスタイル関連のプロパティはありません。 リピータによって生成されるコンテンツの外観を変更するには、ここには、手動で Repeater のテンプレートに直接必要な HTML や CSS のコンテンツを追加する必要があります。

たとえば、s のように、DataList で交互の行の代わりに背景色、カテゴリ列があることができます。 これを実現する必要がありますを割り当てる、 `RowStyle` Repeater の各項目 CSS クラス、`AlternatingRowStyle`を通じて各代替 Repeater 項目 CSS クラス、`ItemTemplate`と`AlternatingItemTemplate`テンプレートでは、次のよう。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

ヘッダー行をテキストの製品カテゴリの出力に追加することも s を使用できます。 ありませんのでがわからない列の数、その結果`<table>`で構成されていますのすべての列にまたがることが保証されるヘッダー行を生成する最も簡単な方法は、使用する*2 つ*`<table>`秒。 最初の`<table>`ヘッダー行と 2 つ目は、単一行を含む行に 2 つの行を含む`<table>`システム内の各カテゴリ列を持ちます。 つまり、次のマークアップを生成します。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

次`HeaderTemplate`と`FooterTemplate`必要なマークアップが発生します。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

図 18 は、これらの変更が行われた後に、Repeater を示します。


[![カテゴリ列を選択し、背景色で交互にヘッダー行が含まれています](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**図 18**:、カテゴリ列代替背景色でヘッダー行が含まれます ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))。


## <a name="summary"></a>まとめ

GridView コントロールは、簡単に表示、編集、削除、並べ替え、および、データのページ、外観は非常に角張ったでグリッドのようにします。 外観をより細かく制御は、DataList または Repeater コントロールを有効にする必要があります。 これらのコントロールは、一連の BoundFields や CheckBoxFields、代わりにテンプレートを使用してレコードを表示します。

DataList は、HTML としてレンダリング`<table>`既定では、表示する各データ ソースのレコードで 1 つ TemplateField GridView と同様、1 つのテーブルの行にします。 今後のチュートリアルでわかる、ように、DataList ではテーブルの行ごとに表示する複数のレコードは許可ただし、します。 その一方で、Repeater、厳密に、そのテンプレートで指定されたマークアップを出力します。追加のマークアップを追加しませんし、したがって以外の HTML 要素にデータを表示する使用よく、 `<table>` (箇条書きリストなど)。

DataList と Repeater、レンダリングされた出力に柔軟性が提供して、GridView で見つかった組み込み機能の多くがありません。 今後のチュートリアルで取り上げるようせず、非常に面倒に戻ってこれらの機能のいくつかプラグインできますがの操作を行います保持に注意してくださいをまたはを使用して DataList Repeater を代わりに、GridView では、これらの機能を実装することがなく使用できる機能を制限します。自分でします。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Yaakov Ellis、Liz Shulok、Randy Schmidt、および Stacy パークでした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
