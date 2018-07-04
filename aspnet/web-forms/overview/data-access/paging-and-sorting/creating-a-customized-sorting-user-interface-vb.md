---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: カスタマイズされた並べ替えユーザー インターフェイス (VB) を作成する |Microsoft Docs
author: rick-anderson
description: 長いリストを表示するには、データが並べ替えられたときに、非常に役に立ちます行の区切り記号を導入することで関連するデータをグループ化します。 このチュートリアルで作成する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 27ff1efb47f8e74c3b9f090af646229a9121661b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394561"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>カスタマイズされた並べ替えユーザー インターフェイス (VB) を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe)または[PDF のダウンロード](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> 長いリストを表示するには、データが並べ替えられたときに、非常に役に立ちます行の区切り記号を導入することで関連するデータをグループ化します。 このチュートリアルでは、このような並べ替えユーザー インターフェイスを作成する方法がわかります。


## <a name="introduction"></a>はじめに

データを並べ替えるの長いリストを表示する場合がごく少数の並べ替えられた列で別の値がある場合、エンドユーザー方が難しい、正確に、違い境界にあります。 たとえば、9 個までのさまざまなカテゴリの選択肢は、データベースで 81 の製品が (8 つの一意のカテゴリだけでなく、`NULL`オプション)。 シーフード カテゴリに該当する製品を調べることに興味を持っているユーザーの場合を考えます。 一覧表示されたページから*すべて*彼女のおすすめをカテゴリ別にグループ化の結果の並べ替えにはの単一の GridView では、製品、ユーザーが決定可能性がありますすべてのシーフード製品化します。 カテゴリで並べ替えの後、ユーザー、必要があります、リストを探してみましょうシーフードでグループ化した製品の開始位置と終了を探してします。 結果はでアルファベット順に並べ替えられますのでシーフード製品カテゴリ名は、難しくはありませんが、グリッド内の項目の一覧を密接にスキャンが必要です。

並べ替えられたグループ間の境界を強調表示するには、多くの web サイトは、このようなグループ間の区切り記号を追加するユーザー インターフェイスを使用します。 図 1 のような区切り記号は、ユーザーはより迅速に特定のグループを検索して、その境界を特定するだけでなく、データにどのような個別のグループの存在を確認できます。


[![各カテゴリ グループが明確に識別](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**図 1**: 各カテゴリ グループが明確に識別される ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image3.png))。


このチュートリアルでは、このような並べ替えユーザー インターフェイスを作成する方法がわかります。

## <a name="step-1-creating-a-standard-sortable-gridview"></a>手順 1: 標準的な並べ替え可能な GridView を作成します。

