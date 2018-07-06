---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: BLL レベルと DAL レベルの例外 (VB) の処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、もらえるように編集可能な DataList の更新のワークフロー中に発生した例外を処理する方法がわかります。
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aee5c26bfbbbc46c2151a56fed60057930cba80
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814253"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>BLL レベルと DAL レベルの例外 (VB) の処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe)または[PDF のダウンロード](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> このチュートリアルでは、もらえるように編集可能な DataList の更新のワークフロー中に発生した例外を処理する方法がわかります。


## <a name="introduction"></a>はじめに

[編集の概要と、DataList でデータを削除する](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)チュートリアルでは、単純な編集と削除機能を提供する DataList を作成しました。 完全に機能しますが、編集中に発生したエラーとして、ほとんどわかりやすいやハンドルされない例外が発生したプロセスを削除しています。 やなどの製品の名前を省略すると、製品を編集するには、非常の価格の値を入力する手頃な価格!、例外をスローします。 コードでは、この例外はキャッチされず後、は、web ページで、例外の詳細を表示する ASP.NET ランタイム バブルアップとします。

説明したように、[処理 BLL - と DAL レベルの例外で、ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)チュートリアルでは、ビジネス ロジックまたはデータ アクセス レイヤーの深さから例外が発生した場合、例外の詳細が返されます、ObjectDataSource にし、GridView には。 これらの例外を作成して適切に処理する方法を説明しました`Updated`または`RowUpdated`ObjectDataSource や GridView、例外を確認し、例外が処理されたことを示す、イベント ハンドラー。

DataList チュートリアル、ただし、データ更新および削除による、ObjectDataSource は t です。 代わりに、BLL に対して直接取り組んでいます。 BLL または DAL から送信された例外を検出するためには、例外処理、ASP.NET ページの分離コード内でコードを実装する必要があります。 このチュートリアルよりもらえるように編集可能な DataList 秒のワークフローを更新中に発生した例外を処理する方法がわかります。

> [!NOTE]
> *編集の 概要と、DataList でデータを削除する*を更新するため、ObjectDataSource を使用して編集および DataList からデータを削除するためのさまざまな手法を説明したチュートリアルでは、いくつかのテクニックを紹介し、削除しています。 これらの手法を採用する場合は、ObjectDataSource s を通じて BLL または DAL から例外を処理することができます`Updated`または`Deleted`イベント ハンドラー。


## <a name="step-1-creating-an-editable-datalist"></a>手順 1: 作成、編集可能な DataList

更新のワークフロー中に発生する例外の処理について気にし、前に、最初に作成、編集可能な DataList s を使用できます。 開く、`ErrorHandling.aspx`ページで、`EditDeleteDataList`フォルダー、DataList をデザイナーに追加設定その`ID`プロパティを`Products`、という名前の新しい ObjectDataSource を追加および`ProductsDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`を選択するためのメソッドは、記録;、insert、UPDATE、ドロップダウン リストを設定し、(None) にタブを削除します。


[![GetProducts() メソッドを使用して、製品情報を返す](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**図 1**: 使用して、製品情報を返す、`GetProducts()`メソッド ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))。


ObjectDataSource ウィザードを完了すると、Visual Studio が自動的に作成、 `ItemTemplate` DataList にします。 これを置き換える、`ItemTemplate`を各製品の名前と価格を表示し、Edit ボタンが含まれています。 次に、作成、`EditItemTemplate`名と価格と更新のキャンセル ボタンのテキスト ボックスに Web コントロールを使用します。 DataList s を最後に、設定`RepeatColumns`プロパティを 2。

これらの変更後、ページ s 宣言型マークアップは、次のようになります。 再確認をとることによって、編集、Cancel、および更新 ボタンがその`CommandName`プロパティを編集、キャンセル、および更新、それぞれに設定します。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> このチュートリアルでは、DataList のビュー ステートを有効にする必要があります。


ブラウザーから進行状況を表示する少し (図 2 参照)。


[![各製品には、[編集] ボタンが含まれています。](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**図 2**: 各製品には、[編集] ボタンが含まれています ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))。


