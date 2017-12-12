---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: "更新および削除する既存のバイナリ データ (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルでどのように GridView コントロールを簡単に編集およびテキスト データを削除しました。 このチュートリアルではどの GridView コントロールを行うことも表示しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e14b19f99e9f41c5a296d73ba689095a686794db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>既存のバイナリ データ (VB) 更新および削除
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe)または[PDF のダウンロード](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> 前のチュートリアルでどのように GridView コントロールを簡単に編集およびテキスト データを削除しました。 このチュートリアルではどの GridView コントロールもなりますを編集し、そのバイナリ データがデータベースに保存またはファイル システムに格納されているかどうか、バイナリ データを削除することがわかります。


## <a name="introduction"></a>はじめに

過去の 3 つのチュートリアルには相当量のバイナリ データを操作するための機能を追加しています。 追加することで開始、`BrochurePath`列を`Categories`テーブルが表示され、アーキテクチャが適宜更新します。 既存のカテゴリ テーブルを使用するデータ アクセス層とビジネス ロジック層のメソッドが追加されています`Picture`列で、イメージ ファイルのバイナリ コンテンツ s を保持します。 提示する GridView のバイナリ データ、パンフレットのダウンロード リンク カテゴリの図に示すように web ページを作成した、`<img>`要素を新しいカテゴリを追加し、そのパンフレットと画像データをアップロードできるようにする DetailsView を追加したとします。

実装するのには編集および編集を使用して、GridView s 組み込みと機能を削除すると、このチュートリアルでは実行を既存のカテゴリを削除する機能。 カテゴリを編集するには、ときに、ユーザーは必要に応じて新しい画像をアップロードまたはカテゴリは、引き続き既存の 1 つを使用できるようになります。 パンフレットをするかを選択できますまたはを使用する既存のパンフレット新しいパンフレットをアップロードするカテゴリは、関連付けられているパンフレットを不要になったことを示すです。 開始 s を let!

## <a name="step-1-updating-the-data-access-layer"></a>手順 1: 更新、データ アクセス レイヤー

DAL が自動生成`Insert`、 `Update`、および`Delete`メソッドがこれらのメソッドを基に生成された、 `CategoriesTableAdapter` s メインのクエリが含まれていない、`Picture`列です。 したがって、`Insert`と`Update`メソッドは、カテゴリの画像のバイナリ データを指定するためのパラメーターを含めないでください。 行ったように、[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-vb.md)、更新するための新しい TableAdapter メソッドを作成する必要があります、`Categories`テーブルのバイナリ データを指定する場合。

型指定されたデータセットを開くし、デザイナーを右クリックし、`CategoriesTableAdapter`のヘッダーを選択してクエリの追加、コンテキスト メニューから launche する TableAdapter クエリの構成ウィザードとします。 このウィザードは、TableAdapter クエリがデータベースにアクセスする方法を求めてで開始します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 次の手順は、生成されるクエリの種類に求めます。 クエリを作成する新しいレコードを追加する re お以降、`Categories`テーブルを更新を選択し、[次へ] をクリックします。


[![更新プログラム を選択します。](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**図 1**: UPDATE オプションを選択 ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


ここで指定する必要があります、 `UPDATE` SQL ステートメント。 ウィザードが自動的に提案、 `UPDATE` TableAdapter のメインのクエリに対応するステートメント (更新する 1 つ、 `CategoryName`、 `Description`、および`BrochurePath`値)。 ステートメントを変更できるように、`Picture`列がと共に含まれる、`@Picture`パラメーター、次のようにします。


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

ウィザードの最終画面では、新しい TableAdapter メソッドの名前を付けることが求められます。 入力`UpdateWithPicture`[完了] をクリックします。


[![新しい TableAdapter メソッド UpdateWithPicture の名前](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**図 2**: 新しい TableAdapter メソッド名前`UpdateWithPicture`([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>手順 2: ビジネス ロジック層のメソッドを追加します。

に加えて、DAL を更新するには、更新およびカテゴリを削除するためのメソッドを含める BLL を更新する必要があります。 これらは、プレゼンテーション層から呼び出されるメソッドです。

カテゴリを削除するを使用できる、`CategoriesTableAdapter`自動生成された s`Delete`メソッドです。 次のメソッドを追加、`CategoriesBLL`クラス。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Let s はこのチュートリアルでは、バイナリの画像データを必要としを起動する 1 つのカテゴリを更新するための 2 つのメソッドを作成、`UpdateWithPicture`メソッドに追加したばかりの`CategoriesTableAdapter`もう 1 つを受け付けるだけを`CategoryName`、 `Description`、および`BrochurePath`値を使用して`CategoriesTableAdapter`クラス s の自動生成された`Update`ステートメントです。 2 つのメソッドを使用して背後にある論理的根拠は、状況によっては、ユーザーはその他のフィールドと共に、新しい画像をアップロードする必要がある場合、ユーザー カテゴリの画像を更新する可能性があります。 アップロードされた画像のバイナリ データで使用する、`UPDATE`ステートメントです。 他の場合、ユーザーのみ興味を更新すると、名前と説明します。 場合は、`UPDATE`ステートメントのバイナリ データが必要ですが、`Picture`列も、その d する必要がありますもその情報を提供します。 編集中のレコードを画像データに戻して、データベースへの追加のトリップが必要になります。 そのため、2 つ必要`UPDATE`メソッドです。 ビジネス ロジック層は、使用するが、カテゴリを更新するときに画像データを提供するかどうかに基づいて決定されます。

この作業を容易にする 2 つのメソッドを追加、`CategoriesBLL`という両方のクラス`UpdateCategory`です。 最初の 1 つには、3 つ受け入れる必要があります`String`、s、`Byte`配列、および`Integer`の入力としてパラメーター、2 番目、3 つ`String`s と`Integer`です。 `String`カテゴリの名前、説明、およびパンフレット ファイル パスでは、入力パラメーター、`Byte`カテゴリの画像のバイナリ コンテンツは、配列、および`Integer`を識別、`CategoryID`を更新するレコードのです。 最初のオーバー ロードが渡された 2 番目の場合を呼び出すことに注意してください`Byte`配列が`Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>手順 3: が Insert およびビューの機能をコピー

[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-vb.md)という名前のページを作成した`UploadInDetailsView.aspx`を GridView のすべてのカテゴリを一覧表示され、システムに新しいカテゴリを追加する DetailsView を提供します。 このチュートリアルでは、編集のサポートを削除するなど、GridView を拡張します。 作業を続行するのではなく`UploadInDetailsView.aspx`、let s は、代わりにこのチュートリアルの変更を配置、 `UpdatingAndDeleting.aspx`  ページで、同じフォルダーから`~/BinaryData`です。 コピー貼り付け宣言型マークアップをコードから`UploadInDetailsView.aspx`に`UpdatingAndDeleting.aspx`です。

開いて開始、`UploadInDetailsView.aspx`ページ。 すべての宣言の構文のコピー、`<asp:Content>`要素、図 3 に示すようにします。 次に、開いて`UpdatingAndDeleting.aspx`内では、このマークアップを貼り付けると、`<asp:Content>`要素。 同様からコードをコピー、 `UploadInDetailsView.aspx` s 分離コード クラスをページ`UpdatingAndDeleting.aspx`です。


[![宣言型マークアップを UploadInDetailsView.aspx からコピーします。](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**図 3**: コピーから宣言型マークアップ`UploadInDetailsView.aspx`([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


宣言型マークアップとコードをコピーした後、次を参照してください。`UpdatingAndDeleting.aspx`です。 同じ出力し、同じユーザー エクスペリエンスの表示と同様に`UploadInDetailsView.aspx`前のチュートリアルのページです。

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>手順 4: 追加 ObjectDataSource、GridView には、サポートを削除します。

説明したよう、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、GridView は、組み込みの削除機能を提供し、これらの機能を有効にできます、ティックのチェック ボックスの場合、グリッドの基になります。データ ソースは、削除をサポートします。 現在、ObjectDataSource、GridView にバインドされて (`CategoriesDataSource`) は削除をサポートしていません。

この問題を解決するには、ObjectDataSource s のスマート タグからウィザードを起動するには、データ ソースの構成オプションをクリックします。 最初の画面を使用する、ObjectDataSource が構成されていることを示しています、`CategoriesBLL`クラスです。 次に達してください。 現時点では、のみ、ObjectDataSource s`InsertMethod`と`SelectMethod`プロパティが指定されています。 ただし、ウィザード自動設定された UPDATE および DELETE タブで、ドロップダウン リスト、`UpdateCategory`と`DeleteCategory`メソッド、それぞれします。 これは、ために、`CategoriesBLL`クラスを使用してこれらのメソッドをマークして、`DataObjectMethodAttribute`更新および削除の既定のメソッドとして。

ここでは、(なし) に更新 タブのドロップダウン リストを設定が削除 s タブのドロップダウン リストに設定をそのまま`DeleteCategory`です。 更新のサポートを追加する手順 6. では、このウィザードを返します。


[![ObjectDataSource DeleteCategory メソッドを使用して構成します。](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**図 4**: 構成を使用する ObjectDataSource、`DeleteCategory`メソッド ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> ウィザードを完了すると、Visual Studio を要求するフィールドの更新およびキーにする場合は、Web のデータが再生成するフィールドを制御します。 はい を選択すると、フィールドが加えたカスタマイズは上書きためいいえ を選択します。


値と表示されます、ObjectDataSource その`DeleteMethod`プロパティと同様に、`DeleteParameter`です。 を、メソッドを指定する、ウィザードを使用する場合は、Visual Studio ObjectDataSource s と設定ことに注意してください`OldValuesParameterFormatString`プロパティを`original_{0}`、更新プログラムの問題を引き起こすし、メソッドの呼び出しを削除します。 そのため、このプロパティを完全に消去またはのいずれか、既定値にリセット`{0}`です。 この ObjectDataSource プロパティ上のメモリを更新する必要がある場合は、次を参照してください。、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルです。

ウィザードの完了し、修正した後、 `OldValuesParameterFormatString`ObjectDataSource s の宣言型マークアップは、次のようなようになります。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

ObjectDataSource を構成した後、GridView に GridView s のスマート タグからの削除を有効にするチェック ボックスをチェックして削除機能を追加します。 これには、GridView に、CommandField が追加が`ShowDeleteButton`プロパティに設定されている`True`です。


[![GridView で削除するためのサポートを有効にします。](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**図 5**: GridView で削除のサポートを有効にする ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


すぐを delete の機能をテストします。 間の外部キーがある、`Products`表 s`CategoryID`と`Categories`表の`CategoryID`ので、最初の 8 つのカテゴリのいずれかを削除しようとする場合、外部キー制約違反の例外が表示されます。 出力には、この機能をテストするには、パンフレットと画像の両方を指定して、新しいカテゴリを追加します。 図 6 に示すように、自分のテスト カテゴリには、という名前のテスト パンフレット ファイルが含まれています。`Test.pdf`とテストの画像。 図 7 は、テスト カテゴリを追加した後、GridView を示します。


[![パンフレット イメージとテスト カテゴリを追加します。](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**図 6**: パンフレット イメージとテスト カテゴリを追加する ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![表示されるテスト カテゴリを挿入すると、GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**図 7**: テスト カテゴリを挿入すると、GridView に表示されます ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


Visual Studio で、ソリューション エクスプ ローラーを更新します。 新しいファイルが表示されます、`~/Brochures`フォルダー、 `Test.pdf` (図 8 を参照してください)。

次に、テスト カテゴリの行で、原因でページのポストバックが [削除] リンクをクリックし、`CategoriesBLL`クラスの`DeleteCategory`メソッドを起動します。 これは、DAL s を呼び出します`Delete`メソッド、原因で、適切な`DELETE`データベースに送信されるステートメントをします。 データが GridView にし、再バインドし、マークアップは、テスト カテゴリが存在しないことをクライアントに送信します。

ワークフローの削除が正常にからテスト カテゴリのレコードを削除中に、`Categories`テーブル、web サーバーのファイル システムからそのパンフレット ファイルを削除しなかったです。 ソリューション エクスプ ローラーを更新していることが分かります`Test.pdf`に残っている間は、`~/Brochures`フォルダーです。


![Test.pdf ファイルは Web サーバーのファイル システムから削除されませんでした。](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**図 8**:`Test.pdf`ファイルは、Web サーバーのファイル システムから削除されませんでした


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>手順 5: 削除されたカテゴリのパンフレット ファイルを削除します。

データベース外部のバイナリ データを格納する欠点の 1 つは、関連付けられているデータベースのレコードが削除されたときに、これらのファイルをクリーンアップする追加の手順を実行する必要があります。 ObjectDataSource、GridView は前に、と delete コマンドが実行された後の両方を起動するイベントを提供します。 実際には、前とアクション後の両方のイベントのイベント ハンドラーを作成する必要があります。 前に、`Categories`レコードが削除された、PDF ファイルのパスを決定する必要がありますが、いくつかの例外が発生し、カテゴリは削除されない場合に備えて、カテゴリが削除される前に、PDF を削除したくありません。

GridView s [ `RowDeleting`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)発生 ObjectDataSource s delete コマンドが呼び出されたときにその[`RowDeleted`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx)後に発生します。 次のコードを使用してこれらの 2 つのイベントのイベント ハンドラーを作成します。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

`RowDeleting` 、イベント ハンドラー、`CategoryID`行の GridView s から削除中はつかむ`DataKeys`でこのイベント ハンドラーからアクセス可能なコレクション、`e.Keys`コレクション。 次に、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`が削除されるレコードに関する情報を返す呼び出されます。 場合、返された`CategoriesDataRow`オブジェクトがない`NULL``BrochurePath`値には、ページ変数に格納されているし、`deletedCategorysPdfPath`でファイルを削除できるように、`RowDeleted`イベント ハンドラー。

> [!NOTE]
> 取得するのではなく、`BrochurePath`の詳細情報を`Categories`記録で削除されています、`RowDeleting`イベント ハンドラー、または追加、 `BrochurePath` GridView s`DataKeyNames`プロパティ レコード秒の値にアクセスし、を介して、`e.Keys`コレクション。 そうは若干 GridView のビュー ステートのサイズを増やすは必要なコードの量を削減され、トリップをデータベースに保存します。


%S 基になる delete コマンドが呼び出されて、ObjectDataSource、GridView s 後`RowDeleted`イベント ハンドラーが発生します。 データの削除中に例外がないの値がある場合`deletedCategorysPdfPath`PDF がファイル システムから削除されます。 この余分なコードは、その画像に関連付けられているカテゴリ s のバイナリ データをクリーンアップするのには必要ありませんに注意してください。 S 画像データが保存されるため、データベースで直接がので削除すると、`Categories`行では、そのカテゴリの画像データも削除されます。

2 つのイベント ハンドラーを追加すると、このテスト_ケースを再度実行します。 カテゴリを削除するときに、関連付けられている PDF も削除されます。

既存のレコードの s が関連付けられているバイナリ データの更新は、いくつかの興味深い課題を提供します。 このチュートリアルの残りの部分パンフレットと画像への更新機能の追加について詳しく説明します。 手順 6 では、手順 7 は、画像を更新中に、パンフレット情報を更新するための手法について説明します。

## <a name="step-6-updating-a-category-s-brochure"></a>手順 6: 更新カテゴリのパンフレット

概要の挿入、更新、および削除するデータのチュートリアルで既に説明した、GridView はその基になるデータ ソースが適切に構成されている場合のチェック ボックスのティックで実装できる組み込みの行レベル編集サポートを提供します。 現時点では、 `CategoriesDataSource` let s を追加するためのサポートもアップグレードする ObjectDataSource がまだ構成されていません。

ObjectDataSource のウィザードでデータ ソースの構成のリンクをクリックして、2 番目の手順に進みます。 ため、`DataObjectMethodAttribute`で使用される`CategoriesBLL`、更新プログラムのドロップダウン リストを自動的に代入する、`UpdateCategory`を 4 つの入力パラメーターを受け入れるオーバー ロード (すべての列が、 `Picture`)。 5 つのパラメーターを持つオーバー ロードを使用するように変更します。


[![パラメーターが含まれる画像の UpdateCategory メソッドを使用して、ObjectDataSource を構成します。](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**図 9**: 構成を使用する ObjectDataSource、`UpdateCategory`メソッドのパラメーターが含まれる`Picture`([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


値と表示されます、ObjectDataSource その`UpdateMethod`プロパティに対応する`UpdateParameter`s。 Visual Studio は、ObjectDataSource s 設定述べたように手順 4. で、`OldValuesParameterFormatString`プロパティを`original_{0}`データ ソース構成ウィザードを使用する場合。 更新プログラムで問題が発生する、メソッドの呼び出しを削除します。 そのため、このプロパティを完全に消去またはのいずれか、既定値にリセット`{0}`です。

ウィザードの完了し、修正した後、 `OldValuesParameterFormatString`ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

GridView s 組み込み編集機能を有効にするには、GridView s のスマート タグの編集を有効にするオプションを確認します。 設定すると、CommandField s`ShowEditButton`プロパティを`True`さらに、[編集] ボタン (および編集されている行を更新し、[キャンセル] ボタン) の結果として得られる、します。


[![編集をサポートする GridView を構成します。](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**図 10**: 構成の編集をサポートする GridView ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


ブラウザーでページを参照してくださいし、行の編集ボタンのいずれかをクリックします。 `CategoryName`と`Description`BoundFields はテキスト ボックスとしてレンダリングされます。 `BrochurePath` TemplateField がない、`EditItemTemplate`ので、表示は引き続きその`ItemTemplate`パンフレットへのリンク。 `Picture`がテキスト ボックスとしてレンダリング ImageField`Text`プロパティには、ImageField s の値が割り当てられた`DataImageUrlField`値、ここでは`CategoryID`します。


[![GridView は、BrochurePath の編集のインターフェイスがないです。](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**図 11**: GridView の編集インターフェイスがない`BrochurePath`([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>カスタマイズ、`BrochurePath`s 編集インターフェイス

編集のインターフェイスを作成する必要があります、 `BrochurePath` TemplateField、いずれかのいずれかにユーザーを許可します。

- としてカテゴリのパンフレットのままでは、
- 新しいパンフレット、アップロードすることにより、カテゴリのパンフレットを更新または
- カテゴリのパンフレットを (カテゴリに関連付けられているパンフレットがない場合) を完全に削除します。

更新する必要があります、 `Picture` ImageField s の編集のインターフェイスですが手順 7. でこれが表示されます。

GridView s のスマート タグからのテンプレートの編集リンクをクリックし、、 `BrochurePath` TemplateField の`EditItemTemplate`ドロップダウン リストからです。 このテンプレートの設定に RadioButtonList Web コントロールを追加、`ID`プロパティを`BrochureOptions`とその`AutoPostBack`プロパティを`True`です。 [プロパティ] ウィンドウ内の省略記号をクリックして、`Items`プロパティが表示される`ListItem`コレクション エディターです。 次の 3 つのオプションを追加`Value`の 1、2 および 3、それぞれします。

- 使用して現在パンフレット
- 現在パンフレットを削除します。
- 新しいパンフレットをアップロードします。

最初の設定`ListItem`s`Selected`プロパティを`True`です。


![RadioButtonList に 3 つのリスト項目を追加します。](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**図 12**: 3 つの追加`ListItem`RadioButtonList s


名前付きファイルアップロード コントロールを追加、RadioButtonList 下にある`BrochureUpload`です。 設定の`Visible`プロパティを`False`です。


[![RadioButtonList とファイルアップロード コントロールを後に追加します。](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**図 13**: RadioButtonList とファイルアップロード コントロールを追加、 `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


この RadioButtonList は、ユーザーの 3 つのオプションを提供します。 つまり、新しいパンフレットのアップロード、最後のオプションが選択されている場合にのみ、ファイルアップロード コントロールが表示されることです。 これを実現するには、RadioButtonList s のイベント ハンドラーを作成`SelectedIndexChanged`イベントし、次のコードを追加します。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

RadioButtonList およびファイルアップロードのコントロールでは、テンプレート内にあるために、これらのコントロールをプログラムでアクセスするコードのビットを記述するお必要。 `SelectedIndexChanged`で RadioButtonList の参照を渡されるイベント ハンドラー、`sender`入力パラメーターです。 ファイルアップロード コントロールを取得する必要がありますおよびを取得する RadioButtonList の親コントロールを使用して、`FindControl("controlID")`そこからのメソッドです。 ファイルアップロードが s を制御 RadioButtonList とファイルアップロードの両方のコントロールへの参照を作成したら、`Visible`プロパティに設定されている`True`場合にのみ、RadioButtonList s`SelectedValue`は 3 に等しい、`Value`アップロード新しいパンフレット`ListItem`.

配置でこのコードでは、すぐを編集インターフェイスをテストします。 行の編集 ボタンをクリックします。 最初に、現在のパンフレットを使用する オプションを選択してください。 選択されたインデックスを変更すると、ポストバックが発生します。 3 番目のオプションが選択されている場合は、それ以外の場合は非表示、ファイルアップロード コントロールが表示されます。 [編集] ボタンがクリックされた最初; ときに、図 14 が、編集用のインターフェイスを示しています図 15 は、アップロード新しいパンフレット オプションが選択された後に、インターフェイスを示します。


[![最初に、使用する現在パンフレット オプションを選択します。](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**図 14**: 最初に、使用する現在パンフレット オプションが選択されている ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![アップロード新しいパンフレット オプションは、表示ファイルアップロード コントロールを選択します。](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**図 15**: アップロード新しいパンフレット オプションは、表示ファイルアップロード コントロールを選択する ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>パンフレットを保存するファイルと更新、`BrochurePath`列

GridView s [更新] ボタンがクリックされたときにその`RowUpdating`イベントが発生します。 S 更新コマンドが呼び出された ObjectDataSource し GridView の`RowUpdated`イベントが発生します。 など、削除のワークフローでは、これらのイベントの両方のイベント ハンドラーを作成する必要があります。 `RowUpdating` 、イベント ハンドラーのアクションがに基づいて決定する必要があります、`SelectedValue`の`BrochureOptions`RadioButtonList:

- 場合、 `SelectedValue` 1 の場合は、同じの使用を継続する`BrochurePath`設定します。 したがって、ObjectDataSource s を設定する必要があります`brochurePath`を既存のパラメーター`BrochurePath`更新されるレコードの値。 ObjectDataSource s`brochurePath`を使用してパラメーターを設定することができます`e.NewValues["brochurePath"] = value`です。
- 場合、 `SelectedValue` 2 では、s のレコードを設定する`BrochurePath`値を`NULL`です。 これには、ObjectDataSource s を設定して`brochurePath`パラメーターを`Nothing`、その結果は、データベースで`NULL`で使用されている、`UPDATE`ステートメントです。 削除されている既存のパンフレット ファイルがある場合、既存のファイルを削除する必要があります。 ただし、おのみこれを行う場合は例外を発生させずに、更新プログラムが完了します。
- 場合、`SelectedValue`をユーザーが PDF ファイルをアップロードされたことを確認し、ファイル システムに保存および s のレコードを更新しますが、3`BrochurePath`列の値。 さらに、置き換えられる既存パンフレット ファイルがある場合、以前のファイルを削除する必要があります。 ただし、おのみこれを行う場合は例外を発生させずに、更新プログラムが完了します。

ときに完了するために必要な手順 RadioButtonList s`SelectedValue`は 3 が実質的に DetailsView s が使用されるものと同じ`ItemInserting`イベント ハンドラー。 DetailsView コントロールで追加してから、新しいカテゴリのレコードが追加されたときに、このイベント ハンドラーが実行される、[前のチュートリアル](including-a-file-upload-option-when-adding-a-new-record-vb.md)です。 そのため、この機能は、異なるメソッドに out をリファクタリングすることを behooves です。 具体的は共通の機能を 2 つのメソッドを移動します。

- `ProcessBrochureUpload(FileUpload, out bool)`ファイルアップロード コントロールのインスタンスと削除または編集の操作を続行するかどうかあるかどうかは、いくつかの検証エラーのため取り消す必要があるかを示す出力のブール値を入力として受け取ります。 このメソッドが、保存したファイルへのパスを返しますまたは`null`場合、ファイルは保存されませんでした。
- `DeleteRememberedBrochurePath`ページ変数のパスで指定されたファイルを削除`deletedCategorysPdfPath`場合`deletedCategorysPdfPath`は`null`します。

これら 2 つのメソッドのコードが次に示します。 間の類似性に注意してください`ProcessBrochureUpload`と DetailsView の`ItemInserting`前のチュートリアルのイベント ハンドラー。 このチュートリアルではこれらの新しいメソッドを使用するには、DetailsView のイベント ハンドラーを更新しました。 DetailsView のイベント ハンドラーへの変更を表示するには、このチュートリアルに関連付けられているコードをダウンロードします。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s`RowUpdating`と`RowUpdated`イベント ハンドラーを使用して、`ProcessBrochureUpload`と`DeleteRememberedBrochurePath`メソッドは、次のコードを示します。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

注方法、`RowUpdating`イベント ハンドラーに基づき、適切なアクションを実行する一連の条件付きステートメントを使用して、 `BrochureOptions` RadioButtonList の`SelectedValue`プロパティの値。

配置でこのコードでは、カテゴリを編集でき、ある、現在パンフレットを使用して、パンフレットを使用しないか、新しいものをアップロードできます。 さあ、お試しください。ブレークポイントを設定、`RowUpdating`と`RowUpdated`イベント ハンドラーをワークフローの意味を取得します。

## <a name="step-7-uploading-a-new-picture"></a>手順 7: 新しい写真をアップロードします。

`Picture` ImageField s が、値を事前設定するテキスト ボックスとしてインターフェイスでの表示を編集、`DataImageUrlField`プロパティです。 編集のワークフローの間に GridView にパラメーターを渡しますパラメーター s の名前を持つ ObjectDataSource ImageField s の値`DataImageUrlField`プロパティおよびパラメーターの値の編集インターフェイスで、テキスト ボックスに入力された値。 イメージは、ファイル システム上のファイルとして保存するときに、この動作は適切なと`DataImageUrlField`イメージの完全な URL が含まれています。 このような状況では、編集のインターフェイスは ボックスで、ユーザーを変更することができますし、データベースに保存したイメージの URL を表示します。 確かに、この既定のインターフェイスでは t を許可する、新しいイメージをアップロードするユーザーが現在の値からイメージの URL を変更することは。 このチュートリアルでは、ただし、ImageField の既定のインターフェイスを編集十分ではないため、`Picture`バイナリ データがデータベースに直接格納されると、`DataImageUrlField`プロパティを保持だけ`CategoryID`です。

ユーザーが、ImageField を持つ行を編集するとき、チュートリアルでの動作を理解するには、次の例を検討してください: ユーザーを含む行を編集する`CategoryID`10、原因、 `Picture` ImageField 値 10 を含むテキスト ボックスとして表示するためにします。 ユーザーが 50 にこのテキスト ボックス内の値を変更し、更新ボタンをクリックすることを想像してください。 ポストバックが発生したし、GridView という名前のパラメーターを最初に作成する`CategoryID`値 50 を使用します。 ただし、GridView は、このパラメーターを送信する前に (および`CategoryName`と`Description`パラメーター) から値を追加、`DataKeys`コレクション。 このため、上書き、`CategoryID`パラメーターと、現在行を基になる`CategoryID`10 の値します。 つまり、編集インターフェイス ImageField の影響を与えません編集のワークフローでこのチュートリアルのため ImageField s の名前`DataImageUrlField`プロパティおよびグリッドの`DataKey`値は、同じ 1 つです。

ImageField は簡単にデータベースのデータに基づくイメージを表示、編集インターフェイスで textbox を提供したくありません。 代わりに、カテゴリの画像を変更する、エンドユーザーが使用できるファイルアップロード コントロールを提供します。 異なり、`BrochurePath`値、これらのチュートリアルを決めたらは各カテゴリが画像を持つ必要がある必要があります。 そのため、ユーザーが関連付けられているユーザーがピクチャしてはならない .vhd ファイル内の新しい画像を示すか、現在の画像としてのままにできるようにする必要ありません-はします。

ImageField s 編集インターフェイスをカスタマイズするには、TemplateField に変換する必要があります。 GridView s のスマート タグからは、[列の編集リンクをクリックして、ImageField を選択して TemplateField リンクにこのフィールドに変換] をクリックします。、


![ImageField を TemplateField に変換します。](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**図 16**: ImageField を TemplateField に変換


ImageField をこの方法で TemplateField に変換するには、2 つのテンプレート TemplateField が生成されます。 次の宣言の構文に示す、`ItemTemplate`イメージ Web が含まれているコントロール`ImageUrl`ImageField s に基づくデータ バインド構文を使用してプロパティが割り当てられた`DataImageUrlField`と`DataImageUrlFormatString`プロパティです。 `EditItemTemplate` TextBox が含まれていますが`Text`プロパティで指定された値にバインド、`DataImageUrlField`プロパティです。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

更新する必要があります、`EditItemTemplate`ファイルアップロード コントロールを使用します。 テンプレートの編集 スマート タグの s GridView からリンクし、選択、 `Picture` TemplateField の`EditItemTemplate`ドロップダウン リストからです。 テンプレートでは、これを削除するテキスト ボックスが表示されます。 ファイルアップロード コントロールをツールボックスから、テンプレートの設定を次に、ドラッグ、`ID`に`PictureUpload`です。 カテゴリの画像を変更するには、新しい画像を指定するテキストを追加します。 同じカテゴリの図を保存、空のままに、フィールド、テンプレートにします。


[![ファイルアップロード コントロール、EditItemTemplate を追加します。](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**図 17**: するファイルアップロード コントロールを追加、 `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


インターフェイスの編集をカスタマイズするには後に、、ブラウザーで進行状況を表示します。 読み取り専用モードで行を表示するときに前に、のファイルアップロード コントロールでテキストとして、画像の列を表示する [編集] ボタンをクリックすると、カテゴリのイメージが表示されます。


[![編集のインターフェイスには、ファイルアップロード コントロールが含まれています。](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**図 18**: 編集インターフェイスにはファイルアップロード コントロールが含まれています ([フルサイズのイメージを表示するをクリックして](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


呼び出して、ObjectDataSource が構成されていることに注意してください、`CategoriesBLL`クラス s`UpdateCategory`として、画像のバイナリ データを入力として受け取るメソッドを`Byte`配列。 この配列がある場合`Nothing`、ただし、代替`UpdateCategory`オーバー ロードが呼び出されると、どの問題、 `UPDATE` SQL ステートメントを変更しない、`Picture`列、それによってままカテゴリ s を現在の画像をそのまま保存します。 そのため、GridView s で`RowUpdating`イベント ハンドラーをプログラムで参照する必要があります、`PictureUpload`ファイルアップロードを制御し、ファイルがアップロードされたかどうかを判断します。 1 つはアップロードされませんでした、し、作業を行うかどうか*いない*の値を指定する、`picture`パラメーター。 その一方で、ファイルの場合にアップロードされた場合、`PictureUpload`ファイルアップロード制御したい JPG ファイルであることを確認します。 かどうかは、おを通じて ObjectDataSource をそのバイナリの内容を送信することができます、`picture`パラメーター。

手順 6 で使用するコードを DetailsView s で既にここに必要なコードの多くが存在するように`ItemInserting`イベント ハンドラー。 そのため、ve 新しいメソッドに共通の機能のリファクタリングを行う`ValidPictureUpload`、更新、`ItemInserting`このメソッドを使用してイベント ハンドラー。

GridView s の先頭に次のコードを追加`RowUpdating`イベント ハンドラー。 T がないため、パンフレット ファイルを保存するコードの前にこのコードを取得することが重要が無効な画像ファイルをアップロードする場合、web サーバーのファイル システムにパンフレットを保存します。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)`画像ファイルをアップロードする場合のみと呼ばれるメソッドは、唯一の入力パラメーターとしてファイルアップロード コントロールでは取得し、アップロードされたファイルは、JPG であることを確認するアップロードされたファイル拡張子を確認します。 かどうかファイルはアップロードされませんが、その画像パラメーターが設定されていないため、既定値を使用`Nothing`です。 画像のアップロードする場合と`ValidPictureUpload`を返します`True`、`picture`メソッドを返す場合、パラメーターは、アップロードされた画像のバイナリ データを割り当てられている`False`、更新プログラム ワークフローは取り消され、イベント ハンドラーが終了しました。

`ValidPictureUpload(FileUpload)` DetailsView s からリファクタリングされたメソッドのコード`ItemInserting`イベント ハンドラーがフォローします。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>手順 8: に置き換えてカテゴリの元の画像の Jpg

8 つのカテゴリの元の画像が OLE ヘッダーでラップ ビットマップ ファイルであることに注意してください。 これで、既存のレコードの画像を編集する機能を追加しましたをとって Jpg でこれらのビットマップを置き換えます。 引き続き現在のカテゴリの画像を使用する場合は、次の手順を実行することによって jpg 形式を変換にできます。

1. ビットマップ イメージをハード ドライブに保存します。 参照してください、`UpdatingAndDeleting.aspx`ページと最初の 8 つのカテゴリごとに、ブラウザーで、イメージを右クリックし、画像の保存を選択します。
2. 任意のイメージ エディターで、イメージを開きます。 たとえば、ペイントを使用できます。
3. ビットマップは、JPG イメージとして保存します。
4. JPG ファイルを使用して、編集のインターフェイス、カテゴリの画像を更新します。

カテゴリを編集すると、JPG イメージをアップロード、イメージは表示されません、ブラウザーのため、`DisplayCategoryPicture.aspx`ページは、最初の 8 つのカテゴリの画像から最初の 78 バイトの分解です。 OLE ヘッダーの削除を実行するコードを削除することで、この問題を解決します。 この後、`DisplayCategoryPicture.aspx``Page_Load`イベント ハンドラーは次のコードだけである必要があります。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx`ページ s を挿入して、インターフェイスの編集はに比べて多くの作業を使用できます。 `CategoryName`と`Description`DetailsView および GridView BoundFields TemplateFields に変換する必要があります。 `CategoryName`できない`NULL`値、RequiredFieldValidator を追加する必要があります。 および`Description` ボックスには、複数行テキスト ボックスに変換するか可能性があります。 課題としてこれら仕上げから退出です。


## <a name="summary"></a>概要

このチュートリアルでは、バイナリ データの操作で、外観を完了します。 このチュートリアルと、前の 3 つでどのようにバイナリ データを説明しましたファイル システム上またはデータベース内に直接格納できます。 ユーザーは、ハード ディスク ドライブからファイルを選択して、ファイル システムに格納されているか、データベースに挿入、web サーバーにアップロードすることによってシステムにバイナリ データを提供します。 ASP.NET 2.0 には、ドラッグ アンド ドロップ簡単このようなインターフェイスを提供する、ファイルアップロード コントロールが含まれます。 ただし、前述の[ファイルのアップロード](uploading-files-vb.md)チュートリアルでは、ファイルアップロード コントロールがのみ 1 メガバイトを超えていないことをお勧め、比較的小さいファイルのアップロードに適してします。 編集し、既存のレコードからバイナリ データを削除する方法と、基になるデータ モデルにアップロードされたデータを関連付ける方法も調査します。

[次へ]、一連のチュートリアルでは、さまざまなキャッシュ手法について説明します。 キャッシュでは、アプリケーション s を改善するための手段を負荷の高い操作の結果を取得して、迅速にアクセスできる場所に格納することで、全体的なパフォーマンスです。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Teresa マーフィーしました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](including-a-file-upload-option-when-adding-a-new-record-vb.md)