強化された並べ替えインターフェイスを提供する GridView を拡張する方法を調査する前に、製品の一覧を表示する標準的な並べ替え可能な GridView をまず作成 s を使用できます。 開いて開始、`CustomSortingUI.aspx`ページで、`PagingAndSorting`フォルダー。 ページに GridView を追加、設定、`ID`プロパティを`ProductList`、新しい ObjectDataSource にバインドします。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`レコードを選択する方法。

のみが含まれるように、GridView を次に、構成、 `ProductName`、 `CategoryName`、 `SupplierName`、および`UnitPrice`BoundFields と提供が中止された CheckBoxField します。 GridView のスマート タグの並べ替えを有効にするチェック ボックスをオンの並べ替えをサポートする GridView を最後に、構成 (設定して、またはその`AllowSorting`プロパティを`true`)。 追加を行った後、 `CustomSortingUI.aspx`  ページで、宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

ブラウザーできませんでしたが、進行状況を表示する時間がかかります。 図 2 は、そのデータがカテゴリでアルファベット順で並べ替えられる場合、並べ替え可能な GridView を示します。


[![並べ替え可能な GridView のデータはカテゴリで並べ替え](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**図 2**: 並べ替え可能な GridView s のデータのカテゴリで並べ替え ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image6.png))。


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>手順 2: 区切り記号の行を追加するための手法を調査します。

ジェネリック、並べ替え可能な GridView で完全な残っているは並べ替えられた各グループの前に、gridview の区切り記号の行を追加できるようにします。 しかし、どのような行に挿入できます GridView でしょうか。 基本的に、必要な GridView、s 行を反復処理する並べ替え済みの列の値の間の相違点が発生するかを判断し、し、適切な区切り行を追加します。 GridView s に、ソリューションがどこかにある自然なようこの問題を考える`RowDataBound`イベント ハンドラー。 説明したように、[カスタム書式設定時にデータ](../custom-formatting/custom-formatting-based-upon-data-vb.md)チュートリアルでは、このイベント ハンドラーは通常、行のデータに基づく行レベルの書式設定を適用する場合に使用します。 ただし、`RowDataBound`イベント ハンドラー ソリューションではない、ここでは、行にことはできませんこのイベント ハンドラーからプログラムで、GridView に追加します。 GridView の`Rows`コレクションは実際には、読み取り専用です。

GridView に追加の行を追加するのには、3 つの選択肢があります。

- GridView にバインドされている実際のデータにこれらのメタデータの区切り記号の行を追加します。
- さらに追加したら、GridView をデータにバインドされていますが、 `TableRow` GridView s インスタンス コレクションの制御
- GridView コントロールを拡張し、GridView 構造の構築を担当のこれらのメソッドをオーバーライドするカスタム サーバー コントロールを作成します。

この機能は、多くの web ページ上または複数の web サイト全体で必要な場合、カスタム サーバー コントロールを作成する最適な方法でしょう。 ただし、非常に大量のコードと GridView s の内部動作の細部まで詳しく分析の完全な探索が必要になることは。 そのため、このオプションは、このチュートリアルを考慮しましたをしません。

実際のデータに区切り行を追加のオプション以外の 2 つ GridView にバインドし、後の GridView のコントロールのコレクションを操作のバインド - 問題を異なる方法で攻撃し、価値について説明されています。

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>GridView にバインドされているデータに行を追加します。

GridView をデータ ソースにバインドすると、ときに作成、`GridViewRow`データ ソースによって返されるレコードごとにします。 そのため、GridView にバインドする前に、データ ソースにレコードの区切り記号を追加することで、必要な区切り行を挿入しましたできます。 図 3 は、この概念を示します。


![1 つの手法では、データ ソースへの行の区切り記号の追加](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**図 3**: 1 つの手法では、データ ソースへの行の区切り記号の追加


特殊な区切りのレコードがないために、引用符で囲まれた用語の区切り記号レコードを使用します。代わりに、私たちする必要があります何らかの方法でフラグ、データ ソースの特定のレコードが通常のデータ行ではなく、区切り記号として機能します。 この例については、再バインドします、`ProductsDataTable`インスタンスから構成されると、GridView を`ProductRows`します。 レコードを区切り記号の行としてフラグを設定して可能性があります、`CategoryID`プロパティを`-1`(ため、通常の値でした t が存在)。

この手法を利用するには、次の手順を実行する d 必要があります。

1. GridView にバインドするデータをプログラムで取得 (、`ProductsDataTable`インスタンス)
2. GridView s に基づいてデータを並べ替える`SortExpression`と`SortDirection`プロパティ
3. 反復処理、`ProductsRows`で、 `ProductsDataTable`、並べ替えられた列の違いがある箇所を探し求めています。
4. 各グループの境界の区切り記号のレコードを挿入`ProductsRow`インスタンスを持つ、s、DataTable に`CategoryID`設定`-1`(または任意指定された区切り記号のレコードとしてレコードをマークする決定)
5. 区切り記号の行を挿入した後、GridView をプログラムでデータをバインドします。

これら 5 つの手順だけでなく d 必要もあります GridView s のイベント ハンドラーを提供する`RowDataBound`イベント。 ここでは、d ごとに確認`DataRow`決定した区切り記号と行、1 つが`CategoryID`設定が`-1`。 そのため、d おそらくたい場合、書式設定またはセルに表示されるテキストを調整します。

この手法を使用しても、GridView のイベント ハンドラーを提供する必要があると、上記で説明したよりもさらに作業を必要と並べ替えのグループの境界を挿入する`Sorting`イベントを引き続き追跡、`SortExpression`と`SortDirection`値。

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>これは、後にコレクションを s 制御 GridView s を操作するデータ バインドされています

GridView にバインドする前に、データをメッセージングではなく、区切り記号の行を追加できます*後*データが GridView にバインドされました。 データ バインディングのプロセスが実際には、GridView のコントロール階層を作成して、単に`Table`セルのコレクションのそれぞれで構成されますが、行のコレクションのインスタンスで構成されます。 具体的には、GridView のコントロール コレクションに含まれる、 `Table` 、そのルートにあるオブジェクトを`GridViewRow`(から派生、`TableRow`クラス) 内の各レコード、 `DataSource` 、GridView にバインドされていると、`TableCell`各内のオブジェクト`GridViewRow`内の各データ フィールドのインスタンス、`DataSource`します。

各並べ替えグループ間の区切り記号の行を追加するには、ことを直接操作できますこのコントロールの階層構造が作成されたら。 ページが表示されるまで、最後に s の GridView コントロール階層が作成されたことを確信できます。 このため、この方法をオーバーライド、`Page`クラスの`Render`メソッド、必要な区切り記号の行に含めるこの時点で GridView s の最終的なコントロール階層を更新します。 図 4 は、このプロセスを示します。


[![代替手法が s の GridView コントロール階層を操作します。](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**図 4**: 代替手法が GridView のコントロール階層を操作する ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image10.png))。


このチュートリアルでは、この後者のアプローチ、並べ替えのユーザー エクスペリエンスのカスタマイズを使用します。

> [!NOTE]
> コードはで提供される例に基づく m は、このチュートリアルでは提示[Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s ブログ エントリ「 [GridView 並べ替えのグループ化ビットを再生](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx)します。


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>手順 3: s の GridView コントロール階層を区切り記号の行を追加します。

この追加の最後に、ページのライフ サイクルが、実際の GridView c を実行するため、そのコントロールの階層構造が作成され、そのページのアクセス時に、最後に作成された後に、GridView s のコントロール階層に区切り行を追加するいたしますソース階層が HTML に表示されています。 これには、最新の可能なポイントは、`Page`クラスの`Render`イベントで、次のメソッド シグネチャを使用して分離コード クラスを無効にできます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

ときに、`Page`クラス元`Render`メソッドが呼び出される`base.Render(writer)`の各ページにコントロールに表示され、コントロールの階層構造に基づくマークアップを生成します。 したがっては両者を呼び出すことが不可欠`base.Render(writer)`ページが表示されるように、GridView s を操作およびコントロールの呼び出しの前に階層`base.Render(writer)`、その前に、GridView のコントロール階層に区切り行が追加されましたs されてレンダリングされます。

並べ替えのグループ ヘッダーを挿入するには、最初のデータが並べ替えられること、ユーザーが要求することを確認する必要があります。 既定では、GridView の内容が並べ替えられていないと、したがってことヘッダーを並べ替え、グループを入力する必要はありません。

> [!NOTE]
> GridView ページが最初に読み込まれたときに、特定の列で並べ替えられる場合は、呼び出す GridView の`Sort`メソッドの最初のページ参照してください (ただし、以降のポストバックではなく)。 これを実現するには、この呼び出しを追加、`Page_Load`内にイベント ハンドラー、`if (!Page.IsPostBack)`条件。 参照、[レポート データの並べ替えとページング](paging-and-sorting-report-data-vb.md)チュートリアルの情報の詳細については、`Sort`メソッド。


データが並べ替えられているを次のタスクではどのような列を確認すると仮定すると、によってデータが並べ替えられたし、から s 列での相違点を検索する行をスキャンする値します。 次のコードでは、データが並べ替えられているし、データが並べ替えられて、列を検索することを保証します。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

GridView がまだを並べ替え、GridView の`SortExpression`プロパティが設定されていません。 そのため、だけ使用するこのプロパティがいくつかの値を持つ場合は、区切り記号の行を追加します。 場合は、次に、データの並べ替え、列のインデックスを確認する必要があります。 これは GridView s ループ`Columns`コレクションを持つ列の検索`SortExpression`プロパティが GridView 秒に等しい`SortExpression`プロパティ。 取得私たちもの列のインデックスだけでなく、`HeaderText`プロパティで、区切り記号の行を表示するときに使用されます。

データを並べ替える列のインデックスを含む最後に、GridView の行を列挙します。 行ごとに並べ替えられた列の値が、前の行の並べ替えられています列の値と異なるかどうかを判断する必要があります。 新しいを挿入する必要がありますので場合、`GridViewRow`コントロール階層にインスタンス。 これは、次のコードで実現されます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

このコードはプログラムで参照することで起動、 `Table` s の GridView コントロール階層のルートにあるオブジェクトし、という名前の文字列変数を作成する`lastValue`します。 `lastValue` 前の行の値を持つ現在の行 s 並べ替え列の値を比較に使用されます。 次に、GridView s`Rows`コレクションが列挙されで行ごとに並べ替えられた列の値が格納されている、`currentValue`変数。

> [!NOTE]
> 特定の行の並べ替え列の値を決定するセル s 使用`Text`プロパティ。 これは、BoundFields に適してがない TemplateFields、CheckBoxFields、必要に応じて機能にします。 間もなく代替 GridView フィールドに対応する方法についてを説明します。


`currentValue`と`lastValue`変数と比較します。 これらが異なる場合は、コントロールの階層構造に新しい区切り記号の行を追加する必要があります。 インデックスを決定することでこれは、`GridViewRow`で、`Table`オブジェクト`Rows`コレクションの新規作成`GridViewRow`と`TableCell`インスタンス、および追加し、`TableCell`と`GridViewRow`に、コントロールの階層。

区切り記号が $s 内の単一行を行に注意してください`TableCell`、GridView の幅全体にまたがるように書式設定を使用して書式設定、 `SortHeaderRowStyle` CSS クラス、ありその`Text`(カテゴリ) など、並べ替えるグループの両方が表示されるような名前プロパティにと(飲み物) などのグループの値です。 最後に、`lastValue`の値が更新され`currentValue`します。

並べ替えのグループ ヘッダー行の書式設定に使用する CSS クラス`SortHeaderRowStyle`で指定する必要があります、`Styles.css`ファイル。 自由にどのようなスタイル設定にアピールします。次を使用しました。


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

BoundField で並べ替えるときに現在のコードでは、並べ替えのインターフェイスは並べ替えグループ ヘッダーを追加 (業者によって並べ替えるときに、スクリーン ショットを表示する図 5 を参照してください)。 ただし、(CheckBoxField や TemplateField) などのフィールドの種類を並べ替えるときに、並べ替えのグループ ヘッダーが (図 6 参照) が見つかりません。


[![並べ替えのインターフェイスに BoundFields で並べ替えるときに並べ替えられたグループ ヘッダーが含まれます](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**図 5**: The 並べ替えインターフェイスが含まれています並べ替えグループ ヘッダーとでの並べ替え BoundFields ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image13.png))。


[![並べ替えのグループ ヘッダーが不足しているときに並べ替え、CheckBoxField です。](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**図 6**:、並べ替えのグループ ヘッダーが不足しているときに並べ替え、CheckBoxField ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image16.png))。


コードは現在だけに使用して、CheckBoxField して並べ替えるときに、並べ替えのグループ ヘッダーが見つからない理由は、 `TableCell` s`Text`行ごとに並べ替えられた列の値を決定するプロパティ。 CheckBoxFields、用、 `TableCell` s`Text`プロパティが空の文字列は、代わりに、値が内にあるチェック ボックスを Web コントロールで使用できる、 `TableCell` s`Controls`コレクション。

BoundFields 以外のフィールド型を処理するために、コードを拡張する必要があります、`currentValue`変数に代入するチェック ボックスでの存在を確認、 `TableCell` s`Controls`コレクション。 使用する代わりに`currentValue = gvr.Cells(sortColumnIndex).Text`、このコードを次に置き換えます。


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

このコードは、並べ替えられた列を調べて`TableCell`の任意のコントロールがないかどうかを確認するのには、現在の行の`Controls`コレクション。 存在し、最初のコントロールは、チェック ボックスをオンの場合、`currentValue`変数が設定を [はい] またはいいえ、チェック ボックス s に応じて`Checked`プロパティ。 値を取得する場合は、 `TableCell` s`Text`プロパティ。 このロジックは、GridView に存在する任意の TemplateFields の並べ替えを処理するためにレプリケートできます。

上記のコードの追加、並べ替えのグループ ヘッダーは廃止された CheckBoxField で並べ替えるときに存在するようになりました (図 7 を参照してください)。


[![並べ替えのグループ ヘッダーは現在存在するときに並べ替え、CheckBoxField です。](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**図 7**:、並べ替えのグループ ヘッダーは現在存在するときに並べ替え、CheckBoxField ([フルサイズの画像を表示する をクリックします](creating-a-customized-sorting-user-interface-vb/_static/image19.png))。


> [!NOTE]
> 製品がある場合`NULL`の値をデータベース、 `CategoryID`、 `SupplierID`、または`UnitPrice`フィールド、それらの値は、GridView で空の文字列として既定で表示でこれらの製品の区切り記号の行のテキストを意味`NULL`はカテゴリのような値を読み取ります: (が s は、カテゴリ後名なし: カテゴリを使用するような: 飲み物)。 ここに表示される値の場合は、BoundFields か設定できます[`NullDisplayText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx)テキストを表示するかを割り当てるときに、Render メソッドで条件付きステートメントを追加することができます、`currentValue`に区切り記号行の`Text`プロパティ。


## <a name="summary"></a>まとめ

GridView では、並べ替えのインターフェイスをカスタマイズするための多くの組み込みオプションは含まれません。 ただし、大量の低レベルのコードよりカスタマイズされたインターフェイスを作成するには、GridView のコントロール階層を調整することも可能です。 このチュートリアルでは、GridView を並べ替え可能な個別のグループとそれらのグループの境界をより簡単に識別するための並べ替えグループ区切り行を追加する方法を説明しました。 カスタマイズされた並べ替えインターフェイスの他の例については、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/) s [ASP.NET 2.0 GridView 並べ替えヒントとテクニック](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx)ブログ エントリ。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](sorting-custom-paged-data-vb.md)
