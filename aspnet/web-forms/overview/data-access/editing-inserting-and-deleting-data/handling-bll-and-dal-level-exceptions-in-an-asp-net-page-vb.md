---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: ASP.NET ページ (VB) で BLL および DAL レベルの例外の処理 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、insert、update、または削除操作の中に例外が発生する必要がありますにわかりやすい、わかりやすいエラー メッセージを表示する方法についておしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b76554b6e8c00dbe3b33de8158b925d7314afb72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>ASP.NET ページ (VB) で BLL および DAL レベルの例外処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe)または[PDF のダウンロード](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> このチュートリアルでは、insert、update、または Web コントロールの ASP.NET データの削除操作中に例外が発生する必要がありますにわかりやすい、わかりやすいエラー メッセージを表示する方法が表示されます。


## <a name="introduction"></a>はじめに

多層アプリケーション アーキテクチャを使用して ASP.NET web アプリケーションからデータを扱うには、次の 3 つの一般的な手順が含まれます。

1. ビジネス ロジック層のどのメソッドが呼び出される必要があるし、どのようなパラメーター値を渡すことを確認します。 入力がユーザーによって入力またはハード コーディングされた、プログラムによって割り当てられた、パラメーターの値が表示できます。
2. メソッドを呼び出します。
3. 結果を処理します。 データを返す BLL メソッドを呼び出すときに必要に応じて、データの Web コントロールにデータをバインドします。 データを変更する BLL メソッド、何らかのアクションは、戻り値に基づくか、手順 2. で発生した任意の例外を適切に処理を実行するこれが含まれます。

説明したとおり、[前のチュートリアル](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)、ObjectDataSource と Web コントロールのデータの両方の手順 1 および 3 の機能拡張ポイントを提供します。 GridView はたとえば、発生の`RowUpdating`、ObjectDataSource にそのフィールドの値を割り当てる前にイベント`UpdateParameters`コレクション以外の場合はその`RowUpdated`ObjectDataSource によって操作が完了した後にイベントが発生します。

イベントを手順 1. の中に発生し、前述のとおりを使用して、入力パラメーターをカスタマイズするか、操作をキャンセルする方法を既にについて説明しました。 このチュートリアルではおありますに注目操作が完了した後に起動するイベントです。 ごことができます、特に、これらの後のレベルのイベント ハンドラーで操作中に例外が発生したかどうかを判断し、標準の ASP.NET には、既定よりも、画面のわかりやすい、わかりやすいエラー メッセージを表示する適切に処理例外のページです。

これらの後のレベルのイベントの処理を示すためには、編集可能な GridView では、製品を一覧するページを作成してみましょう。 例外がある場合、製品の更新、ASP.NET が発生すると、問題が発生したことを説明する GridView の上に短いメッセージをページが表示されます。 開始しましょう!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>手順 1: 製品の編集可能な GridView の作成

2 つのフィールドの編集可能な GridView で作成した前のチュートリアルで`ProductName`と`UnitPrice`です。 この必須の他のオーバー ロードを作成する、`ProductsBLL`クラスの`UpdateProduct`メソッドは、次の 3 つの入力パラメーター (製品の名前、unit price、および ID) に製品の各フィールドの対照的なパラメーターのみを受け入れるものです。 このチュートリアルでは、在庫、製品の名前、unit、unit price、および単位あたりの数量が表示されますが、株価を編集することで名前、単価、および単位の許可だけを編集可能な GridView を作成するこの手法をもう一度を練習してみましょう。

このシナリオに対応する必要がありますの別のオーバー ロード、`UpdateProduct`メソッドは、次の 4 つのパラメーターを受け取る 1: 製品の名前、単価、株価、および ID ユニット 次のメソッドを追加、`ProductsBLL`クラス。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

この方法で完了、4 つの特定の製品フィールドを編集できます。 ASP.NET ページを作成する準備ができました。 開いている、 `ErrorHandling.aspx`  ページで、`EditInsertDelete`フォルダー GridView ページを追加、デザイナーを使用しています。 新しい ObjectDataSource、GridView にバインド マッピング、`Select()`メソッドを`ProductsBLL`クラスの`GetProducts()`メソッドおよび`Update()`メソッドを`UpdateProduct`先ほど作成したオーバー ロードします。


[![次の 4 つの入力パラメーターを受け取る UpdateProduct メソッド オーバー ロードを使用します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**図 1**: を使用して、`UpdateProduct`メソッド オーバー ロードすることを受け入れる次の 4 つ入力パラメーター ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


これで、ObjectDataSource が作成されます、 `UpdateParameters` 4 つのパラメーター、およびフィールドを持つ GridView の各製品フィールドのコレクション。 ObjectDataSource の宣言型マークアップを割り当てます、`OldValuesParameterFormatString`プロパティ値`original_{0}`、BLL クラスのという名前の入力パラメーターは必要ないために、例外が発生する`original_productID`で渡されます。 忘れずにこの設定はまったく宣言構文から削除する (既定値に設定または`{0}`)。

次に、のみを含む GridView サイズを縮小、 `ProductName`、 `QuantityPerUnit`、 `UnitPrice`、および`UnitsInStock`BoundFields です。 自由に、フィールド レベルの書式を適用するために必要な設定 (変更など、`HeaderText`プロパティ)。

前のチュートリアルで書式設定する方法を説明しました、`UnitPrice`読み取り専用モードと編集モードの両方の通貨として BoundField です。 ここでは、同じしましょう。 これの必要な BoundField を設定することに注意してください`DataFormatString`プロパティを`{0:c}`、その`HtmlEncode`プロパティを`false`、およびその`ApplyFormatInEditMode`に`true`図 2 に示すように、します。


[![通貨として表示する UnitPrice BoundField を構成します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**図 2**: 構成、 `UnitPrice` BoundField 通貨として表示する ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


書式設定、 `UnitPrice` gridview のイベント ハンドラーの作成、編集するインターフェイスに通貨の必要に応じて`RowUpdating`に通貨形式の文字列を解析するイベント、`decimal`値。 注意してください、`RowUpdating`前回のチュートリアルからのイベント ハンドラーをチェックすることを確認するユーザーによって指定された、`UnitPrice`値。 ただし、このチュートリアルでは許可価格を省略する場合、ユーザーがみましょう。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

含まれています、GridView、 `QuantityPerUnit` BoundField がこの BoundField 表示のためだけにする必要があり、ユーザーが編集できないする必要があります。 これを配置する BoundFields' を設定するだけ`ReadOnly`プロパティを`true`です。


[![QuantityPerUnit BoundField を読み取り専用](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**図 3**: 作成、 `QuantityPerUnit` BoundField 読み取り専用 ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


最後に、GridView のスマート タグからの編集を有効にするチェック ボックスをチェックします。 次の手順を完了した後、`ErrorHandling.aspx`ページのデザイナーはよう図 4 になります。


[![残してすべて削除して、必要な BoundFields とチェック有効にする チェック ボックスを編集](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**図 4**: 削除以外のすべての必要な BoundFields とチェックを有効にする編集チェック ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


この時点でのすべての製品の一覧がある`ProductName`、 `QuantityPerUnit`、 `UnitPrice`、および`UnitsInStock`フィールドです。 ただし、のみ、 `ProductName`、 `UnitPrice`、および`UnitsInStock`フィールドを編集することができます。


[![ユーザーを今すぐを簡単に編集の製品の名前、価格、およびストックのフィールド内のユニット](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**図 5**: ユーザーできるようになりました簡単に編集の製品の名前、価格、および単位の在庫フィールド ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>手順 2: 正常に DAL レベルの例外の処理

編集可能な GridView は、ユーザーが編集された製品の名前、価格、および在庫の有効な値を入力するときにすばらしくは、無効な値を入力するで例外が発生します。 などを省略すると、`ProductName`値の原因として、 [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp)以降にスローされる、`ProductName`プロパティに、`ProdcutsRow`クラスにはその`AllowDBNull`プロパティに設定`false`以外の場合、データベースがダウン、`SqlException`データベースに接続しようとするときに、TableAdapter がスローされます。 操作を行わずに、これらの例外のバブルをデータ アクセス層からビジネス ロジック層で、[ASP.NET] ページで、最後に、ASP.NET ランタイムにします。

Web アプリケーションを構成する方法と、アプリケーションからアクセスしているかどうかに応じて`localhost`、一般的なサーバー エラー ページ、詳細なエラー レポート、またはわかりやすい web ページのいずれかでハンドルされない例外が発生します。 参照してください[Web アプリケーション Error Handling in ASP.NET](http://www.15seconds.com/issue/030102.htm)と[customErrors 要素](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx)ASP.NET ランタイムがキャッチされない例外に応答する方法の詳細についてです。

図 6 を指定せず、製品を更新しようとするときに発生した画面を示しています、`ProductName`値。 これはを介して取り込まれたときに、詳細なエラー レポートが表示される既定の`localhost`します。


[![製品の名前が表示されます例外の詳細を省略すること](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**図 6**: 製品の名前は例外の詳細を表示を省略すると ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


このような例外の詳細がアプリケーションをテストする際に役立つ、エンド ユーザー例外が発生した場合、このような画面提示してきたより小さい最適です。 可能性の高いエンド ユーザーが不明な`NoNullAllowedException`が発生した理由や、します。 製品を更新しようとしています。 問題が発生したことを説明するわかりやすいメッセージをユーザーに表示することをお勧めします。

操作を実行するときに、例外が発生する場合、ObjectDataSource とデータの Web コントロールの両方で後レベルのイベントは、それを検出し、ASP.NET ランタイムにバブルアップから例外をキャンセルするための手段を提供します。 例では、作成しましょうイベント ハンドラーの GridView の`RowUpdated`かどうかを例外が発生した場合は、ラベルの Web コントロールで例外の詳細を表示するイベントです。

ASP.NET ページの設定にラベルを追加して、開始、`ID`プロパティを`ExceptionDetails`を消去して、`Text`プロパティです。 このメッセージに、ユーザーの目を描画するために次のように設定します。 その`CssClass`プロパティを`Warning`、CSS クラスを追加しましたが、`Styles.css`前のチュートリアルでのファイルです。 再呼び出しと、赤、斜体、太字、特大のフォントで表示されるラベルのテキストがこの CSS クラス。


[![ラベルの Web コントロールをページに追加します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**図 7**: ラベル Web コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


例外が発生しましたのでこのラベルの Web コントロールを表示するだけ即座にした後、設定、`Visible`プロパティが false で、`Page_Load`イベントのハンドラー。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

以降のポストバックが、最初のページ アクセスでこのコードを使って、`ExceptionDetails`コントロールが持つ、`Visible`プロパティに設定`false`です。 GridView ので検出したことができます、DAL または BLL レベルの例外の発生時に`RowUpdated`設定は、イベント ハンドラー、`ExceptionDetails`コントロールの`Visible`プロパティを true にします。 Web コントロールのイベント ハンドラーの後に行われるため、`Page_Load`ページ ライフ サイクルのイベント ハンドラーをラベルが表示されます。 ただし、ポストバック時に、[次へ] を`Page_Load`イベント ハンドラーに戻す、`Visible`プロパティ`false`、もう一度ビューから非表示にします。

> [!NOTE]
> または、削除することでした設定の必要性、`ExceptionDetails`コントロールの`Visible`プロパティ`Page_Load`を割り当てることでその`Visible`プロパティ`false`宣言の構文とそのビュー ステートを (そのの設定を無効にするのに`EnableViewState`プロパティを`false`)。 この代替方法は、今後のチュートリアルでは使用されます。


次の手順が gridview のイベント ハンドラーを作成するのにはラベル コントロールを追加、`RowUpdated`イベント。 デザイナーで GridView を選択し、[プロパティ] ウィンドウに移動 GridView のイベントを一覧表示、表示される稲妻アイコンをクリックします。 既にあります gridview のエントリがある`RowUpdating`イベントでは、このチュートリアルで以前にこのイベントのイベント ハンドラーが作成しました。 イベント ハンドラーを作成、`RowUpdated`イベントもします。


![GridView の RowUpdated イベントのイベント ハンドラーを作成します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**図 8**: gridview のイベント ハンドラーを作成する`RowUpdated`イベント


> [!NOTE]
> 分離コード クラス ファイルの上部にあるドロップダウン リストを使用して、イベント ハンドラーを作成することもできます。 GridView を左側のドロップダウン リストから選択し、`RowUpdated`右側の 1 つからのイベントです。


このイベント ハンドラーを作成すると、次のコードが ASP.NET ページの分離コード クラスに追加されます。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

このイベント ハンドラーの 2 番目の入力パラメーターの型のオブジェクトは、 [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)、例外処理の目的の 3 つのプロパティを持ちます。

- `Exception` スローされた例外への参照このプロパティの値には例外がスローされなかった場合に、 `null`
- `ExceptionHandled` 例外が処理されたかどうかを示すブール値、`RowUpdated`イベント ハンドラーです`false`(既定)、例外が再スローされる、最大で ASP.NET ランタイム percolating。
- `KeepInEditMode` 場合設定`true`場合に、編集モードで編集された GridView 行まま`false`(既定)、GridView の行が、読み取り専用モードに戻ります

次に、コードがかどうかをチェックする必要があります`Exception`は`null`操作の実行中に例外が発生したことを意味します。 大文字と小文字の場合は、します。

- わかりやすいメッセージを表示、`ExceptionDetails`ラベル
- 例外が処理されたことを示します
- GridView の行を編集モードにしてください。

次のコードには、これらの目標が達成されます。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

このイベント ハンドラーが始まるかどうかをチェックして`e.Exception`は`null`します。 ない場合は、`ExceptionDetails`ラベルの`Visible`プロパティに設定されている`true`とその`Text`プロパティを「問題があった製品を更新します」。 実際にスローされた例外の詳細が存在する、`e.Exception`オブジェクトの`InnerException`プロパティです。 この内部例外が確認され、特定の型は場合に、便利な追加のメッセージが追加されます。、`ExceptionDetails`ラベルの`Text`プロパティです。 最後に、`ExceptionHandled`と`KeepInEditMode`プロパティに設定されて`true`です。

図 9; 製品の名前を省略する場合は、このページのスクリーン ショットを示しています図 10 が不正なを入力するときに結果を示しています`UnitPrice`値 (-50)。


[![ProductName BoundField は値を含める必要があります。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**図 9**: `ProductName` BoundField が値を含める必要があります ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![負の UnitPrice の値が許可されていません](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**図 10**: 負`UnitPrice`値は許可されていません ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


設定して、`e.ExceptionHandled`プロパティを`true`、`RowUpdated`イベント ハンドラーに例外が処理されることが示されます。 そのため、例外は、ASP.NET ランタイムまで反映されません。

> [!NOTE]
> 図 9 および 10 は、ユーザー入力が無効のために発生する例外を処理する適切な方法を示しています。 理想的には、ただし、このような入力が無効です、決して reach ビジネス ロジック層最初の段階で、ASP.NET ページを呼び出す前に、ユーザーの入力が有効なことを確認する必要があります、`ProductsBLL`クラスの`UpdateProduct`メソッドです。 ビジネス ロジック層に送信されたデータを確実に編集および挿入のインターフェイスに検証コントロールを追加する方法をお見せ [次へ]、チュートリアルでは、ビジネス ルールに準拠しています。 検証コントロールの呼び出しを防ぐだけでなく、`UpdateProduct`メソッドまで、ユーザーが指定したデータが有効ですが、データ エントリの問題を識別するためも有益なユーザー エクスペリエンスを提供します。


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>手順 3: が BLL レベルの例外を適切に処理

挿入すると、更新、またはデータの削除、データ アクセス層可能性があります例外がスロー データ関連のエラーが発生した場合。 データベースがオフラインになる、必要なデータベース テーブルの列がされていない値が指定されて、またはテーブル レベル制約が違反している可能性があります。 厳密にデータ関連の例外に加えて、ビジネス ロジック層はビジネス ルールに違反しているときを示すために例外を使用できます。 [ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md)チュートリアルでは、たとえば、ビジネス ルール チェックを元に追加されます。`UpdateProduct`オーバー ロードします。 具体的には、ユーザーは、提供が中止されたとして製品をマークするが場合、おために必要な製品が 1 つだけをその業者によって提供されるをされないことです。 このような状況が発生した場合、`ApplicationException`がスローされました。

`UpdateProduct`オーバー ロードは、このチュートリアルで作成、禁止するビジネス ルールを追加してみましょう。、`UnitPrice`から元の 2 倍以上である新しい値に設定されているフィールド`UnitPrice`値。 これを実現する、調整、`UpdateProduct`をスローし、このチェックを実行できるように、オーバー ロード、`ApplicationException`規則に違反する場合。 更新されたメソッドが次に示します。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

この変更は、既存の価格 2 倍以上である価格の更新が発生することは、`ApplicationException`がスローされます。 この BLL 発生した、DAL から発生する例外と同じように`ApplicationException`を検出および gridview の処理`RowUpdated`イベント ハンドラー。 実際には、`RowUpdated`イベント ハンドラーのコードでは、書き込まれるは正しくこの例外を検出し、表示、`ApplicationException`の`Message`プロパティの値。 図 11 は、スクリーン ショット、ユーザーがその現在の価格 $19.95 の 2 倍以上である、50.00 ドルを Chai の価格を更新しようとしたときを示します。


[![ビジネス ルールは、製品の価格が 2 倍以上価格の上昇を禁止します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**図 11**: ビジネス ルールの製品の価格が 2 倍以上 Disallow 価格の上昇 ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> 理想的には、ビジネス ロジック ルールはリファクタリングを行うことのうち、`UpdateProduct`メソッドのオーバー ロードと、一般的なメソッドにします。 これは、操作は、リーダーの練習としてままです。


## <a name="summary"></a>まとめ

挿入、更新、および操作を削除すると、データ Web コントロールと関連する ObjectDataSource イベントを発生させる前と後のレベルそのブックエンド実際の操作です。 示したようにこのチュートリアルと前の 1 つでは、編集可能な GridView を操作するとき、GridView の`RowUpdating`ObjectDataSource の後に、イベントの起動`Updating`この時点で、更新コマンドに対する ObjectDataSource のイベント基になるオブジェクト。 操作が完了すると、ObjectDataSource の`Updated`GridView の後に、イベントの起動`RowUpdated`イベント。

後レベルのイベントを検査し、操作の結果に対応するために、入力パラメーターをカスタマイズするために事前レベルのイベント用のイベント ハンドラーを作成できます。 後のレベルのイベント ハンドラーは、操作中に例外が発生したかどうかを検出するために最もよく使用されます。 例外を発生した場合にこれらの後のレベルのイベント ハンドラーは必要に応じて、独自の例外を処理できます。 このチュートリアルでは、わかりやすいエラー メッセージを表示することによってこのような例外を処理する方法を説明しました。

次のチュートリアルでは、データの書式設定に関する問題によって発生した例外の可能性を軽減する方法を会いしましょう (、負の値を入力するなど`UnitPrice`)。 具体的には、編集および挿入のインターフェイスに検証コントロールを追加する方法について見ていきます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Liz Shulok しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [次へ](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
