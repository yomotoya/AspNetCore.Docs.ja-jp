---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: ASP.NET ページ (VB) で BLL レベルと DAL レベルの例外処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、挿入、更新、または削除操作の中に例外が発生する必要があります、親しみやすい、わかりやすいエラー メッセージを表示する方法をわかる.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 41a0ec0db4cb2093d7fadd2de7e65ee4874f0063
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394684"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>ASP.NET ページ (VB) で BLL レベルと DAL レベルの例外処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe)または[PDF のダウンロード](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> このチュートリアルは、挿入、更新、または ASP.NET データ Web コントロールの削除操作中に例外が発生する必要があります、親しみやすい、わかりやすいエラー メッセージを表示する方法が表示されます。


## <a name="introduction"></a>はじめに

階層化されたアプリケーション アーキテクチャを使用して ASP.NET web アプリケーションからのデータの操作には、次の 3 つの一般的な手順が含まれます。

1. ビジネス ロジック層のどのようなメソッドが呼び出される必要があり、どのようなパラメーターの値を渡すことを確認します。 パラメーターの値は、ハード コーディングされたは、プログラムによって割り当てまたは入力がユーザーによって入力します。
2. メソッドを呼び出します。
3. 結果を処理します。 データを返す BLL メソッドを呼び出すときに、データ Web コントロールにデータをバインドこれが含まれる可能性があります。 BLL メソッドのデータを変更する場合は、何らかのアクションは、戻り値に基づくまたはに手順 2. で発生した例外を適切に処理を実行するこの含めることができます。

説明したように、[前のチュートリアル](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)ObjectDataSource とデータ Web コントロールの両方は、手順 1. と 3. の拡張ポイントを提供します。 GridView、たとえば、発生の`RowUpdating`にその ObjectDataSource のフィールドの値を割り当てる前にイベント`UpdateParameters`コレクション、 `RowUpdated` ObjectDataSource には、操作が完了した後にイベントが発生します。

手順 1 の中に発生し、それらを使用して入力パラメーターをカスタマイズするか、操作をキャンセルする方法を見てきましたが、イベントを既にについて説明しました。 このチュートリアルでは、操作が完了した後に発生するイベントを有効にします。 その他のものは、これらの投稿レベルのイベント ハンドラーで操作中に例外が発生したかどうかを確認し、既定では、標準の ASP.NET ではなく、親しみやすい、わかりやすいエラー メッセージを表示する画面に正常に処理例外ページ。

これらの投稿レベルのイベントの処理を示すためには、編集可能な GridView では、製品を一覧表示されたページを作成しましょう。 例外の場合、製品の更新が、ASP.NET に発生したときにページに問題が発生したことを説明する GridView の上に短いメッセージが表示されます。 それでは、始めましょう!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>手順 1: 製品の編集可能な GridView を作成します。

前のチュートリアルでは 2 つのフィールドを編集可能な GridView を作成した`ProductName`と`UnitPrice`します。 この必須の追加のオーバー ロードを作成、`ProductsBLL`クラスの`UpdateProduct`メソッドは、次の 3 つの入力パラメーター (製品の名前、unit price、および ID) に製品の各フィールドの対照的パラメーターのみを許可します。 このチュートリアルでは、この手法、もう一度作成、編集可能な GridView、在庫製品の名前、unit、unit price、およびユニットあたりの数量が表示されますが、名、unit price、および単位を編集する在庫でしか使用を実践しましょう。

このシナリオに対応する必要がありますの別のオーバー ロード、`UpdateProduct`メソッドは、4 つのパラメーターを受け取る 1: 製品の名前、単価、在庫、および ID の単位 次のメソッドを追加、`ProductsBLL`クラス。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

