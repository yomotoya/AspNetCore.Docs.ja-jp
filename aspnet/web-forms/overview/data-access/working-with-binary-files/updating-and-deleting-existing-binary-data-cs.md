---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: 更新および削除の既存のバイナリ データ (c#) |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、方法、GridView コントロールを簡単に編集し、テキスト データを削除しました。 このチュートリアルでは GridView コントロールも作成する方法に表示しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eb9b4cb9403f5d793605fe023e4e0757bcda6f4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372013"
---
<a name="updating-and-deleting-existing-binary-data-c"></a>既存のバイナリ データ (c#) 更新および削除
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe)または[PDF のダウンロード](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> 前のチュートリアルでは、方法、GridView コントロールを簡単に編集し、テキスト データを削除しました。 このチュートリアルではどの GridView コントロールもなりますを編集し、そのバイナリ データをデータベースに保存またはファイル システムに格納されているかどうか、バイナリ データを削除することがわかります。


## <a name="introduction"></a>はじめに

過去の 3 つのチュートリアルで非常に大量のバイナリ データを操作するための機能を追加しています。 追加することで開始した、`BrochurePath`列を`Categories`テーブルが表示され、アーキテクチャを適宜更新します。 既存のカテゴリ表 s を使用するデータ アクセス層とビジネス ロジック層のメソッドを追加しました`Picture`列は、イメージ ファイルのバイナリ コンテンツ s を保持します。 内に表示されるカテゴリの画像に、GridView でバイナリ データ、パンフレットのダウンロード リンクを提示する web ページを構築しました、`<img>`要素が、新しいカテゴリを追加し、そのパンフレットと画像データをアップロードするための DetailsView を追加します。

実装する残っているを編集および GridView s 組み込み編集を使用して、機能を削除して、このチュートリアルでは作業の既存のカテゴリを削除できるは。 カテゴリを編集するときに、ユーザーは必要に応じて新しい写真をアップロードまたはカテゴリは、引き続き既存のものを使用できるようになります。 パンフレットのか、新しいカタログをアップロードするかを示す、カテゴリが関連付けられているパンフレットになくなりましたに既存のパンフレットを使用する選択ができます。 Let s を始めましょう。

## <a name="step-1-updating-the-data-access-layer"></a>手順 1: データ アクセス層を更新しています

DAL が自動生成`Insert`、 `Update`、および`Delete`メソッドがこれらのメソッドを基に生成された、 `CategoriesTableAdapter` s メイン クエリが含まれていない、`Picture`列。 そのため、`Insert`と`Update`メソッドは、カテゴリの画像のバイナリ データを指定するためのパラメーターを含めないでください。 行ったように、[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-cs.md)、更新するための新しい TableAdapter メソッドを作成する必要があります、`Categories`バイナリ データを指定するときにテーブルです。

型指定されたデータセットを開くし、デザイナーを右クリックし、`CategoriesTableAdapter`のヘッダー選択クエリの追加、コンテキスト メニューから launche に TableAdapter クエリの構成ウィザードとします。 このウィザードは、TableAdapter のクエリがデータベースにアクセスする方法を求めてして開始します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 次の手順では、生成されるクエリの種類を要求します。 クエリを作成する新しいレコードを追加する re 経過、`Categories`テーブルを更新プログラムを選択し、[次へ] をクリックします。


[![更新オプションを選択します。](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**図 1**: 更新 オプションを選択します ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image2.png))。


ここで指定する必要があります、 `UPDATE` SQL ステートメント。 ウィザードが自動的に提案を`UPDATE`TableAdapter のメイン クエリに対応するステートメント (更新する 1 つ、 `CategoryName`、 `Description`、および`BrochurePath`値)。 ステートメントを変更できるように、`Picture`列がスコープに含める、`@Picture`パラメーター、次のように。


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

ウィザードの最後の画面を使用して、新しい TableAdapter メソッドの名前を付けるよう求められます。 入力`UpdateWithPicture`[完了] をクリックします。


[![新しい TableAdapter メソッド UpdateWithPicture 名](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**図 2**: 新しい TableAdapter メソッド名前`UpdateWithPicture`([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image4.png))。


## <a name="step-2-adding-the-business-logic-layer-methods"></a>手順 2: ビジネス ロジック層のメソッドを追加します。

DAL を更新するだけでなく、BLL、更新、およびカテゴリを削除するためのメソッドを更新する必要があります。 これらは、プレゼンテーション層から呼び出されるメソッドです。

カテゴリを削除するを使用してできます、`CategoriesTableAdapter`が自動生成`Delete`メソッド。 次のメソッドを追加、`CategoriesBLL`クラス。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Let s はこのチュートリアルでは、代わりに、バイナリの画像データを呼び出す 1 つのカテゴリを更新するための 2 つのメソッドを作成、`UpdateWithPicture`メソッドに追加した、`CategoriesTableAdapter`を受け取るもうだけ`CategoryName`、 `Description`、および`BrochurePath`値を使用して`CategoriesTableAdapter`クラスの自動生成`Update`ステートメント。 2 つのメソッドを使用して背後にある"the rationale"では、状況によっては、ユーザーはその他のフィールドと共に、新しい画像をアップロードする場合、ユーザーが必要がありますカテゴリの画像を更新する可能性があります。 アップロードされた画像のバイナリ データで使用できます、`UPDATE`ステートメント。 それ以外の場合は、ユーザーを更新すると、名前と説明の利用のみ可能性があります。 場合、`UPDATE`ステートメントのバイナリ データが必要ですが、`Picture`列も、その d する必要がありますもその情報を提供します。 画像データ編集中のレコードを戻して、データベースへの追加のトリップが必要になります。 そのため、2 つ必要`UPDATE`メソッド。 ビジネス ロジック層は、どれを使用するがカテゴリを更新するときに画像データを提供するかどうかに基づいて決定されます。

そのため、2 つの方法を追加、`CategoriesBLL`という名前の両方のクラス`UpdateCategory`します。 1 つ目は 3 つをそのまま使用する必要があります`string`s、`byte`配列、および`int`入力としてパラメーターは 2 番目、3 つ`string`s と`int`。 `string`カテゴリの名前、説明、およびパンフレット ファイルのパスは、入力パラメーター、`byte`配列は、カテゴリの画像のバイナリ コンテンツ、`int`識別、`CategoryID`を更新するレコードの。 最初のオーバー ロードが、渡されたで 2 番目の場合を呼び出す通知`byte`配列が`null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>手順 3: コピーの挿入およびビューの機能

[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-cs.md)という名前のページを作成した`UploadInDetailsView.aspx`を GridView ですべてのカテゴリを一覧表示し、システムに新しいカテゴリを追加する、DetailsView を提供します。 このチュートリアルでは、編集のサポートを削除するなど、GridView を拡張します。 操作を続けるのではなく`UploadInDetailsView.aspx`、let s は、代わりにこのチュートリアルの変更を配置、 `UpdatingAndDeleting.aspx` 、同じフォルダーからページ`~/BinaryData`します。 コピーし宣言型マークアップを貼り付け、コードから`UploadInDetailsView.aspx`に`UpdatingAndDeleting.aspx`します。

開いて開始、`UploadInDetailsView.aspx`ページ。 宣言型構文内のすべてのコピー、`<asp:Content>`要素を図 3 に示すようにします。 次に、`UpdatingAndDeleting.aspx`内では、このマークアップを貼り付けると、`<asp:Content>`要素。 同様からコードをコピー、`UploadInDetailsView.aspx`ページの分離コード クラスを`UpdatingAndDeleting.aspx`します。


[![宣言型マークアップを UploadInDetailsView.aspx からコピーします。](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**図 3**: コピーから宣言型マークアップ`UploadInDetailsView.aspx`([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image6.png))。


宣言型マークアップとコードをコピーした後、次を参照してください。`UpdatingAndDeleting.aspx`します。 出力は同じで、同じユーザー エクスペリエンスの表示と同様`UploadInDetailsView.aspx`ページ、前のチュートリアルから。

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>手順 4: 追加 ObjectDataSource と GridView のサポートを削除します。

説明したよう、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルでは、GridView には、組み込みの削除機能が用意されていて、場合、チェック ボックスのティックでこれらの機能を有効にできます、グリッドの基になります。データ ソースは、削除をサポートします。 現在、ObjectDataSource GridView にバインドされます (`CategoriesDataSource`) は削除をサポートしていません。

これを解決するには、ObjectDataSource s のスマート タグから、ウィザードを起動するには、データ ソースの構成オプションをクリックします。 最初の画面を使用する、ObjectDataSource が構成されていることを示しています、`CategoriesBLL`クラス。 [次へ] をクリックします。 現時点では、のみ、ObjectDataSource s`InsertMethod`と`SelectMethod`プロパティを指定します。 ただし、ウィザード自動設定、ドロップ ダウン リストで、UPDATE および DELETE タブ、`UpdateCategory`と`DeleteCategory`メソッドでは、それぞれします。 これは、ために、`CategoriesBLL`クラスを使用してこれらのメソッドをマークします、`DataObjectMethodAttribute`更新および削除の既定のメソッドとして。

ここでは、(None) に更新 タブのドロップダウン リストの設定が削除 s タブのドロップダウン リストに設定をそのまま`DeleteCategory`します。 更新のサポートを追加する手順 6 では、このウィザードに戻っています。


[![ObjectDataSource DeleteCategory メソッドを使用して構成します。](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**図 4**: 構成に使用する ObjectDataSource、`DeleteCategory`メソッド ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image8.png))。


> [!NOTE]
> ウィザードを完了すると、Visual Studio はキーおよびフィールドの更新する場合は、Web のデータが再生成するフィールドを制御を依頼することがあります。 [はい] を選択した、フィールドのカスタマイズが上書きされますので、いいえを選択します。


ObjectDataSource の値を含めるようになりましたが、`DeleteMethod`プロパティと同様に、`DeleteParameter`します。 ウィザードを使用して、メソッドを指定する、Visual Studio が ObjectDataSource s を設定することを思い出してください`OldValuesParameterFormatString`プロパティを`original_{0}`、更新プログラムの問題を引き起こすし、メソッドの呼び出しを削除します。 そのため、このプロパティを完全に消去またはのいずれか、既定値にリセット`{0}`します。 この ObjectDataSource プロパティ上のメモリを更新する必要がある場合を参照してください、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアル。

ウィザードを完了し、修正した後、 `OldValuesParameterFormatString`ObjectDataSource s の宣言型マークアップは次のようになります。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

ObjectDataSource を構成した後に、GridView s のスマート タグから削除を有効にするチェック ボックスをオン GridView に削除する機能を追加します。 これは、[commandfield] が GridView に追加されますが`ShowDeleteButton`プロパティに設定されて`true`。


[![Gridview を削除するためのサポートを有効にします。](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**図 5**: 削除すると、gridview のサポートを有効にする ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image10.png))。


削除の機能をテストする時間がかかります。 間の外部キーがある、`Products`テーブル s`CategoryID`と`Categories`テーブルの`CategoryID`ので、最初の 8 つのカテゴリのいずれかを削除しようとした場合、外部キー制約の違反例外が表示されます。 Out には、この機能をテストするには、パンフレットと図の両方を提供する、新しいカテゴリを追加します。 図 6 に示すように、私のテスト カテゴリには、という名前のテスト パンフレット ファイルが含まれています。`Test.pdf`とテスト画像。 図 7 は、テスト カテゴリを追加した後に、GridView を示します。


[![パンフレットと画像があるテスト カテゴリを追加します。](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**図 6**: パンフレットとイメージのテスト カテゴリを追加 ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image12.png))。


[![テスト カテゴリを挿入した後、GridView に表示されます。](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**図 7**: テスト カテゴリを挿入した後、GridView に表示されます ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image14.png))。


Visual Studio で、ソリューション エクスプ ローラーを更新します。 新しいファイルを表示する必要があります、`~/Brochures`フォルダー、 `Test.pdf` (図 8 参照)。

次に、ページがポストバックの原因と、テスト カテゴリの行の削除のリンクをクリックし、`CategoriesBLL`クラスの`DeleteCategory`メソッドを起動します。 これは、DAL s を呼び出します`Delete`原因で、適切なメソッド`DELETE`データベースに送信されるステートメントをします。 データが GridView にバインドし、され、マークアップは、テスト カテゴリが存在しないことをクライアントに送信されます。

ワークフローの削除が正常にからテスト カテゴリのレコードを削除中に、`Categories`テーブル、web サーバーのファイル システムからそのパンフレット ファイルを削除しなかった。 ソリューション エクスプ ローラーを更新し、そのことがわかります`Test.pdf`に残っている間は、`~/Brochures`フォルダー。


![Test.pdf ファイルは、Web サーバーのファイル システムから削除されませんでした。](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**図 8**:`Test.pdf`ファイルは、Web サーバーのファイル システムから削除されませんでした


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>手順 5: 削除されたカテゴリのパンフレットのファイルの削除

データベース外部のバイナリ データを格納する欠点の 1 つは、関連付けられているデータベースのレコードが削除されたときに、これらのファイルをクリーンアップする追加の手順を実行する必要があります。 GridView と ObjectDataSource は前に、と delete コマンドが実行された後の両方を起動するイベントを提供します。 実際には、事前および事後アクションの両方のイベントのイベント ハンドラーを作成する必要があります。 前に、`Categories`レコードが削除された、PDF ファイルのパスを決定する必要がありますが、いくつかの例外が発生し、カテゴリは削除されない場合に、カテゴリが削除される前に、PDF を削除することはありません。

GridView s [ `RowDeleting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)発生 ObjectDataSource s delete コマンドが呼び出されたときにその[`RowDeleted`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx)後に起動します。 次のコードを使用してこれら 2 つのイベントのイベント ハンドラーを作成します。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

`RowDeleting`イベント ハンドラー、`CategoryID`行の GridView %s から削除されているがグラブ`DataKeys`を使用してこのイベント ハンドラーにアクセスできる、コレクション、`e.Keys`コレクション。 次に、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`が削除されるレコードに関する情報を返すために呼び出されます。 場合、返された`CategoriesDataRow`オブジェクトがない`NULL``BrochurePath`ページ変数に格納し、値`deletedCategorysPdfPath`でファイルを削除できるように、`RowDeleted`イベント ハンドラー。

> [!NOTE]
> 取得するのではなく、`BrochurePath`に関する詳細情報、`Categories`レコードの削除中、`RowDeleting`イベント ハンドラー、または追加、 `BrochurePath` GridView s`DataKeyNames`プロパティ レコードの値にアクセスを通じて、`e.Keys`コレクション。 そうは若干 GridView のビュー ステートのサイズを増やすが必要なコードの量を減らすになり、旅行をデータベースに保存します。


%S 基になる delete コマンドが呼び出された ObjectDataSource GridView s 後`RowDeleted`イベント ハンドラーが発生します。 データの削除中に例外がない場合は、値を`deletedCategorysPdfPath`PDF は、ファイル システムから削除されます。 この余分なコードはその画像に関連付けられているカテゴリのバイナリ データをクリーンアップするには必要ないことに注意してください。 S 削除ためデータベースに直接画像データが格納されている、ため、`Categories`行では、そのカテゴリの画像データも削除されます。

2 つのイベント ハンドラーを追加した後は、このテスト_ケースを再度実行します。 カテゴリを削除するときに、関連付けられている PDF も削除されます。

既存のレコードの s が関連付けられているバイナリ データを更新するには、いくつかの興味深い課題が提供します。 このチュートリアルの残りの部分、パンフレットと画像への更新機能の追加について詳しく説明します。 手順 6 では、手順 7 は、画像を更新中に、パンフレットの情報を更新するための手法について説明します。

## <a name="step-6-updating-a-category-s-brochure"></a>手順 6: カテゴリのパンフレットの更新

説明したように、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルでは、GridView がその基になるデータ ソースがある場合は、チェック ボックスをオンにして実装できる組み込みの行レベルの編集サポートを提供します。適切に構成されます。 現時点では、`CategoriesDataSource`を更新のサポート、let s を追加するには、ObjectDataSource がまだ構成されていません。

ObjectDataSource のウィザードからのデータ ソースの構成のリンクをクリックして、2 番目の手順に進みます。 ため、`DataObjectMethodAttribute`で使用される`CategoriesBLL`の更新プログラムのドロップダウン リストを自動的に代入する、`UpdateCategory`を 4 つの入力パラメーターを受け取るオーバー ロード (すべての列が`Picture`)。 5 つのパラメーターでオーバー ロードを使用するように変更します。


[![画像のパラメーターを含む UpdateCategory メソッドを使用して ObjectDataSource を構成します。](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**図 9**: 構成に使用する ObjectDataSource、`UpdateCategory`メソッドのパラメーターを含む`Picture`([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image16.png))。


ObjectDataSource の値を含めるようになりましたが、`UpdateMethod`プロパティに対応する`UpdateParameter`s。 Visual Studio が ObjectDataSource s を設定するように手順 4. で、`OldValuesParameterFormatString`プロパティを`original_{0}`データ ソース構成ウィザードを使用する場合。 更新プログラムで問題が発生するをメソッドの呼び出しを削除します。 そのため、このプロパティを完全に消去またはのいずれか、既定値にリセット`{0}`します。

ウィザードを完了し、修正した後、 `OldValuesParameterFormatString`ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

GridView s 組み込み編集機能を有効にするには、GridView s のスマート タグの編集を有効にするオプションをオンにします。 設定すると、[commandfield] s`ShowEditButton`プロパティを`true`、さらに編集ボタン (および編集される行の更新プログラムおよび [キャンセル] ボタン) の結果として得られる。


[![サポートの編集を GridView を構成します。](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**図 10**: 構成のサポートを編集する GridView ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image18.png))。


ブラウザーでページにアクセスし、いずれかの行の編集ボタンをクリックします。 `CategoryName`と`Description`BoundFields がテキスト ボックスとしてレンダリングされます。 `BrochurePath` TemplateField がない、`EditItemTemplate`を表示し続けるため、その`ItemTemplate`パンフレットへのリンク。 `Picture` ImageField を持つテキスト ボックスとしてレンダリング`Text`プロパティには、ImageField s の値が割り当てられている`DataImageUrlField`値、ここで`CategoryID`します。


[![GridView の BrochurePath の編集インターフェイスが不足しています](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**図 11**: GridView を編集するためのインターフェイスがない`BrochurePath`([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image20.png))。


## <a name="customizing-thebrochurepaths-editing-interface"></a>カスタマイズ、`BrochurePath`s 編集インターフェイス

編集インターフェイスを作成する必要があります、 `BrochurePath` TemplateField、いずれかのいずれかにユーザーを許可します。

- としてカテゴリのパンフレットのままでは、
- 新しいパンフレットでは、アップロードすることで、カテゴリのパンフレットを更新または
- カテゴリのパンフレットを (この場合は、カテゴリが関連付けられている、パンフレットになくなりました) を完全に削除します。

更新する必要があります、 `Picture` ImageField s の編集インターフェイスですが手順 7. でこれが表示されます。

GridView s のスマート タグからのテンプレートの編集リンクをクリックし、選択、 `BrochurePath` TemplateField の`EditItemTemplate`ドロップダウン リストから。 このテンプレートの設定に RadioButtonList Web コントロールを追加、`ID`プロパティを`BrochureOptions`とその`AutoPostBack`プロパティを`true`します。 [プロパティ] ウィンドウからの省略記号をクリックします。、`Items`プロパティが表示されますが、`ListItem`コレクション エディター。 次の 3 つのオプションを追加`Value`の 1、2 および 3 にそれぞれ。

- 現在のパンフレットを使用します。
- 現在のパンフレットを削除します。
- 新しいパンフレットをアップロードします。

最初の設定`ListItem`s`Selected`プロパティを`true`します。


![次の 3 つのリスト項目を RadioButtonList に追加します。](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**図 12**: 3 つ追加`ListItem`RadioButtonList s


という名前の FileUpload コントロールを追加、RadioButtonList、下にある`BrochureUpload`します。 設定の`Visible`プロパティを`false`します。


[![後に、RadioButtonList と FileUpload コントロールを追加します。](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**図 13**: RadioButtonList と FileUpload コントロールを追加、 `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image22.png))。


この RadioButtonList は、ユーザーの 3 つのオプションを提供します。 考え方は、アップロードの新しいパンフレット、最後のオプションが選択されている場合にのみ、FileUpload コントロールが表示されることです。 これを実現するには、RadioButtonList s のイベント ハンドラーを作成`SelectedIndexChanged`イベントと、次のコードを追加します。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

RadioButtonList と FileUpload コントロールでは、テンプレート内であるためのこれらのコントロールをプログラムでアクセスするコードを書かなければなりません。 `SelectedIndexChanged`で RadioButtonList の参照がイベント ハンドラーに渡されますが、`sender`入力パラメーター。 FileUpload コントロールを取得する必要があります、RadioButtonList の親コントロールと使用、`FindControl("controlID")`そこからのメソッド。 FileUpload が s を制御、RadioButtonList と FileUpload コントロールへの参照を取得したら、`Visible`プロパティに設定されて`true`場合にのみ、RadioButtonList s`SelectedValue`が 3 に等しい、これは、`Value`アップロード新しいパンフレット`ListItem`.

このコードを少し編集インターフェイスをテストします。 行の編集 ボタンをクリックします。 最初に、現在のパンフレットを使用する オプションを選択してください。 選択されたインデックスの変更がポストバックを発生します。 3 番目のオプションが選択されている場合は、FileUpload コントロールが表示されたら、それ以外の場合は表示されません。 図 14 は、編集 ボタンがクリックされた最初; ときに編集インターフェイスを示しています図 15 は、新しいパンフレットのアップロード オプションを選択した後に、インターフェイスを示します。


[![最初に、使用して現在パンフレット オプションを選択します。](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**図 14**: 最初に、使用して現在パンフレット オプションが選択されている ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image24.png))。


[![FileUpload コントロール アップロード新しいパンフレット オプションの表示の選択](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**図 15**: FileUpload コントロール アップロード新しいパンフレット オプションの表示の選択 ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image26.png))。


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>パンフレットを保存するファイルと更新、`BrochurePath`列

GridView s の Update ボタンがクリックされたときにその`RowUpdating`イベントが発生します。 S update コマンドが呼び出された ObjectDataSource し GridView の`RowUpdated`イベントが発生します。 など、削除のワークフローでは、2 つのイベントのイベント ハンドラーを作成する必要があります。 `RowUpdating`イベント ハンドラーを実行するには、どのようなアクションに基づいて決定する必要があります、`SelectedValue`の`BrochureOptions`RadioButtonList:

- 場合、`SelectedValue`は 1 です。 同じを引き続き使用したい`BrochurePath`設定します。 そのため、ObjectDataSource s を設定する必要があります`brochurePath`を既存のパラメーター`BrochurePath`更新されるレコードの値。 ObjectDataSource s`brochurePath`を使用してパラメーターを設定できる`e.NewValues["brochurePath"] = value`します。
- 場合、 `SelectedValue` 2 では、s のレコードを設定する`BrochurePath`値を`NULL`します。 これは、ObjectDataSource s を設定して実行できます`brochurePath`パラメーターを`Nothing`、その結果は、データベースで`NULL`で使用されている、`UPDATE`ステートメント。 削除されている既存のパンフレット ファイルがある場合、既存のファイルを削除する必要があります。 ただし、だけ使用を行う場合、例外を発生させずに、更新が完了します。
- 場合、`SelectedValue`が 3 では、ユーザーが PDF ファイルをアップロードすることを確認し、ファイル システムに保存し、s のレコードを更新する`BrochurePath`列の値。 さらに、既存の置き換えられるパンフレット ファイルがある場合、前のファイルを削除する必要があります。 ただし、だけ使用を行う場合、例外を発生させずに、更新が完了します。

ときに完了するために必要な手順、RadioButtonList s`SelectedValue`は 3 が実質的に DetailsView s で使用されるものと同じ`ItemInserting`イベント ハンドラー。 追加された DetailsView コントロールから新しいカテゴリのレコードが追加されたときに、このイベント ハンドラーが実行される、[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-cs.md)します。 そのため、個別のメソッドに out は、この機能をリファクタリングすることをプレイヤーします。 具体的は共通の機能を 2 つのメソッドを移動しました。

- `ProcessBrochureUpload(FileUpload, out bool)` FileUpload コントロールのインスタンスと、削除または編集操作を続行するかどうか、またはかどうかによって検証エラーのためキャンセルする必要がありますを指定する出力のブール値を入力として受け取ります。 このメソッドが、保存したファイルへのパスを返しますまたは`null`場合ファイルは保存されませんでした。
- `DeleteRememberedBrochurePath` ページ変数のパスで指定されたファイルを削除します。`deletedCategorysPdfPath`場合`deletedCategorysPdfPath`ない`null`します。

これら 2 つのメソッドのコードに従います。 間の類似性に注意してください`ProcessBrochureUpload`と DetailsView の`ItemInserting`前のチュートリアルからのイベント ハンドラー。 このチュートリアルではこれらの新しいメソッドを使用するには、DetailsView のイベント ハンドラーを更新しました。 DetailsView s のイベント ハンドラーへの変更を表示するには、このチュートリアルに関連付けられているコードをダウンロードします。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

GridView s`RowUpdating`と`RowUpdated`イベント ハンドラーを使用して、`ProcessBrochureUpload`と`DeleteRememberedBrochurePath`メソッドは、次のコードに示すとして。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

注方法、`RowUpdating`イベント ハンドラーに基づき、適切なアクションを実行する一連の条件付きステートメントを使用して、 `BrochureOptions` RadioButtonList の`SelectedValue`プロパティの値。

配置でこのコードでは、カテゴリを編集でき、その現在パンフレットを使用して、パンフレットを使用しないか、新しくアップロードがあります。 さあ、やってみてください。ブレークポイントを設定、`RowUpdating`と`RowUpdated`イベント ハンドラーをワークフローを把握します。

## <a name="step-7-uploading-a-new-picture"></a>手順 7: 新しい写真をアップロードします。

`Picture`から値を含むテキスト ボックスとしてレンダリングのインターフェイスを編集 ImageField s その`DataImageUrlField`プロパティ。 編集のワークフロー中に、GridView にパラメーターを渡しますパラメーター s の名前を持つ ObjectDataSource ImageField s の値`DataImageUrlField`プロパティおよびパラメーターの値を編集インターフェイスにテキスト ボックスに入力された値。 イメージは、ファイル システム上のファイルとして保存するときに、この動作は適切な`DataImageUrlField`イメージの完全な URL が含まれています。 このような状況では、編集インターフェイスには、テキスト ボックスに、ユーザーを変更することができますし、元のデータベースに保存したイメージの URL が表示されます。 確かに、この既定のインターフェイスは t は、新しいイメージをアップロードするユーザーを許可しますが、現在の値からのイメージの URL の変更には。 このチュートリアルでは、ただし、インターフェイスの編集 ImageField の既定十分ではないため、`Picture`バイナリ データがデータベースに直接格納される、`DataImageUrlField`プロパティの保留、 `CategoryID`。

ユーザーが、ImageField を持つ行を編集するときに、チュートリアルでは動作をよく理解するには、次の例を検討してください。: ユーザーを持つ行が編集`CategoryID`10、原因、 `Picture` ImageField 値 10 を含むテキスト ボックスとして表示するためにします。 ユーザーがこのテキスト ボックスに値を 50 に変更し、更新 ボタンをクリックを想像してください。 ポストバックの発生し、GridView という名前のパラメーターを最初に作成する`CategoryID`値 50。 ただし、GridView は、このパラメーターを送信する前に (および`CategoryName`と`Description`パラメーター) から値を追加します、`DataKeys`コレクション。 このため、上書き、`CategoryID`パラメーターを基になる現在行の`CategoryID`10 の値します。 つまり、インターフェイスの編集 ImageField s 効力はなくなりました編集ワークフローでこのチュートリアルのため ImageField s の名前`DataImageUrlField`プロパティおよびグリッドの`DataKey`値は、同じ 1 つ。

ImageField は簡単にデータベースのデータに基づくイメージの表示、編集インターフェイス内のテキスト ボックスを提供することはありません。 代わりに、エンドユーザーがカテゴリ s の画像の変更に使用できる FileUpload コントロールを提供します。 異なり、`BrochurePath`値、これらのチュートリアルを決めたらは各カテゴリが画像を持つ必要がある必要があります。 そのため、こと、ユーザーに関連付けられているユーザーがピクチャしてはならない .vhd ファイルを新しい図が示すか、現在の画像としてのままにする必要はありません-です。

ImageField s 編集インターフェイスをカスタマイズするには、TemplateField に変換する必要があります。 GridView s のスマート タグから列の編集リンクをクリックしてを選択、ImageField TemplateField リンクにこのフィールドに変換 をクリックします。


![ImageField を TemplateField に変換します。](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**図 16**: ImageField を TemplateField に変換します。


ImageField をこの方法で TemplateField に変換するには、2 つのテンプレートを TemplateField が生成されます。 次の宣言型構文に示すよう、 `ItemTemplate` 、イメージ Web が含まれているコントロール`ImageUrl`ImageField s に基づくデータ バインディング構文を使用してプロパティに割り当てられている`DataImageUrlField`と`DataImageUrlFormatString`プロパティ。 `EditItemTemplate` TextBox が含まれていますが`Text`プロパティによって指定された値にバインドする、`DataImageUrlField`プロパティ。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

更新する必要があります、 `EditItemTemplate` FileUpload コントロールを使用します。 テンプレートの編集 、s のスマート タグの GridView からリンクを選び、 `Picture` TemplateField の`EditItemTemplate`ドロップダウン リストから。 テンプレートでは、これを削除するテキスト ボックスが表示されます。 次に、ツールボックスから FileUpload コントロールをドラッグして、テンプレートの設定にその`ID`に`PictureUpload`します。 また、カテゴリの画像を変更するには、新しい画像を指定するテキストを追加します。 同じカテゴリの画像を維持するには、空のままに、フィールド、テンプレートにします。


[![FileUpload コントロールを後に追加します。](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**図 17**: FileUpload コントロールを追加、 `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image28.png))。


編集インターフェイスをカスタマイズしたら、ブラウザーで、進行状況を表示します。 読み取り専用モードで行を表示するときに前に、が、画像の列を表示します FileUpload コントロールでテキストとして編集 ボタンをクリックすると、カテゴリ s の画像が表示されます。


[![編集インターフェイスには、FileUpload コントロールが含まれています。](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**図 18**: FileUpload コントロールが編集インターフェイスに含まれています ([フルサイズの画像を表示する をクリックします](updating-and-deleting-existing-binary-data-cs/_static/image30.png))。


呼び出す、ObjectDataSource が構成されていることを思い出してください、`CategoriesBLL`クラス s`UpdateCategory`として画像のバイナリ データを入力として受け取るメソッドを`byte`配列。 この配列がある場合、`null`値、ただし、代替`UpdateCategory`オーバー ロードを呼び出すと、問題、 `UPDATE` SQL ステートメントを変更しない、`Picture`のままとなりますカテゴリの現在の列の画像をそのまま保存されます。 そのため、GridView s で`RowUpdating`イベント ハンドラーをプログラムで参照する必要があります、 `PictureUpload` FileUpload を制御し、ファイルがアップロードされたかどうかを決定します。 1 つはアップロードされませんでしたとしている場合、私たち*いない*の値を指定する、`picture`パラメーター。 その一方でファイルがアップロードされた場合、 `PictureUpload` JPG ファイルであることを確認する、FileUpload コントロール。 であるかどうかは、そのバイナリ コンテンツを ObjectDataSource に送信できます、`picture`パラメーター。

手順 6 で使用されるコード、DetailsView s で既にここに必要なコードの多くが存在するように`ItemInserting`イベント ハンドラー。 そのため、ve は、新しいメソッドに共通の機能をリファクタリング`ValidPictureUpload`、更新、`ItemInserting`このメソッドを使用してイベント ハンドラー。

GridView s の先頭に次のコードを追加`RowUpdating`イベント ハンドラー。 このコードが t たくないので、パンフレット ファイルを保存するコードの前にすることが重要ですが、無効な画像ファイルをアップロードする場合は、web サーバーのファイル システムにパンフレットを保存します。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

`ValidPictureUpload(FileUpload)`画像ファイルがアップロードされた場合のみ呼び出されます。 メソッドは、その唯一の入力パラメーターとして FileUpload コントロールで受け取り、JPG にアップロードされたファイルがアップロードされたファイルの拡張機能を確認します。 ファイルをアップロードしないかどうか、画像のパラメーターが設定されていないと、そのため、既定値を使用`null`します。 画像がアップロードされた場合と`ValidPictureUpload`返します`true`、`picture`メソッドを返す場合は、パラメーターにアップロードされた画像のバイナリ データが割り当てられている`false`更新のワークフローは取り消され、イベント ハンドラーが終了しました。

`ValidPictureUpload(FileUpload)`メソッドのコードは、DetailsView s からリファクタリングされました`ItemInserting`イベントのハンドラーします。


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>手順 8: カテゴリの元の画像を Jpg に置き換える

8 つのカテゴリの元の画像にビットマップ ファイルが、OLE ヘッダーでラップがあることを思い出してください。 これで、既存のレコードの画像を編集する機能が追加されましたが、少しにこれらのビットマップを Jpg に置き換えます。 引き続き現在のカテゴリの画像を使用する場合は、次の手順を実行することによって Jpg に変換にできます。

1. ビットマップ イメージをハード ドライブに保存します。 参照してください、`UpdatingAndDeleting.aspx`ページの最初の 8 つのカテゴリごとと、ブラウザーで、イメージを右クリックし、画像の保存を選択します。
2. イメージを任意のイメージ エディターで開きます。 たとえば、Microsoft ペイントを使用できます。
3. ビットマップを JPG イメージとして保存します。
4. JPG ファイルを使用して、編集インターフェイスは、カテゴリの画像を更新します。

カテゴリを編集すると、JPG イメージをアップロードして、イメージ レンダリングされません、ブラウザーでため、`DisplayCategoryPicture.aspx`ページが最初の 8 つのカテゴリの画像から最初の 78 バイトを削除します。 OLE ヘッダーの削除を実行するコードを削除することで、これを修正します。 この後、`DisplayCategoryPicture.aspx``Page_Load`イベント ハンドラーは次のコードだけである必要があります。


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx`ページ s を挿入して、インターフェイスの編集は、さらに作業を使用できます。 `CategoryName`と`Description`GridView、DetailsView で BoundFields TemplateFields に変換する必要があります。 `CategoryName`により`NULL`値、RequiredFieldValidator を追加する必要があります。 および`Description`テキスト ボックスが複数行テキスト ボックスに変換される可能性があります。 ままにこれらの最後の仕上げを演習としてできます。


## <a name="summary"></a>まとめ

このチュートリアルでは、バイナリ データの使用方法、外観を完了します。 このチュートリアルと、前の 3 つの場合は、どのバイナリ データを説明しましたファイル システム上またはデータベース内に直接格納できます。 ユーザーは、ハード ドライブからファイルを選択して、ファイル システムに格納されているか、データベースに挿入される、web サーバーにアップロードすることによってシステムにバイナリ データを提供します。 ASP.NET 2.0 には、ドラッグ アンド ドロップするだけこのようなインターフェイスを提供することにより、FileUpload コントロールが含まれます。 ただしで説明するよう、[のファイルのアップロード](uploading-files-cs.md)チュートリアルでは、FileUpload コントロールはのみ比較的小さなファイルのアップロード、理想的には 1 メガバイトを超えていないに最適です。 編集し、既存のレコードからバイナリ データを削除する方法と、基になるデータ モデルにアップロードされたデータを関連付ける方法も検討しました。

[次へ]、一連のチュートリアルでは、さまざまなキャッシュ手法について説明します。 アプリケーション s を向上させる方法は、キャッシュ全体のパフォーマンス負荷の高い操作の結果を取得しより迅速にアクセスできる場所に格納します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [次へ](uploading-files-vb.md)
