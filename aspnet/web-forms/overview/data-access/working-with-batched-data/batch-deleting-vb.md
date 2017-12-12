---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: "(VB) を削除するバッチ |Microsoft ドキュメント"
author: rick-anderson
description: "単一の操作で複数のデータベース レコードを削除する方法を説明します。 ユーザー インターフェイス レイヤー内で作成された以前 tut 拡張の GridView に構築しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: cce3876ae0fab66f617d77abdb8777f9865c7514
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="batch-deleting-vb"></a>バッチ (VB) を削除します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip)または[PDF のダウンロード](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> 単一の操作で複数のデータベース レコードを削除する方法を説明します。 ユーザー インターフェイス レイヤーでは、以前のチュートリアルで作成された拡張の GridView に基づいてお構築します。 データ アクセス層は、すべての削除が成功するすべての削除をロールバックすることを確認するトランザクション内で複数の削除操作をラップします。


## <a name="introduction"></a>はじめに

[前のチュートリアル](batch-updating-vb.md)編集インターフェイスが完全に編集可能な GridView を使用して、バッチを作成する方法について説明します。 ユーザーがよく編集箇所多数のレコードを一度にの場合、編集インターフェイス バッチが必要になりますはるかに少ないポストバックとキーボード、マウスのコンテキスト スイッチ、それによってエンドユーザーの効率が向上します。 この手法は、通常は、1 つの go の複数のレコードを削除するユーザーがページで同様に便利です。

オンラインの電子メール クライアントを使用したすべてのユーザーがインターフェイスを削除すると、最も一般的なバッチのいずれかに慣れて既に: (図 1 を参照してください)、対応する削除すべてのチェック項目がグリッド内の各行のチェック ボックス ボタンをクリックします。 このチュートリアルではなく短いため web ベースのインターフェイスと 1 つのアトミック操作として、一連のレコードを削除する方法の両方を作成する前のチュートリアルで面倒な作業のすべてが既に完了しました。 [GridView 列のチェック ボックスを追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)と対応するチェック ボックスの列を含む GridView を作成したチュートリアル、[トランザクション内でデータベースの変更をラッピング](wrapping-database-modifications-within-a-transaction-vb.md)内のメソッドを作成したチュートリアル削除するトランザクションを使用する BLL、`List<T>`の`ProductID`値。 このチュートリアルでおは時にビルドし、例を削除すると、作業バッチを作成する過去の経験をマージします。


[![各行には、チェック ボックスが含まれています。](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**図 1**: 各行には、チェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>手順 1: インターフェイスを削除するバッチを作成します。

インターフェイスを削除するバッチ既に作成したので、 [GridView 列のチェック ボックスを追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)チュートリアルでは、おは単にコピーする`BatchDelete.aspx`最初から作成することではなくです。 開いて開始、 `BatchDelete.aspx`  ページで、`BatchData`フォルダーおよび`CheckBoxField.aspx` ページで、`EnhancedGridView`フォルダーです。 `CheckBoxField.aspx`  ページで、ソース ビューに移動して間のマークアップをコピー、`<asp:Content>`図 2 に示すようにタグを付けます。


[![CheckBoxField.aspx の宣言型マークアップをクリップボードにコピーします。](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**図 2**: コピーの宣言型マークアップ`CheckBoxField.aspx`をクリップボードに ([フルサイズのイメージを表示するをクリックして](batch-deleting-vb/_static/image4.png))


次に、ビューに移動して、ソースで`BatchDelete.aspx`内でのクリップボードの内容を貼り付けると、`<asp:Content>`タグ。 コピーしてからの分離コード クラス内のコードを貼り付けます`CheckBoxField.aspx.vb`をで分離コード クラス内で`BatchDelete.aspx.vb`(、`DeleteSelectedProducts`ボタン s `Click` 、イベント ハンドラー、`ToggleCheckState`メソッド、および`Click`イベント ハンドラー`CheckAll`と`UncheckAll`ボタン)。 このコンテンツをコピーした後、`BatchDelete.aspx`ページの分離コード クラスは、次のコードを含める必要があります。


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

宣言型マークアップとソース コードをコピー後すぐをテスト`BatchDelete.aspx`ブラウザーで表示しています。 各行が s の製品名、カテゴリ、およびチェック ボックスと共に価格の一覧を表示する GridView の最初の 10 製品を一覧表示する GridView が表示されます。 3 つのボタンがあります。 すべてをチェックをオフに、すべて、その選択した製品を削除します。 すべてをオフにしてすべてのチェック ボックスをクリアするときに、すべてのチェック ボックスを選択すべてチェック ボタンをクリックします。 選択した製品の削除 をクリックを一覧表示するメッセージが表示されます、`ProductID`選択した製品の値が製品を実際に削除されません。


[![CheckBoxField.aspx からインターフェイスは BatchDeleting.aspx に移動されました](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**図 3**:、インターフェイスから`CheckBoxField.aspx`に移動しました`BatchDeleting.aspx`([フルサイズのイメージを表示するをクリックして](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>手順 2: トランザクションを使用して、チェックされた製品を削除します。

インターフェイスに正常にコピーを削除するバッチに`BatchDeleting.aspx`、ままが選択されている製品の削除 ボタンを使用して、チェックされた製品を削除するようにコードを更新することがすべて、`DeleteProductsWithTransaction`メソッドで、`ProductsBLL`クラス。 追加した、このメソッド、[トランザクション内でデータベースの変更をラッピング](wrapping-database-modifications-within-a-transaction-vb.md)チュートリアルでは、その入力として受け取ります、`List(Of T)`の`ProductID`値し、対応する各を削除`ProductID`のスコープ内で、トランザクションです。

`DeleteSelectedProducts`ボタン s`Click`イベント ハンドラーは、現在、次を使用して`For Each`GridView 各行を反復処理するループ。


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

行ごとに、 `ProductSelector` CheckBox Web コントロールがプログラムによって参照されています。 オンの場合行 s`ProductID`から取得した、`DataKeys`コレクションおよび`DeleteResults`s のラベル`Text`プロパティが更新され、行が削除対象として選択されたことを示すメッセージが含まれます。

上記のコードは実際に削除されませんレコードへの呼び出しとして、`ProductsBLL`クラスの`Delete`メソッドのコメント アウトします。この削除ロジックを適用するが、コードは、製品を削除と分割不可能な操作内ではありません。 シーケンス内の最初のいくつか削除は成功しましたが、後で、1 は (外部キー制約違反) のため失敗した場合、は、例外がスローされますが、既に削除されて、これらの製品が削除された残っていません。

原子性を確保するために必要がありますを使用する、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッドです。 このメソッドの一覧を受け付けるため`ProductID`値を最初にグリッドから、この一覧をコンパイルし、パラメーターとして渡す必要があります。 最初のインスタンスを作成、`List(Of T)`型の`Integer`します。 内で、`For Each`ループを選択した製品を追加する必要があります`ProductID`値をこの`List(Of T)`です。 ループの後にこの`List(Of T)`に渡される必要があります、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッドです。 更新プログラム、`DeleteSelectedProducts`ボタンの`Click`イベント ハンドラーを次のコードで。


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

更新されたコードを作成、`List(Of T)`型の`Integer`(`productIDsToDelete`) 設定と、`ProductID`削除する値。 後に、`For Each`ループ、選択すると、少なくとも 1 つの製品がある場合、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッドが呼び出され、このリストに渡されます。 `DeleteResults`ラベルが表示もされ、データが GridView にバインドし直す (できるようにする新しく削除されたレコードは、不要になった、グリッド内の行として表示されます)。

図 4 は、削除の行の数が選択された後に、GridView を示します。 図 5 は、選択されている製品の削除 ボタンがクリックしてされた後にすぐに、画面を示しています。 図 5 で、 `ProductID` GridView の下にラベルに削除されたレコードの値が表示され、これらの行が GridView ではなくなりました。


[![選択した製品が削除されます。](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**図 4**: 選択した製品が削除されます ([フルサイズのイメージを表示するをクリックして](batch-deleting-vb/_static/image8.png))


[![削除された製品 ProductID 値は、「GridView の下に一覧表示](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**図 5**:「削除製品`ProductID`値は、「GridView の下に表示されている ([フルサイズのイメージを表示するには、をクリックして](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> テストする、`DeleteProductsWithTransaction`メソッド s 原子性で、製品のエントリを手動で追加、`Order Details`の表に、(およびその他) には、その製品を削除しようとします。 関連付けられている注文の製品を削除が方法、その他の選択した製品の削除がロールバックしようとするとき、外部キー制約違反を受け取ります。


## <a name="summary"></a>概要

インターフェイスを削除するバッチを作成するには、対応するチェック ボックスの列を含む GridView を追加し、ボタン Web ときを制御するには、クリックされると、1 つのアトミック操作として選択された行のすべてが削除されます。 このチュートリアルでは前の 2 つのチュートリアルで実行された作業を整理してこのようなインターフェイスを作成お[GridView 列のチェック ボックスを追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md)と[トランザクション内でデータベースの変更をラッピング](wrapping-database-modifications-within-a-transaction-vb.md)です。 チェック ボックスの列を含む GridView で作成した最初のチュートリアルでおよび後者の場合、BLL 内のメソッドを実装しましたを渡されたときに、`List(Of T)`の`ProductID`値、すべてのトランザクションのスコープ内で削除します。

次のチュートリアルでは、バッチ挿入操作を実行するためのインターフェイスを作成します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Hilton Giesenow および Teresa マーフィーがいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](batch-updating-vb.md)
[次へ](batch-inserting-vb.md)
