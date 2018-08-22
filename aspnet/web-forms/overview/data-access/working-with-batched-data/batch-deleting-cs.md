---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: バッチを削除する (c#) |Microsoft Docs
author: rick-anderson
description: 1 回の操作で複数のデータベース レコードを削除する方法について説明します。 ユーザー インターフェイス層では、作成された以前 tut 拡張の GridView に構築します.
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b4d3c21fad9000ae50ecb35a5d94d176a135ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827325"
---
<a name="batch-deleting-c"></a>バッチを削除する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip)または[PDF のダウンロード](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> 1 回の操作で複数のデータベース レコードを削除する方法について説明します。 ユーザー インターフェイス層では、前のチュートリアルで作成された拡張の GridView に構築します。 データ アクセス層は、すべての削除の成功に、すべての削除はロールバック、トランザクション内で複数の削除操作をラップします。


## <a name="introduction"></a>はじめに

[前のチュートリアル](batch-updating-cs.md)編集インターフェイスが完全に編集可能な GridView を使用してバッチを作成する方法を説明しました。 ポストバックとキーボードのマウスのコンテキストをはるかに少ない状況は、ユーザーがよくレコードの編集多く一度に、インターフェイスの編集のバッチは、スイッチ、エンドユーザーの効率が向上します。 この手法は通常、1 つの go の多くのレコードを削除するユーザーがページで同様に便利です。

使い慣れたインターフェイスを削除する最も一般的なバッチのいずれかがオンラインの電子メール クライアントを使用するすべてのユーザーが既に: 対応する削除すべてチェック項目を含むグリッドの各行のチェック ボックス ボタン (図 1 参照)。 このチュートリアルではなく短いため既にすべての web ベースのインターフェイスと 1 つのアトミック操作として一連のレコードを削除するメソッドの両方を作成する前のチュートリアルで困難な作業を完了しました。 [のチェック ボックスの GridView 列を追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)チュートリアルで、チェック ボックスの列を含む GridView を作成した、[トランザクション内のデータベース変更のラッピング](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアル内のメソッドを作成しましたトランザクションを使用して、削除は BLL、`List<T>`の`ProductID`値。 このチュートリアルではに基づいて進められますし例を削除する作業バッチを作成する過去の経験をマージします。


[![各行には、チェック ボックスが含まれています。](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**図 1**: 各行には、チェック ボックスが含まれています ([フルサイズの画像を表示する をクリックします](batch-deleting-cs/_static/image2.png))。


## <a name="step-1-creating-the-batch-deleting-interface"></a>手順 1: インターフェイスの削除バッチを作成します。

インターフェイスの削除バッチを既に作成したので、[のチェック ボックスの GridView 列を追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)チュートリアルでは単にコピーできます`BatchDelete.aspx`ゼロから作成するのではなく。 開いて開始、`BatchDelete.aspx`ページで、`BatchData`フォルダーと`CheckBoxField.aspx`ページで、`EnhancedGridView`フォルダー。 `CheckBoxField.aspx`  ページで、ソース ビューに移動して、マークアップ間のコピー、`<asp:Content>`図 2 に示すようにタグを付けます。


[![CheckBoxField.aspx の宣言型マークアップをクリップボードにコピーします。](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**図 2**: コピーの宣言型マークアップ`CheckBoxField.aspx`をクリップボードに ([フルサイズの画像を表示する をクリックします](batch-deleting-cs/_static/image4.png))。


次に、ソース ビューに移動`BatchDelete.aspx`内でのクリップボードの内容を貼り付けると、`<asp:Content>`タグ。 コピーしてからの分離コード クラス内のコードを貼り付けます`CheckBoxField.aspx.cs`をで分離コード クラス内で`BatchDelete.aspx.cs`(、`DeleteSelectedProducts`ボタン s`Click`イベント ハンドラー、`ToggleCheckState`メソッド、および`Click`イベント ハンドラー`CheckAll`と`UncheckAll`ボタン)。 このコンテンツでは、経由でコピーした後、`BatchDelete.aspx`ページ分離コード クラスは、次のコードを含める必要があります。


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

宣言型マークアップとソース コードをコピー後をテストするのには少し`BatchDelete.aspx`ブラウザーで表示することによって。 製品の名前、カテゴリ、およびチェック ボックスと共に価格を一覧表示する行ごとに GridView で最初の 10 個の製品を一覧表示する GridView が表示されます。 3 つのボタンがあります。 すべて、すべてオフにして、および選択した製品を削除します。 すべてをオフにしてすべてのチェック ボックスをクリア中に、すべてのチェック ボックスを選択すべてチェック ボタンをクリックします。 選択した製品の削除 をクリックしてには、一覧表示するメッセージが表示されます、 `ProductID` 、選択した製品の値は、製品を実際に削除されません。


[![CheckBoxField.aspx からインターフェイス BatchDeleting.aspx に移動しました](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**図 3**: The インターフェイスから`CheckBoxField.aspx`に移動しました`BatchDeleting.aspx`([フルサイズの画像を表示する をクリックします](batch-deleting-cs/_static/image6.png))。


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>手順 2: トランザクションを使用してチェックされている製品を削除します。

インターフェイスに正常にコピーを削除するバッチに`BatchDeleting.aspx`、すべてのままですが選択されている製品の削除 ボタンを使用してチェックされている製品を削除するように、コードを更新すること、`DeleteProductsWithTransaction`メソッドで、`ProductsBLL`クラス。 このメソッドで追加、[トランザクション内でのデータベース変更をラップ](wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルでは、その入力として受け取ります、`List<T>`の`ProductID`値し、対応する各を削除します`ProductID`のスコープ内、トランザクションです。

`DeleteSelectedProducts`ボタン s`Click`イベント ハンドラーは現在、次を使用して`foreach`GridView の各行を反復処理するループ。


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

行ごとに、 `ProductSelector` CheckBox Web コントロールがプログラムで参照されています。 オンの場合行 s`ProductID`から取得されます、`DataKeys`コレクションと`DeleteResults`ラベルの`Text`プロパティを更新して、削除、行が選択されていることを示すメッセージが含まれます。

上記のコードですべてのレコードが実際にへの呼び出しとして削除されません、`ProductsBLL`クラスの`Delete`メソッドをコメント アウトします。この削除ロジックを適用するが、コードは、製品を削除するアトミック操作内ではありません。 つまり、シーケンス内の最初のいくつか削除に成功した場合は、以降は、(外部キー制約の違反) のため失敗しました例外がスローされますは既に削除されてこれらの製品が削除されたままとします。

代わりに使用する原子性を確保するために、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッド。 このメソッドのリストを受け入れるため`ProductID`値を最初、グリッドからには、この一覧をコンパイルして、パラメーターとして渡します必要があります。 最初のインスタンスを作成、`List<T>`型の`int`します。 内で、`foreach`ループが選択されている製品を追加する必要があります`ProductID`値をこの`List<T>`します。 ループの後にこの`List<T>`に渡される必要があります、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッド。 更新プログラム、`DeleteSelectedProducts`ボタンの`Click`イベント ハンドラーを次のコード。


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

更新されたコードを作成、`List<T>`型の`int`(`productIDsToDelete`) 設定されると、`ProductID`を削除する値。 後に、`foreach`ループで選択すると、少なくとも 1 つの製品がある場合、`ProductsBLL`クラスの`DeleteProductsWithTransaction`メソッドが呼び出され、この一覧に渡されます。 `DeleteResults`ラベルが表示もされ (その新しく削除レコードが、グリッド内の行として表示されなく) にデータが GridView にバインドします。

図 4 は、削除の行の数を選択して、GridView を示します。 図 5 は、選択した製品の削除 ボタンがクリックしてされた後すぐに画面を示しています。 図 5 に注意してください、`ProductID`削除されたレコードの値は、GridView の下のラベルに表示され、これらの行が GridView ではなくなりました。


[![選択した製品は削除されます。](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**図 4**: [選択した製品は削除されます ([フルサイズの画像を表示する] をクリックします](batch-deleting-cs/_static/image8.png))。


[![削除された製品の ProductID 値は、GridView の下に一覧表示](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**図 5**:、削除された製品`ProductID`値は、GridView の下に表示されている ([フルサイズの画像を表示する をクリックします](batch-deleting-cs/_static/image10.png))。


> [!NOTE]
> テストする、`DeleteProductsWithTransaction`メソッド s 原子性で製品のエントリを手動で追加、`Order Details`の表に、(とその他) には、その製品を削除しようとします。 関連付けられている注文の製品を削除するが、他に、選択した製品の削除がロールバックする方法に注意してください。 しようとしています。 外部キー制約違反を受け取ります。


## <a name="summary"></a>まとめ

チェック ボックスの列を含む GridView を追加するには、インターフェイスの削除バッチを作成し、ボタン Web ときを制御するには、クリックされると、単一のアトミック操作として選択された行のすべてが削除されます。 このチュートリアルで前の 2 つのチュートリアルでの作業をつなぎ合わせたによってこのようなインターフェイスを構築しました[のチェック ボックスの GridView 列を追加する](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)と[トランザクション内のデータベース変更のラッピング](wrapping-database-modifications-within-a-transaction-cs.md)します。 チェック ボックスの列を含む GridView で作成した最初のチュートリアルでは、後者の場合、BLL のメソッドを実装しました、渡されたときに、`List<T>`の`ProductID`値では、すべてのトランザクションのスコープ内で削除します。

次のチュートリアルでは、バッチ挿入操作を実行するためのインターフェイスを作成します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者では、Hilton Giesenow Teresa Murphy はでした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](batch-updating-cs.md)
> [次へ](batch-inserting-cs.md)
