---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: カスタマイズした並べ替えユーザー インターフェイスの作成 (VB) |Microsoft ドキュメント
author: rick-anderson
description: 長い一覧を表示するには、データが並べ替えられたときに、非常に役に立ちます区切り行を導入することによって関連するデータをグループ化します。 このチュートリアルでは会いしましょうを作成する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 35144907aceda6ece56d91b24aba15ef951ed99a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892674"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>カスタマイズされた並べ替えのユーザー インターフェイス (VB) を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe)または[PDF のダウンロード](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> 長い一覧を表示するには、データが並べ替えられたときに、非常に役に立ちます区切り行を導入することによって関連するデータをグループ化します。 このチュートリアルではこのような並べ替えユーザー インターフェイスを作成する方法が表示されます。


## <a name="introduction"></a>はじめに

データを並べ替えるの長い一覧を表示するときに、少数の並べ替えられた列の異なる値の場合は、エンドユーザーが解決しない場合、正確に相違点の境界の実行く先難しいです。 たとえば、9 個までの別のカテゴリの選択肢は、データベース内の 81 の製品が (8 つの一意のカテゴリと`NULL`オプション)。 Seafood カテゴリの下にある製品を調べることに興味を持っているユーザーの場合を考えます。 一覧表示するページから*すべて*カテゴリで、グループ化して結果を並べ替えるには、自分のおすすめの 1 つの GridView では、製品、ユーザーが決定可能性がありますすべてまとめて Seafood 製品のです。 カテゴリで並べ替えの後に、ユーザーする必要があります、リストを探してみましょう Seafood にグループ化された製品の開始位置と終了を探してです。 Seafood 製品カテゴリ名が困難ですが、この結果はアルファベット順ためがグリッド内の項目の一覧を密接にスキャンが必要な。

並べ替えられたグループ間の境界を強調表示するには、多くの web サイトは、このようなグループ間の区切り記号を追加するユーザー インターフェイスを使用します。 図 1 のような区切り記号によりをより迅速に特定のグループを検索し、その境界を識別できるだけでなく、データにどのような個別のグループの存在を確認できます。


