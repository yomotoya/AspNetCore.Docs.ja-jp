---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: "バッチ挿入する (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "単一の操作で複数のデータベース レコードを挿入する方法を説明します。 ユーザー インターフェイス レイヤーは、ユーザーに複数の n の入力を許可する GridView を拡張しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 9dc18e259da24d71464a156a70a85cfc9a1745ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="batch-inserting-c"></a>バッチ挿入する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip)または[PDF のダウンロード](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> 単一の操作で複数のデータベース レコードを挿入する方法を説明します。 ユーザー インターフェイス レイヤーは、ユーザーに新しいレコードの複数の入力を許可する GridView を拡張します。 データ アクセス層は、すべての挿入が成功するすべての挿入をロールバックすることを確認するトランザクション内で複数の Insert 操作をラップします。


## <a name="introduction"></a>はじめに

[バッチ更新](batch-updating-cs.md)チュートリアルは複数のレコードが編集可能なインターフェイスを表示する GridView コントロールのカスタマイズについて説明しました。 ページにアクセスしたユーザーでした一連の変更を行い、1 つのボタンのクリックで、バッチ更新を実行します。 ユーザーが通常 1 つの go の複数のレコードを更新の場合、このようなインターフェイスが無数のクリックを保存できます既定値と比較した場合、キーボード、マウスのコンテキストの切り替え行ごとに探索された最初の編集機能、 [、。挿入、更新、および削除するデータの概要](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルです。

この概念は、レコードを追加するときにも適用できます。 想像してみてくださいここで Northwind traders 一般的お客様からの出荷を特定のカテゴリの製品の数を含むサプライヤーからです。 例として、6 つの異なる tea とコーヒー製品の出荷東京 Traders から受け取った場合があります。 DetailsView コントロールを通じて、一度に 1 つの 6 つの製品を入力すると、それらが何度も多くの同じ値を選択する: 同じカテゴリ (飲み物)、同じ製造元 (東京 Traders) を選択する必要があります、同じ値 (が廃止されましたFalse の場合)、および順序の値 (0) で同じ単位です。 この反復的なデータ エントリだけ時間がかかりはありませんが間違いを犯しにくいです。

簡単な作業では、ユーザーが、業者とカテゴリ 1 回、一連の製品名および単位の価格を入力し、データベースに新しい製品を追加するボタンをクリックを選択できるようにするインターフェイスを挿入するバッチを作成できます (図 1 を参照してください)。 各製品が追加されるため、その`ProductName`と`UnitPrice`データ フィールドには、テキスト ボックスに入力した値が割り当てられますときにその`CategoryID`と`SupplierID`値には上位 fo フォームで DropDownLists から値が割り当てられます。 `Discontinued`と`UnitsOnOrder`値のハードコード値に設定されます`false`と 0、それぞれします。


[![バッチ挿入インターフェイス](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**図 1**:、バッチ挿入インターフェイス ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image3.png))


このチュートリアルでは図 1 に示すインターフェイスを挿入するバッチを実装するページを作成します。 として前の 2 つのチュートリアルでは、ラップ、挿入原子性が確保するトランザクションのスコープ内でします。 開始 s を let!

## <a name="step-1-creating-the-display-interface"></a>手順 1: 作成、表示インターフェイス

このチュートリアルは 2 つの領域に分割された 1 つのページで構成されます。 表示領域および挿入する領域です。 このステップを作成、表示インターフェイスの製品 GridView で示し製品の出荷を処理するというタイトルのボタンが含まれています。 このボタンがクリックされたときに表示インターフェイスは図 1 に示されている挿入のインターフェイスに置き換えられます。 [キャンセル] ボタンをクリックするか、または表示インターフェイスが出荷から商品の追加後に返します。 手順 2. で挿入するインターフェイスを作成します。

これの 1 つのみが表示されている、一度に 2 つのインターフェイスを持つページを作成する場合の各インターフェイス通常内に配置されて、[パネル Web コントロール](http://www.w3schools.com/aspnet/control_panel.asp)の他のコントロールのコンテナーとして機能します。 そのため、当社のページには、インターフェイスごとに 2 つのパネル コントロール 1 つがあります。

開いて開始、 `BatchInsert.aspx`  ページで、`BatchData`フォルダーと、ツールボックスからデザイナーにドラッグするパネル (図 2 を参照してください)。 パネル s 設定`ID`プロパティを`DisplayInterface`です。 デザイナーにパネルを追加するときにその`Height`と`Width`プロパティがそれぞれの「50 px」と 125px、に設定されます。 [プロパティ] ウィンドウからこれらのプロパティ値をクリアします。


[![ツールボックスからデザイナーに表示するパネルをドラッグします。](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**図 2**: ツールボックスからデザイナーに表示するパネルをドラッグして ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image6.png))


次に、ボタンと GridView コントロールをパネルにドラッグします。 ボタン s 設定`ID`プロパティを`ProcessShipment`とその`Text`製品の出荷を処理するプロパティです。 集合 GridView s`ID`プロパティを`ProductsGrid`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`ProductsDataSource`です。 構成からそのデータをプルする ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッドです。 この GridView はデータ表示のみに使用されるため、更新、挿入でドロップダウン リストを設定し、(なし) タブを削除します。 データ ソース構成ウィザードを完了するには、[完了] をクリックします。


[![ProductsBLL クラスの GetProducts メソッドから返されるデータを表示します。](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**図 3**: から返されたデータを表示、`ProductsBLL`クラス s`GetProducts`メソッド ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image9.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**図 4**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image12.png))


ObjectDataSource ウィザードの完了後は、Visual Studio は BoundFields と製品データ フィールドの CheckBoxField を追加します。 削除以外のすべての`ProductName`、 `CategoryName`、 `SupplierName`、 `UnitPrice`、および`Discontinued`フィールドです。 自由見た目をカスタマイズしてください。 書式を設定することにしました、`UnitPrice`を通貨値としてのフィールド、フィールドの順序を変更し、フィールドのいくつかの名前を変更`HeaderText`値。 ページング サポートを GridView s のスマート タグのページングを有効にして並べ替えを有効にするチェック ボックスをチェックして並べ替えなど、GridView 構成もできます。

パネル、ボタン、GridView、ObjectDataSource コントロールを追加して GridView のフィールドのカスタマイズした後、ページの宣言型マークアップを次のようになります。


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

開始タグと終了内のボタンと GridView マークアップに表示されるメモ`<asp:Panel>`タグ。 これらのコントロール内にあるため、`DisplayInterface`パネルで非表示にできますパネル s を設定するだけで`Visible`プロパティを`false`です。 手順 3 をプログラムで、s パネルを変更する見ます`Visible`ボタンへの応答のプロパティを非表示にする他の中に 1 つのインターフェイスを表示する をクリックします。

ブラウザーを使って作業の進行状況を表示するのにはしばらくを実行します。 図 5 では、一度に 10 個の製品を一覧表示する GridView 上の製品の出荷を処理するボタンが表示されます。


[![GridView は、製品を一覧表示し、並べ替えとページング機能を提供しています](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**図 5**: GridView、製品と一覧表示し、並べ替え、ページング機能 ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>手順 2: 挿入インターフェイスの作成

表示インターフェイスと完了すると、挿入のインターフェイスを作成する準備ができたらします。 このチュートリアルでは、業者とカテゴリの 1 つの値の入力を要求し、最大 5 つの製品の名前と価格の単位の値を入力するユーザーを挿入するインターフェイスを作成する s を使用できます。 このインターフェイスを持つユーザーは 1 ~ 5 の新製品の同じカテゴリと供給業者、すべてを共有しますが、一意の製品の名前と価格を追加できます。

ツールボックスから、デザイナーの既存の下に配置することにパネルをドラッグして開始`DisplayInterface`パネルです。 設定、`ID`このプロパティは、パネルを新しく追加された`InsertingInterface`設定とその`Visible`プロパティを`false`です。 設定するコードを追加して、`InsertingInterface`パネル s`Visible`プロパティを`true`手順 3. でします。 パネル s もクリア`Height`と`Width`プロパティの値。

次に、図 1 に示す挿入インターフェイスを作成する必要があります。 このインターフェイスは、さまざまな HTML 技法で作成できますが、非常に簡単な 1 つを使用します。 4 列、7 つの行のテーブルです。

> [!NOTE]
> HTML マークアップを入力するときに`<table>`要素 (_n)、ソース ビューを使用します。 Visual Studio に追加するためのツールは、 `<table>` 、デザイナー内の要素、デザイナーを組み込むすべてすぎる許容を排除そらせる`style`マークアップに設定します。 作成したところ、`<table>`マークアップ、通常に返す Web コントロールを追加し、それらのプロパティを設定するデザイナー。 静的な HTML を使用する (_n) であらかじめ決められた列と行のテーブルを作成するときではなく、 [Table Web コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx)テーブル Web コントロール内にある Web コントロールにアクセスできるだけを使用しているため、`FindControl("controlID")`パターン。 コントロールをプログラムで構築するテーブル Web から動的にサイズ変更テーブル (行または列を持つがいくつかのデータベースまたはユーザー指定の条件に基づくもの) に対してテーブル Web コントロールを使用には、ただし、します。


次のマークアップ内の入力、`<asp:Panel>`のタグ、`InsertingInterface`パネル。


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

これは、`<table>`マークアップでは、すべての Web コントロールは含まれませんまだ、それらを一時的に追加します。 `<tr>`要素には、特定の CSS クラス設定が含まれています:`BatchInsertHeaderRow`業者とカテゴリ DropDownLists はどこで; ヘッダー行に`BatchInsertFooterRow`出荷とキャンセル ボタンから追加の製品がどこで; フッター行と交互`BatchInsertRow`と`BatchInsertAlternatingRow`TextBox コントロールの価格の製品と単位を格納する行の値。 I の対応する CSS クラスを作成したら、 `Styles.css` ƒošâ、GridView と DetailsView 挿入のインターフェイスを提供するファイルで制御はこれらのチュートリアル全体で使用します。 これらの CSS クラスは、以下に示します。


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

このマークアップを入力すると、デザイン ビューに戻ります。 これは、`<table>`図 6 に示すように、デザイナーで、4 列、7 つの行のテーブルとして表示する必要があります。


[![挿入するインターフェイスがあり、4 つの列、7 つの行のテーブルの](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**図 6**:、挿入するインターフェイスがあり、4 つの列、7 つの行のテーブルの ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image18.png))


今すぐ再おの Web コントロールを挿入するインターフェイスに追加する準備はできました。 2 つの DropDownLists をツールボックスから、供給業者およびカテゴリのいずれかのいずれかのテーブルに該当するセルにドラッグします。

業者の DropDownList の s を設定`ID`プロパティを`Suppliers`という名前の新しい ObjectDataSource にバインド`SuppliersDataSource`です。 構成からそのデータを取得する新しい ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers` タブ (なし) をドロップダウン リストのメソッドと、更新プログラムを設定します。 ウィザードを完了するには、[完了] をクリックします。


[![ObjectDataSource SuppliersBLL クラスの GetSuppliers メソッドを使用して構成します。](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**図 7**: 構成を使用する ObjectDataSource、`SuppliersBLL`クラス s`GetSuppliers`メソッド ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image21.png))


`Suppliers` DropDownList の表示、`CompanyName`データ フィールドと使用、`SupplierID`データがフィールドとしてその`ListItem`の値。


[![CompanyName データ フィールドが表示され、値としての SupplierID の使用](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**図 8**: 表示、`CompanyName`データ フィールドと使用`SupplierID`値として ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image24.png))


2 番目の DropDownList の名前を付けます`Categories`という名前の新しい ObjectDataSource にバインド`CategoriesDataSource`です。 構成、 `CategoriesDataSource` ObjectDataSource を使用する、`CategoriesBLL`クラスの`GetCategories`メソッド以外の設定を (なし)、UPDATE および DELETE のタブで、ドロップダウン リストを一覧表示が完了ウィザードを完了します。 最後に、DropDownList 表示がある、`CategoryName`データ フィールドと使用、`CategoryID`値として。

これら 2 つの DropDownLists を追加し、適切に構成された ObjectDataSources にバインドした後、画面はよう図 9 になります。


[![ヘッダー行が含まれています、サプライヤーとカテゴリ DropDownLists](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**図 9**:、ヘッダーには行が含まれています、`Suppliers`と`Categories`DropDownLists ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image27.png))


これで、名前と各新製品の価格を収集するためのテキスト ボックスを作成する必要があります。 ツールボックスからデザイナーに 5 つの製品の名前と価格の行の各テキスト ボックス コントロールをドラッグします。 設定、`ID`にテキスト ボックスのプロパティ`ProductName1`、 `UnitPrice1`、 `ProductName2`、 `UnitPrice2`、 `ProductName3`、`UnitPrice3`のようにします。

単位の価格設定、テキスト ボックスの後に、CompareValidator を追加、`ControlToValidate`プロパティを適切な`ID`します。 設定も、`Operator`プロパティを`GreaterThanEqual`、 `ValueToCompare` 0、および`Type`に`Currency`です。 これらの設定では、CompareValidator、価格を入力した場合、有効な通貨値である 0 以上であることを確認するように指示します。 設定、`Text`プロパティを\*、および`ErrorMessage`価格にする必要がありますより大きいまたは 0 に等しい。 また、通貨記号を省略してください。

> [!NOTE]
> 挿入のインターフェイスには含まれません、RequiredFieldValidator コントロールも、`ProductName`フィールドで、`Products`データベース テーブルが許可しない`NULL`値。 これは、ユーザーが最大 5 つの製品を入力できるようにする必要があるためです。 たとえば、ユーザーの最初の 3 つの行の製品名および単価を提供する場合は、最後の 2 つの行を空白 d だけ新しい 3 つの製品に追加システム。 `ProductName`は必要に応じて、ただし、する必要がプログラムで確認している場合、単価が入力された対応する製品名の値が指定されています。 手順 4 で、この確認を取り上げるです。


ユーザー入力を検証するには、値には、通貨記号が含まれている場合に、CompareValidator が無効なデータを報告します。 各単価の価格を入力するときに、通貨記号を省略する場合、ユーザーに指示する視覚上の手掛かりとして機能するテキスト ボックスの前に、$ を追加します。

最後に、内の ValidationSummary コントロールを追加、`InsertingInterface`パネルで、設定、`ShowMessageBox`プロパティを`true`とその`ShowSummary`プロパティを`false`です。 これらの設定で害のある TextBox コントロールの横にアスタリスクが表示場合は、ユーザーは、無効な単位の価格値を入力、および、ValidationSummary 以前指定したエラー メッセージを表示するクライアント側のメッセージ ボックスが表示されます。

この時点では、画面は図 10 のようになります。


[![挿入インターフェイス製品のテキスト ボックスは、名前と価格](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**図 10**:、挿入インターフェイスようになりましたが含まれていますテキスト ボックスの製品の名前と価格 ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image30.png))


次に、製品の追加を出荷し、[キャンセル] ボタンからフッター行に追加する必要があります。 ドラッグ 2 ボタン コントロールをツールボックスから、挿入するインターフェイスのフッターにボタンを設定`ID`プロパティ`AddProducts`と`CancelButton`と`Text`出荷し、[キャンセル] から製品をそれぞれ追加するプロパティです。 さらに、設定、`CancelButton`コントロール s`CausesValidation`プロパティを`false`です。

最後に、2 つのインターフェイスのステータス メッセージを表示するラベル Web コントロールを追加する必要があります。 たとえば、ユーザーでは、新しい製品の出荷が正常に追加、たい表示インターフェイスを取得して、確認メッセージを表示します。 ただし、ユーザーは、新しい製品では、製品名をオフのままの価格を提供する場合は、以降の警告メッセージを表示する必要があります、`ProductName`フィールドは必須です。 このメッセージの両方のインターフェイスを表示する必要があります、ため、パネルの外部でページの上部に配置します。

Label Web コントロールをツールボックスからデザイナー内のページのトップへドラッグします。 設定、`ID`プロパティを`StatusLabel`を out オフ、`Text`プロパティ、およびセット、`Visible`と`EnableViewState`プロパティを`false`です。 前のチュートリアルでこれまで見てきたようを設定、`EnableViewState`プロパティを`false`をプログラムで s のラベルのプロパティ値を変更し、それ以降のポストバック時に、既定値に自動的に元に戻すことができます。 これには、それ以降のポストバック時に表示されなくなります何らかのユーザー アクションへの応答で、ステータス メッセージを表示するためのコードが簡略化します。 最後に、設定、`StatusLabel`コントロール s`CssClass`で警告で、CSS クラスの名前を指定するプロパティが定義されている`Styles.css`大きな、斜体、太字、赤いフォントでテキストを表示します。

図 11 は、ラベルを追加し、構成した後、Visual Studio デザイナーを示しています。


[![2 つのパネル コントロール上 StatusLabel コントロールを配置します。](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**図 11**: の場所、`StatusLabel`コントロール上の 2 つパネル コントロール ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>手順 3: 表示の切り替えとインターフェイスを挿入します。

この時点で、表示と 2 つのタスクとままインターフェイス、ですが再挿入するマークアップを完了しました。

- 表示の切り替えとインターフェイスを挿入します。
- データベースに配送中、製品の追加

現時点では、表示インターフェイスは表示が、挿入のインターフェイスが非表示にします。 これは、ため、`DisplayInterface`パネル s`Visible`プロパティに設定されている`true`(既定値) の場合は、中に、`InsertingInterface`パネル s`Visible`プロパティに設定されている`false`です。 単に秒の各コントロールを切り替える必要があります、2 つのインターフェイス間で切り替える`Visible`プロパティの値。

製品の出荷を処理するボタンがクリックされたときに、表示インターフェイスから挿入インターフェイスに移動することができます。 そのため、このボタンのイベント ハンドラーを作成`Click`を次のコードを含むイベント。


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

このコードは単に非表示に、`DisplayInterface`パネルを表示して、`InsertingInterface`パネルです。

次に、製品を出荷し、[キャンセル] ボタンの挿入インターフェイス コントロールからの追加のイベント ハンドラーを作成します。 これらのボタンのいずれかをクリックすると表示インターフェイスに戻す必要があります。 作成`Click`両方のイベント ハンドラー ボタン コントロールを呼び出すことができるように`ReturnToDisplayInterface`メソッドはすぐに追加します。 非表示にするだけでなく、`InsertingInterface`パネルとを示す、`DisplayInterface`パネル、`ReturnToDisplayInterface`メソッドは、編集済みの状態の Web コントロールを返す必要があります。 DropDownLists を設定する必要があります`SelectedIndex`0 およびを消去するプロパティを`Text`の TextBox コントロールのプロパティです。

> [!NOTE]
> 何が起こるかを検討場合おでした t は、表示インターフェイスに戻る前に編集済みの状態にコントロールを返します。 ユーザーが製品の出荷を処理する をクリックします。 可能性があります、出荷から製品を入力および出荷から商品の追加 をクリックします。 これは、製品に追加. ユーザー表示インターフェイスを返す この時点で、ユーザーは別の出荷を追加する場合があります。 挿入のインターフェイスが DropDownList に戻り、[製品の出荷を処理する] ボタンをクリックすると選択やテキスト ボックスの値がまだ設定して以前の値。


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

両方`Click`イベント ハンドラーを呼び出すだけ、`ReturnToDisplayInterface`メソッド、出荷から商品の追加を返しますが`Click`内のイベント ハンドラーは、手順 4 と、製品を保存するコードを追加します。 `ReturnToDisplayInterface`返すことによって開始、`Suppliers`と`Categories`DropDownLists の最初のオプションにします。 2 つの定数`firstControlID`と`lastControlID`、開始、製品名および単価、挿入するテキスト ボックスのインターフェイスし、の境界内で使用される名前付けで使用されるコントロールのインデックス値と終了をマーク、`for`設定、ループ`Text`の TextBox コントロールのプロパティが空の文字列をバックアップします。 最後に、パネル`Visible`挿入インターフェイスが表示されないようにし、示されている表示インターフェイスのプロパティはリセットされます。

すぐをブラウザーでこのページをテストします。 最初のページにアクセスすると、図 5 のように、表示インターフェイスが表示されます。 製品の出荷を処理する ボタンをクリックします。 ページがポストバックと図 12 に示すように、挿入インターフェイス表示されます。 いずれかの追加から製品出荷または [キャンセル] ボタンをクリックすると、表示インターフェイスを返します。

> [!NOTE]
> 挿入のインターフェイスを表示するには、中にすぐを単価のテキスト ボックスに CompareValidators をテストします。 クライアント側のメッセージ ボックスから無効な通貨値と共に出荷ボタンまたは 0 より小さい値での価格の製品の追加をクリックしたときに警告が表示されます。


[![プロセスの製品の出荷ボタンをクリックした後、挿入するインターフェイスが表示されます。](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**図 12**: プロセスの製品の出荷ボタンをクリックすると、挿入するインターフェイスが表示されます ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>手順 4: 製品の追加

このチュートリアルは、出荷ボタン s から商品の追加データベースに製品を保存するために残っているすべて`Click`イベント ハンドラー。 これには、作成することで、`ProductsDataTable`を追加して、`ProductsRow`指定された製品名のそれぞれのインスタンス。 1 回これら`ProductsRow`への呼び出しを行う場合、s が追加されて、`ProductsBLL`クラス s`UpdateWithTransaction`メソッドに渡す、`ProductsDataTable`です。 注意してください、`UpdateWithTransaction`メソッドで、バックアップで作成された、[トランザクション内でデータベースの変更をラッピング](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアル、パス、`ProductsDataTable`を`ProductsTableAdapter`s`UpdateWithTransaction`メソッドです。 ADO.NET トランザクションが開始されてから、および TableAdatper 問題点、`INSERT`ステートメントに追加された各データベースに`ProductsRow`DataTable にします。 すべての製品が追加、エラーが発生せず、トランザクションがコミットされたと仮定すると、それ以外の場合はロールバックされます。

出荷ボタン s から商品の追加のコード`Click`イベント ハンドラーも少しエラー チェックを実行する必要があります。 挿入のインターフェイスで使用される RequiredFieldValidators がないので、ユーザーでした価格の製品のときに入力の名前を省略すること。 S の製品名が必要ですが、ためユーザーに通知して、挿入を続行する必要がある場合は、このような条件の展開します。 完全な`Click`イベント ハンドラーのコードに依存します。


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

確認して、イベント ハンドラーを起動、`Page.IsValid`プロパティの値を返します`true`です。 返された場合`false`、し、つまり、1 つ、CompareValidators の詳細が無効なデータをレポート以外の場合はこのようなケースでたくない、入力した製品を挿入しようとするまたはお、最終的には、例外、ユーザーが入力した単価を割り当てようとするときに値を`ProductsRow`s`UnitPrice`プロパティです。

次に、新しい`ProductsDataTable`インスタンスが作成された (`products`)。 A`for`製品名および単価のテキスト ボックスを反復処理するループを使用し、`Text`プロパティはローカル変数に格納する読み取り`productName`と`unitPrice`です。 単価のではなく、対応する製品名、ユーザーの値が入力する場合、`StatusLabel`製品の名前をインクルードする単位の価格を提供する場合、メッセージにもする必要がありますを表示し、イベント ハンドラーが終了しました。

製品名が指定された、新しい場合`ProductsRow`を使用してインスタンスを作成、 `ProductsDataTable` s`NewProductsRow`メソッドです。 この新しい`ProductsRow`インスタンス s`ProductName`プロパティが、現在の製品に設定中にテキスト ボックスの名前、`SupplierID`と`CategoryID`に割り当てられているプロパティ、`SelectedValue`挿入インターフェイスのヘッダーで DropDownLists のプロパティです。 割り当てられている場合は、ユーザーが製品の価格の値を入力、`ProductsRow`インスタンス s`UnitPrice`プロパティです。 プロパティは、それ以外の場合、原因となる、割り当てられていない左側、`NULL`値を`UnitPrice`データベースにします。 最後に、`Discontinued`と`UnitsOnOrder`プロパティは、ハード コーディングされた値に割り当てられている`false`と 0、それぞれします。

プロパティに割り当てられた後、`ProductsRow`インスタンスに追加されて、`ProductsDataTable`です。

完了時に、`for`すべての製品が追加されているかどうかを確認おループします。 ユーザーは、結局のところ、任意の製品名や価格を入力する前に出荷から追加の製品をクリックしている可能性があります、します。 少なくとも 1 つの製品がある場合、 `ProductsDataTable`、`ProductsBLL`クラスの`UpdateWithTransaction`メソッドが呼び出されます。 データが次に、再バインド、 `ProductsGrid` GridView 表示インターフェイスで新しく追加された製品が表示されるようにします。 `StatusLabel`が更新され、確認メッセージを表示し、`ReturnToDisplayInterface`呼び出されると、非表示、インターフェイスを挿入して表示のインターフェイスを表示します。

製品が入力されていません、挿入するインターフェイスが追加された製品がメッセージが表示されているは保持されません。 製品名を入力してください、テキスト ボックス内の単位の価格が表示されます。

図の 13、14、15、挿入して表示のインターフェイスの動作します。 図 13 に、ユーザーが、対応する製品名のない単位の価格値を入力します。 図 14 インターフェイスを示します表示後に 3 つの新しい製品が追加または正常に、(3 番目、1 つは、前のページでは) GridView で新しく追加された製品の 2 つの図 15 を示します。


[![製品名が必要なときに入力 Unit Price です。](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**図 13**: A 製品名が必要なときに入力 Unit Price ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image39.png))


[![仕入先の追加された 3 つの新しい Veggies Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**図 14**: 次の 3 つ新しい Veggies が追加されました業者 Mayumi s ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image42.png))


[![GridView の最後のページで、新しい製品が見つかりません](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**図 15**: 新しい製品で見つかります GridView の最後のページ ([フルサイズのイメージを表示するをクリックして](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> このチュートリアルで使用されるロジックを挿入するバッチは、トランザクションのスコープ内で、挿入をラップします。 これを確認するには、データベース レベルのエラーを意図的に紹介します。 新しいを割り当てるのではなく、たとえば、`ProductsRow`インスタンス s`CategoryID`プロパティで選択した値を`Categories`DropDownList などの値を割り当てる`i * 5`です。 ここで`i`ループ インデクサーは、1 から 5 までの値があります。 したがって、ときにバッチ内の 2 つまたは複数の製品が最初の製品を挿入を追加する必要が有効な`CategoryID`値 (5) が、それ以降の製品が`CategoryID`までと一致しない値`CategoryID`の値が、`Categories`テーブル。 最終的な結果は、最初の中に`INSERT`成功すると、以降は、外部キー制約違反で失敗します。 一括挿入は、アトミックなので最初`INSERT`はロールバックされます、状態、バッチ処理を挿入する前に、データベースの開始を取得します。


## <a name="summary"></a>まとめ

これと前の 2 つのチュートリアルを用意しておりますできるインターフェイスの更新、削除、およびのデータ アクセス層に追加したトランザクションのサポートを使用するすべてのデータのバッチを挿入するには、[ラッピング データベースの変更トランザクション内で](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルです。 特定のシナリオでは、このようなバッチ処理のユーザー インターフェイスが大幅に効率を向上エンドユーザーを削減してクリック、ポストバック、およびキーボード、マウスのコンテキストの切り替えの数も、基になるデータの整合性を維持しながら。

このチュートリアルでは、バッチ化されたデータの操作で、外観を完了します。 チュートリアルの次のセットでは、さまざまなメソッドでは、TableAdapter s、DAL、暗号化の接続文字列で接続し、コマンド レベルの設定を構成するストアド プロシージャの使用など、データ アクセス層の高度なシナリオについて説明します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 Hilton Giesenow と S ren 一 Lauritsen されたこのチュートリアルのレビュー担当者が生じる。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](batch-deleting-cs.md)
[次へ](wrapping-database-modifications-within-a-transaction-vb.md)
