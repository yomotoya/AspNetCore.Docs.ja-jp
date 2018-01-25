---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: "一括更新 (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "単一の操作で複数のデータベース レコードを更新する方法を説明します。 ユーザー インターフェイス レイヤーは、各行が編集可能 GridView を構築します。 データにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 1210f9048401ca1b4e29d6dde9bf5dbef987091f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="batch-updating-c"></a>一括更新 (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip)または[PDF のダウンロード](batch-updating-cs/_static/datatutorial64cs1.pdf)

> 単一の操作で複数のデータベース レコードを更新する方法を説明します。 ユーザー インターフェイス レイヤーは、各行が編集可能 GridView を構築します。 データ アクセス層は、すべての更新プログラムが成功するすべての更新をロールバックすることを確認するトランザクション内で複数の更新操作をラップします。


## <a name="introduction"></a>はじめに

[前のチュートリアル](wrapping-database-modifications-within-a-transaction-cs.md)データベース トランザクションのサポートを追加するデータ アクセス層を拡張する方法を説明しました。 データベースのトランザクションでは、一連のデータ変更ステートメントが確実にすべての変更は失敗または成功すべてが 1 つのアトミックの操作として扱われますことを保証します。 低レベル DAL、この機能を邪魔にバッチのデータ変更のインターフェイスを作成することに注目する re おを準備します。

このチュートリアルでは各行が編集可能な (図 1 参照) が GridView ビルドします。 各行がレンダリング インターフェイスには編集、そこ s されません編集の列の必要性、ため、更新、およびキャンセル ボタン。 代わりに、2 つの製品の更新ボタンにあるページをクリックすると、GridView 行を列挙し、データベースを更新します。


[![GridView の各行は編集可能であります。](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**図 1**: GridView で各の行が編集可能 ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image2.png))


開始 s を let!

> [!NOTE]
> [バッチ更新を実行する](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)DataList コントロールを使用してバッチの編集を作成したチュートリアルのインターフェイスです。 このチュートリアルに 1 つは、使用前、GridView と異なりバッチ更新は、トランザクションのスコープ内で実行します。 このチュートリアルを完了した後はことをお勧めすると、以前のチュートリアルに戻るし、前のチュートリアルで追加されたデータベースのトランザクションに関連する機能を使用するように更新します。


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>GridView のすべての行を編集可能にするための手順を確認します。