この方法で完全なこれら 4 つの特定の製品フィールドを編集できます。 ASP.NET ページを作成する準備ができました。 開く、`ErrorHandling.aspx`ページで、`EditInsertDelete`フォルダー、デザイナーを使用してページに GridView を追加します。 新しい ObjectDataSource では、GridView にバインド マッピング、`Select()`メソッドを`ProductsBLL`クラスの`GetProducts()`メソッドと`Update()`メソッドを`UpdateProduct`オーバー ロードを作成します。


[![次の 4 つの入力パラメーターを受け取る UpdateProduct メソッド オーバー ロードを使用します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**図 1**: 使用して、`UpdateProduct`メソッドをオーバー ロードすることを受け入れる次の 4 つ入力パラメーター ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))。


これで、ObjectDataSource が作成されます、 `UpdateParameters` 4 つのパラメーターおよびフィールドを持つ GridView の各製品のフィールドのコレクション。 ObjectDataSource の宣言型マークアップを割り当てます、`OldValuesParameterFormatString`プロパティ値`original_{0}`、という名前の入力パラメーターの予定がない、BLL クラスのために例外が発生する`original_productID`で渡されます。 この設定を完全宣言型構文を削除することを忘れないでください (既定の値に設定または`{0}`)。

次に、のみを含む GridView ところを省く、 `ProductName`、 `QuantityPerUnit`、 `UnitPrice`、および`UnitsInStock`BoundFields します。 自由と思われるために必要なフィールド レベルの書式を適用する (変更など、`HeaderText`プロパティ)。

前のチュートリアルで書式を設定する方法を説明しました、 `UnitPrice` BoundField 読み取り専用モードと編集モードの両方の通貨として。 同じここで見ていきます。 BoundField の設定が必要ですこのことを思い出してください`DataFormatString`プロパティを`{0:c}`その`HtmlEncode`プロパティを`false`、およびその`ApplyFormatInEditMode`に`true`図 2 に示すように、します。


[![通貨として表示する UnitPrice BoundField を構成します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**図 2**: 構成、 `UnitPrice` BoundField を通貨として表示する ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))。


書式設定、`UnitPrice`編集インターフェイスでの通貨での GridView のイベント ハンドラーの作成を必要とされる`RowUpdating`を通貨形式の文字列を解析するイベントを`decimal`値。 いることを思い出してください、`RowUpdating`の最後のチュートリアルからのイベント ハンドラーが、ユーザーが指定したことを確認するチェックもを`UnitPrice`値。 ただし、このチュートリアルでは、価格を省略するユーザーがみましょう。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

含まれています、GridView、 `QuantityPerUnit` BoundField がこの BoundField 表示用のみにする必要があり、ユーザーは編集できません。 これを配置する BoundFields' を設定するだけ`ReadOnly`プロパティを`true`します。


[![読み取り専用 QuantityPerUnit BoundField を行う](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**図 3**: 作成、 `QuantityPerUnit` BoundField 読み取り専用 ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))。


最後に、GridView のスマート タグの編集を有効にするチェック ボックスを確認します。 次の手順を完了した後、`ErrorHandling.aspx`ページのデザイナーはよう図 4 になります。


[![すべて削除、必要な BoundFields とチェック チェック ボックスの編集を有効にします。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**図 4**: 削除以外のすべての必要な BoundFields とチェックを有効にする編集ボックスをオン ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))。


この時点でのすべての製品の一覧がある`ProductName`、 `QuantityPerUnit`、 `UnitPrice`、および`UnitsInStock`フィールドです。 ただし、のみ、 `ProductName`、 `UnitPrice`、と`UnitsInStock`のフィールドを編集できます。


[![ユーザー今すぐ簡単に編集できます製品の名前、価格、およびストック フィールド内のユニット](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**図 5**: ユーザーできます今すぐ簡単に編集の製品の名前、価格、および単位で在庫フィールド ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))。


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>手順 2: DAL レベルの例外を適切に処理します。

