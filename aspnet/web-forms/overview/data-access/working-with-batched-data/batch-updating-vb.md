---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: 一括更新 (VB) |Microsoft Docs
author: rick-anderson
description: 1 回の操作で複数のデータベース レコードを更新する方法について説明します。 ユーザー インターフェイス層では、各行が編集可能な GridView を構築します。 データにしています.
ms.author: riande
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 76c475b67943b77d99630e087ed46fe6d5f11a03
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823814"
---
<a name="batch-updating-vb"></a>一括更新 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip)または[PDF のダウンロード](batch-updating-vb/_static/datatutorial64vb1.pdf)

> 1 回の操作で複数のデータベース レコードを更新する方法について説明します。 ユーザー インターフェイス層では、各行が編集可能な GridView を構築します。 データ アクセス層は、すべての更新プログラムの成功に、すべての更新プログラムをロールバック、トランザクション内で複数の更新操作をラップします。


## <a name="introduction"></a>はじめに

[前のチュートリアル](wrapping-database-modifications-within-a-transaction-vb.md)データベース トランザクションのサポートを追加するデータ アクセス層を拡張する方法を説明しました。 データベースのトランザクションでは、一連のデータ変更ステートメントが確実にすべての変更は失敗または成功すべてが 1 つのアトミック操作として扱われますことを保証します。 低レベル DAL、この機能の使用には、バッチのデータ変更インターフェイスを作成することに注目しました re 準備が整いました。

このチュートリアルでは、それぞれの行が編集可能な (図 1 参照) は GridView を作成します。 各行がレンダリングされるので編集インターフェイス、s をそこでない列の編集の必要性、更新、およびキャンセル ボタン。 ページ上の 2 つの製品の更新ボタンがあります、代わりにをクリックすると、GridView の行を列挙し、データベースを更新します。


[![GridView の各行は編集可能なです。](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**図 1**: GridView で各の行が編集可能な ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image2.png))。


Let s を始めましょう。

> [!NOTE]
> [バッチ更新を実行する](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)DataList コントロールを使用するインターフェイスのチュートリアルのバッチの編集を作成しました。 このチュートリアルで前に 1 つは、使用する GridView とは異なるし、バッチ更新は、トランザクションのスコープ内で実行されます。 このチュートリアルを完了後すると、以前のチュートリアルに戻るし、前のチュートリアルで追加されたデータベースのトランザクションに関連する機能を使用するように更新をぜひ。


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>GridView のすべての行を編集可能にするための手順を調べる