現時点では、[編集] ボタンのみポストバックが発生することは t まだ、製品を編集できるようにします。 DataList s のイベント ハンドラーの作成に必要な編集を有効にする`EditCommand`、 `CancelCommand`、および`UpdateCommand`イベント。 `EditCommand`と`CancelCommand`イベントは、DataList s を簡単に更新`EditItemIndex`プロパティは、DataList にデータを再バインドします。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand`イベント ハンドラーは、少し複雑です。 編集した製品 s の読み取りに必要な`ProductID`から、`DataKeys`および s の製品名と価格のテキスト ボックスからコレクション、`EditItemTemplate`を呼び出して、`ProductsBLL`クラスの`UpdateProduct`DataList を返す前にメソッド編集済みの状態。

ここでは、let s だけを使用して、まったく同じコードから、`UpdateCommand`内のイベント ハンドラー、*編集の概要と、DataList でデータを削除する*チュートリアル。 手順 2 で例外を適切に処理するコードを追加します。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

無効な入力が発生した場合の適切な形式で単価、5.00 ドルなどの無効な単位価格の値または例外が発生する製品の名前の省略形でとります。 以降、`UpdateCommand`イベント ハンドラーが例外コードをこの時点で処理を含まない、例外が、ASP.NET の実行時までバブルは、エンドユーザーに表示されます (図 3 を参照してください)。


![ハンドルされない例外が発生したときに、エンドユーザーがエラー ページ](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**図 3**: ハンドルされない例外が発生したときに、エンドユーザーがエラー ページ


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>手順 2: 正常に UpdateCommand イベント ハンドラーで例外の処理

例外が発生することで更新のワークフロー中に、`UpdateCommand`イベント ハンドラー、BLL は、または、DAL します。 たとえば、ユーザーがすぎますの価格を入力、高価な`Decimal.Parse`内のステートメント、`UpdateCommand`イベント ハンドラーがスローされます、`FormatException`例外。 ユーザーは、製品の名前を付ける場合、または、価格が負の値を持つ場合は、DAL で例外が発生します。

例外が発生したときに、ページ自体内で情報メッセージを表示するいたします。 追加ラベル Web コントロールをページの`ID`に設定されている`ExceptionDetails`します。 ラベルのテキストを割り当てることで太字と斜体のフォントが非常に大規模な赤で表示する構成の`CssClass`プロパティを`Warning`で定義されている CSS クラス、`Styles.css`ファイル。

エラーが発生したときに、表示されるラベルを 1 回のみします。 以降のポストバックでは、s のラベルの警告メッセージが表示されます。 S のラベルをクリアするかこれを実現できます`Text`プロパティや設定、`Visible`プロパティを`False`で、`Page_Load`イベント ハンドラー (で行ったよう、[処理 BLL - と ASP の DAL レベルの例外.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアル) またはラベルの表示状態のサポートを無効にします。 後者のオプションを使用して、s のことができます。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

例外が発生したときに例外の詳細を割り当てます、`ExceptionDetails`ラベル コントロールの`Text`プロパティ。 以降のポストバックでは、そのビュー ステートが無効になっているため、`Text`プロパティ %s プログラムによる変更が失わ、既定のテキスト (空の文字列)、これにより、警告メッセージを非表示に戻します。

追加する便利なメッセージがページに表示するには、エラーが発生した場合を判断する、`Try ... Catch`へのブロック、`UpdateCommand`イベント ハンドラー。 `Try`部分には、例外につながる可能性のあるコードが含まれています。 中に、`Catch`ブロックに例外が発生した場合に実行されるコードが含まれています。 チェック アウト、[例外処理の基本事項](https://msdn.microsoft.com/library/2w8f0bss.aspx)詳細については、.NET Framework のドキュメントのセクションで、`Try ... Catch`ブロックします。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

内のコードで任意の型の例外がスローするときに、`Try`ブロック、`Catch`ブロックのコードの実行が開始します。 スローされる例外の種類`DbException`、 `NoNullAllowedException`、 `ArgumentException`、正確には、何に依存で、最初に、エラーの原因となったとします。 場合、データベース レベルで問題があります、`DbException`がスローされます。 場合の無効な値を入力、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、または`ReorderLevel`、フィールド、`ArgumentException`でこれらのフィールド値を検証するコードを追加しましたとしてスローされます、`ProductsDataTable`クラス (を参照してください、 [ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md)チュートリアル)。

詳細については、キャッチされた例外の種類に基づいて、メッセージ テキストを作成して、エンドユーザーに提供できます。 次のコードとほぼ同じフォームで使用されていたに戻り、[処理 BLL - と DAL レベルの例外で、ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアルでは、このレベルの詳細。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

このチュートリアルでは、単に呼び出す、`DisplayExceptionDetails`からメソッド、`Catch`ブロックでキャッチされた渡す`Exception`インスタンス (`ex`)。

`Try ... Catch`インプレース ブロック、ユーザーがよりわかりやすいエラー メッセージでは、図 4 と 5 つのスライド ショーとして表示されます。 DataList の例外が発生した場合に残っているメモは編集モードです。 制御フローに直ちにリダイレクト例外が発生した場合、ため、これは、`Catch`ブロック、編集済みの状態、DataList を返すコードをバイパスします。


[![ユーザーが必要なフィールドを付ける場合、エラー メッセージが表示されます。](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**図 4**: ユーザーが必要なフィールドを付ける場合、エラー メッセージが表示されます ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))。


[![エラー メッセージが表示されるときに入力を負の価格](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**図 5**: An のエラー メッセージが表示されるときに入力を負の価格 ([フルサイズの画像を表示する をクリックします](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))。


## <a name="summary"></a>まとめ

GridView と ObjectDataSource を例外がされているかどうかを示すために設定できるプロパティに加えて、更新および削除のワークフロー中に発生した例外に関する情報を含む投稿レベルのイベントのハンドラーを提供します。処理されます。 これらは機能をただし、使用可能なときに、DataList 操作および BLL を直接使用します。 代わりに、例外処理を実装する責任を負いますいます。

このチュートリアルでは、編集可能な DataList 秒のワークフローを追加することで更新する例外処理を追加する方法を説明しました、`Try ... Catch`へのブロック、`UpdateCommand`イベント ハンドラー。 更新のワークフロー中に例外が発生した場合、`Catch`ブロックのコードの実行に役立つ情報を表示する、`ExceptionDetails`ラベル。

この時点では、DataList では、最初の段階で発生してから例外を回避するための努力はしません。 負の価格は、例外発生、したがってことがわかっていて t はまだ未然に防ぐため、ユーザーから、このような無効な入力機能を追加します。 [次へ]、チュートリアルでは無効なユーザー入力の検証コントロールを追加することでが原因の例外を削減する方法を見ていきます、`EditItemTemplate`します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [例外のデザインのガイドライン](https://msdn.microsoft.com/library/ms298399.aspx)
- [エラー ログのモジュールとハンドラー (ELMAH)](http://workspaces.gotdotnet.com/elmah) (エラーのログ記録のオープン ソース ライブラリ)
- [Enterprise Library for .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (Exception Management Application Block を含む)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Ken Pespisa でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](performing-batch-updates-vb.md)
> [次へ](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