説明したように、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルでは、GridView は、行ごとに、基になるデータを編集するための組み込みサポートを提供します。 内部的には、GridView ノート、どのような行を使用して編集その[`EditIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)です。 GridView は、そのデータ ソースにバインドされているが、そのチェックして、行のインデックスの値に等しいかどうかは行ごと`EditIndex`です。 場合は、その行の編集を使用してフィールドを表示する s はインターフェイスです。 BoundFields、編集のインターフェイスは、テキスト ボックスが`Text`プロパティには、BoundField 秒で指定されたデータ フィールドの値が割り当てられた`DataField`プロパティです。 TemplateFields、用、`EditItemTemplate`の代わりに使用される、`ItemTemplate`です。

ユーザーが行の編集 ボタンをクリックしたときに編集ワークフローが開始されることに注意してください。 これには、ポストバックを発生させる、GridView s を設定`EditIndex`s クリックされた行のインデックス、およびグリッドにデータを再バインドするプロパティです。 ポストバック時に、行 s [キャンセル] ボタンをクリックすると、`EditIndex`の値に設定されている`-1`グリッドにデータを再バインドする前にします。 GridView の行は、0 でのインデックス作成を開始するための設定`EditIndex`に`-1`読み取り専用モードでの GridView の表示の効果があります。

`EditIndex`プロパティは、行ごとの編集に適してがバッチを編集するためのものではありません。 全体の GridView を編集するためは、各行の編集用のインターフェイスを使用して表示する必要があります。 これを実現する最も簡単な方法は編集可能な各フィールドが実装されている編集インターフェイスと TemplateField で定義されている場所を作成する、`ItemTemplate`です。

次にいくつかの手順が作成されます GridView を完全に編集可能です。 手順 1. では、GridView と、ObjectDataSource を作成することでし TemplateFields その BoundFields と CheckBoxField に変換します。 手順 2 および 3 の TemplateFields から編集インターフェイスに移動します`EditItemTemplate`s、 `ItemTemplate` s。

## <a name="step-1-displaying-product-information"></a>手順 1: 製品情報を表示します。

GridView の作成と心配する前に、行は、ここでは、編集、s、製品情報を表示するだけで開始します。 開く、 `BatchUpdate.aspx`  ページで、`BatchData`フォルダーと、ツールボックスからデザイナーにドラッグする GridView。 集合 GridView s`ID`に`ProductsGrid`し、そのスマート タグからという名前の新しい ObjectDataSource にバインドする選択`ProductsDataSource`です。 構成からそのデータを取得する ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッドです。


[![構成、ObjectDataSource ProductsBLL クラスを使用するには](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**図 2**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image4.png))


[![GetProducts メソッドを使用して、製品データを取得します。](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**図 3**: 製品データを使用して、取得、`GetProducts`メソッド ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image6.png))


GridView のように、行ごとに、ObjectDataSource の変更機能が設計されています。 一連のレコードを更新するのには ASP.NET ページの分離コード クラスは、データをバッチ処理を BLL に渡すことで少量のコードを記述する必要になります。 そのため、ドロップ ダウン リスト ObjectDataSource s で UPDATE、INSERT、および DELETE タブ設定を (なし) します。 ウィザードを完了するには、[完了] をクリックします。


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**図 4**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image8.png))


データ ソース構成ウィザードを完了すると、ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

GridView で BoundFields と製品データ フィールドの CheckBoxField を作成する Visual Studio とも、データ ソースの構成ウィザードを完了します。 このチュートリアルでは、アクセス許可のみを表示および製品の名前、カテゴリ、価格、および提供が中止された状態を編集 s を使用できます。 削除以外のすべての`ProductName`、 `CategoryName`、 `UnitPrice`、および`Discontinued`フィールドと名前の変更、`HeaderText`製品カテゴリ、および価格、フィールドは最初の 3 つのプロパティがそれぞれします。 最後に、GridView s のスマート タグのページングを有効にして、並べ替えを有効にするチェック ボックスを確認します。

これで、GridView、3 つ BoundFields (`ProductName`、`CategoryName`と`UnitPrice`) と、CheckBoxField (`Discontinued`)。 TemplateFields に 4 つのフィールドを変換し、TemplateField s から編集インターフェイスを移動する必要があります`EditItemTemplate`にその`ItemTemplate`です。

> [!NOTE]
> 作成とカスタマイズに TemplateFields おため、探索、[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)チュートリアルです。 TemplateFields BoundFields と CheckBoxField に変換する手順を説明しに含まれるインターフェイスの編集を定義する、`ItemTemplate`行き詰まった場合や、必要がある、この以前のチュートリアルを参照するなる躊躇リフレッシャー、don t は s です。


GridView s のスマート タグから、[フィールド] ダイアログ ボックスを開くための列の編集リンクをクリックします。 次に、各フィールドを選択し、このフィールドに変換を TemplateField のリンクをクリックします。


![既存の BoundFields や CheckBoxField を TemplateField に変換します。](batch-updating-cs/_static/image5.gif)

**図 5**: 既存 BoundFields や CheckBoxField を TemplateField に変換します。


これで各フィールドを TemplateField、編集を移動する準備ができたらインターフェースからは`EditItemTemplate`s、 `ItemTemplate` s。

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>手順 2: 作成、`ProductName`、`UnitPrice`、および`Discontinued`インターフェイスの編集

作成する、 `ProductName`、 `UnitPrice`、および`Discontinued`インターフェイスの編集は、この手順のトピック、非常に簡単ですが、各インターフェイスが TemplateField s で既に定義されている`EditItemTemplate`です。 作成する、`CategoryName`該当するカテゴリの DropDownList を作成する必要がありますので、少し複雑ですインターフェイスを編集します。 これは、`CategoryName`手順 3. で取り組むはインターフェイスを編集します。

%S を開始できるように、 `ProductName` TemplateField です。 GridView s のスマート タグからテンプレートの編集リンクをクリックし、ドリルダウン、 `ProductName` TemplateField の`EditItemTemplate`です。 テキスト ボックスをオンに、クリップボードにコピーを貼り付けます、 `ProductName` TemplateField の`ItemTemplate`です。 TextBox s を変更する`ID`プロパティを`ProductName`です。

次に、追加する RequiredFieldValidator、`ItemTemplate`をユーザーが各製品の名の値を提供することを確認します。 設定、 `ControlToValidate` ProductName、プロパティ、`ErrorMessage`にプロパティが、製品の名前を指定する必要があります。 および`Text`プロパティを\*です。 追加を行った後、`ItemTemplate`画面が図 6 のようになります。


[![ProductName TemplateField ここでには、テキスト ボックスと、RequiredFieldValidator が含まれます。](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**図 6**: `ProductName` TemplateField が含まれています テキスト ボックスと RequiredFieldValidator ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image10.png))


`UnitPrice`からテキスト ボックスをコピーしてインターフェイスを編集するには、開始、`EditItemTemplate`を`ItemTemplate`です。 テキスト ボックスとセットの前に、$ を次に、配置、 `ID` UnitPrice にプロパティとその`Columns`8 にプロパティです。

追加する CompareValidator ことも、 `UnitPrice` s`ItemTemplate`ユーザーが入力した値が有効な通貨値 $0.00 以上であることを確認します。 バリデーター s を設定`ControlToValidate`UnitPrice、プロパティ、`ErrorMessage`にプロパティが有効な通貨値を入力する必要があります。 省略してください、通貨記号。 には、その`Text`プロパティを\*、その`Type`プロパティを`Currency`、その`Operator`プロパティを`GreaterThanEqual`、およびその`ValueToCompare`プロパティを 0 にします。


[![追加の価格が入力することを確認する CompareValidator が負でない通貨値](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**図 7**: 価格を入力、確認して CompareValidator は負でない通貨値を追加 ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image12.png))


`Discontinued` TemplateField で既に定義されているチェック ボックスを使用することができます、`ItemTemplate`です。 単に設定、`ID`生産中止して、その`Enabled`プロパティを`true`です。

## <a name="step-3-creating-thecategorynameediting-interface"></a>手順 3: 作成、`CategoryName`編集インターフェイス

編集、インターフェイス、 `CategoryName` TemplateField s`EditItemTemplate`の値を表示するテキスト ボックスが含まれています、`CategoryName`データ フィールドです。 ここには、可能なカテゴリを一覧表示する DropDownList が必要です。

> [!NOTE]
> [データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)チュートリアルには、テキスト ボックスではなく DropDownList を含めるテンプレートをカスタマイズする方法のより完全で完全なディスカッションが含まれています。 ここに示す手順は完了しましたが、記載されている答えましたです。 作成すると、カテゴリの DropDownList の構成に関する詳細についてを参照してください戻る、[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)チュートリアルです。


DropDownList をツールボックスからドラッグして、 `CategoryName` TemplateField s `ItemTemplate`、設定、`ID`に`Categories`です。 この時点でおは通常 DropDownLists のデータ ソースの定義のスマート タグから新しい ObjectDataSource を作成します。 ただし、この追加内 ObjectDataSource、 `ItemTemplate`、その結果、GridView 行ごとに作成された ObjectDataSource インスタンス。 代わりに、s、ObjectDataSource、GridView の TemplateFields の外部での作成を使用できます。 テンプレート編集を終了し、ObjectDataSource を下に、デザイナーには、ツールボックスからドラッグして、 `ProductsDataSource` ObjectDataSource です。 名前を新しい ObjectDataSource`CategoriesDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategories`メソッドです。


[![CategoriesBLL 付記を使用する ObjectDataSource を構成します。](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**図 8**: 構成を使用する ObjectDataSource、`CategoriesBLL`付記 ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image14.png))


[![メソッドを使用してカテゴリのデータを取得します。](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**図 9**: カテゴリ データを使用して、取得、`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image16.png))