説明したように、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、GridView が行ごとに、基になるデータを編集するための組み込みのサポートを提供します。 GridView にどのような行が使用して編集できますがノートで内部的には、その[`EditIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)します。 GridView は、そのデータ ソースにバインドされていると、行のインデックスの値に等しいかどうかに表示するには、各行を確認します。`EditIndex`します。 そうである場合は、その行の編集を使用してフィールドがレンダリングされますがインターフェイスです。 BoundFields、編集インターフェイスは、テキスト ボックスが`Text`プロパティには、BoundField 秒で指定されたデータ フィールドの値が割り当てられている`DataField`プロパティ。 TemplateFields、用、`EditItemTemplate`の代わりに使用されて、`ItemTemplate`します。

ユーザーが行の編集 ボタンをクリックしたときに編集ワークフローを起動することを思い出してください。 これには、ポストバックが発生する、GridView s 設定`EditIndex`プロパティをクリックした行のインデックス、およびグリッドにデータの再バインド数。 行のキャンセル ボタンがクリックされたとき、ポストバック時に、`EditIndex`の値に設定されている`-1`グリッドにデータを再バインドする前にします。 GridView の行では、0 からインデックス作成を開始、ために設定`EditIndex`に`-1`読み取り専用モードで、GridView の表示の効果があります。

`EditIndex`プロパティは、行ごとの編集に適してがバッチを編集するためのものではありません。 全体の GridView を編集するためには、その編集インターフェイスを使用してレンダリングの各行が必要です。 これを実現する最も簡単な方法が編集可能な各フィールドが実装されている編集インターフェイスを TemplateField で定義されている場所を作成するには、`ItemTemplate`します。

次にいくつかの手順を作成します GridView を完全に編集可能です。 手順 1 で、GridView コントロールとその ObjectDataSource を作成して開始がされその BoundFields と CheckBoxField TemplateFields に変換します。 手順 2 および 3 の TemplateFields から編集インターフェイスに移動します`EditItemTemplate`s、`ItemTemplate`秒。

## <a name="step-1-displaying-product-information"></a>手順 1: 製品情報を表示します。

GridView を作成するかと心配する前に編集可能な行が、s の製品情報を表示するだけで開始できるようにします。 開く、`BatchUpdate.aspx`ページで、`BatchData`フォルダーとツールボックスからデザイナーにドラッグする GridView。 GridView s 設定`ID`に`ProductsGrid`という名前の新しい ObjectDataSource にバインドするを選択して、スマート タグからとは、`ProductsDataSource`します。 構成からそのデータを取得する ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッド。


[![ProductsBLL クラスを使用する ObjectDataSource を構成します。](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**図 2**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image4.png))。


[![GetProducts メソッドを使用して、製品データを取得します。](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**図 3**: 使用して、製品データを取得、`GetProducts`メソッド ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image6.png))。


GridView のような ObjectDataSource 変更機能が行ごとに設計されています。 一連のレコードを更新するには、データをバッチ処理し、BLL に渡します ASP.NET ページの分離コード クラスでのコードを記述する必要があります。 そのため、ドロップ ダウン リスト ObjectDataSource s で UPDATE、INSERT、および削除タブ設定を (なし)。 ウィザードを完了するには、[完了] をクリックします。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**図 4**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image8.png))。


データ ソース構成ウィザードを完了すると、ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

データ ソースの構成ウィザードの完了と、Visual Studio で、GridView BoundFields と製品のデータ フィールドの CheckBoxField を作成します。 このチュートリアルでは、s のみを表示および製品の名前、カテゴリ、価格、および提供が中止された状態を編集するユーザーを許可することができます。 削除以外のすべての`ProductName`、 `CategoryName`、 `UnitPrice`、および`Discontinued`フィールドと名前の変更、`HeaderText`最初の 3 つのプロパティのフィールドには、製品、カテゴリ、および価格、それぞれします。 最後に、GridView s のスマート タグのページングを有効にして、並べ替えを有効にするチェック ボックスを確認します。

GridView の 3 つ BoundFields がこの時点で (`ProductName`、 `CategoryName`、および`UnitPrice`) と、CheckBoxField (`Discontinued`)。 TemplateFields これら 4 つのフィールドに変換し、TemplateField s から、編集用のインターフェイスを移動する必要があります`EditItemTemplate`にその`ItemTemplate`します。

> [!NOTE]
> 作成とカスタマイズ TemplateFields で検討しました、[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアル。 TemplateFields BoundFields と CheckBoxField に変換する手順を説明し、その編集を定義するインターフェイス、 `ItemTemplate` s、行き詰まった場合や、この前のチュートリアルを参照する、リフレッシャーでは、don t を躊躇する必要があります。


GridView のスマート タグから、[フィールド] ダイアログ ボックスを開くための列の編集リンクをクリックします。 次に、各フィールドを TemplateField リンクにこのフィールドに変換をクリックします。


![既存の BoundFields や CheckBoxField を TemplateFields に変換します。](batch-updating-vb/_static/image5.gif)

**図 5**: 既存 BoundFields や CheckBoxField を TemplateFields に変換します。


インターフェイスの編集を移動する準備ができたら各フィールドを TemplateField ができた、 `EditItemTemplate` s、`ItemTemplate`秒。

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>手順 2: 作成、`ProductName`、`UnitPrice`、および`Discontinued`インターフェイスの編集

作成、 `ProductName`、 `UnitPrice`、および`Discontinued`インターフェイスの編集は、この手順のトピックで、非常に簡単 TemplateField s で各インターフェイスが既に定義されているよう`EditItemTemplate`します。 作成、`CategoryName`の該当するカテゴリの DropDownList を作成する必要がありますのでインターフェイスの編集は少し複雑です。 これは、`CategoryName`手順 3. で取り組むはインターフェイスを編集します。

%S を開始できるように、 `ProductName` TemplateField します。 GridView のスマート タグからテンプレートの編集リンクをクリックし、ドリルダウン、 `ProductName` TemplateField の`EditItemTemplate`します。 テキスト ボックスを選択、クリップボードにコピーおよび貼り付けます、 `ProductName` TemplateField の`ItemTemplate`します。 変更するテキスト ボックス s`ID`プロパティを`ProductName`します。

RequiredFieldValidator を次に、追加、`ItemTemplate`にユーザーが各製品の名前の値を提供することを確認します。 設定、`ControlToValidate`プロパティを ProductName、`ErrorMessage`プロパティには、製品の名前を指定する必要があります。 および`Text`プロパティを\*します。 追加を行った後、`ItemTemplate`図 6 のような画面になります。


[![ProductName TemplateField ここでには、テキスト ボックスと、RequiredFieldValidator が含まれます。](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**図 6**: `ProductName` TemplateField にはテキスト ボックスおよび RequiredFieldValidator を今すぐが含まれています ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image10.png))。


`UnitPrice`からテキスト ボックスにコピーすることでインターフェイスを編集するには、開始、`EditItemTemplate`を`ItemTemplate`します。 テキスト ボックスとセットの前に、$ を次に、配置、 `ID` UnitPrice プロパティとその`Columns`8 プロパティ。

追加する、CompareValidator ことも、 `UnitPrice` s`ItemTemplate`ユーザーが入力した値が有効な通貨値 0.00 ドル以上であることを確認します。 検証の設定`ControlToValidate`UnitPrice、プロパティ、`ErrorMessage`にプロパティが有効な通貨値を入力する必要があります。 省略してください、通貨記号。 は、その`Text`プロパティを\*その`Type`プロパティを`Currency`その`Operator`プロパティを`GreaterThanEqual`、とその`ValueToCompare`プロパティを 0 にします。


[![価格の入力を確認の CompareValidator は負でない通貨の値を追加します。](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**図 7**: 追加の価格が入力することを確認する CompareValidator が負でない通貨値 ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image12.png))。


`Discontinued` TemplateField で既に定義されているチェック ボックスを使用することができます、`ItemTemplate`します。 設定するだけです、`ID`生産中止にし、その`Enabled`プロパティを`True`します。

## <a name="step-3-creating-thecategorynameediting-interface"></a>手順 3: 作成、`CategoryName`インターフェイスの編集

編集インターフェイスで、 `CategoryName` TemplateField s`EditItemTemplate`の値を表示するテキスト ボックスが含まれています、`CategoryName`データ フィールド。 可能なカテゴリを一覧表示の DropDownList で置き換える必要があります。

> [!NOTE]
> [データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアルには、テキスト ボックスではなく DropDownList を含めるテンプレートをカスタマイズする方法のより完全で完全なディスカッションが含まれています。 次の手順が完了したら、中に答えました表示されます。 作成すると、カテゴリの DropDownList の構成に関する詳細についてを参照して戻り、[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアル。


ツールボックスからドラッグして、DropDownList、 `CategoryName` TemplateField s`ItemTemplate`設定、その`ID`に`Categories`。 この時点では通常、Dropdownlist のデータ ソースを定義、スマート タグを新しい ObjectDataSource を作成します。 ただし、これで ObjectDataSource により追加されます、 `ItemTemplate`、その結果、GridView の行ごとに作成された ObjectDataSource インスタンス。 代わりに、s GridView の TemplateFields の外部で ObjectDataSource を作成することができます。 テンプレートの編集を終了し、下にあるデザイナーには、ツールボックスからドラッグして、ObjectDataSource、 `ProductsDataSource` ObjectDataSource します。 名前を新しい ObjectDataSource`CategoriesDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategories`メソッド。


[![CategoriesBLL クラスを使用する ObjectDataSource を構成します。](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**図 8**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image14.png))。


[![メソッドを使用してカテゴリ データを取得します。](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**図 9**: 使用して、カテゴリ データを取得、`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image16.png))。


