---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: バッチを挿入する (c#) |Microsoft Docs
author: rick-anderson
description: 1 回の操作で複数のデータベース レコードを挿入する方法について説明します。 ユーザー インターフェイス レイヤーは、複数の n を入力するユーザーを許可する GridView を拡張しています.
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 561acc9b473bac7d39e7ed4d511d8b979657131d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833591"
---
<a name="batch-inserting-c"></a>バッチを挿入する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip)または[PDF のダウンロード](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> 1 回の操作で複数のデータベース レコードを挿入する方法について説明します。 ユーザー インターフェイス レイヤーは、複数の新しいレコードを入力するユーザーを許可する GridView を拡張します。 データ アクセス層は、すべての挿入が成功に、すべての挿入をロールバック、トランザクション内で複数の挿入操作をラップします。


## <a name="introduction"></a>はじめに

[バッチ更新](batch-updating-cs.md)チュートリアルは複数のレコードが編集可能なインターフェイスを表示する GridView コントロールのカスタマイズについて説明しました。 ページにアクセスするユーザーは、一連の変更を行うし、次に、1 つのボタンのクリックでは、バッチ更新を実行可能性があります。 ユーザーが通常 1 つの go の複数のレコードを更新の場合、このようなインターフェイスが数えきれないほどのクリックを保存できます既定値と比較した場合、キーボードのマウスのコンテキストの切り替え行ごとに探索された最初の編集機能、 [、。挿入、更新、および削除するデータの概要](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアル。

この概念は、レコードを追加するときにも適用できます。 Imagine をここで Northwind traders よく出荷から受け取る特定のカテゴリの製品が多数存在する仕入先。 たとえば、6 つの異なる見落とさなくなるとコーヒー製品の出荷東京 Traders から受け取る可能性があります。 多くの同じ値を何度も選択する必要がある DetailsView コントロールで一度に 1 つの 6 つの製品を入力すると場合、: 同じカテゴリ (飲み物) を同じ製造元 (東京 Traders) を選択する必要があります、同じ値 (が廃止されましたFalse の場合) と同じ順序の値 (0) の単位。 この反復的なデータのエントリは、のみ、時間がかかるではありませんはエラーを起こしやすくなります。

簡単な作業では、業者とカテゴリ 1 回、一連の製品名および単位の価格を入力し、データベースに新しい製品を追加するためのボタンをクリックして、ユーザーができるようにするインターフェイスの挿入のバッチを作成できます (図 1 参照)。 各製品が追加されるため、その`ProductName`と`UnitPrice`データ フィールドには、テキスト ボックスに入力された値が割り当てられているときにその`CategoryID`と`SupplierID`上位 fo フォームでの Dropdownlist の値に値が割り当てられます。 `Discontinued`と`UnitsOnOrder`値のハードコード値に設定されます`false`と 0 で、それぞれします。


[![バッチ挿入インターフェイス](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**図 1**:、バッチを挿入するインターフェイス ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image3.png))。


このチュートリアルでは図 1 に示すインターフェイスを挿入するバッチを実装するページを作成します。 として前の 2 つのチュートリアルでは、私たちがラップして、挿入に原子性が確保のトランザクションのスコープ内でいます。 Let s を始めましょう。

## <a name="step-1-creating-the-display-interface"></a>手順 1: 表示インターフェイスの作成

このチュートリアルは、2 つのリージョンに分割された 1 つのページから構成されます。 表示領域と挿入のリージョン。 表示インターフェイスは、この手順で作成する、GridView で、製品が表示され、製品出荷を処理するというタイトルのボタンが含まれています。 このボタンがクリックされたときに表示インターフェイスは、図 1 には、挿入インターフェイスに置き換えられます。 出荷から商品の追加後、表示のインターフェイスを返します。 または、[キャンセル] ボタンがクリックされました。 手順 2. で挿入のインターフェイスを作成します。

内に一度にうちの 1 つだけが表示されて、2 つのインターフェイスを持つページを作成するときにする各インターフェイスが通常に配置されます、[パネルの Web コントロール](http://www.w3schools.com/aspnet/control_panel.asp)、他のコントロールのコンテナーとして機能します。 したがって、このページでは、インターフェイスごとに 2 つのパネル コントロール 1 つがあります。

開いて開始、`BatchInsert.aspx`ページで、`BatchData`フォルダーと、ツールボックスからデザイナーにドラッグ パネル (図 2 参照)。 設定パネル s`ID`プロパティを`DisplayInterface`します。 パネルをデザイナーに追加するときにその`Height`と`Width`プロパティがそれぞれの「50 px」と 125px、に設定されます。 [プロパティ] ウィンドウからこれらのプロパティ値をクリアします。


[![ツールボックスからデザイナーにパネルをドラッグします。](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**図 2**: パネルをツールボックスからデザイナーにドラッグします ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image6.png))。


次に、ボタンと GridView コントロールをパネルにドラッグします。 設定ボタン s`ID`プロパティを`ProcessShipment`とその`Text`製品出荷を処理するプロパティ。 GridView s 設定`ID`プロパティを`ProductsGrid`し、スマート タグ、という名前の新しい ObjectDataSource にバインドする`ProductsDataSource`します。 構成からそのデータをプルする ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッド。 この GridView を使用して、データを表示してのみ、いるので、UPDATE、INSERT でドロップダウン リストを設定し、(None) にタブを削除します。 データ ソース構成ウィザードを完了するには、[完了] をクリックします。


[![ProductsBLL クラスの GetProducts メソッドから返されるデータを表示します。](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**図 3**: から返されたデータを表示、`ProductsBLL`クラス s`GetProducts`メソッド ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image9.png))。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**図 4**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image12.png))。


ObjectDataSource ウィザードの完了後は、Visual Studio は BoundFields と製品のデータ フィールドの CheckBoxField を追加します。 削除以外のすべて、 `ProductName`、 `CategoryName`、 `SupplierName`、 `UnitPrice`、および`Discontinued`フィールド。 自由に見た目をカスタマイズできます。 書式を設定することにしました、`UnitPrice`を通貨値としてフィールド、フィールドの順序を変更して、フィールドのいくつかの名前を変更`HeaderText`値。 ページングと並べ替えを GridView s のスマート タグのページングを有効にして並べ替えを有効にするチェック ボックスをチェックしてサポートを含める GridView 構成もできます。

パネル、ボタン、GridView、ObjectDataSource コントロールを追加して、GridView のフィールドをカスタマイズすると、ページ、宣言型マークアップを次のようになります。


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

開始タグと終了内のボタンや GridView マークアップが表示される`<asp:Panel>`タグ。 これらのコントロール内にあるため、`DisplayInterface`パネルで非表示にできますパネル s を設定するだけで`Visible`プロパティを`false`します。 手順 3 は、s パネルをプログラムで変更`Visible`ボタンへの応答のプロパティは、他の非表示中に 1 つのインターフェイスを表示する をクリックします。

ブラウザーから進行状況を表示する時間がかかります。 図 5 に示すよう、一度に 10 個の製品を一覧表示する GridView 上に製品の出荷を処理するボタンが表示されます。


[![GridView、製品と並べ替えとページング機能を提供しています](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**図 5**: GridView には、製品とは、並べ替えとページング機能が一覧表示されます ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image15.png))。


## <a name="step-2-creating-the-inserting-interface"></a>手順 2: 挿入のインターフェイスを作成します。

完全な表示のインターフェイスで、挿入を作成する準備ができたらインターフェイスです。 このチュートリアルでは、挿入インターフェイスは、1 つの仕入先、カテゴリ値の入力を要求し、最大 5 つの製品名と価格の単位の値を入力するユーザーを作成して使用できます。 このインターフェイスは、ユーザーは 1 ~ 5 の新製品すべてを同じ category と supplier、共有が一意の製品の名前と価格を追加できます。

ツールボックスから、デザイナーの既存の下に配置することにパネルをドラッグして開始`DisplayInterface`パネル。 設定、`ID`このプロパティは、パネルを新しく追加された`InsertingInterface`設定とその`Visible`プロパティを`false`します。 設定するコードを追加します、`InsertingInterface`パネル s`Visible`プロパティを`true`手順 3. でします。 S パネルもクリア`Height`と`Width`プロパティの値。

次に、図 1 に示す挿入インターフェイスを作成する必要があります。 このインターフェイスは、さまざまな HTML の手法で作成できますが、非常に簡単です 1 つを使用します。 4 列、7 行のテーブル。

> [!NOTE]
> HTML マークアップを入力するときに`<table>`ソース ビューを使用する場合は、要素。 Visual Studio に追加するためのツールはありますが`<table>`デザイナーを介して要素は、デザイナーがすべてを挿入するもかまわないの聞か`style`をマークアップに設定します。 作成したところ、`<table>`マークアップは、通常はデザイナーに戻り、Web コントロールを追加し、そのプロパティを設定します。 事前に決められた列と行を含むテーブルを作成するときに、静的な HTML を使いたいなく[テーブル Web コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx)テーブル Web コントロール内にある Web コントロールにアクセスできるだけを使用しているため、`FindControl("controlID")`パターン。 テーブル Web コントロールをプログラムで構築するため、テーブル (いくつかのデータベースまたはユーザーが指定した条件に基づく行または列を持つもの) に動的に規模のテーブル Web コントロールを使用するには、ただし。


内で次のマークアップを入力、`<asp:Panel>`のタグ、`InsertingInterface`パネル。


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

これは、`<table>`マークアップでは、すべての Web コントロールは含まれませんまだ、それらを一時的に追加します。 各に注意してください`<tr>`要素に CSS クラスの特定の設定が含まれています:`BatchInsertHeaderRow`業者とカテゴリ Dropdownlist はどこで; ヘッダー行`BatchInsertFooterRow`出荷およびキャンセル ボタンから追加の製品はどこで; フッター行を交互に`BatchInsertRow`と`BatchInsertAlternatingRow`の TextBox コントロールの価格の製品と単位を含む行の値。 私の対応する CSS クラスを作成したら、 `Styles.css` GridView および DetailsView ような外観の挿入のインターフェイスを提供するファイルがこれらのチュートリアル全体で使用を制御します。 これらの CSS クラスは、以下に示します。


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

このマークアップを入力すると、デザイン ビューに戻ります。 これは、`<table>`図 6 に示すように、デザイナーで、4 つの列と 7 行がテーブルとして表示する必要があります。


[![挿入するインターフェイスの構成の 4 つの列、7 行のテーブル](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**図 6**: で構成される挿入のインターフェイスを 4 つの列、7 行のテーブルの ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image18.png))。


挿入インターフェイスに Web コントロールを追加今すぐ再準備が整いました。 2 つの Dropdownlist をツールボックスから、サプライヤーとカテゴリのいずれかのいずれかのテーブルに適切なセルにドラッグします。

サプライヤー DropDownList s 設定`ID`プロパティを`Suppliers`という名前の新しい ObjectDataSource にバインド`SuppliersDataSource`します。 構成からそのデータを取得する新しい ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers`メソッドと、更新プログラムを (なし) s のドロップダウン リストをタブします。 ウィザードを完了するには、[完了] をクリックします。


[![ObjectDataSource SuppliersBLL クラスの GetSuppliers メソッドを使用して構成します。](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**図 7**: 構成に使用する ObjectDataSource、`SuppliersBLL`クラス s`GetSuppliers`メソッド ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image21.png))。


`Suppliers` DropDownList の表示、`CompanyName`データ フィールドと使用、`SupplierID`のデータがフィールドとしてその`ListItem`の値。


[![CompanyName データ フィールドの表示し、SupplierID を値として使用します。](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**図 8**: 表示、`CompanyName`データ フィールドと使用`SupplierID`値として ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image24.png))。


名前を 2 つ目の DropDownList`Categories`という名前の新しい ObjectDataSource にバインド`CategoriesDataSource`します。 構成、 `CategoriesDataSource` ObjectDataSource を使用する、`CategoriesBLL`クラスの`GetCategories`ウィザードを完了する 完了 で、メソッド、セットのドロップダウン リストの一覧を (なし) の UPDATE および DELETE のタブをクリックします。 最後に、DropDownList 表示がある、`CategoryName`データ フィールドと使用、`CategoryID`値として。

これら 2 つの Dropdownlist を追加し、適切に構成された ObjectDataSources にバインドした後、画面は図 9 のようなります。


[![ヘッダー行は、サプライヤーとカテゴリの Dropdownlist を含む](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**図 9**:、ヘッダー行に、`Suppliers`と`Categories`Dropdownlist ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image27.png))。


これで、新しい各製品の価格と名前を収集するテキスト ボックスを作成する必要があります。 ツールボックスからデザイナーにそれぞれの 5 つの製品名と価格の行の TextBox コントロールをドラッグします。 設定、`ID`にテキスト ボックスのプロパティ`ProductName1`、 `UnitPrice1`、 `ProductName2`、 `UnitPrice2`、 `ProductName3`、`UnitPrice3`など。

それぞれの単位価格を設定するテキスト ボックスの後、CompareValidator を追加、`ControlToValidate`プロパティを適切な`ID`します。 設定も、`Operator`プロパティを`GreaterThanEqual`、`ValueToCompare`を 0 にし、`Type`に`Currency`します。 これらの設定に CompareValidator、価格を入力した場合、有効な通貨値である 0 以上であることを確認するように指示します。 設定、`Text`プロパティを\*、および`ErrorMessage`価格より大きいまたは 0 と等しくする必要があります。 また、通貨記号を省略してください。

> [!NOTE]
> 挿入インターフェイスには含まれません、RequiredFieldValidator コントロール場合でも、`ProductName`フィールドに、`Products`データベース テーブルが許可しない`NULL`値。 これは、ユーザーが最大 5 つの製品を入力できるたいからです。 たとえば、最初の 3 つの行を製品名および単価を提供する場合、ユーザーは、最後の 2 つの行を空白のまま d だけ追加 3 つの新しい製品システムに。 `ProductName`は必要に応じて、ただし、必要があります。 場合単位価格を確認する入力対応する製品名の値が提供されることをプログラムで確認します。 このチェック手順 4. で取り上げます。


ユーザーの入力を検証するには、値には、通貨記号が含まれている場合に、CompareValidator が無効なデータを報告します。 各ユニットの価格の価格を入力するときに、通貨記号を省略する場合、ユーザーに指示する視覚的な合図として機能するテキスト ボックスの前に、$ を追加します。

ValidationSummary コントロール内の最後に、追加、`InsertingInterface`パネルで、設定、`ShowMessageBox`プロパティを`true`とその`ShowSummary`プロパティを`false`します。 これらの設定で無効な単位価格の値を入力すると、問題のある TextBox コントロールの横にアスタリスクが表示し、ValidationSummary 以前指定したエラー メッセージを表示するクライアント側のメッセージ ボックスが表示されます。

この時点で、画面は図 10 のようなはずです。


[![挿入のインターフェイス、製品のテキスト ボックスは、名前と価格](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**図 10**: The 挿入インターフェイスようになりましたが含まれていますテキスト ボックス製品の名前と価格 ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image30.png))。


次に、追加製品を出荷し、[キャンセル] ボタンからフッター行に追加する必要があります。 ドラッグ 2 ボタン コントロールをツールボックスから挿入のインターフェイスのフッターに、ボタンの設定`ID`プロパティ`AddProducts`と`CancelButton`と`Text`出荷し、[キャンセル] から製品をそれぞれ追加するプロパティ。 さらに、設定、`CancelButton`コントロール s`CausesValidation`プロパティを`false`します。

最後に、2 つのインターフェイスのステータス メッセージを表示するラベル Web コントロールを追加する必要があります。 たとえば、ユーザーは、新しい製品の出荷を正常に追加、する表示インターフェイスに戻るし、確認メッセージを表示します。 ただし、ユーザーは、新しい製品では、製品名をオフのままの価格を提供する場合は、以降の警告メッセージを表示する必要があります、`ProductName`フィールドは必須です。 このメッセージの両方のインターフェイスを表示する必要があります、ため、パネルの外部でページの上部に配置します。

Label Web コントロールをツールボックスからデザイナーでページの上部にドラッグします。 設定、`ID`プロパティを`StatusLabel`チェック ボックスをオフに、`Text`プロパティ、およびセット、`Visible`と`EnableViewState`プロパティ`false`します。 前のチュートリアルで述べたように、設定、`EnableViewState`プロパティを`false`により、プログラムでラベルのプロパティの値を変更し、それ以降のポストバック時に既定値に自動的に戻すもらいます。 これには、以降のポストバック時に表示されなくなります。 一部のユーザーの操作への応答で状態メッセージを表示するためのコードが簡略化します。 最後に、設定、`StatusLabel`コントロール s`CssClass`で警告で、CSS クラスの名前を指定するプロパティが定義されている`Styles.css`大きな、斜体、太字、赤いフォントでテキストを表示します。

図 11 は、ラベルを追加し、構成した後、Visual Studio デザイナーを示します。


[![2 つのパネル コントロール上には、ある StatusLabel コントロールの配置します。](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**図 11**: の場所、`StatusLabel`コントロール上の 2 つパネル コントロール ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image33.png))。


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>手順 3: 表示の切り替えおよび挿入インターフェイス

この時点で、表示したり、2 つのタスクのままに、インターフェイスが再挿入するため、マークアップを実施してきました。

- 表示の切り替えおよび挿入インターフェイス
- データベースに発送、製品の追加

現時点では、表示インターフェイスが表示されるが、挿入のインターフェイスは表示されません。 これは、ため、`DisplayInterface`パネル s`Visible`プロパティに設定されて`true`(既定値) の場合は、中に、`InsertingInterface`パネル s`Visible`プロパティに設定されて`false`。 単に s の各コントロールに切り替える必要があります、2 つのインターフェイス間で切り替え`Visible`プロパティの値。

製品出荷を処理するボタンがクリックされたときに、表示インターフェイスから挿入インターフェイスに移動します。 そのため、このボタンのイベント ハンドラーを作成`Click`イベントを次のコードが含まれています。


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

このコードは単に非表示に、`DisplayInterface`パネルと示しています、`InsertingInterface`パネル。

次に、製品出荷し、[キャンセル] ボタンの挿入のインターフェイス コントロールからの追加のイベント ハンドラーを作成します。 いずれかのボタンをクリックすると表示のインターフェイスに戻す必要があります。 作成`Click`両方のイベント ハンドラー ボタン コントロールを呼び出すことができるように`ReturnToDisplayInterface`メソッドを一時的に追加します。 非表示に加え、`InsertingInterface`パネルと表示されている、`DisplayInterface`パネル、`ReturnToDisplayInterface`メソッドは、編集済みの状態に Web コントロールを返す必要があります。 これには、Dropdownlist の設定が含まれます`SelectedIndex`0 およびを消去するプロパティを`Text`の TextBox コントロールのプロパティ。

> [!NOTE]
> 何が起こりうるを検討してください場合私たちでした t が編集済みの状態に表示インターフェイスに戻る前にコントロールを返します。 ユーザーは、製品出荷を処理する ボタンをクリックして、出荷から製品を入力および製品出荷の追加 をクリックし、可能性があります。 製品を追加して、ユーザー表示インターフェイスを返すこのは。 この時点で、ユーザーは別の出荷を追加する場合があります。 挿入のインターフェイスが、DropDownList に戻ると、製品出荷を処理するボタンをクリックすると選択、およびテキスト ボックスの値が引き続き反映されます。 して以前の値。


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

両方`Click`イベント ハンドラーを呼び出すだけ、`ReturnToDisplayInterface`メソッド、出荷の追加の製品に戻ってが`Click`内のイベント ハンドラーは、手順 4 と製品を保存するコードを追加します。 `ReturnToDisplayInterface` 返すことによって開始、`Suppliers`と`Categories`Dropdownlist の最初のオプションにします。 2 つの定数`firstControlID`と`lastControlID`マーク開始と終了 挿入するテキスト ボックスのインターフェイスし、の境界で使用される製品名および単価を名前付けで使用されるコントロールのインデックス値、`for`設定、ループ`Text`の TextBox コントロールのプロパティが空の文字列にバックアップします。 最後に、パネル`Visible`挿入のインターフェイスは非表示にするためと示されている表示インターフェイスのプロパティをリセットします。

このページで、ブラウザーでテストする時間がかかります。 最初のページにアクセスすると、図 5 のように、表示インターフェイスが表示されます。 製品出荷を処理するボタンをクリックします。 ページがポストバックし、図 12 に示すようにインターフェイスを挿入することがわかります。 いずれかの追加からの製品出荷または [キャンセル] ボタンをクリックすると、表示のインターフェイスを返します。

> [!NOTE]
> 挿入のインターフェイスを表示するには、中に少しテキスト ボックスの単価で CompareValidators してテストします。 クライアント側のメッセージ ボックスから無効な通貨の値を持つ Shipment ボタンまたは 0 より小さい値での価格の製品の追加 をクリックすると警告が表示されます。


[![製品出荷のプロセス ボタンをクリックした後、挿入するインターフェイスが表示されます。](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**図 12**: プロセスの製品出荷のボタンをクリックした後、挿入のインターフェイスが表示されます ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image36.png))。


## <a name="step-4-adding-the-products"></a>手順 4: 製品の追加

このチュートリアルは、出荷ボタン s から、追加の製品内のデータベースに製品を保存するのに残っているすべて`Click`イベント ハンドラー。 これは、作成して実行できます、`ProductsDataTable`を追加して、`ProductsRow`指定された製品名の各インスタンス。 これらは 1 回`ProductsRow`への呼び出しを行います s が追加されています、`ProductsBLL`クラス s`UpdateWithTransaction`メソッドに渡して、 `ProductsDataTable`。 いることを思い出してください、`UpdateWithTransaction`メソッドで、バックアップで作成された、[トランザクション内のデータベース変更のラッピング](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアル、パス、`ProductsDataTable`を`ProductsTableAdapter`s`UpdateWithTransaction`メソッド。 ADO.NET トランザクションの開始から、および TableAdatper 問題、`INSERT`ステートメントに追加された各データベースに`ProductsRow`DataTable にします。 すべての製品がエラーを発生させず追加は、トランザクションがコミットされたと仮定すると、それ以外の場合はロールバックされます。

製品出荷ボタン %s からの追加のコード`Click`イベント ハンドラーも少しエラー チェックを実行する必要があります。 挿入のインターフェイスで使用される RequiredFieldValidators がないので、ユーザーでした価格を入力の製品の中に、その名前を省略するとします。 S を製品名は必須であるため、ユーザーにアラートを生成し、挿入を続行する場合は、このような条件で色付けされていく様子する必要があります。 完全な`Click`イベント ハンドラーのコードに従います。


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

イベント ハンドラーを開始することを確認して、`Page.IsValid`プロパティの値を返します`true`します。 返された場合`false`、つまり、1 つまたは無効なデータを報告、CompareValidators の; ようたくない、入力した製品を挿入しようとするまたはため例外と、ユーザーが入力した単価を割り当てようとしました値を`ProductsRow`s`UnitPrice`プロパティ。

次に、新しい`ProductsDataTable`インスタンスが作成されます (`products`)。 A`for`製品名および単価テキスト ボックスを反復処理するループを使用し、`Text`プロパティは、ローカル変数に読み取られる`productName`と`unitPrice`します。 単価の対応する、製品名ではなく、ユーザーの値が入力する場合、`StatusLabel`単位の価格を提供する場合は、メッセージにもする必要がありますが表示されますが、製品の名前を含めるし、イベント ハンドラーが終了しました。

製品名が提供されている場合、新しい`ProductsRow`を使用してインスタンスを作成、 `ProductsDataTable` s`NewProductsRow`メソッド。 この新しい`ProductsRow`インスタンス`ProductName`プロパティが、現在の製品に設定中にテキスト ボックスの名前、`SupplierID`と`CategoryID`に割り当てられているプロパティ、`SelectedValue`挿入インターフェイスのヘッダーで Dropdownlist のプロパティ。 割り当てられている場合は、ユーザーが製品の価格の値を入力、`ProductsRow`インスタンス`UnitPrice`プロパティ、プロパティは、それ以外の場合になりますが、割り当てられていない左、`NULL`値`UnitPrice`データベース内。 最後に、`Discontinued`と`UnitsOnOrder`プロパティは、ハード コーディングされた値に割り当てられている`false`と 0 で、それぞれします。

プロパティに割り当てられた後、`ProductsRow`インスタンスに追加されて、`ProductsDataTable`します。

完了時に、`for`製品が追加されているかどうかを確認しましたループします。 ユーザーは、結局のところ、製品任意の製品名または価格を入力する前に、出荷からの追加をクリックしている可能性があります。 少なくとも 1 つの製品がある場合、 `ProductsDataTable`、`ProductsBLL`クラスの`UpdateWithTransaction`メソッドが呼び出されます。 データが次に、再バインド、 `ProductsGrid` GridView 表示インターフェイスで新しく追加された製品が表示されるようにします。 `StatusLabel`が更新され、確認メッセージを表示し、`ReturnToDisplayInterface`呼び出されると、インターフェイスを挿入して表示のインターフェイスを表示の非表示します。

製品が入力されていない場合、挿入のインターフェイスは製品が追加されていないメッセージが表示されているままです。 製品名を入力してください、テキスト ボックスにユニットの料金が表示されます。

図の 13、14、および 15 では、表示、挿入し、アクションにインターフェイスを表示します。 図 13 に、ユーザーが対応する製品名のない単位価格の値を入力します。 図 14 インターフェイスを示します、表示後に 3 つの新しい図 15 が表示されますが、新しく追加された製品の 2 つ (3 つ目は、前のページでは)、gridview に製品を正常に追加されています。


[![製品名が必要なときに入力 Unit Price です。](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**図 13**: A 製品名が必要なときに入力 Unit Price ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image39.png))。


[![供給業者に追加された 3 つの新しい Veggies Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**図 14**: 次の 3 つ新しい Veggies に追加された業者 Mayumi s ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image42.png))。


[![GridView の最後のページで、新しい製品が見つかりません](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**図 15**:、新しい製品で見つかる GridView の最後のページ ([フルサイズの画像を表示する をクリックします](batch-inserting-cs/_static/image45.png))。


> [!NOTE]
> このチュートリアルで使用されるロジックを挿入バッチでは、トランザクションのスコープ内で、挿入をラップします。 これを確認するには、データベース レベルのエラーを意図的紹介します。 新しい割り当てではなく、たとえば、`ProductsRow`インスタンス`CategoryID`プロパティで選択した値を`Categories`DropDownList などの値を割り当てる`i * 5`します。 ここで`i`ループ インデクサーは、1 から 5 までの値があります。 そのため、ときに最初の製品を挿入するバッチ内の 2 つ以上の製品を追加する必要があります、有効な`CategoryID`値 (5) が、それ以降の製品が`CategoryID`最大と一致しない値`CategoryID`値、`Categories`テーブル。 実際の効果は、最初の中に`INSERT`は成功しますが、以降は外部キー制約違反で失敗します。 一括挿入はアトミックであるため、最初`INSERT`はロールバックされますを返すバッチ挿入処理前に、の状態にデータベースを開始します。


## <a name="summary"></a>まとめ

これと前の 2 つのチュートリアルは、削除、更新、実現するインターフェイスを作成したし、のデータ アクセス層に追加されたトランザクション サポートを使用データのバッチを挿入するには、これらはすべて、[ラッピング データベースの変更トランザクション内で](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアル。 特定のシナリオでは、このようなバッチ処理のユーザー インターフェイスを大幅に改善エンドユーザーの効率性を削減して数回のクリックの数、ポストバック、キーボードのマウスのコンテキスト スイッチの場合は、基になるデータの整合性を維持しながらします。

このチュートリアルでは、バッチ化されたデータの使用方法、外観を完了します。 次の一連のチュートリアルでは、DAL、暗号化の接続文字列などの接続とコマンド レベルの設定の構成、TableAdapter のメソッドでストアド プロシージャの使用など、データ アクセス層の高度なシナリオのさまざまなについて説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 レビュー担当者を潜在顧客、Hilton Giesenow と S ren Jacob Lauritsen がこのチュートリアルでは。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](batch-deleting-cs.md)
> [次へ](wrapping-database-modifications-within-a-transaction-vb.md)
