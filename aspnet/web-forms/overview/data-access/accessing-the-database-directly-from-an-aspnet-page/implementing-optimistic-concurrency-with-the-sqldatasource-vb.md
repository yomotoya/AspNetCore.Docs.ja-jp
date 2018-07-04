---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: SqlDataSource (VB) によるオプティミスティック同時実行制御を実装する |Microsoft Docs
author: rick-anderson
description: このチュートリアルではオプティミスティック同時実行制御の基礎を確認し、SqlDataSource コントロールを使用して実装する方法を調べます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: a71e5ee06e4a970e7e6d6c3b5b0351ad218e0253
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391048"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>SqlDataSource (VB) によるオプティミスティック同時実行制御を実装します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe)または[PDF のダウンロード](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> このチュートリアルではオプティミスティック同時実行制御の基礎を確認し、SqlDataSource コントロールを使用して実装する方法を調べます。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、挿入、更新、および削除 SqlDataSource コントロールに機能を追加する方法について確認しました。 つまり、対応するを指定するためこれらの機能を提供する`INSERT`、 `UPDATE`、または`DELETE`s コントロールでの SQL ステートメント`InsertCommand`、 `UpdateCommand`、または`DeleteCommand`と共に、適切なプロパティ内のパラメーター、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。 データ ソースの構成ウィザードの [詳細設定] で、生成を提供中に、これらのプロパティとコレクションを手動で指定することができます、 `INSERT`、 `UPDATE`、および`DELETE`が自動的に作成これらのステートメントのステートメントのチェック ボックスに基づいて、`SELECT`ステートメント。

生成と共に`INSERT`、 `UPDATE`、および`DELETE`ステートメントのチェック ボックス、SQL 生成の詳細オプション ダイアログ ボックスにはに、オプション使用してオプティミスティック同時実行制御にはが含まれています (図 1 参照)。 選択した場合、`WHERE`句で、自動生成された`UPDATE`と`DELETE`のみ更新を実行するステートメントが変更または削除、ユーザーから基になるデータベースのデータから t が変更された場合最後に読み込まれるデータ グリッドです。