この ObjectDataSource は、データを取得するだけで使用される、ため (None) に、UPDATE および DELETE のタブで、ドロップダウン リストを設定します。 ウィザードを完了するには、[完了] をクリックします。


[![セットの UPDATE および DELETE のタブ (なし) をドロップダウン リスト](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**図 10**: (None) に、更新および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image18.png))。


ウィザードの完了後、 `CategoriesDataSource` s 宣言型マークアップは次のようになります。


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

`CategoriesDataSource`を作成および構成を返す、 `CategoryName` TemplateField の`ItemTemplate`DropDownList s のスマート タグから データ ソースのリンクをクリックして、します。 データ ソース構成ウィザードで選択、`CategoriesDataSource`最初のドロップダウン リストからオプションとを持っている`CategoryName`表示に使用し、`CategoryID`値として。


[![CategoriesDataSource DropDownList をバインドします。](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**図 11**: に DropDownList をバインド、 `CategoriesDataSource` ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image20.png))。


この時点で、 `Categories` DropDownList では、すべてのカテゴリ、表示されていますが、まだ自動的に選択せず、GridView の行にバインドされている製品の適切なカテゴリ。 設定する必要があります。 これを実現する、 `Categories` DropDownList s`SelectedValue`製品 s`CategoryID`値。 DropDownList s のスマート タグから DataBindings の編集リンクをクリックし、関連付ける、`SelectedValue`プロパティを`CategoryID`データ フィールドの図 12 に示すようにします。


