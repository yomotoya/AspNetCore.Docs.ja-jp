---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: SqlDataSource (c#) によるオプティミスティック同時実行制御を実装する |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルではオプティミスティック同時実行制御の essentials を確認し、SqlDataSource コントロールを使用してそれを実装する方法を調査します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 246e8d0c2aee7358680fbca7229cc9b05ceca1cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>SqlDataSource (c#) によるオプティミスティック同時実行制御を実装します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe)または[PDF のダウンロード](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> このチュートリアルではオプティミスティック同時実行制御の essentials を確認し、SqlDataSource コントロールを使用してそれを実装する方法を調査します。


## <a name="introduction"></a>はじめに

前のチュートリアルは、挿入、更新、および削除、SqlDataSource コントロールする機能を追加する方法を調査します。 つまり、これらの機能を提供する必要がある、対応するを指定する`INSERT`、 `UPDATE`、または`DELETE`s コントロール内の SQL ステートメント`InsertCommand`、 `UpdateCommand`、または`DeleteCommand`プロパティは、適切なと共に内のパラメーター、 `InsertParameters`、 `UpdateParameters`、および`DeleteParameters`コレクション。 データ ソースの構成ウィザードの [詳細設定] には、生成中に、これらのプロパティとコレクションを手動で指定することができます、 `INSERT`、 `UPDATE`、および`DELETE`は自動的に作成するこれらのステートメントをステートメントのチェック ボックスがに基づいて、`SELECT`ステートメントです。

生成と共に`INSERT`、 `UPDATE`、および`DELETE`SQL 生成の詳細オプション ダイアログ ボックス、ステートメントのチェック ボックスには使用するオプティミスティック同時実行制御オプションが含まれています (図 1 を参照してください)。 オンにした場合、`WHERE`自動生成された句`UPDATE`と`DELETE`だけ更新プログラムを実行するステートメントが変更または削除のユーザーから、基になるデータベース データいない t が変更された最後に読み込まれ、データ グリッドです。


![オプティミスティック同時実行制御のサポートを追加するには、高度なから SQL 生成のオプション ダイアログ ボックス](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**図 1**: オプティミスティック同時実行制御のサポートを追加するには、高度なから SQL 生成のオプション ダイアログ ボックス


戻り、[オプティミスティック同時実行制御で実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)チュートリアル オプティミスティック同時実行制御および、ObjectDataSource を追加する方法の基礎について説明しました。 このチュートリアルでは、オプティミスティック同時実行制御の essentials レタッチを SqlDataSource を使用してそれを実装する方法を調査します。

## <a name="a-recap-of-optimistic-concurrency"></a>オプティミスティック同時実行制御の要約

複数、web アプリケーションの同時ユーザーを編集または削除は、同一データが存在する可能性のあるユーザーが別の s の変更を誤って上書き可能性があります。 [オプティミスティック同時実行制御で実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)チュートリアルの次の例を入力します。

ある Jisun と Sam、2 人のユーザーが両方のページにアクセス更新および削除の GridView コントロールを使用して製品への訪問者を許可するアプリケーションで想像してください。 両方を Chai の同時期、[編集] ボタンがクリックします。 Jisun はチャイに製品名を変更し、[更新] ボタンをクリックします。 最終的には、`UPDATE`を設定すると、データベースに送信されるステートメント*すべて*製品の更新可能なフィールドの (Jisun では、1 つのフィールドのみ更新される場合でも`ProductName`)。 この時点では、データベースでは、チャイ、飲料カテゴリ供給業者の特別な液体この特定の製品についての値があります。 ただし、Sam の画面の GridView であっても、製品名の編集可能な GridView 行 Chai として。 Jisun の変更がコミットされた後、数秒 Sam は調味料にカテゴリを更新し、更新プログラムをクリックします。 これは、結果、 `UPDATE` Chai に製品名を設定しているデータベースに送信されたステートメント、`CategoryID`対応する調味料カテゴリ ID というようにします。 Jisun の製品名の変更が上書きされました。

図 2 は、この操作を示します。


[![2 人のユーザーがレコードを同時に更新があります s れる可能性がある 1 つのユーザーの変更を他の s を上書き](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**図 2**: ときに 2 つのユーザーに同時に更新、レコードが潜在的な 1 つのユーザーの変更を上書きする他のため ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


このシナリオのフォームを開くことを防ぐため[同時実行制御](http://en.wikipedia.org/wiki/Concurrency_control)実装する必要があります。 [オプティミスティック同時実行制御](http://en.wikipedia.org/wiki/Optimistic_concurrency_control)このチュートリアルの焦点はこともありますが同時実行の競合し、このような競合で優先 t 時間の大部分が発生することを前提に動作します。 そのため場合は、競合が発生した場合、オプティミスティック同時実行制御だけでユーザーに通知して、別のユーザーには、同じデータが変更されたために、その変更が t が保存されることです。

> [!NOTE]
> 場所と見なされます多くの同時実行の競合があるか、このような競合が許容されないかどうかは、アプリケーションをペシミスティック同時実行制御ことがでく代わりに使用します。 戻って、[オプティミスティック同時実行制御で実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)の詳細については、ペシミスティック同時実行制御のためのチュートリアルです。


オプティミスティック同時実行制御は、更新または削除されたレコードの同じ値は、更新または削除プロセスが開始されたときと同様にあることを確認することによって機能します。 たとえば、編集可能な GridView [編集] ボタンをクリックすると、レコード秒の値をデータベースからの読み取りし、がテキスト ボックスおよびその他の Web コントロールに表示されます。 元の値が GridView で保存されます。 その後、ユーザーは自分が加えた変更は、し、更新ボタンをクリックすると、`UPDATE`ステートメントを使用が元の値と新しい値を考慮し、元の値は、ユーザーが編集を開始した場合にのみ、基になるデータベースのレコードを更新する必要があります引き続き、データベース内の値と同じです。 図 3 は、このイベントのシーケンスを示しています。


[![正常に更新または削除は、元の値を現在のデータベースの値と同じにする必要があります。](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**図 3**: For Update または Delete の成功を元の値必要がある値に等しく、現在データベースに ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


オプティミスティック同時実行制御を実装するためのさまざまな方法があります (を参照してください[Peter A. 作成](http://www.eggheadcafe.com/articles/pbrombergresume.asp)s [Optmistic 同時実行更新ロジック](http://www.eggheadcafe.com/articles/20050719.asp)いくつかのオプションについて簡単に説明用)。 SqlDataSource (別、およびデータ アクセス層で使用される ADO.NET 型指定されたデータセット) に使用される手法の強化、`WHERE`に含めるすべての元の値の比較句。 次`UPDATE`ステートメントでは、たとえば、更新プログラム名と製品の価格データベースの現在の値が GridView でレコードを更新するときに取得された元の値に等しい場合のみです。 `@ProductName`と`@UnitPrice`パラメーターに対し、ユーザーが入力した新しい値を含める`@original_ProductName`と`@original_UnitPrice`もともと読み込まれた GridView [編集] ボタンがクリックしてされたときに値が含まれます。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

このチュートリアルで後ほどお見せ、としては、チェック ボックスをオンなどの単純なは、SqlDataSource によるオプティミスティック同時実行制御を有効にします。

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>手順 1: は、オプティミスティック同時実行制御をサポートする、SqlDataSource の作成

開いて開始、`OptimisticConcurrency.aspx`ページから、`SqlDataSource`フォルダーです。 SqlDataSource コントロールをドラッグして、デザイナーの設定には、ツールボックスからその`ID`プロパティを`ProductsDataSourceWithOptimisticConcurrency`です。 次に、コントロールのスマート タグからのデータ ソースの構成のリンクをクリックします。 ウィザードの最初の画面から作業を選択すると、 `NORTHWINDConnectionString` [次へ] をクリックします。


[![NORTHWINDConnectionString を選択します。](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**図 4**: を選択して、 `NORTHWINDConnectionString` ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


この例で追加されます。 ユーザーが編集できるようにする GridView、`Products`テーブル。 そのため、Select ステートメントの画面の構成 を選択、`Products`ドロップダウン リストからテーブルを選択して、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列、図 5 に示すようにします。


[![Products テーブルから ProductID、ProductName、単価、および提供が中止された列を返す](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**図 5**: から、`Products`テーブルで、返す、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`列 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


列を選択した後に、SQL 生成の詳細オプション ダイアログ ボックスを表示する高度なボタンをクリックします。 確認の生成`INSERT`、 `UPDATE`、および`DELETE`ステートメント オプティミスティック同時実行制御チェック ボックスを使用し、[ok] をクリックして (戻る図 1 を参照スクリーン ショットの)。 [次へ] をクリックしてウィザードを完了し、完了します。

データ ソース構成ウィザードを完了すると、結果を確認する少し時間を取って`DeleteCommand`と`UpdateCommand`プロパティおよび`DeleteParameters`と`UpdateParameters`コレクション。 これを行う最も簡単な方法では、ページの s 宣言の構文を表示する左下隅の ソース タブをクリックします。 サイトでは、`UpdateCommand`の値。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

7 つのパラメーターを持つ、`UpdateParameters`コレクション。


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

同様に、`DeleteCommand`プロパティおよび`DeleteParameters`コレクションは、次のようになります。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

拡張だけでなく、`WHERE`の句、`UpdateCommand`と`DeleteCommand`プロパティ (とそれぞれのパラメーター コレクションに追加のパラメーターを追加する)、オプティミスティック同時実行オプションを調整しますその他の 2 つの使用を選択します。プロパティ:

- 変更、 [ `ConflictDetection`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx)から`OverwriteChanges`(既定) に `CompareAllValues`
- 変更、 [ `OldValuesParameterFormatString`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx){0} (既定) に元から\_{0}。

データの Web コントロールが SqlDataSource s を呼び出す場合`Update()`または`Delete()`元の値で渡す方法、します。 場合は、SqlDataSource s`ConflictDetection`プロパティに設定されている`CompareAllValues`、元の値は、コマンドに追加されます。 `OldValuesParameterFormatString`プロパティはこれらの元の値パラメーターに使用する名前付けパターンを提供します。 データ ソース構成ウィザードを使用して元\_{0} し名前を元のパラメーターごと、`UpdateCommand`と`DeleteCommand`プロパティおよび`UpdateParameters`と`DeleteParameters`コレクションに応じて。

> [!NOTE]
> 使用していない機能を挿入する SqlDataSource コントロール s re お気軽を削除するため、`InsertCommand`プロパティとその`InsertParameters`コレクション。


## <a name="correctly-handlingnullvalues"></a>適切な処理`NULL`値

残念ながら、augmented`UPDATE`と`DELETE`ステートメント オプティミスティック同時実行制御を使用するときに、データ ソースの構成ウィザードによって自動的に生成しないで*いない*を含むレコードを操作`NULL`値。 その理由を表示するには、当社 SqlDataSource s を検討してください`UpdateCommand`:。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice`内の列、`Products`テーブルが持つことができます`NULL`値。 特定のレコードがある場合、`NULL`値を`UnitPrice`では、`WHERE`句部分`[UnitPrice] = @original_UnitPrice`は*常に*ので False に評価される`NULL = NULL`常に False を返します。 そのため、レコードが含まれている`NULL`値を編集または削除すると、できませんとして、`UPDATE`と`DELETE`ステートメント`WHERE`句 won t の戻り値の行を更新または削除します。

> [!NOTE]
> このバグが最初の 2004 年 6 月に Microsoft に報告[生成の不適切な SQL ステートメントを SqlDataSource](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937)が ASP.NET の次のバージョンで修正されるとスケジュールされているとします。


手動で更新する必要はこれを解決するには、`WHERE`の両方の句、`UpdateCommand`と`DeleteCommand`プロパティを**すべて**ことのある列`NULL`値。 一般に、変更`[ColumnName] = @original_ColumnName`に。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

この変更が [プロパティ] ウィンドウから抽出または DeleteQuery オプションを使用して、宣言型マークアップを使用して直接、または、UPDATE を通じて実行でき、カスタム SQL ステートメントまたはストアド プロシージャのオプションを構成するデータの指定でのタブを削除します。ウィザードのソース。 このような変更を再度行う必要があります*すべて*内の列、`UpdateCommand`と`DeleteCommand`s`WHERE`句を含めることができる`NULL`値。

この例に適用するこの結果、次の変更`UpdateCommand`と`DeleteCommand`値。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>手順 2: 追加の編集および削除オプションの GridView

オプティミスティック同時実行制御をサポートするように構成 SqlDataSource、によるはこの同時実行制御を利用したページにデータ Web コントロールを追加します。 このチュートリアルでは、s の両方の編集を提供する GridView の追加し、削除機能を使用できます。 これを実現するには、ツールボックスからデザイナーとセットに GridView をドラッグ、`ID`に`Products`です。 GridView s のスマート タグからバインドを`ProductsDataSourceWithOptimisticConcurrency`SqlDataSource コントロールを手順 1. で追加します。 最後に、スマート タグから編集を有効にして削除を有効にするオプションを確認します。


[![SqlDataSource GridView にバインドし、編集および削除を有効にします。](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**図 6**: GridView にバインド SqlDataSource と編集を有効にして削除 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


GridView を追加すると、削除することで、外観を設定、 `ProductID` BoundField、変更、 `ProductName` BoundField s`HeaderText`製品、および更新するプロパティ、 `UnitPrice` BoundField ようにその`HeaderText`プロパティは、単に価格です。 含めるの RequiredFieldValidator 編集インターフェイスを拡張 d お理想的には、`ProductName`値との CompareValidator、`UnitPrice`値 (s、正しく書式設定された数値ことを確認してください)。 参照してください、[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)編集インターフェイス GridView s のカスタマイズをより詳しくのためのチュートリアルです。

> [!NOTE]
> ビューステートに GridView から SqlDataSource に渡された元の値は s ビュー ステートを有効にする必要があります GridView 格納します。


GridView に次の変更を加えたら、GridView と SqlDataSource の宣言型マークアップを次のようになります。


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

アクションでオプティミスティック同時実行制御を表示するには、2 つのブラウザー ウィンドウを開くし、読み込み、`OptimisticConcurrency.aspx`両方のページです。 両方のブラウザーで最初の製品の編集ボタンをクリックします。 1 つのブラウザーで、製品名を変更し、[更新] をクリックします。 ブラウザーがポストバックをおよび GridView はモードに戻ります、前の編集、編集したレコードに対して新しい製品名を表示します。

2 番目のブラウザーのウィンドウで (ただし、元の値と製品名のままにして) 価格を変更し、[更新] をクリックします。 ポストバックでグリッドをあらかじめ編集モードを返しますが、価格の変更は記録されません。 2 番目のブラウザーは、1 つ目と同じ値に古い価格を含む新しい製品名を表示します。 2 番目のブラウザー ウィンドウで行った変更は失われました。 さらに、変更が失われたサイレント モードではなく、例外または同時実行制御違反が発生したことを示すメッセージがないためです。


[![2 番目のブラウザーのウィンドウで変更は自動的に失われました](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**図 7**: 2 番目のブラウザー ウィンドウ サイレント モードで失われた、変更 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


なぜブラウザー s の 2 番目の変更はコミットされていなかったためがあるため、`UPDATE`ステートメント`WHERE`句のすべてのレコードをフィルター処理され、したがってしなかったすべての行に影響しません。 Let s を見て、`UPDATE`ステートメントをもう一度。


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

元の製品名が指定された 2 番目のブラウザー ウィンドウには、レコードが更新され、ときに、`WHERE`句では t との一致を (最初のブラウザーによって変更された) してから、既存の製品名。 そのため、ステートメント`[ProductName] = @original_ProductName`は False を返しますと`UPDATE`いくつかのレコードには影響しません。

> [!NOTE]
> 同様に動作を削除します。 2 つのブラウザー ウィンドウが開き、編集、1 つの特定の製品とその変更を保存して起動します。 1 つのブラウザーで、変更を保存した後、他の同じ製品の削除 ボタンをクリックします。 一致、元の値がないのため、`DELETE`ステートメント`WHERE`句、削除は失敗します。


エンド ユーザー s の観点から 2 番目のブラウザーのウィンドウで、[更新] ボタンをクリックした後、グリッド返します事前編集モードが、その変更は失われました。 ただし、ある s なし、変更されなかった t スティックを視覚的なフィードバックです。 理想的には、ユーザーが変更されたが、同時実行制御違反が失われた場合は、d はその旨を通知、おそらくを維持する. グリッド編集モードで これを実現する方法について見て s を使用できます。

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>手順 3: は、同時実行制御違反が発生した場合を決定します。

同時実行制御違反は、いずれかが行われた変更を拒否するため、同時実行制御違反が発生したときにユーザーに警告する nice はなります。 Let s がという名前のページの上部にラベル Web コントロールを追加して、警告、`ConcurrencyViolationMessage`が`Text`プロパティには、次のメッセージが表示されます。 更新または別のユーザーによって同時に更新されたレコードを削除しようとしました。 他のユーザーの変更の確認し、更新プログラムを再実行か削除してください。 ラベル コントロール s 設定`CssClass`が警告には、CSS クラスのプロパティで定義`Styles.css`赤、斜体、太字、および大きなフォントでテキストを表示します。 最後に、s のラベルを設定`Visible`と`EnableViewState`プロパティ`false`です。 これは明示的に設定されているポストバックのみを除く、ラベルを非表示にその`Visible`プロパティを`true`です。


[![ラベル コントロールに、警告を表示するページを追加します。](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**図 8**: ラベル コントロールに、警告を表示するページを追加 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


更新または削除、GridView s を実行するときに`RowUpdated`と`RowDeleted`イベント ハンドラーがそのデータ ソース コントロールは、要求された更新または削除が実行後に起動します。 これらのイベント ハンドラーからの操作によって影響を受けた行の数を判断できます。 表示する場合は 0 行が影響を受けた、`ConcurrencyViolationMessage`ラベル。

両方のイベント ハンドラーを作成、`RowUpdated`と`RowDeleted`イベントし、次のコードを追加します。


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

確認の両方のイベント ハンドラー、`e.AffectedRows`プロパティと、0 の場合、設定、`ConcurrencyViolationMessage`ラベル s`Visible`プロパティを`true`です。 `RowUpdated` 、イベント ハンドラーもよう指示し、GridView を設定して編集モードで維持、`KeepInEditMode`プロパティを true にします。 これにより、編集のインターフェイスにその他のユーザー データが読み込まれるように、グリッドにデータを再バインドする必要があります。 GridView s を呼び出すことによってこれを行う`DataBind()`メソッドです。

これら 2 つのイベント ハンドラーを使用して、図 9 に示すように、同時実行制御違反が発生するたびに、非常に大きなメッセージが表示されます。


[![同時実行制御違反が発生した場合、メッセージが表示されます。](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**図 9**: 同時実行制御違反が発生した場合、メッセージが表示されます ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>まとめ

Web アプリケーションを作成するときに、複数、同時実行ユーザーが、同じデータを編集する同時実行制御オプションを検討することが重要です。 既定では、Web コントロールの ASP.NET データとデータ ソース コントロールを使用していないすべての同時実行制御。 このチュートリアルで説明したとおり、SqlDataSource でオプティミスティック同時実行制御を実装することは比較的迅速かつ簡単です。 SqlDataSource 補完された、追加の作業の大半を処理する`WHERE`自動生成された句`UPDATE`と`DELETE`ステートメントがありますが、処理のいくつかの微妙な`NULL`で説明したように、列の値、適切な処理`NULL`セクションの値します。

このチュートリアルでは、SqlDataSource 弊社の調査で終了します。 残りのチュートリアルは、ObjectDataSource と階層化されたアーキテクチャを使用してデータを扱うに戻ります。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [次へ](querying-data-with-the-sqldatasource-control-vb.md)