この ObjectDataSource はデータを取得するだけで使用されるため、(なし)、UPDATE および DELETE のタブで、ドロップダウン リストを設定します。 ウィザードを完了するには、[完了] をクリックします。


[![セット UPDATE と DELETE タブ (なし) をドロップダウン リスト](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**図 10**: (None) に更新と削除のタブにドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image18.png))


ウィザードの完了後に、 `CategoriesDataSource` s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

`CategoriesDataSource`作成および構成に戻り、 `CategoryName` TemplateField の`ItemTemplate`DropDownList s のスマート タグからリンクをクリックして、データ ソースの選択とは、します。 データ ソース構成ウィザードで選択、`CategoriesDataSource`最初のドロップダウン リストからオプションを選択して、`CategoryName`表示に使用し、`CategoryID`値として。


[![次のドロップダウン リストを CategoriesDataSource にバインドします。](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**図 11**: に DropDownList のバインド、 `CategoriesDataSource` ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image20.png))


この時点で、 `Categories` DropDownList を一覧表示、カテゴリのすべてがそのはまだ自動的に選択されません GridView の行にバインドされている製品の適切なカテゴリ。 設定する必要があります。 これを実現する、 `Categories` DropDownList s`SelectedValue`製品 s`CategoryID`値。 DropDownList s のスマート タグから databindings の編集 リンクをクリックし、関連付ける、`SelectedValue`を持つプロパティ、`CategoryID`データ フィールドの図 12 に示すようにします。