![製品の CategoryID 値を DropDownList の SelectedValue プロパティにバインドします。](batch-updating-vb/_static/image12.gif)

**図 12**: バインド製品 s `CategoryID` DropDownList s 値`SelectedValue`プロパティ


最後の 1 つの問題が: 製品されない場合、`CategoryID`で指定された値、データ バインド ステートメント`SelectedValue`例外が発生します。 DropDownList カテゴリの項目のみが含まれており、ある製品のためのオプションを提供していません、`NULL`の値をデータベース`CategoryID`します。 これを解決するには、DropDownList s を設定`AppendDataBoundItems`プロパティを`True`、DropDownList に新しい項目を追加し、省略すると、`Value`プロパティ宣言の構文をします。 つまり、いることを確認してください、 `Categories` DropDownList s の宣言構文は次のようにします。


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

注方法、 `<asp:ListItem Value="">` --選択 1--がその`Value`属性が明示的に空の文字列に設定します。 参照、[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)この追加の DropDownList の項目が処理するために必要な理由についての詳しいチュートリアル、`NULL`ケースとその理由の割り当て、 `Value`プロパティを空の文字列が不可欠です。

> [!NOTE]
> パフォーマンスとスケーラビリティの問題をここで説明が必要です。 各行がある DropDownList を使用するため、 `CategoriesDataSource` 、データ ソースとして、`CategoriesBLL`クラス s`GetCategories`メソッドが呼び出される*n* 1 ページあたりの時間を参照してください、場所*n*の数ですGridView の行。 これら*n*呼び出し`GetCategories`結果*n*データベースに対するクエリ。 要求ごとのキャッシュまたは SQL キャッシュ依存関係または非常に短い時間ベースの有効期限を使用してキャッシュ層のいずれかに返されるカテゴリをキャッシュすることによって、データベースに対するこの影響を軽減でした。 詳細については、要求ごとのキャッシュ オプションを参照してください[ `HttpContext.Items` 1 秒あたりの要求のキャッシュ ストア](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx)します。


## <a name="step-4-completing-the-editing-interface"></a>手順 4: 完了編集インターフェイス

私たちを進行状況を表示するを一時停止せず、GridView のテンプレートに変更の数を作成しています。 ブラウザーから進行状況を表示する時間がかかります。 使用して各行を表示する図 13 に示すよう、`ItemTemplate`インターフェイスの編集 cell が含まれています。


[![GridView の各行は編集可能なです。](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**図 13**: 各 GridView 行が編集可能な ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image22.png))。


この時点で処理をする必要がありますが、いくつかの軽微な書式設定の問題があります。 最初に、注意、`UnitPrice`値には、次の 4 つの 10 進数のポイントが含まれています。 これを解決するに戻る、 `UnitPrice` TemplateField の`ItemTemplate`し、テキスト ボックス s のスマート タグから DataBindings の編集リンクをクリックします。 次に、指定する、`Text`プロパティは、数値として書式設定する必要があります。


![Text プロパティを数値として書式設定します。](batch-updating-vb/_static/image14.gif)

**図 14**: 形式、`Text`を数値としてプロパティ


第 2 に、チェック ボックスの中央 let s、 `Discontinued` (左側に配置することのではなく) 列。 GridView のスマート タグから列の編集 を選択します、 `Discontinued` TemplateField 左下隅のフィールドの一覧から。 ドリル ダウン`ItemStyle`設定と、`HorizontalAlign`センター図 15 に示すようにプロパティ。


![センターの提供が中止されたチェック ボックス](batch-updating-vb/_static/image15.gif)

**図 15**: Center、`Discontinued`チェック ボックス


