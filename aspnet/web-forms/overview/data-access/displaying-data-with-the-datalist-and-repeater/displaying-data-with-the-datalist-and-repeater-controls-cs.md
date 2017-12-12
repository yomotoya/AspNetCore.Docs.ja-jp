---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: "リピータ コントロール (c#) と DataList でのデータの表示 |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルでは、データを表示するのに GridView コントロールを使用しています。 以降では、このチュートリアルを見てで共通のレポート パターンを構築しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f626731e79d83785057498c53cdf49aecb90261
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>リピータ コントロール (c#) と DataList でデータを表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe)または[PDF のダウンロード](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> 前のチュートリアルでは、データを表示するのに GridView コントロールを使用しています。 以降このチュートリアルでは、ここを見て、DataList とリピータ コントロールに共通のレポート パターンを構築の基本をこれらのコントロールでデータの表示の開始。


## <a name="introduction"></a>はじめに

過去で説明する例のすべてのページで 28 チュートリアル、ここをオン GridView コントロールにデータ ソースから複数のレコードを表示する必要がある場合。 GridView は、レコードのデータ フィールドを列に表示する行をデータ ソース内の各レコードを表示します。 GridView ようには、スナップインの表示、使用、並べ替え、編集、およびデータを削除してページを外観が少し角張ったです。 さらに、マークアップ担当するには GridView の構造は固定されているが含まれています、HTML には`<table>`テーブル行を持つ (`<tr>`) の各レコードは、表のセル (`<td>`) フィールドごとにします。

複数のレコードを表示するときに、レンダリングされるマークアップと外観のカスタマイズの詳細を提供するには、ASP.NET 2.0 は、DataList およびリピータ コントロールを提供しています (これの両方で使用できたも ASP.NET バージョン 1.x)。 DataList リピータ コントロールでは、BoundFields、CheckBoxFields、ButtonFields ではなく、テンプレートを使用してコンテンツを表示およびなどです。 GridView のように、DataList は HTML としてレンダリング`<table>`が複数のデータのソース テーブルの行ごとに表示されるレコードを許可します。 リピータ、その一方で、マークアップなし追加よりも明示的に指定、および対象最適な候補は、出力されるマークアップを正確に制御が必要な場合です。

数十個が次のチュートリアルを見ていきます、DataList とリピータ コントロールに共通のレポート パターンを構築の基本をこれらのコントロール テンプレートを使用してデータを表示する開始します。 これらのコントロールの書式を設定する方法をお見せを編集し、データを削除するには、DataList、マスター/詳細の一般的なシナリオ、方法でデータ ソースのレコードのレイアウトを変更する方法、レコードを複数のページになどの方法です。

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>手順 1: DataList およびリピータ チュートリアルの Web ページを追加します。

このチュートリアルを始める前に、まずこのチュートリアルおよび DataList および Repeater を使用してデータの表示を処理する次のいくつかチュートリアルが必要です、ASP.NET ページを追加する少し時間を取って s を使用できます。 という名前のプロジェクトに新しいフォルダーを作成して開始`DataListRepeaterBasics`です。 次に、すべてのマスター ページを使用するように構成して、このフォルダーに次の 5 つの ASP.NET ページを追加`Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![DataListRepeaterBasics フォルダーを作成し、チュートリアルの ASP.NET ページを追加](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**図 1**: 作成、`DataListRepeaterBasics`フォルダー チュートリアルの ASP.NET ページを追加


開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン画面上のフォルダーです。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップを列挙し、箇条書きリストの現在のセクションのチュートリアルが表示されます。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


箇条書きリスト表示するために作成する、DataList とリピータ チュートリアル必要がありますサイト マップに追加します。 開く、`Web.sitemap`ファイルし、マークアップの後にカスタム ボタンの追加サイト マップ ノード、次のマークアップを追加します。


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![マップを更新するサイトに新しい ASP.NET ページを含める](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**図 3**: 新規の ASP.NET ページを含むサイト マップを更新


## <a name="step-2-displaying-product-information-with-the-datalist"></a>手順 2: DataList で情報が製品を表示します。

テンプレートではなく BoundFields CheckBoxFields、やなどによって異なります DataList コントロール s が表示される出力をフォーム ビューと同様に。 FormView とは異なり、DataList は単独の 1 つではなく、レコードのセットを表示する設計されています。 S バインド DataList に製品情報を見るには、このチュートリアルを開始できるようにします。 開いて開始、 `Basics.aspx`  ページで、`DataListRepeaterBasics`フォルダーです。 次に、DataList をツールボックスからデザイナーにドラッグします。 DataList のテンプレートを指定する前に、図 4 に示すように、デザイナーは、灰色のボックスとして表示します。


[![DataList をツールボックスからデザイナーにドラッグします。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**図 4**: DataList から、[ツールボックス] に、デザイナーをドラッグして ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


スマート タグの DataList 秒から、新しい ObjectDataSource を追加および使用するように構成、`ProductsBLL`クラスの`GetProducts`メソッドです。 お読み取り専用 DataList を作成するこのチュートリアルでは再設定されるので (なし) をドロップダウン リスト ウィザード s 挿入、更新、およびタブを削除します。


[![新しい ObjectDataSource を作成することを選択します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**図 5**: 新しい ObjectDataSource を作成すること ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![構成、ObjectDataSource ProductsBLL クラスを使用するには](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**図 6**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![すべての GetProducts メソッドを使用して製品に関する情報を取得します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**図 7**: 取得情報に関するすべての製品を使用して、`GetProducts`メソッド ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


後、ObjectDataSource を構成して、そのスマート タグから DataList に関連付けることが、Visual Studio が自動的に作成、`ItemTemplate`名とデータ ソースによって返される各データ フィールドの値を表示する DataList で (を参照してください、マークアップ以下)。 この既定`ItemTemplate`の外観が FormView デザイナーを使用するデータ ソースをバインドするときに自動的に作成したテンプレートの場合と同じです。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> FormView s のスマート タグから FormView コントロールをデータ ソースをバインドするときに Visual Studio 作成されたことに注意してください、 `ItemTemplate`、 `InsertItemTemplate`、および`EditItemTemplate`です。 ただし、DataList でのみ、`ItemTemplate`を作成します。 これは、DataList に同じ組み込みの編集と FormView で提供されるサポートの挿入があるないためです。 DataList は編集および削除に関連するイベントは含まし、編集およびサポートを削除することができます追加ほんの少しのコードが含まれていますされなくなります FormView でとして、標準のサポートが単純なです。 編集、および今後のチュートリアルでは、DataList のサポートの削除をインクルードする方法を会いしましょう。


このテンプレートの外観を向上させるために少し時間を取って s を使用できます。 すべてのデータ フィールドを表示するではなく s 製品の名前、供給業者、カテゴリ、単位、および単価ごとの数を表示のみを使用できます。 Let s がさらに、内の名前を表示、`<h4>`見出しと、その他のフィールドを使用してレイアウト、`<table>`見出しの下にします。

これらの変更することができます DataList s タグが、テンプレートの編集リンクをクリックするか、ページ s の宣言構文から手動でテンプレートを変更することができますスマートからデザイナーの機能を編集するテンプレートを使用するか。 デザイナーでテンプレートの編集オプションを使用する場合、結果として得られるマークアップで、次のマークアップを正確に、一致しない可能性がありますが、を通じて表示されるときに、ブラウザーは次のようにスクリーン ショット図 8 に示す。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> 使用して上記の例ラベル Web コントロールが`Text`プロパティには、データ バインド構文の値が割り当てられます。 または、でしたを省略したラベル、databinding 構文だけを入力します。 つまり、使用する代わりに`<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />`お代わりに使用することも、宣言構文`<%# Eval("CategoryName") %>`です。


ラベルの Web コントロールのままにして、ただし、2 つの利点を提供します。 最初に、次のチュートリアルで後ほどお見せ同様に、データに基づくデータを書式設定の簡単な手段を提供します。 次に、デザイナーではありません t 表示データ バインドを宣言の構文では、テンプレートの編集オプション表示される一部の Web コントロールの外部でします。 代わりに、テンプレートの編集インターフェイスが static のマークアップでの作業を容易にするように設計し、Web コントロールを任意のデータ バインドは操作には、Web コントロールのスマート タグからアクセス可能な databindings の編集 ダイアログ ボックスを前提としています。

そのため、デザイナーでテンプレートを編集するオプションを提供するには、DataList を使用する場合 (_n) ラベル Web コントロールを使用して、コンテンツは、テンプレートの編集インターフェイス経由でアクセスできるようにします。 思います間もなく、リピータでは、ソース ビューからテンプレートの内容を編集することが必要です。 その結果、書式を設定する必要がありますはわからないコントロール ラベル Web 多くの場合は省略しますリピータのテンプレートを作成するとき、データの外観には、プログラム ロジックに基づいて、テキストがバインドされます。


[![各製品の出力は、DataList の ItemTemplate を使用して表示されます。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**図 8**: 各製品の出力が DataList s を使用してレンダリング`ItemTemplate`([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>手順 3: DataList の外観の向上

GridView のように、DataList は数多くのスタイル関連プロパティなど`Font`、 `ForeColor`、 `BackColor`、 `CssClass`、 `ItemStyle`、 `AlternatingItemStyle`、`SelectedItemStyle`のようにします。 内のスキン ファイルを作成した、GridView と DetailsView コントロールを使用する場合、`DataWebControls`事前定義のテーマ、`CssClass`これら 2 つのコントロールのプロパティと`CssClass`、サブプロパティのいくつかのプロパティ (`RowStyle`、`HeaderStyle`など)。 S DataList に対して同じ操作を実行できるようにします。

説明したように、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)スキン ファイルは、Web コントロールの既定の外観に関連するプロパティを指定しますテーマを定義する、スキン、CSS、画像、および JavaScript ファイルのコレクションは、チュートリアル。web サイトの特定ルック アンド フィールです。 *、ObjectDataSource でデータを表示する*チュートリアルでは、作成した、`DataWebControls`テーマ (は内のフォルダーとして実装されている、`App_Themes`フォルダー) を持つ、現在のところ、2 つのスキン ファイル -`GridView.skin`と`DetailsView.skin`. S サードパーティ DataList の定義済みのスタイルの設定を指定するためのスキン ファイルを追加することができます。

スキン ファイルを追加するを右クリックし、`App_Themes/DataWebControls`フォルダー、新しい項目の追加を選択し、一覧から、スキン ファイルのオプションを選択します。 そのファイルに `DataList.skin` という名前を付けます。


[![DataList.skin をという名前の新しいスキン ファイルを作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**図 9**: 新しいスキン ファイルの名前付き作成`DataList.skin`([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


次のマークアップを使用して、`DataList.skin`ファイル。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

これらの設定は、as、GridView と DetailsView コントロールで使用された、適切なプロパティを同じの CSS クラスを割り当てます。 ここで使用される CSS クラス`DataWebControlStyle`、 `AlternatingRowStyle`、`RowStyle`などで定義されて、`Styles.css`ファイルし、前のチュートリアルで追加されました。

このスキン ファイルの追加により、DataList の外観は、(;、表示 メニューから新しいスキン ファイルの効果を参照して、更新 を選択するデザイナー ビューを更新する必要があります)、デザイナーで更新されます。 図 10 では、交互の各製品が明るいピンクの背景色にします。


[![DataList.skin をという名前の新しいスキン ファイルを作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**図 10**: 新しいスキン ファイルの名前付き作成`DataList.skin`([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>手順 4: 探索、DataList s 他のテンプレート

加え、 `ItemTemplate`DataList は、その他の 6 つの省略可能なテンプレートをサポートしています。

- `HeaderTemplate`指定した場合、出力にヘッダー行を追加し、この行を表示するために使用します。
- `AlternatingItemTemplate`交互の項目を表示するために使用します。
- `SelectedItemTemplate`選択した項目を表示するために使用します。選択した項目は、DataList s に対応してインデックスを持つ項目[`SelectedIndex`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`編集中の項目を表示するために使用します。
- `SeparatorTemplate`指定した場合、各項目間の区切り記号を追加し、この区切り記号を表示するために使用します。
- `FooterTemplate`-指定した場合、出力にフッター行を追加し、この行を表示するために使用します。

指定する場合、`HeaderTemplate`または`FooterTemplate`DataList、表示される出力に追加のヘッダーまたはフッター行を追加します。 ように GridView のヘッダーとフッター行のヘッダーとフッター、DataList にバインドされていないデータ。 そのため、すべてデータ バインドの構文で、`HeaderTemplate`または`FooterTemplate`にバインドされたデータにアクセスしようとは、空の文字列を返します。

> [!NOTE]
> 説明したとおり、 [GridView のフッターで概要情報を表示する](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)からこれらの行に直接挿入できますチュートリアルでは、ヘッダーとフッター行 t サポート データ バインドの構文、データに固有の情報のないときに、GridView の`RowDataBound`イベント ハンドラー。 この手法に使用できる集計途中経過を計算両方またはその他の情報、データからコントロールにバインドされているだけでなく、フッターにその情報を割り当てます。 これと同じ考え方は、DataList とリピータ コントロールに適用できます。リピータ、DataList のイベント ハンドラーを作成する唯一の違いは、`ItemDataBound`イベント (の代わりの`RowDataBound`イベント)。


たとえば、let s したタイトルが、DataList s の結果の上部に表示される製品情報、`<h3>`見出し。 これを実現するには追加、`HeaderTemplate`適切なマークアップ。 これは、デザイナーでは、DataList s スマート タグのテンプレートの編集リンクをクリックすると、ドロップダウン リストからヘッダーのテンプレートを選択して、テキスト スタイル ドロップダウンから見出し 3 オプションを選択した後に入力して実行できます (図 11 を参照してください) を一覧表示します。


[![テキストの製品情報 HeaderTemplate を追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**図 11**: 追加、`HeaderTemplate`テキスト製品情報 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


また、この追加できます宣言内で次のマークアップを入力して、`<asp:DataList>`タグ。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

各製品の一覧項目間のスペースのビットを追加するにように追加 s、`SeparatorTemplate`各セクションの間に行が含まれています。 水平線タグ (`<hr>`)、このような区分線を追加します。 作成、`SeparatorTemplate`に、次のマークアップを持つようにします。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> 同様に、`HeaderTemplate`と`FooterTemplates`、`SeparatorTemplate`データ ソースからすべてのレコードにバインドされていないと、そのため、データ ソース、DataList にバインドされているレコードのアクセスを直接ことはできません。


この追加を行った後、ブラウザーからページを表示するときに、ようになります図 12 です。 ヘッダー行および各製品の一覧項目の間の線に注意してください。


[![DataList には、ヘッダー行および各製品の一覧項目の間の区切り線が含まれています](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**図 12**: ヘッダー行と、水平方向の規則の間で各製品を一覧表示する、DataList が含まれます ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>手順 5: 特定のマークアップ Repeater コントロールでのレンダリング

図 12 から DataList の例にアクセスしたときに、お使いのブラウザーから/ソースの表示を行う場合、DataList が HTML を生成することが表示されます`<table>`テーブルの行を格納している (`<tr>`) に 1 つのテーブル セル (`<td>`) の各項目にバインドしますDataList です。 実際には、この出力は、単一 TemplateField GridView からどのような出力と同じです。 今後のチュートリアルで後ほどお見せ、DataList はこのように、出力をさらにカスタマイズ テーブルの各行につき複数のデータ ソースのレコードを表示することを有効にすることができます。

HTML を生成したくない場合`<table>`が、ですか? データ Web コントロールによって生成されたマークアップの合計で、完全な制御は、Repeater コントロールは使用する必要があります。 DataList と同様にリピータがテンプレートに基づいて構築されます。 リピータ、ただし、のみには次の 5 つのテンプレート。

- `HeaderTemplate`指定した場合、項目の前に、指定したマークアップを追加します。
- `ItemTemplate`項目を表示するために使用します。
- `AlternatingItemTemplate`指定した場合、交互の項目を表示するために使用します。
- `SeparatorTemplate`指定した場合、各項目の間、指定したマークアップを追加します。
- `FooterTemplate`-指定した場合、項目の後、指定したマークアップを追加します。

ASP.NET で 1.x では、コントロールがいくつかのデータ ソースからデータを含むが付属して箇条書きリストの表示に使用されたよくリピータします。 このような場合は、`HeaderTemplate`と`FooterTemplates`開始タグと終了が含まれます`<ul>`タグはそれぞれ、while、`ItemTemplate`が含まれます`<li>`databinding 構文を持つ要素。 2 つの例で示したように、この方法を ASP.NET 2.0 の使用ができます、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアル。

- `Site.master`マスター ページで、リピータが (基本レポート、レポートのフィルター処理、カスタマイズされた書式、およびなど) は、最上位サイト マップの内容の箇条書きリストの表示に使用された以外の場合は別に、入れ子になったリピータの子のセクションの表示に使用された、最上位セクション
- `SectionLevelTutorialListing.ascx`リピータが、現在のサイト マップ セクションの子セクションの行頭文字付き一覧を表示する使用されました

> [!NOTE]
> ASP.NET 2.0 が導入されていますが、新しい[BulletedList コントロール](https://msdn.microsoft.com/en-us/library/ms228101.aspx)、する単純な箇条書きリストを表示するためにデータ ソース コントロールにバインドできます。 BulletedList コントロールには必要はありません; リストに関連する HTML のいずれかを指定するには代わりに、各リスト項目のテキストとして表示するデータ フィールドだけで指定します。


リピータは、Web コントロールのすべてのデータを catch として機能します。 必要なマークアップを生成する既存のコントロールがない場合は、Repeater コントロールを使用できます。 リピータの使用を示すためには、手順 2. で作成された製品情報 DataList 上に表示されるカテゴリの一覧がある s を使用できます。 Let s が単一行の HTML に表示されるカテゴリを持つ具体的には、`<table>`テーブル内の列として表示される各カテゴリを使用します。

これを実現するには、製品情報 DataList 上、デザイナーには、ツールボックスから Repeater コントロールをドラッグして開始します。 同様に、DataList リピータは最初に表示灰色のボックスとしてそのテンプレートが定義されるまでです。


[![リピータをデザイナーに追加します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**図 13**: リピータをデザイナーに追加 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


リピータ s スマート タグのない s のみ 1 つのオプション: データ ソースを選択します。 新しい ObjectDataSource を作成および使用するように構成することを選択、`CategoriesBLL`クラスの`GetCategories`メソッドです。


[![新しい ObjectDataSource を作成します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**図 14**: 新しい ObjectDataSource を作成する ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![構成、ObjectDataSource CategoriesBLL クラスを使用するには](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**図 15**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラス ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![すべてのメソッドを使用してカテゴリの情報を取得します。](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**図 16**: 取得情報に関するすべてのカテゴリを使用して、`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


DataList とは異なり Visual Studio は自動的に作成されません ItemTemplate リピータのデータ ソースにバインドした後。 さらに、リピータのテンプレートは、デザイナーでは構成できませんし、宣言によって指定する必要があります。

単一行として、カテゴリを表示する`<table>`の各カテゴリの列に、次のようなマークアップを生成する中継ぎ局必要があります。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

以降、`<td>Category X</td>`テキストは、部分の繰り返し、リピータの ItemTemplate に表示されます。 前に、表示されるマークアップ`<table><tr>`-に配置されます、`HeaderTemplate`終了マークアップの中に`</tr></table>`-に配置されますが、`FooterTemplate`です。 これらのテンプレート設定を入力するには、左下隅の [ソース] ボタンをクリックし、次の構文で型をクリックすると、ASP.NET ページの宣言型の部分に移動します。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

リピータは、そのテンプレート、詳細 nothing, nothing 以下で指定された正確なマークアップを生成します。 図 17 では、ブラウザーで表示したときにリピータの出力を示します。


[![単一行 HTML&lt;テーブル&gt;別の列の各カテゴリの一覧表示](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**図 17**: A 単一行の HTML`<table>`別の列の各カテゴリの一覧 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>手順 6: リピータの外観の向上

リピータでは、そのテンプレートで指定されたマークアップを正確が出力、ために、当然となる必要があります、リピータのスタイル関連のプロパティがないことです。 リピータによって生成されるコンテンツの外観を変更するには、おには、手動でリピータのテンプレートに直接必要な HTML や CSS のコンテンツを追加する必要があります。

この例と同様に、背景色が交互 DataList で交互の行にカテゴリ列がある秒を使用できます。 これを実現する必要がありますを割り当てる、`RowStyle`リピータの各アイテムに CSS クラス、および`AlternatingRowStyle`CSS クラスを使用して各交互リピータ アイテムを`ItemTemplate`と`AlternatingItemTemplate`テンプレートでは、次のように。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

秒も、ヘッダー行の出力に追加する製品カテゴリのテキストを使用できます。 たくないのでがわからない列の数、結果として得られる`<table>`で構成されていますのすべての列にまたがることが保証されているヘッダー行を生成する最も簡単な方法は、使用する*2 つ* `<table>` s。 最初の`<table>`、ヘッダー行と、2 番目の単一行を格納する行に 2 つの行を含む`<table>`システム内の各カテゴリ列を持ちます。 つまり、次のマークアップを生成します。


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

次`HeaderTemplate`と`FooterTemplate`目的のマークアップが発生します。


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

図 18 は、これらの変更が加えられた後にリピータを示します。


[![カテゴリ列が別の背景色でと、ヘッダー行が含まれています](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**図 18**:、カテゴリ列代替背景色と、ヘッダー行が含まれています ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>概要

GridView コントロールでは、簡単に表示、編集、削除、並べ替え、および、データのページ、外観は非常に角張った、グリッドのようにします。 外観をより細かく制御には、DataList またはリピータのいずれかのコントロールを有効にする必要があります。 これらのコントロールは、一連の BoundFields CheckBoxFields、やなどの代わりにテンプレートを使用してレコードを表示します。

DataList は HTML としてレンダリング`<table>`既定では、表示する各データ ソースのレコード単一 TemplateField GridView と同じように、1 つのテーブルの行にします。 ように今後のチュートリアルでは、DataList は許可されている複数のレコードをテーブルの行ごとに表示されます。 その一方で、リピータ、厳密に、そのテンプレートで指定されたマークアップを生成します。マークアップを追加は追加されませんし、一般的であるため以外の HTML 要素にデータを表示する、 `<table>` (箇条書きリストなど)。

DataList とリピータ柔軟性が高く、レンダリングされた出力で中が GridView での組み込み機能の多くが不足しています。 今後のチュートリアルで説明します、これらの機能の一部が多すぎる作成作業なしに接続できるようは引き続きことに注意を使用して、DataList またはリピータ GridView の代わりでは、これらの機能を実装することがなく使用できる機能を制限します。自分でします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Yaakov Ellis、Liz Shulok、ものです。 Schmidt、Stacy 公園でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[次へ](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
