---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: BLL および DAL レベル例外 (VB) の処理 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、もらえるように編集可能な DataList の更新ワークフロー内で発生した例外を処理する方法が表示されます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ee0eeff08b6ba490b401540de833eecd7122a17
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880103"
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>BLL および DAL レベル例外 (VB) の処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe)または[PDF のダウンロード](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> このチュートリアルでは、もらえるように編集可能な DataList の更新ワークフロー内で発生した例外を処理する方法が表示されます。


## <a name="introduction"></a>はじめに

[DataList でのデータの削除と編集の概要](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)チュートリアルでは、単純な編集と削除機能を提供する DataList を作成しました。 完全に機能が、編集するときに発生したエラーとほとんどわかりやすいやプロセスを削除すると、ハンドルされない例外が発生しました。 たとえば、s の製品名を省略することや、製品を編集するときは、非常に価格値を入力する手頃な価格!、例外をスローします。 コードでこの例外がキャッチされないので、web ページで、s の例外の詳細を表示する ASP.NET ランタイムまでバブルします。

説明したとおり、[処理 BLL-DAL レベルでの例外および ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)チュートリアルでは、ビジネス ロジックまたはデータ アクセス層の深さから例外が発生した場合、例外の詳細が返されます、ObjectDataSource とGridView にし。 これらの例外を作成することで適切に処理する方法を説明しました`Updated`または`RowUpdated`ObjectDataSource、GridView、例外を確認し、例外が処理されたことを示す用のイベント ハンドラー。

DataList チュートリアル、ただし、データ更新および削除による、ObjectDataSource は t です。 代わりに、BLL に対して直接動作しています。 BLL または DAL から送信された例外を検出するためには、例外処理、ASP.NET ページの分離コード内でコードを実装する必要があります。 このチュートリアルよりもらえるように編集可能な DataList のワークフローを更新中に発生した例外を処理する方法が表示されます。

> [!NOTE]
> *編集の概要と、DataList のデータを削除する*を更新するため、ObjectDataSource を使用する必要を編集して、DataList からデータを削除するさまざまな手法を説明したチュートリアルでは、いくつかのテクニックと削除しています。 これらの手法を採用する場合は、ObjectDataSource s を通じた BLL または DAL から例外を処理することができます`Updated`または`Deleted`イベント ハンドラー。


## <a name="step-1-creating-an-editable-datalist"></a>手順 1: 作成、編集可能な DataList

更新のワークフローの中に発生する例外処理と心配、前に、最初に作成、編集可能な DataList s を使用できます。 開く、 `ErrorHandling.aspx`  ページで、`EditDeleteDataList`フォルダー セットをデザイナーに DataList を追加、`ID`プロパティを`Products`、という名前の新しい ObjectDataSource を追加および`ProductsDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`を選択するためのメソッドは、記録以外の場合は、insert、UPDATE、ドロップダウン リストを設定し、(なし) にタブを削除します。


[![GetProducts() メソッドを使用して製品情報を返す](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**図 1**: 製品を使用して情報を返す、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


ObjectDataSource ウィザードを完了すると、Visual Studio が自動的に作成、 `ItemTemplate` DataList のです。 これを置き換える、`ItemTemplate`各製品の名前と価格を表示して、[編集] ボタンが含まれています。 次に、作成、`EditItemTemplate`と名前と価格および更新し、[キャンセル] ボタンのテキスト ボックスの Web コントロールです。 DataList s を最後に、設定`RepeatColumns`プロパティを 2 です。

これらの変更後にページ s の宣言型マークアップは、次のようになります。 とることによって、編集、キャンセル をもう一度確認して更新 ボタン、`CommandName`プロパティを編集、キャンセル、および更新、それぞれに設定します。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> このチュートリアルでは、DataList のビュー ステートを有効にする必要があります。


ブラウザーを使って作業の進行状況を表示するのにはしばらく時間かかる (図 2 を参照してください)。


[![各製品には、[編集] ボタンが含まれています。](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**図 2**: 各製品には、[編集] ボタンが含まれています ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


現時点では、[編集] ボタンのみポストバックを発生させる、されないまだ製品を編集できるようにします。 編集を有効にする必要があります DataList s のイベント ハンドラーを作成する`EditCommand`、 `CancelCommand`、および`UpdateCommand`イベント。 `EditCommand`と`CancelCommand`イベントは、DataList s を単に更新`EditItemIndex`プロパティは、DataList にデータを再バインド。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand`イベント ハンドラーは、少し複雑です。 編集した製品 s を読み取る必要があります`ProductID`から、`DataKeys`および製品の名前と価格のテキスト ボックスからコレクション、 `EditItemTemplate`、まず、`ProductsBLL`クラスの`UpdateProduct`DataList を返す前にメソッド編集済みの状態。

ここでは、let s だけ、正確な同じコードを使用して、`UpdateCommand`内のイベント ハンドラー、 *DataList でのデータの削除と編集の概要*チュートリアルです。 手順 2. で例外を適切に処理するコードを追加します。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

無効な入力発生した場合に、またはフォームで不適切な書式の単価、$5.00 など、無効な単位の価格値、例外が発生する製品の名前の省略できます。 以降、`UpdateCommand`処理コードをこの時点ですべての例外を含まないイベント ハンドラー、例外をバブリング ASP.NET ランタイムをエンドユーザーに表示されます (図 3 を参照してください)。


![未処理の例外が発生すると、エンドユーザーのエラー ページ](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**図 3**: ハンドルされない例外が発生すると、エンドユーザーのエラー ページ


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>手順 2: 正常に UpdateCommand イベント ハンドラーで例外の処理

更新のワークフローの例外を実行するために、`UpdateCommand`イベント ハンドラー、BLL、または、DAL です。 たとえば、ユーザーが多すぎますの価格を入力した場合、高価な`Decimal.Parse`内のステートメント、`UpdateCommand`イベント ハンドラーがスローされます、`FormatException`例外。 ユーザーが製品の名前を省略した場合、または、価格が負の値を持つ場合は、DAL には、例外が発生します。

例外が発生するときに表示するページ自体内で情報メッセージです。 ラベルの web コントロールをページの`ID`に設定されている`ExceptionDetails`です。 S のラベル テキストを割り当てることで太字と斜体のフォントが非常に大規模な赤で表示する構成、`CssClass`プロパティを`Warning`で定義されている CSS クラス、`Styles.css`ファイル。

エラーが発生する場合にのみ、表示されるラベルを 1 回が必要です。 つまり、以降のポストバックでラベルの警告メッセージが表示されます。 これには、s のラベルをクリアするか`Text`プロパティや設定、`Visible`プロパティを`False`で、`Page_Load`イベント ハンドラー (で行ったように、[処理 BLL と ASP DAL レベルの例外.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアル) または s のラベルの表示状態のサポートを無効にします。 S、後者のオプションを使用することができます。


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

例外の詳細を割り当てます例外が発生したときに、`ExceptionDetails`ラベル コントロールの`Text`プロパティです。 以降のポストバックで、そのビュー ステートが無効になるので、`Text`どうかを示すプロパティ %s、失われます、既定のテキスト (空の文字列)、これにより、警告メッセージを非表示に戻します。

ページで便利なメッセージを表示するために、エラーが発生したときを特定する必要がありますを追加する、`Try ... Catch`へのブロック、`UpdateCommand`イベント ハンドラー。 `Try`部分には、例外につながる可能性のあるコードが含まれています。 中に、`Catch`ブロックには、例外発生した場合に実行されるコードが含まれています。 チェック アウト、[例外処理の基本事項](https://msdn.microsoft.com/library/2w8f0bss.aspx)詳細については、.NET Framework のドキュメントのセクションで、`Try ... Catch`ブロックします。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

内のコードによって任意の型の例外がスローされたときに、`Try`ブロック、`Catch`ブロックのコードは実行を開始します。 スローされる例外の種類`DbException`、 `NoNullAllowedException`、`ArgumentException`で異なります、正確に、まず第一に、エラーの原因となったとします。 場合、データベース レベルで問題があります、`DbException`がスローされます。 場合の無効な値を入力、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、または`ReorderLevel`、フィールド、`ArgumentException`でこれらのフィールド値を検証するコードを追加しましたとしてスローされます、`ProductsDataTable`クラス (を参照してください、 [ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md)チュートリアル)。

キャッチされた例外の種類のメッセージ テキストを基盤としてより役に立つ説明をエンドユーザーに提供できます。 次のコードとほぼ同じフォームで使用されていたに戻り、[処理 BLL-DAL レベルでの例外および ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアルでは、このレベルの詳細。


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

このチュートリアルを完了するを呼び出すだけで、`DisplayExceptionDetails`メソッドから、`Catch`ブロックでキャッチされた渡す`Exception`インスタンス (`ex`)。

`Try ... Catch`インプレース ブロックは、図 4 と 5 の表示に関する詳細情報のエラー メッセージをユーザーに提供します。 DataList で例外が発生した場合に残っている注は編集モードです。 これは、例外が発生した場合、制御フローに直ちにリダイレクトため、`Catch`ブロック、編集済みの状態、DataList を返すコードをバイパスします。


[![ユーザーが必要なフィールドを省略した場合、エラー メッセージが表示されます。](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**図 4**: ユーザーが必要なフィールドを省略した場合、エラー メッセージが表示されます ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![エラー メッセージが表示されるときに入力マイナスの価格](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**図 5**: のエラー メッセージが表示されるときに入力マイナスの価格 ([フルサイズのイメージを表示するをクリックして](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>まとめ

ObjectDataSource、GridView、更新と削除のワークフローの間に発生した例外だけでなく、例外がされているかどうかを示すために設定できるプロパティに関する情報を含むレベルの後のイベント ハンドラーを提供します。処理されます。 これらの機能では、ただし、利用できない場合に、DataList 操作および BLL を直接使用します。 代わりに、例外処理を実装する責任はします。

このチュートリアルでは、編集可能な DataList s を追加することでワークフローを更新する例外処理を追加する方法を説明しました、`Try ... Catch`へのブロック、`UpdateCommand`イベント ハンドラー。 更新のワークフローの中に例外が発生した場合、`Catch`ブロックのコードの実行に役立つ情報を表示する、`ExceptionDetails`ラベル。

この時点では、DataList は、最初の場所が行われない例外を防ぐために努力を行いません。 マイナスの価格、例外が発生、だことはわかっている場合でも t はまだ事前にこのような無効な入力できないようにする機能を追加します。 [次へ]、チュートリアルのこれについて扱いますの検証コントロールを追加することで、無効なユーザー入力によって引き起こされる例外を減らすために、`EditItemTemplate`です。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [例外のデザインのガイドライン](https://msdn.microsoft.com/library/ms298399.aspx)
- [エラー ログ モジュールとハンドラー (ELMAH)](http://workspaces.gotdotnet.com/elmah) (エラーのログ記録のオープン ソース ライブラリ)
- [Enterprise Library for .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (例外管理アプリケーション ブロックが含まれています)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客は、Ken Pespisa をでした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](performing-batch-updates-vb.md)
> [次へ](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