![値のバインド製品の CategoryID DropDownList の SelectedValue プロパティ](batch-updating-cs/_static/image12.gif)

**図 12**: 製品 s をバインド`CategoryID`DropDownList s 値`SelectedValue`プロパティ


最後の 1 つの問題まま: 製品されない場合、`CategoryID`で指定された値、データ バインド ステートメント`SelectedValue`例外が発生します。 これは、DropDownList のカテゴリの項目のみが含まれています、ある製品のオプションが提供されていないため、`NULL`の値をデータベース`CategoryID`です。 この問題を解決するには、DropDownList s を設定`AppendDataBoundItems`プロパティを`true`、DropDownList に新しい項目を追加およびを省略すると、`Value`プロパティ宣言構文からです。 つまり、ことを確認、 `Categories` DropDownList s 宣言の構文は、次のようになります。


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

注方法、 `<asp:ListItem Value="">` --選択 1--がその`Value`属性は、空の文字列に明示的に設定します。 戻って、[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)の詳細については、この追加の DropDownList の項目が処理に必要な理由のためのチュートリアル、`NULL`ケースとその理由の割り当て、 `Value`空の文字列にプロパティが不可欠です。

> [!NOTE]
> 問題がある潜在的なパフォーマンスとスケーラビリティここで説明が必要です。 行ごとに使用する DropDownList があるため、 `CategoriesDataSource` 、データ ソースとして、`CategoriesBLL`クラス s`GetCategories`メソッドが呼び出される *n*  1 ページあたりの回数を参照してください、場所 *n*  GridView の行の数です。 これら *n* 呼び出し`GetCategories`結果 *n* データベースに対するクエリ。 要求ごとのキャッシュまたは SQL キャッシュ依存関係または非常に短い形式の時刻ベースの有効期限を使用して、キャッシュ レイヤーを通じて返されるカテゴリをキャッシュすることによって、データベースに対するこの影響を軽減可能性があります。 詳細については、要求ごとのキャッシュ オプションを参照してください[`HttpContext.Items`キャッシュ ストアの 1 秒あたりの要求](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx)です。


## <a name="step-4-completing-the-editing-interface"></a>手順 4: 完了編集インターフェイス

作業の進行状況を表示するを一時停止せず GridView のテンプレートに対する変更の数も作成しています。 ブラウザーを使って作業の進行状況を表示するのにはしばらくを実行します。 使用して各行をレンダリングする図 13 では、その`ItemTemplate`、編集インターフェイス cell が含まれています。


[![GridView の各行は編集可能であります。](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**図 13**: GridView の各行は編集可能である ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image22.png))


おはずの時点で、いくつかの軽微な書式設定問題があります。 まず、なお、`UnitPrice`値には、4 つの 10 進数のポイントが含まれています。 この問題を解決するにを返します、 `UnitPrice` TemplateField の`ItemTemplate`TextBox s のスマート タグから databindings の編集 リンクをクリックします。 次に、指定する、`Text`プロパティは数値として書式設定する必要があります。


![Text プロパティを数値として書式設定します。](batch-updating-cs/_static/image14.gif)

**図 14**: 形式、`Text`数値としてプロパティ


次のチェック ボックスの中央 let s、 `Discontinued` (左揃えさせることのではなく) 列。 GridView s のスマート タグからの列の編集 をクリックし、選択、`Discontinued`左下隅のフィールドの一覧から TemplateField です。 ドリル ダウン`ItemStyle`設定と、`HorizontalAlign`センターの図 15 に示すようにプロパティです。


![Center 提供が中止されたチェック ボックス](batch-updating-cs/_static/image15.gif)

**図 15**: Center、 `Discontinued`  チェック ボックス