編集可能な GridView は、ユーザーが編集された製品の名前、価格、および在庫数の有効な値を入力するときにすばらしく、不正な値を入力することで例外が発生します。 などを省略すると、`ProductName`値の原因を[NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp)からスローされる、`ProductName`プロパティ、`ProdcutsRow`クラスがその`AllowDBNull`プロパティに設定`false`場合、データベースが、停止、`SqlException`データベースに接続するときに、TableAdapter がスローされます。 操作を行わずに、これらの例外バブル アップ データ アクセス層からビジネス ロジック層で、[ASP.NET] ページで、最後に、ASP.NET ランタイムにします。

Web アプリケーションを構成する方法と、アプリケーションからアクセスしているかどうかに応じて`localhost`、ハンドルされない例外が発生する一般的なサーバー エラー ページ、詳細なエラー レポート、またはユーザー フレンドリな web ページのいずれかにします。 参照してください[ASP.NET のアプリケーション エラーの処理を Web](http://www.15seconds.com/issue/030102.htm)と[customErrors 要素](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx)ASP.NET ランタイムが、キャッチされない例外に応答する方法の詳細についてはします。

図 6 を指定せず、製品を更新する際に発生した画面を示しています、`ProductName`値。 これは、既定の送信されるときに詳細なエラー レポートが表示される`localhost`します。


[![製品の名前が表示されます例外の詳細を省略します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**図 6**: 製品の名前は例外の詳細を表示を省略すると ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))。


このような例外の詳細は、アプリケーションをテストするときに便利ですが、例外が発生した場合、このような画面で、エンド ユーザーを表示するは未満に最適です。 可能性の高いエンドユーザーが不明な`NoNullAllowedException`は理由が原因か。 製品を更新しようとしています。 問題があったことを説明するわかりやすいメッセージをユーザーに表示することをお勧めします。

操作を実行するときに、例外が発生する場合、ObjectDataSource とデータ Web コントロールの両方で投稿レベルのイベントは、それを検出し、ASP.NET ランタイムにバブルアップから例外をキャンセルするための手段を提供します。 この例での GridView のみましょうイベント ハンドラーを作成`RowUpdated`イベントとを決定するかどうか、例外が発生した、そうである場合は、Label Web コントロールで例外の詳細を表示します。

ASP.NET ページの設定にラベルを追加して、開始、`ID`プロパティを`ExceptionDetails`を消去して、`Text`プロパティ。 このメッセージに、ユーザーの目を描画するために次のように設定します。 その`CssClass`プロパティを`Warning`、に追加の CSS クラスは、`Styles.css`前のチュートリアルでのファイル。 この CSS クラスにより、ラベルのテキストは、赤、斜体、太字、特大のフォントで表示されることを思い出してください。


[![ラベルの Web コントロールをページに追加します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**図 7**: ラベルの Web コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))。


例外が発生するこのラベルの Web コントロールは表示のみすぐにした後、設定、`Visible`プロパティを false に、`Page_Load`イベント ハンドラー。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

以降のポストバックでは、最初のページ アクセスでは、次のコードで、`ExceptionDetails`コントロールが持つその`Visible`プロパティに設定`false`します。 GridView ので検出できる、DAL または BLL レベルの例外が発生した場合`RowUpdated`設定は、イベント ハンドラー、`ExceptionDetails`コントロールの`Visible`プロパティを true にします。 Web コントロールのイベント ハンドラーの後に行われるため、`Page_Load`ページ ライフ サイクルのイベント ハンドラーをラベルが表示されます。 ただし、次のポストバックで、`Page_Load`イベント ハンドラーは元に戻す、`Visible`プロパティ`false`、もう一度ビューから非表示にします。

> [!NOTE]
> 設定の必要性を削除または、`ExceptionDetails`コントロールの`Visible`プロパティ`Page_Load`を割り当てることによってその`Visible`プロパティ`false`宣言の構文とそのビュー ステートを (そのの設定を無効にします。`EnableViewState`プロパティを`false`)。 今後のチュートリアルでは、この代替アプローチを使用します。


次の手順は GridView のイベント ハンドラーを作成する、ラベル コントロールを追加、`RowUpdated`イベント。 デザイナーで、GridView を選択、[プロパティ] ウィンドウに移動し、稲妻のアイコン、GridView のイベントを一覧表示をクリックします。 GridView のあるエントリはず`RowUpdating`イベント、このチュートリアルで前にこのイベントのイベント ハンドラーを作成しました。 イベント ハンドラーを作成、`RowUpdated`イベントもします。


![GridView の RowUpdated イベントのイベント ハンドラーを作成します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**図 8**: の GridView のイベント ハンドラーを作成`RowUpdated`イベント


> [!NOTE]
> 分離コード クラス ファイルの上部にあるドロップダウン リストにより、イベント ハンドラーを作成することもできます。 GridView を左側のドロップダウン リストから選択し、`RowUpdated`右側の 1 つからのイベント。


このイベント ハンドラーを作成すると、次のコードが ASP.NET ページの分離コード クラスに追加されます。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

このイベント ハンドラーの 2 番目の入力パラメーターの型のオブジェクトは、 [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx)目的の例外を処理するための 3 つのプロパティを持ちます。

- `Exception` スローされた例外; への参照このプロパティの値には例外がスローされていない場合 `null`
- `ExceptionHandled` 例外が処理されたかどうかを示すブール値、`RowUpdated`イベント ハンドラー場合`false`(既定)、例外が再スローされますが、ASP.NET ランタイムまで percolating
- `KeepInEditMode` 場合設定`true`場合に、編集した GridView 行が編集モードで保持`false`(既定)、GridView の行が、読み取り専用モードに戻ります

次に、コードがかどうかを確認する必要があります`Exception`ない`null`操作の実行中に例外が発生したことを意味します。 この場合にします。

- わかりやすいメッセージを表示、`ExceptionDetails`ラベル
- 例外が処理されたことを示します
- GridView の行を編集モードにしてください。

この次のコードでは、これらの目標を実現します。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

このイベント ハンドラーが始まるかどうかをチェックして`e.Exception`は`null`します。 そうでない場合、`ExceptionDetails`ラベルの`Visible`プロパティに設定されて`true`とその`Text`プロパティを「問題があった、製品を更新しています」。 スローされた実際の例外の詳細が存在する、`e.Exception`オブジェクトの`InnerException`プロパティ。 この内部例外を調べるしに、便利な追加のメッセージが追加されます、特定の型の場合、`ExceptionDetails`ラベルの`Text`プロパティ。 最後に、`ExceptionHandled`と`KeepInEditMode`プロパティに設定されて`true`します。

図 9 は、製品の名前を省略する場合にこのページのスクリーン ショットを示します図 10 では、不正なを入力するときに、結果が表示されます`UnitPrice`値 (-50 の場合)。


[![ProductName BoundField が値を含める必要があります。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**図 9**: `ProductName` BoundField が値を含める必要があります ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))。


[![UnitPrice の負の値は許可されていません](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**図 10**: 負`UnitPrice`値は許可されていません ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))。


設定して、`e.ExceptionHandled`プロパティを`true`、`RowUpdated`イベント ハンドラーに例外を処理したことが示されます。 そのため、例外が ASP.NET ランタイムに伝達されません。

> [!NOTE]
> 図 9 および 10 は、ユーザー入力が無効のために発生した例外を処理するために正常な方法を表示します。 理想的には、ただし、このような無効な入力は、決してリーチ、ビジネス ロジック層最初の段階で、ASP.NET ページを呼び出す前に、ユーザーの入力が有効なことを確認する必要があります、`ProductsBLL`クラスの`UpdateProduct`メソッド。 ビジネス ロジック層に送信されたデータを確実に編集および挿入インターフェイスに検証コントロールを追加する方法を見て、次のチュートリアルでは、ビジネス ルールに準拠しています。 検証コントロールの呼び出しを防ぐだけでなく、`UpdateProduct`メソッドまで、ユーザーが指定したデータが有効ですが、データ エントリの問題を識別するためも、ユーザー エクスペリエンスをより多くの情報を提供します。


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>手順 3: BLL レベルの例外を適切に処理します。

挿入すると、更新、またはデータの削除、データ アクセス層例外がスローされるデータに関連するエラーが発生した場合。 データベースがオフラインである、必要なデータベース テーブルの列に指定すると、値をなかった可能性があります。 またはテーブル レベル制約が違反している可能性があります。 厳密にデータ関連の例外に加えて、ビジネス ロジック層は、ビジネス ルールに違反しているときを示す例外を使用できます。 [ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md)チュートリアルでは、たとえば、ビジネス ルールのチェックを元に追加されます。`UpdateProduct`オーバー ロードします。 具体的には、ユーザーは、廃止として製品をマークが場合製品いないも必要その供給業者によって提供される 1 つだけ。 この条件に違反していた場合、`ApplicationException`がスローされました。

`UpdateProduct`オーバー ロードは、このチュートリアルで作成、禁止するビジネス規則を追加して、`UnitPrice`フィールドを元の 2 倍以上である新しい値に設定されてから`UnitPrice`値。 これを行うには、調整、`UpdateProduct`オーバー ロードするように、このチェックを実行し、スロー、`ApplicationException`規則に違反する場合。 更新されたメソッドが次に示します。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

この変更により、既存の価格に 2 回以上である価格の更新が発生、`ApplicationException`がスローされます。 この BLL 発生した、DAL から発生する例外と同様に`ApplicationException`検出され、gridview の処理は`RowUpdated`イベント ハンドラー。 実際には、`RowUpdated`イベント ハンドラーのコードでは、書き込まれるが正しくこの例外を検出し、表示、`ApplicationException`の`Message`プロパティの値。 図 11 は、ユーザーがその現在の価格 19.95 ドルの 2 倍以上である、50.00 ドルに Chai の価格を更新しようとしたとき画面を示しています。


[![ビジネス ルールは、製品の価格が 2 倍以上の価格の上昇を禁止します。](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**図 11**: ビジネス ルールの製品の価格が 2 倍以上 Disallow 価格の上昇 ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))。


> [!NOTE]
> 理想的には、ビジネス ロジック ルール リファクタリングのうち、`UpdateProduct`メソッドのオーバー ロードと共通のメソッドにします。 これについては、リーダーの演習として残してください。


## <a name="summary"></a>まとめ

挿入、更新、および削除の各操作、データ Web コントロールと ObjectDataSource の関連するイベントを発生させる前と後のレベルをブックエンド実際の操作。 説明したようにこのチュートリアルで前のものでは、編集可能な GridView を使用する場合、GridView の`RowUpdating`ObjectDataSource の後にイベントの起動`Updating`この時点で、更新コマンドが ObjectDataSource に加えられたイベント基になるオブジェクト。 操作が完了すると、ObjectDataSource の`Updated`イベントの起動後に、GridView の`RowUpdated`イベント。

入力パラメーターをカスタマイズするために事前にレベルのイベントまたは投稿レベルのイベントを検査し、操作の結果に応答するために、イベント ハンドラーを作成できます。 後のレベルのイベント ハンドラーは、操作中に例外が発生したかどうかを検出するために最もよく使用されます。 例外が発生した場合、これらの投稿レベルのイベント ハンドラーは、独自の例外を必要に応じて処理できます。 このチュートリアルでは、わかりやすいエラー メッセージを表示することでこのような例外を処理する方法を説明しました。

次のチュートリアルではデータの書式設定に関する問題によって発生した例外が発生する可能性を低減するための方法を見ていきます (、負の値を入力するなど`UnitPrice`)。 具体的には、編集および挿入インターフェイスに検証コントロールを追加する方法について説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [次へ](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