次に、ページに ValidationSummary コントロールを追加し、設定、`ShowMessageBox`プロパティを`True`とその`ShowSummary`プロパティを`False`します。 ボタンの Web コントロールがクリックされたときにも追加、ユーザーの変更が更新されます。 具体的には、GridView 上記と両方のコントロールを設定する 1 つ下にある、2 つのボタンの Web コントロールを追加して`Text`更新プログラムの製品のプロパティ。

GridView s 以降編集インターフェイスで定義されているその TemplateFields `ItemTemplate` 、s、`EditItemTemplate`が余分なされ削除される可能性があります。

上記で作成するには、書式設定の変更を説明したように後、ボタン コントロールの追加と削除、不要な`EditItemTemplate`s、ページの宣言構文は、次のようになる必要があります。


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

図 16 は、ボタンの Web コントロールを追加した後、ブラウザーで表示したときに、このページおよび書式設定の変更を示します。


[![ページここには、2 つの更新プログラムの製品ボタンが含まれています。](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**図 16**:、ページようになりましたが含まれています 2 つの更新の製品のボタン ([フルサイズの画像を表示する をクリックします](batch-updating-vb/_static/image24.png))。


## <a name="step-5-updating-the-products"></a>手順 5: 製品の更新

このページにアクセスするユーザーは、修正はされ、2 つの更新プログラムの製品ボタンをクリックします。 その時点で何らかの方法での行ごとにユーザーが入力した値を保存する必要があります、`ProductsDataTable`インスタンスし、するには合格しした BLL メソッドに渡す`ProductsDataTable`DAL s インスタンス`UpdateWithTransaction`メソッド。 `UpdateWithTransaction`メソッドで作成した、[前のチュートリアル](wrapping-database-modifications-within-a-transaction-vb.md)変更のバッチをアトミック操作として更新されます。

という名前のメソッドを作成する`BatchUpdate`で`BatchUpdate.aspx.vb`し、次のコードを追加します。


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

このメソッドがすべての製品を取得することによってまずに戻り、 `ProductsDataTable` BLL s への呼び出しを使用して`GetProducts`メソッド。 列挙し、 `ProductGrid` GridView s [ `Rows`コレクション](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)します。 `Rows`コレクションに含まれる、 [ `GridViewRow`インスタンス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx)GridView に表示される行はごとです。 ページで、GridView 秒あたり最大で 10 行が表示されているため`Rows`コレクションが 10 個の項目が必要があります。

行ごとに、`ProductID`から取得したが、`DataKeys`コレクションと、適切な`ProductsRow`からが選択されている、`ProductsDataTable`します。 TemplateField の 4 つの入力コントロールがプログラムで参照されているし、その値に割り当てられます、`ProductsRow`プロパティをインスタンス化します。 各 GridView 後は、s 行の値を更新に使用されている、 `ProductsDataTable`、BLL s に s が渡される`UpdateWithTransaction`、前のチュートリアルで説明したように単純に呼び出すメソッド DAL s に`UpdateWithTransaction`メソッド。

このチュートリアルで使用されるバッチ更新アルゴリズム内の各行の更新、`ProductsDataTable`製品の情報が変更されているかどうかに関係なく、gridview の行に対応します。 このような blind 更新は t は、通常、パフォーマンスの問題、発生する可能性余分なレコード re 監査するが、データベース テーブルに変更された場合。 戻り、[バッチ更新を実行する](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)チュートリアル、DataList でインターフェイスの更新バッチを探索し、ユーザーが実際に変更されたレコードのみを更新するコードを追加します。 自由に技法を使用して[バッチ更新を実行する](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md)必要な場合は、このチュートリアルでは、コードを更新します。

> [!NOTE]
> GridView にデータ ソースの主キー値を Visual Studio が自動的に割り当てられます、スマート タグを GridView にデータ ソースをバインドするときに`DataKeyNames`プロパティ。 かどうか、手順 1. で説明したように GridView s のスマート タグを GridView ObjectDataSource にバインドすることできませんでしたし、GridView s を手動で設定する必要があります`DataKeyNames`プロパティにアクセスするには、ProductID、`ProductID`を各行の値、`DataKeys`コレクション。


使用するコード`BatchUpdate`BLL s で使用されているような`UpdateProduct`メソッド、記述されていることの主な違い、`UpdateProduct`メソッドが 1 つだけ`ProductRow`アーキテクチャからインスタンスを取得します。 プロパティを代入するコード、`ProductRow`間で同じでは、`UpdateProducts`メソッドと、コード内で、`For Each`ループ`BatchUpdate`全体的なパターンは、します。

このチュートリアルを完了する必要がありますが、`BatchUpdate`メソッドが呼び出されたときに更新プログラムの製品ボタンのいずれかをクリックします。 イベント ハンドラーを作成、`Click`これらの 2 つのイベント コントロールのボタンをクリックし、イベント ハンドラーで、次のコードを追加します。


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

呼び出しが行われた最初`BatchUpdate`します。 次に、 [ `ClientScript`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx)を読み取る、製品が更新されてメッセージ ボックスを表示する JavaScript を挿入するために使用します。

このコードをテストする時間がかかります。 参照してください`BatchUpdate.aspx`ブラウザーで、行の番号を編集し、いずれかの更新プログラムの製品ボタンをクリックします。 入力の検証エラーがないと仮定すると、製品が更新されたというメッセージ ボックスが表示されます。 更新の原子性を確認するには、ランダムな追加を検討する`CHECK`が禁止されているいずれかのように、制約`UnitPrice`1234.56 の値。 次に、 `BatchUpdate.aspx`、s の製品のいずれかを設定することを確認して、レコードの番号を編集`UnitPrice`禁止されている値 (1234.56) する値。 これには、元の値にそのバッチ操作中に、その他の変更と更新プログラムの製品をクリックすると、ロールバック時に、エラーが発生する必要があります。

## <a name="an-alternativebatchupdatemethod"></a>代替`BatchUpdate`メソッド

`BatchUpdate`ばかりメソッドの検証を取得します*すべて*BLL s から、製品の`GetProducts`メソッドし、GridView に表示されるには、これらのレコードを更新します。 このアプローチは、GridView では、ページングは使用しませんが、場合は、ある可能性があります数百、数千または数万の製品が GridView に 10 個の行の場合に最適です。 このような場合は、10 個の変更にのみ、データベースから取得するすべての製品より小さいに最適です。

これらの種類の状態を次の使用を検討`BatchUpdateAlternate`メソッド代わりにします。


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` 開始すると、新しい空の作成、`ProductsDataTable`という`products`します。 これは、後で、GridView s をステップ実行`Rows`コレクションと、各行は、BLL s を使用して、特定の製品情報を取得します。`GetProductByProductID(productID)`メソッド。 取得して、`ProductsRow`インスタンスがそのプロパティと同じように更新`BatchUpdate`にインポートされた行を更新した後、 `products` `ProductsDataTable` DataTable s を介して[`ImportRow(DataRow)`メソッド](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

後に、`For Each`ループが完了したら、 `products` 1 つ含まれる`ProductsRow`GridView の行ごとのインスタンス。 以降の各、`ProductsRow`インスタンスに追加された、 `products` (の代わりに更新されます)、無条件に渡す場合、`UpdateWithTransaction`メソッド、`ProductsTableAdatper`は各レコードをデータベースに挿入ましょう。 代わりに、それぞれの行が変更されたこと (追加されません) を指定する必要があります。

これは、名前付き BLL に新しいメソッドを追加することで実現できます`UpdateProductsWithTransaction`します。 `UpdateProductsWithTransaction`、、下図のようにセット、`RowState`のそれぞれの、`ProductsRow`インスタンス、`ProductsDataTable`に`Modified`し渡します、 `ProductsDataTable` DAL s`UpdateWithTransaction`メソッド。


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>まとめ

GridView では、行ごとの組み込みの編集機能を提供しますが、完全に編集可能なインターフェイスを作成するためのサポートされていません。 このチュートリアルで説明したようにこのようなインターフェイスが可能ですの作業が必要です。 すべての行が編集可能な GridView を作成する必要があります TemplateFields GridView のフィールドに変換し、内の編集インターフェイスを定義する、`ItemTemplate`秒。 さらに、更新すべてのボタンの Web コントロールの種類は GridView とは別のページに追加する必要があります。 これらのボタン`Click`イベント ハンドラーは、GridView s を列挙する必要があります`Rows`コレクションで変更を保存、 `ProductsDataTable`、更新された情報を適切な BLL メソッドに渡すとします。

次のチュートリアルでは、バッチを削除するためのインターフェイスを作成する方法がわかります。 GridView の行ごとのチェック ボックスを含めるし、の代わりにすべての更新は具体的には、-入力ボタンを選択した行の削除ボタンがあります。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy、David Suru でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](wrapping-database-modifications-within-a-transaction-vb.md)
> [次へ](batch-deleting-vb.md)