[![各カテゴリ グループが明確に識別](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**図 1**: 各カテゴリ グループとは、明確に識別 ([フルサイズのイメージを表示するをクリックして](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


このチュートリアルではこのような並べ替えユーザー インターフェイスを作成する方法が表示されます。

## <a name="step-1-creating-a-standard-sortable-gridview"></a>手順 1: 標準、並べ替え可能な GridView の作成

強化された並べ替えインターフェイスを提供する GridView を強化する方法をについて説明する前に製品を一覧表示する、標準的な並べ替え可能な GridView をまず作成 s を使用できます。 開いて開始、 `CustomSortingUI.aspx`  ページで、`PagingAndSorting`フォルダーです。 GridView をページに追加、設定、`ID`プロパティを`ProductList`、新しい ObjectDataSource にバインドします。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`レコードを選択するためのメソッドです。

のみが格納されるように、GridView を次に、構成、 `ProductName`、 `CategoryName`、 `SupplierName`、および`UnitPrice`BoundFields と提供が中止された CheckBoxField です。 最後に、構成、並べ替えを有効にするチェック ボックスをオン GridView s のスマート タグの並べ替えをサポートする GridView (かを設定してその`AllowSorting`プロパティを`true`)。 追加を行った後、 `CustomSortingUI.aspx`  ページで、宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

ブラウザーでこれまで、進行状況を表示するのにはしばらくを実行します。 図 2 は、そのデータがカテゴリでアルファベット順で並べ替えられた、並べ替え可能な GridView を示します。


[![並べ替え可能な GridView のデータはカテゴリによって順序付けられます。](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**図 2**: 並べ替え可能な GridView s データのカテゴリで並べ替え ([フルサイズのイメージを表示するをクリックして](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>手順 2: 区切り記号の行を追加するための手法の調査

ジェネリック、並べ替え可能な GridView 完了には区切り記号の行を並べ替えられた各グループの前に GridView に追加できます。 しかし、どのような行に挿入できます GridView でしょうか。 基本的には、必要なは GridView の行を反復処理する並べ替え済みの列の値の間で相違点が発生する可能性を判断し、適切な区切り行を追加します。 ソリューションが GridView s で任意の場所にある自然ようこの問題を考えると`RowDataBound`イベント ハンドラー。 説明したよう、[カスタム書式指定ベース時にデータ](../custom-formatting/custom-formatting-based-upon-data-vb.md)チュートリアルでは、このイベント ハンドラーは通常、行のデータに基づく行レベルの書式を設定する場合に使用します。 ただし、`RowDataBound`イベント ハンドラー ソリューションではない、ここでは、行は追加できません GridView にプログラムでこのイベント ハンドラーからです。 GridView の`Rows`コレクションは実際には、読み取り専用です。

GridView に追加の行を追加するには、3 つの選択肢があります。

- GridView にバインドされている実際のデータにこれらのメタデータの区切り記号の行を追加します。
- GridView は、データにバインドされましたが後に、追加`TableRow`GridView s をインスタンス コレクションを制御します。
- GridView コントロールを拡張し、責任 GridView の構造を構築するためにこれらのメソッドをオーバーライドするカスタムのサーバー コントロールを作成します。

この機能は、多くの web ページ上または複数の web サイト全体で必要な場合、カスタム サーバー コントロールを作成する最適な方法でしょう。 ただし、相当量のコードと GridView s の内部動作の深さに完全な探索が必要になることがします。 そのため、このオプションは、このチュートリアルと見なしませんおします。

GridView にバインドされているし、後に、GridView s のコントロール コレクションを操作する、実際のデータになるように区切り行を追加のオプション、他の 2 つのバインド - 問題を異なる方法で攻撃し、価値について説明されています。

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>GridView にバインドされているデータに行を追加します。

GridView は、データ ソースにバインドすると作成、`GridViewRow`データ ソースによって返されるレコードごとにします。 そのため、必要な区切り記号の行を挿入します GridView にバインドする前に、データ ソースの区切り記号のレコードを追加することでことができます。 図 3 は、この概念を示します。


![1 つの手法では、データ ソースに区切り記号の行の追加](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**図 3**: 1 つの手法では、データ ソースに区切り記号の行の追加


特殊な区切り記号のレコードがないために、引用符で囲まれた用語の区切り記号レコードを使用しました代わりに、お必要があります何らかの理由でフラグを設定、データ ソースの特定のレコードが通常のデータ行ではなく、区切り記号として機能します。 この例については、バインディング re お、`ProductsDataTable`から構成される GridView インスタンス`ProductRows`です。 お可能性がありますとしてフラグを付けるレコード区切り行を設定してその`CategoryID`プロパティを`-1`(値でした t が通常どおり存在) ためです。

この手法を利用するには、次の手順を実行する d お必要があります。

1. GridView にバインドするデータをプログラムで取得 (、`ProductsDataTable`インスタンス)
2. GridView s に基づいてデータを並べ替える`SortExpression`と`SortDirection`プロパティ
3. 反復処理する、`ProductsRows`で、 `ProductsDataTable`、並べ替えられた列の相違点がある箇所を探し求めています。
4. 各グループの境界の区切り記号のレコードを挿入`ProductsRow`インスタンス s を持つ DataTable に`CategoryID`'éý' `-1` (任意指定は、時に、区切り記号は 1 レコードとしてレコードをマークすると判断されました)
5. 区切り記号の行を挿入するには後のプログラムで GridView にデータをバインドします。

これらの 5 つの手順だけでなく d 必要も GridView s のイベント ハンドラーを指定する`RowDataBound`イベント。 ここでは、d ごとに確認`DataRow`、区切り記号がかどうかを確認して 1 つの行が`CategoryID`設定が`-1`です。 場合は、d おする場合、書式設定操作またはセルに表示されるテキストを調整します。

この手法を使用しても GridView s のイベント ハンドラーを指定する必要がある並べ替えのグループの境界を提供するには、上記で説明したよりも少しの作業が必要`Sorting`イベントと保持の追跡、`SortExpression`と`SortDirection`値。

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>秒後にコレクションを制御 GridView s を操作するデータ バインドされました。

GridView にバインドする前に、データをメッセージングではなく、区切り記号の行を追加できる*後*データが GridView にバインドされました。 GridView のコントロール実際には、階層を作成して、データ バインディングのプロセスが、単に`Table`インスタンスは、セルのコレクションのそれぞれで構成されますが、行のコレクションで構成されます。 具体的には、GridView のコントロールのコレクションが含まれています、`Table`のルート オブジェクト、 `GridViewRow` (から派生した、`TableRow`クラス) の各レコードに対して、 `DataSource` GridView にバインドされていると`TableCell`各内のオブジェクト`GridViewRow`の各データ フィールドのインスタンス、`DataSource`です。

並べ替え各グループの間の区切り記号の行を追加するには直接操作できますこのコントロールの階層が作成されます。 前回のページを表示している時点での GridView のコントロールの階層が作成されていることを確信できます。 そのため、このアプローチがよりも優先、`Page`クラスの`Render`メソッドは、この時点で必要な区切り行が含まれる GridView s 最終的なコントロールの階層が更新されます。 図 4 は、このプロセスを示しています。


[![別の手法が GridView のコントロールの階層構造を操作します。](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**図 4**: 別の手法が GridView s コントロールの階層構造を操作する ([フルサイズのイメージを表示するをクリックして](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


このチュートリアルを並べ替えのユーザー エクスペリエンスをカスタマイズするのに、この後者のアプローチを使用します。

> [!NOTE]
> コードはで提供される例に基づいてこのチュートリアルで m が提示する[Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s ブログ エントリ「 [GridView 並べ替えのグループ化のビット再生](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx)です。


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>手順 3: GridView のコントロール階層に区切り行を追加します。

この加算、最後のページのライフ サイクルが、実際の GridView c 前に実行するのでのみ行を追加する、区切り記号、GridView のコントロール階層にそのコントロールの階層が作成され、ページを訪問で最後に作成された後、ソース階層が HTML に表示されています。 これでこれを行う、最新の可能なポイントが、`Page`クラスの`Render`イベントで、次のメソッド署名を使用して、分離コード クラスでオーバーライドできます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

ときに、`Page`クラス元`Render`メソッドが呼び出される`base.Render(writer)`の各ページにコントロールに表示され、コントロールの階層構造に基づくマークアップを生成します。 そのために両者を呼び出すことが不可欠が`base.Render(writer)`ページを表示できるように、および GridView s を操作してコントロールの呼び出しの前に階層`base.Render(writer)`区切り記号行が前に、GridView のコントロールの階層構造に追加されているため、s されて表示されます。

並べ替えのグループ ヘッダーを挿入するには、最初、ユーザーが、データが並べ替えられることを要求したことを確認する必要があります。 既定では、GridView の内容が並べ替えられていないと、したがってヘッダーを並べ替え、グループを入力する必要ありません。

> [!NOTE]
> GridView ページが最初に読み込まれたときに、特定の列でソートする場合は、呼び出す GridView の`Sort`メソッドの最初のページ アクセスで (ただし、以降のポストバックではなく)。 これを実現するには、この呼び出しを追加、`Page_Load`内のイベント ハンドラー、`if (!Page.IsPostBack)`条件。 戻って、[レポート データの並べ替えとページング](paging-and-sorting-report-data-vb.md)チュートリアルについての詳細については、`Sort`メソッドです。


データが並べ替えられている、どのような列を調べるには、次のタスクは、ことを想定して、によってデータが並べ替えられたし、し、その列 s の相違点を探して、行をスキャンする値します。 次のコードにより、データが並べ替えられているし、データの並べ替えられて、列を検索します。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

GridView がまだする並べ替えられた、GridView の`SortExpression`プロパティが設定されていません。 したがって、のみが必要にこのプロパティは、いくつかの値を持つ場合、区切り記号の行を追加します。 場合は、次に、データの並べ替え、列のインデックスを確認する必要があります。 GridView s をループしてこれを行う`Columns`コレクション、列の検索が`SortExpression`プロパティに等しい GridView の`SortExpression`プロパティです。 列のインデックスだけでなくも取得、`HeaderText`区切り行を表示する際に使用されるプロパティです。

データを並べ替える列のインデックス、最後の手順は GridView の行を列挙します。 行ごとには、並べ替えられた列の値が、前の行並べ替えられています列の値と異なるかどうかを決定する必要があります。 場合は、新しいを挿入する必要がありますので、`GridViewRow`インスタンスへのコントロールの階層構造にします。 これは、次のコードで行われます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

このコードをプログラムで参照することで開始、 `Table` GridView のコントロールの階層のルートにある検出されたオブジェクトし、という名前の文字列変数を作成する`lastValue`です。 `lastValue` 前の行の値を持つ現在の行 s 並べ替え列の値の比較に使用します。 次に、GridView s`Rows`コレクションが列挙され、行ごとに、並べ替えられた列の値は、`currentValue`変数。

> [!NOTE]
> セル s を使用すれば、特定の行の並べ替えられています列の値を決定する`Text`プロパティです。 これは、BoundFields に適してが TemplateFields、CheckBoxFields、必要に応じて最適し、はなどです。 処理する方法の代替の GridView フィールド間もなく見ていきます。


`currentValue`と`lastValue`変数が比較されます。 これらが異なる場合、コントロールの階層構造に新しい区切り行を追加する必要があります。 インデックスを決定することでこれを行う、`GridViewRow`で、`Table`オブジェクト s`Rows`新規に作成するコレクション`GridViewRow`と`TableCell`インスタンス、および追加し、`TableCell`と`GridViewRow`に、コントロールの階層です。

区切り記号が s 内の単一行を行ことに注意してください`TableCell`GridView の幅全体にまたがるように書式設定を使用して書式設定、 `SortHeaderRowStyle` CSS クラスは、その`Text`プロパティ (カテゴリ) など名など、両方の並べ替えグループが表示されることと(飲み物) などのグループの値です。 最後に、`lastValue`の値に更新`currentValue`です。

並べ替えのグループ ヘッダー行の書式設定に使用される CSS クラス`SortHeaderRowStyle`で指定する必要があります、`Styles.css`ファイル。 自由に不服がある。 どのようなスタイルの設定を使用して以下を使用します。


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

任意 BoundField で並べ替えるときに現在のコードでは、並べ替えのインターフェイスは並べ替えグループ ヘッダーを追加 (図 5 の「業者によって並べ替えるときに、スクリーン ショット示すを参照してください)。 ただし、(CheckBoxField や TemplateField) などのフィールドの種類を並べ替えるときに、並べ替えのグループ ヘッダーが (図 6 を参照) が見つかりません。


[![並べ替えのインターフェイスに BoundFields で並べ替えるときに並べ替えられたグループ ヘッダーが含まれます](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**図 5**:「並べ替えインターフェイスが含まれています並べ替えグループ ヘッダーと順に並べ替え BoundFields ([フルサイズ イメージを表示するに、をクリックして](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![並べ替えのグループ ヘッダーが見つからないときに並べ替え、CheckBoxField です。](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**図 6**:「並べ替えグループ ヘッダーが見つからないときに並べ替え、CheckBoxField ([フルサイズのイメージを表示するには、をクリックして](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


CheckBoxField でソートする場合は、並べ替えのグループ ヘッダーが不足している理由は、コードは現在だけ使用するため、 `TableCell` s`Text`行ごとに並べ替えられた列の値を決定するプロパティです。 CheckBoxFields、用、 `TableCell` s`Text`プロパティは、空の文字列は代わりに、値内に存在する チェック ボックス Web コントロールで使用できる、 `TableCell` s`Controls`コレクション。

BoundFields 以外の型はフィールドを処理する必要があります、コードを拡張するためにここで、`currentValue`内のチェック ボックスの存在を確認する変数が割り当てられている、 `TableCell` s`Controls`コレクション。 使用せずに`currentValue = gvr.Cells(sortColumnIndex).Text`、このコードを次に置き換えます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

このコードは、並べ替えられた列を調べて`TableCell`現在の行の任意のコントロールがあるかを確認、`Controls`コレクション。 ある場合と最初のコントロールは、チェック ボックス、`currentValue`変数が設定を はい または いいえ、CheckBox s に応じて`Checked`プロパティです。 値を取得するそれ以外の場合、 `TableCell` s`Text`プロパティです。 このロジックは、任意の TemplateFields GridView に存在するための振り分けを処理するレプリケートできます。

上記のコードの追加すると、並べ替えのグループ ヘッダー存在しています廃止 CheckBoxField でソートする場合 (図 7 を参照してください)。


[![並べ替えのグループ ヘッダーは現在存在と並べ替え、CheckBoxField です。](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**図 7**:「並べ替えグループ ヘッダーは現在存在と並べ替え、CheckBoxField ([フルサイズのイメージを表示するには、をクリックして](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> 製品を使用していれば`NULL`の値をデータベース、 `CategoryID`、 `SupplierID`、または`UnitPrice`フィールド、それらの値が GridView で空の文字列として既定では、これらの製品に対して区切り行のテキストを意味`NULL`カテゴリのような値は読み取り: (が s は、カテゴリの後に名前のない: などのカテゴリを持った: 飲み物)。 BoundFields を設定できますか、ここに表示される値を求める場合[`NullDisplayText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx)テキストに表示するか、割り当てるときに、Render メソッドで条件付きステートメントを追加することができます、`currentValue`区切り記号を行の`Text`プロパティです。


## <a name="summary"></a>まとめ

GridView は、並べ替えのインターフェイスをカスタマイズするための多くの組み込みオプションには含まれません。 は、低レベルのコードのビットをそれをより細かくカスタマイズされたインターフェイスを作成する GridView のコントロールの階層構造を微調整することが可能です。 このチュートリアルでは、並べ替え可能な GridView、個別のグループとそれらのグループの境界をより簡単に識別するための並べ替えグループ区切り行を追加する方法を説明しました。 カスタマイズした並べ替えインターフェイスの他の例については、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/) s[いくつか ASP.NET 2.0 GridView の並べ替えのヒントとテクニック](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx)ブログ エントリです。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](sorting-custom-paged-data-vb.md)