次に、ValidationSummary コントロールをページに追加し、設定、`ShowMessageBox`プロパティを`true`とその`ShowSummary`プロパティを`false`です。 Button Web コントロールがクリックされたときにも追加、ユーザーの変更が更新されます。 GridView 上と下に、両方のコントロールを設定する 2 つのボタンの Web コントロールを具体的には、追加`Text`更新プログラムの製品のプロパティです。

GridView s 以降編集インターフェイスがで定義されているその TemplateFields `ItemTemplate` 、s、 `EditItemTemplate` s 余分なされ、削除することがあります。

上記を行うには、書式設定の変更説明したように後のボタン コントロールの追加と削除、不要な`EditItemTemplate`s、ページの宣言型の構文は次のようになります必要があります。


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

図 16 では、ボタンの Web コントロールを追加した後にブラウザーで表示したときに、このページおよび書式設定の変更を示します。


[![ページ Now には、2 つの更新プログラムの製品ボタンが含まれています。](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**図 16**:、ページようになりましたが含まれています 2 つ更新プログラムの製品のボタン ([フルサイズのイメージを表示するをクリックして](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>手順 5: 製品の更新

ユーザーがこのページを訪問したときに、その変更を行い、2 つの更新プログラムの製品ボタンのいずれかをクリックして、されます。 その時点で何らかの理由で、ユーザーが入力した各行の値を保存する必要があります、`ProductsDataTable`をインスタンス化し、されるには合格し、BLL メソッドを渡す`ProductsDataTable`DAL s インスタンス`UpdateWithTransaction`メソッドです。 `UpdateWithTransaction`メソッドで作成した、[前のチュートリアル](wrapping-database-modifications-within-a-transaction-cs.md)変更のバッチをアトミックな操作として更新することを確認します。

という名前のメソッドを作成する`BatchUpdate`で`BatchUpdate.aspx.cs`し、次のコードを追加します。


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

このメソッドは、まずすべての製品の取得に戻り、 `ProductsDataTable` BLL の呼び出しを経由して`GetProducts`メソッドです。 列挙し、 `ProductGrid` GridView s [ `Rows`コレクション](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)です。 `Rows`コレクションが含まれています、 [ `GridViewRow`インスタンス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx)GridView に表示される行はごとです。 最大で 10 行、1 ページあたり GridView s を示していますので`Rows`コレクションには、10 個の項目があります。

行ごとに、`ProductID`から取得、`DataKeys`コレクションと、適切な`ProductsRow`からが選択されている、`ProductsDataTable`です。 TemplateField の 4 つの入力コントロールがプログラムによって参照されているし、その値を割り当てる、`ProductsRow`のプロパティをインスタンス化します。 更新する各 GridView の後に行の値が使用されて、 `ProductsDataTable`、BLL s に渡される s `UpdateWithTransaction` 、前のチュートリアルで示したように単純を呼び出すメソッド DAL の`UpdateWithTransaction`メソッドです。

このチュートリアルで使用されるバッチ更新アルゴリズム内の各行の更新、`ProductsDataTable`製品の情報が変更されたかどうかに関係なく、GridView の行に対応します。 このような blind は t 通常、パフォーマンスの問題を更新中にデータベース テーブルの変更の監査する場合に余分なレコードをつながることができます。 戻り、[バッチ更新を実行する](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)チュートリアル DataList でインターフェイスの更新バッチに調査し、ユーザーが実際に変更されたレコードを更新のみコードを追加します。 自由に技法を使用して[を実行するバッチ更新](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md)必要な場合、このチュートリアルでは、コードを更新します。

> [!NOTE]
> ときにデータ ソースをバインドすると、そのスマート タグから GridView に、Visual Studio に自動的にデータ ソースの主キー値を割り当てます GridView の`DataKeyNames`プロパティです。 バインドされなかった、ObjectDataSource、GridView s のスマート タグから GridView に、手順 1. で説明したかどうかは、GridView s を手動で設定する必要があります`DataKeyNames`プロパティにアクセスするために ProductID を`ProductID`を介して各の行の値、`DataKeys`コレクション。


使用するコード`BatchUpdate`BLL s で使用されているような`UpdateProduct`メソッド、記述されていることの主な違い、`UpdateProduct`メソッドが 1 つだけ`ProductRow`アーキテクチャからインスタンスを取得します。 プロパティを割り当てるコード、`ProductRow`間で同じでは、`UpdateProducts`メソッドと、コード内で、`foreach`ループ`BatchUpdate`全体的なパターンが格納される、します。

このチュートリアルを完了する必要がありますが、`BatchUpdate`メソッドが呼び出されたときに更新プログラムの製品ボタンのいずれかをクリックします。 イベント ハンドラーを作成、`Click`これらの 2 つのイベントのコントロールのボタンをクリックし、イベント ハンドラーに次のコードを追加します。


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

呼び出しが行われた最初`BatchUpdate`です。 次に、`ClientScript property`製品が更新されましたが読み取るメッセージ ボックスを表示する JavaScript を挿入するために使用します。

このコードをテストする分ほどをかかります。 参照してください`BatchUpdate.aspx`ブラウザーから、行の数を編集し、いずれかの更新プログラムの製品ボタンをクリックします。 入力の検証エラーがないと仮定した場合、製品が更新されましたが読み取るメッセージ ボックスが表示されます。 更新の原子性を確認するため、ランダムな追加を検討して`CHECK`が禁止されているいずれかのように、制約`UnitPrice`1234.56 の値。 `BatchUpdate.aspx`、S、製品のいずれかを設定することを確認のレコードの数を編集`UnitPrice`値は禁止されている値 (1234.56) にします。 元の値をそのバッチ操作中に、その他の変更と更新プログラムの製品をクリックすると、ロールバックされたときに、エラーこの発生する必要があります。

## <a name="an-alternativebatchupdatemethod"></a>代わりに`BatchUpdate`メソッド

`BatchUpdate`おメソッド取得の検査*すべて*BLL s から製品の`GetProducts`メソッドと GridView に表示されるには、これらのレコードを更新します。 GridView は、ページングを使用しない場合は、される可能性しますが、あります数百、何千も、または数万人の製品が GridView で 10 個までの行の場合に、この方法を使用することが理想的です。 このような場合は、そのうち 10 を変更するだけデータベースからすべての製品の取得より小さい最適です。

これらの種類の状況の使用を検討して、次`BatchUpdateAlternate`メソッド代わりにします。


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate`新しい空を作成して開始`ProductsDataTable`という`products`です。 これは、後、GridView s をステップ実行`Rows`コレクション各行 BLL s を使用して特定の製品情報を取得するのと`GetProductByProductID(productID)`メソッドです。 取得した`ProductsRow`インスタンスと同じ方法で更新のプロパティがあります`BatchUpdate`が、インポートを行を更新した後、 `products``ProductsDataTable` DataTable s を介して[`ImportRow(DataRow)`メソッド](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx)です。

後に、`foreach`ループが完了すると、 `products` 1 つ含む`ProductsRow`GridView の行ごとのインスタンス。 以降の各、`ProductsRow`インスタンスに追加された、 `products` (の代わりに更新されます)、お無条件に渡す場合、`UpdateWithTransaction`メソッド、`ProductsTableAdatper`各レコードをデータベースに挿入しようとしてがします。 代わりに、それぞれの行が変更されたこと (追加されません) を指定する必要があります。

これには、名前付き BLL に新しいメソッドを追加して`UpdateProductsWithTransaction`です。 `UpdateProductsWithTransaction`、、次のように設定、`RowState`のそれぞれの、`ProductsRow`のインスタンスにある、`ProductsDataTable`に`Modified`し渡します、 `ProductsDataTable` DAL s`UpdateWithTransaction`メソッドです。


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>まとめ

GridView は、行ごとの組み込みの編集機能を提供が完全に編集可能なインターフェイスを作成するためのサポートがありません。 このチュートリアルで説明したとおり、このようなインターフェイスが、作業のビットを必要とします。 すべての行は編集 GridView を作成する必要があります TemplateFields GridView のフィールドに変換して内で編集のインターフェイスを定義する、 `ItemTemplate` s。 また、更新すべてのボタン Web コントロールの種類は、GridView とは別のページに追加する必要があります。 これらのボタン`Click`イベント ハンドラーが GridView s を列挙する必要があります`Rows`、コレクション内の変更を保存、 `ProductsDataTable`、更新された情報を適切な BLL メソッドに渡すとします。

次のチュートリアルは、バッチを削除するためのインターフェイスを作成する方法が表示されます。 具体的には、GridView の各行はチェック ボックスなどの代わりにすべての更新-入力ボタンを選択した行の削除ボタン必要があります。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよび David Suru がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](wrapping-database-modifications-within-a-transaction-cs.md)
[次へ](batch-deleting-cs.md)