![オプティミスティック同時実行制御のサポートを追加するには、高度なから SQL 生成のオプション ダイアログ ボックス](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**図 1**: オプティミスティック同時実行制御のサポートを追加するには、高度なから SQL 生成のオプション ダイアログ ボックス


戻り、[オプティミスティック同時実行を実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)チュートリアル オプティミスティック同時実行制御と ObjectDataSource に追加する方法の基本について確認しました。 このチュートリアルでは、オプティミスティック同時実行制御の essentials レタッチを SqlDataSource を使用して実装する方法を調べます。

## <a name="a-recap-of-optimistic-concurrency"></a>オプティミスティック同時実行制御のまとめ

Web アプリケーションを複数の同時ユーザーを編集または削除は、同一データが存在する可能性を 1 人のユーザーが別の変更を誤って上書き可能性があります。 [オプティミスティック同時実行を実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)チュートリアルの次の例を提供しました。

Jisun と Sam、2 人のユーザーが両方にアクセスして更新および削除の GridView コントロールを使用して製品への訪問者を許可されているアプリケーションでのページを想像してください。 Chai の編集 をクリックして両方、同時期にします。 Jisun は Chai 紅茶に製品名を変更し、[更新] ボタンをクリックします。 最終的には、`UPDATE`を設定すると、データベースに送信されるステートメント*すべて*の製品の更新可能なフィールド (Jisun では、1 つのフィールドのみ更新される場合でも`ProductName`)。 この時点では、データベースでは、Chai 紅茶、飲料カテゴリ サプライヤー風変わり液体のこの特定の製品の値があります。 ただし、Sam の画面に GridView として表示されます、製品名の編集可能な GridView 行 Chai。 Jisun s の変更がコミットされた後、数秒 Sam は調味料にカテゴリを更新し、更新プログラムをクリックします。 これは、結果、 `UPDATE` Chai、製品名に設定するデータベースに送信されたステートメント、`CategoryID`対応する調味料カテゴリ ID、および具合に。 Jisun の製品名の変更が上書きされました。

図 2 は、この相互作用を示しています。


[![2 人のユーザー レコードを同時に更新するときに、その他の s を上書きするがあります s の潜在的 1 人のユーザーの変更](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**図 2**: ときに 2 つのユーザーに同時に更新プログラム、レコードが潜在的な 1 人のユーザーの変更を他を上書きするため ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))。


このシナリオがの形式を開くことを防ぐために[同時実行制御](http://en.wikipedia.org/wiki/Concurrency_control)実装する必要があります。 [オプティミスティック同時実行制御](http://en.wikipedia.org/wiki/Optimistic_concurrency_control)がある同時実行の競合し、このような競合が t が勝利した時間の大部分が発生したことを前提としてこのチュートリアルの目的が動作します。 そのため場合は、競合が発生した場合、オプティミスティック同時実行制御単にユーザーに通知、別のユーザーには、同じデータが変更されたために、その変更は t が保存されること。

> [!NOTE]
> 多くの同時実行の競合があるか、このような競合が許容されないかどうかの想定をアプリケーションでは、し、ペシミスティック同時実行制御する代わりに使用できます。 参照、[オプティミスティック同時実行を実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md)ペシミスティック同時実行制御についてはより詳細なチュートリアルです。


オプティミスティック同時実行制御は、更新または削除プロセスを開始するときと同様、更新または削除されるレコードが同じ値があることを確認することによって機能します。 たとえば、編集可能な GridView で [編集] ボタンをクリックすると s のレコードの値はデータベースからの読み取りし、テキスト ボックスや他の Web コントロールに表示されます。 これらの元の値は、GridView で保存されます。 後で、ユーザーが自分の変更を行いますが、更新ボタンをクリックした後、`UPDATE`ステートメントを使用する必要があります、元の値と新しい値を考慮し、元の値は、ユーザーが編集を開始した場合にのみ、基になるデータベース レコードを更新引き続き、データベース内の値と同じです。 図 3 は、このイベントのシーケンスを示しています。


[![正常に更新または削除、元の値は現在のデータベースの値と等しくする必要があります。](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**図 3**: For Update または成功を元の値必要がありますと等しいデータベースの現在の値を Delete ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))。


オプティミスティック同時実行制御を実装するためのさまざまな方法はあります (を参照してください[Peter A. 作成](http://peterbromberg.net/)s [Optmistic 同時実行更新ロジック](http://www.eggheadcafe.com/articles/20050719.asp)のさまざまなオプションについて簡単に説明)。 SqlDataSource (および、データ アクセス層で使用される ADO.NET 型指定されたデータセット) を使用する手法は、`WHERE`に含めるすべての元の値の比較句。 次`UPDATE`ステートメントでは、たとえば、更新プログラム名と製品の価格データベースの現在の値が、GridView でレコードを更新するときに取得された元の値に等しい場合のみです。 `@ProductName`と`@UnitPrice`パラメーターには、ユーザーが入力した新しい値が含まれて`@original_ProductName`と`@original_UnitPrice`編集ボタンがクリックされたときに、GridView に読み込まれた最初の値が含まれます。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

このチュートリアルでは表示されるようは、チェック ボックスを同じくらい簡単です、SqlDataSource でオプティミスティック同時実行制御を有効にします。

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>手順 1: は、オプティミスティック同時実行制御をサポートする、SqlDataSource を作成します。

開いて開始、`OptimisticConcurrency.aspx`ページから、`SqlDataSource`フォルダー。 SqlDataSource コントロールをドラッグして、デザイナーの設定には、ツールボックスからその`ID`プロパティを`ProductsDataSourceWithOptimisticConcurrency`します。 次に、コントロールのスマート タグからのデータ ソースの構成のリンクをクリックします。 ウィザードの最初の画面で選択を使用する、 `NORTHWINDConnectionString` [次へ] をクリックします。


[![選択、NORTHWINDConnectionString を操作する](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**図 4**: を選択、 `NORTHWINDConnectionString` ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))。


この例ではユーザーが編集できるようにする GridView に追加する、`Products`テーブル。 そのため、ステートメントの選択画面の構成 から選択、`Products`ドロップダウン リストからテーブルを選択、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列、図 5 に示すようにします。


[![Products テーブルから ProductID、ProductName、UnitPrice、および提供が中止された列を返す](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**図 5**: から、`Products`テーブルを返す、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))。


列を選択した後は、SQL 生成の詳細オプション ダイアログ ボックスを表示する 詳細設定 をクリックします。 確認の生成`INSERT`、 `UPDATE`、および`DELETE`ステートメントとオプティミスティック同時実行制御チェック ボックスを使用し、[ok] をクリックします (参照図 1 のスクリーン ショット)。 [次へ] をクリックすると、ウィザードを完了し、完了します。

データ ソース構成ウィザードを完了すると、実行結果を確認するには、少し`DeleteCommand`と`UpdateCommand`プロパティおよび`DeleteParameters`と`UpdateParameters`コレクション。 これを行う最も簡単な方法では、ページの宣言型構文を参照する左下隅の [ソース] タブをクリックします。 サイトでは、`UpdateCommand`の値。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

7 つのパラメーターを持つ、`UpdateParameters`コレクション。


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

同様に、`DeleteCommand`プロパティと`DeleteParameters`コレクションは、次のようになります。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

追加することだけでなく、`WHERE`句では、`UpdateCommand`と`DeleteCommand`プロパティ (と、それぞれのパラメーター コレクションに追加のパラメーターを追加する)、オプティミスティック同時実行オプションを調整します他の 2 つの使用を選択します。プロパティ:

- 変更、 [ `ConflictDetection`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx)から`OverwriteChanges`(既定値) に `CompareAllValues`
- 変更、 [ `OldValuesParameterFormatString`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx)から{0}(既定) 元のイメージに\_{0}します。

データ Web コントロールが SqlDataSource s を呼び出すときに`Update()`または`Delete()`メソッド、元の値を渡されます。 場合、SqlDataSource s`ConflictDetection`プロパティに設定されて`CompareAllValues`、元の値は、コマンドに追加されます。 `OldValuesParameterFormatString`プロパティはこれらの元の値パラメーターに使用する名前付けパターンを提供します。 データ ソース構成ウィザードを使用して元\_{0}し名前を元の各パラメーターに、`UpdateCommand`と`DeleteCommand`プロパティと`UpdateParameters`と`DeleteParameters`コレクションに応じて。

> [!NOTE]
> 削除を自由に再挿入する機能、SqlDataSource コントロール s を使用していないことがあるため、`InsertCommand`プロパティとその`InsertParameters`コレクション。


## <a name="correctly-handlingnullvalues"></a>適切に処理`NULL`値

残念ながら、augmented`UPDATE`と`DELETE`オプティミスティック同時実行制御を使用する場合は、データ ソースの構成ウィザードでのステートメントの自動生成されたは*いない*レコードが含まれていると連携して`NULL`値。 その理由を表示するには、SqlDataSource、s を検討してください`UpdateCommand`:。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice`内の列、`Products`テーブル`NULL`値。 特定のレコードがある場合、`NULL`値`UnitPrice`、`WHERE`句部分`[UnitPrice] = @original_UnitPrice`は*常に*False に評価されたため、`NULL = NULL`常に False を返します。 そのため、レコードを含む`NULL`値を編集したり、削除できないとして、`UPDATE`と`DELETE`ステートメント`WHERE`句は t の戻り値を更新または削除する対象の行。

> [!NOTE]
> このバグが最初に 2004 年 6 月に Microsoft に報告[生成の不適切な SQL ステートメントを SqlDataSource](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937)され、次のバージョンの ASP.NET で修正される予定とです。


手動で更新するがこれを解決する、`WHERE`の両方の句、`UpdateCommand`と`DeleteCommand`プロパティを**すべて**ことができる列`NULL`値。 一般に、変更`[ColumnName] = @original_ColumnName`に。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

この変更のプロパティ ウィンドウから UpdateQuery または DeleteQuery オプションを使用して、宣言型マークアップまたは、UPDATE から直接実行でき、カスタム SQL ステートメントまたはストアド プロシージャのオプション データの構成での指定タブを削除ウィザードのソース。 この変更を再度行う必要があります*すべて*内の列、`UpdateCommand`と`DeleteCommand`s`WHERE`句を含めることができます`NULL`値。

結果の例にこれを適用する変更は次`UpdateCommand`と`DeleteCommand`値。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>手順 2: GridView 編集および削除のオプションを追加します。

オプティミスティック同時実行制御をサポートするように構成 SqlDataSource ではこの同時実行制御を使用するページにデータ Web コントロールを追加します。 このチュートリアルでは、s の両方の編集を提供する GridView の追加し、削除機能を使用できます。 これを実現するデザイナーとセットには、ツールボックスから、GridView をドラッグします。 その`ID`に`Products`します。 GridView のスマート タグからバインドを`ProductsDataSourceWithOptimisticConcurrency`SqlDataSource コントロールを手順 1. で追加します。 最後に、スマート タグから編集の有効化および削除を有効にするオプションを確認します。


[![SqlDataSource GridView にバインドし、編集および削除を有効にします。](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**図 6**: SqlDataSource と編集の有効化および削除すると、GridView にバインド ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))。


GridView を追加すると、削除することによって、外観を設定、 `ProductID` BoundField、変更、 `ProductName` BoundField s`HeaderText`製品、および更新するプロパティ、 `UnitPrice` BoundField ようにその`HeaderText`プロパティは、単に価格です。 理想的には、d を向上させ、編集インターフェイスを含めるための RequiredFieldValidator、`ProductName`値との CompareValidator、 `UnitPrice` (s 数値の値を正しく書式設定されたように) する値。 参照してください、[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)」チュートリアルをさらに詳しい編集インターフェイス GridView s をカスタマイズします。

> [!NOTE]
> ビュー内の状態で、GridView が SqlDataSource を GridView から渡された元の値があるために、ビュー状態を有効にする必要がありますが格納されています。


GridView に次の変更を加えたら、GridView や SqlDataSource 宣言型マークアップは次のようになります。


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

アクションでオプティミスティック同時実行制御を表示する 2 つのブラウザー ウィンドウを開くし、ロード、`OptimisticConcurrency.aspx`両方のページ。 両方のブラウザーで最初の製品の編集ボタンをクリックします。 1 つのブラウザーで、製品名を変更し、[更新] をクリックします。 ブラウザーがポストバックをし、編集したレコードに対して新しい製品の名前が表示、編集済みのモードに戻りますが、GridView。

2 番目のブラウザー ウィンドウで (ただし、元の値として製品の名前のままに) 価格を変更し、[更新] をクリックします。 ポストバックでは、グリッドが、事前の編集モードに戻りますが、価格に変更は記録されません。 2 番目のブラウザーでは、1 つ目と同じ値に古い価格に新しい製品名が表示されます。 2 番目のブラウザー ウィンドウで行った変更が失われました。 さらに、変更が失われましたサイレント モードではなく、例外または同時実行制御違反が発生したことを示すメッセージがないです。


[![2 番目のブラウザー ウィンドウで変更が自動的に失われました](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**図 7**: で、2 番目のブラウザー ウィンドウがサイレント モードで失われる、変更 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))。


なぜ 2 つ目のブラウザーの変更でコミットされていない理由でしたので、`UPDATE`ステートメント`WHERE`句は、すべてのレコードをフィルター処理し、任意の行によって影響されなかったためです。 S を確認できるように、`UPDATE`ステートメントをもう一度。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

元の製品名が指定された 2 番目のブラウザー ウィンドウが、レコードを更新すると、`WHERE`句は t との一致を既存の製品名 (最初のブラウザーでの変更) 以降。 そのため、ステートメント`[ProductName] = @original_ProductName`False を返します、`UPDATE`レコードには影響しません。

> [!NOTE]
> 同様に、delete の動作。 2 つのブラウザー ウィンドウが開き、まず、1 つの特定の製品を編集し、その変更を保存します。 1 つのブラウザーで、変更を保存した後、それ以外の同じ製品の削除 ボタンをクリックします。 内元の値は t が同一であるため、`DELETE`ステートメント`WHERE`句では、削除は失敗します。


2 番目のブラウザー ウィンドウでのエンド ユーザー s の観点からの更新ボタンをクリックした後グリッドが編集済みのモードを返しますが、変更が失われました。 ただし、s が、変更は一致しませんでした t が引き続き使用する視覚的フィードバックがありません。 理想的には、ユーザーの変更が同時実行制御違反が失われた場合は、d に通知し、おそらく、グリッドを編集モードで保持します。 これを実現する方法を見て s を使用できます。

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>手順 3: 同時実行制御違反が発生した場合を決定します。

同時実行制御違反は、いずれかが行われた変更を拒否するため、同時実行制御違反が発生した場合にアラートをユーザーになります。 Let s がという名前のページの上部にラベル Web コントロールを追加して、警告、`ConcurrencyViolationMessage`が`Text`プロパティには、次のメッセージが表示されます。 更新または別のユーザーによって同時に更新されたレコードを削除しようとしました。 その他のユーザーの変更を確認し、更新プログラムを再実行か削除してください。 ラベル コントロールの設定`CssClass`で定義されているプロパティは、CSS クラスの警告を`Styles.css`赤、斜体、太字、および大規模なフォントでテキストを表示します。 最後に、s のラベルを設定`Visible`と`EnableViewState`プロパティ`False`します。 これは明示的に設定します、ポストバックのみを除く、ラベルを非表示にその`Visible`プロパティを`True`します。


[![警告を表示するページにラベル コントロールを追加します。](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**図 8**: 警告を表示するページにラベル コントロールを追加 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))。


更新または削除、GridView s を実行するときに`RowUpdated`と`RowDeleted`イベント ハンドラーがそのデータ ソース コントロールは、要求された更新または削除が実行後に起動します。 これらのイベント ハンドラーから、操作によって影響を受けた行の数を判断できます。 表示する場合は 0 行が影響を受けた、`ConcurrencyViolationMessage`ラベル。

両方のイベント ハンドラーを作成、`RowUpdated`と`RowDeleted`イベントし、次のコードを追加します。


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

確認の両方のイベント ハンドラー、`e.AffectedRows`プロパティと、0 の場合、設定、`ConcurrencyViolationMessage`ラベル s`Visible`プロパティを`True`。 `RowUpdated`イベント ハンドラーもよう指示し編集モードを設定して維持するために、GridView、`KeepInEditMode`プロパティを true にします。 そのため、その他のユーザー データが編集インターフェイスに読み込まれるように、グリッドにデータを再バインドする必要があります。 これは、GridView s を呼び出すことによって実現`DataBind()`メソッド。

これら 2 つのイベント ハンドラーを使用して、図 9 に示すように、同時実行制御違反が発生するたびに非常に顕著なメッセージが表示されます。


[![同時実行制御違反が発生した場合、メッセージが表示されます。](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**図 9**: 同時実行制御違反が発生した場合、メッセージが表示されます ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))。


## <a name="summary"></a>まとめ

Web アプリケーションを作成するときに、複数の同時実行ユーザーが、同じデータを編集する同時実行制御オプションを検討することが重要です。 既定では、ASP.NET のデータ Web コントロールとデータ ソース コントロールを使用していない、同時実行制御。 このチュートリアルで説明したように比較的迅速かつ簡単には、SqlDataSource でオプティミスティック同時実行制御を実装します。 SqlDataSource の拡張、追加の処理の大半を`WHERE`句を自動生成された`UPDATE`と`DELETE`ステートメントがありますが、処理のいくつかの微妙な`NULL`で説明したように、列の値、適切に処理`NULL`セクションの値します。

このチュートリアルでは、SqlDataSource、調査を終了します。 ObjectDataSource と階層型アーキテクチャを使用してデータを扱うには、残りのチュートリアルを返します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
