---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: 新しいレコード (VB) を追加するときにオプションをアップロードするファイルを含む |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ユーザーがテキスト データを入力して、バイナリ ファイルをアップロードできる Web インターフェイスを作成する方法を示します。 オプションの使用可能な t をについて説明しています.
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: eac86318136d6f28ecf6551b8b8a6b691a8969d7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807753"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>新しいレコード (VB) を追加するときに、ファイル アップロード オプションを含める
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe)または[PDF のダウンロード](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> このチュートリアルでは、ユーザーがテキスト データを入力して、バイナリ ファイルをアップロードできる Web インターフェイスを作成する方法を示します。 バイナリ データを格納するためのオプションを示すためにはファイル システムに格納されている場合は、その他に 1 つのファイル データベースに保存されます。


## <a name="introduction"></a>はじめに

FileUpload コントロールを使用して、web サーバーをクライアントからファイルを送信し、データ W でこのバイナリ データを表示する方法を説明する方法を見て、アプリケーションのデータ モデルに関連付けられているバイナリ データを格納するための手法について学習しました前の 2 つのチュートリアルeb コントロール。 私たちは、データ モデルにアップロードされたデータを関連付ける方法について説明するには、まだ ve します。

このチュートリアルでは、新しいカテゴリを追加する web ページを作成します。 だけでなく、カテゴリの名前と説明のテキスト ボックスは、このページは、2 つの FileUpload コントロールの新しいカテゴリの画像やパンフレットのいずれかを含める必要があります。 アップロードされた画像は、新しいレコード s に直接格納されます`Picture`列、パンフレットに保存されますが、 `~/Brochures` s の新しいレコードに保存されているファイルへのパスを持つフォルダー`BrochurePath`列。

この新しい web ページを作成する前に、アーキテクチャを更新する必要になります。 `CategoriesTableAdapter` S メイン クエリを取得することはありません、`Picture`列。 その結果、自動生成された`Insert`メソッドのみの入力には、 `CategoryName`、 `Description`、および`BrochurePath`フィールド。 したがって、4 つすべての入力を求めるは TableAdapter の追加のメソッドを作成する必要があります`Categories`フィールド。 `CategoriesBLL`ビジネス ロジック層のクラスは、更新する必要があります。

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>手順 1: 追加、`InsertWithPicture`メソッドを`CategoriesTableAdapter`

作成したときに、`CategoriesTableAdapter`に戻り、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-vb.md)チュートリアルでは、構成が自動的に生成`INSERT`、 `UPDATE`、および`DELETE`ステートメントは、メインのクエリに基づきます。 さらに、使用、DB の直接的な方法は、メソッドを作成する TableAdapter よう`Insert`、 `Update`、および`Delete`します。 自動生成されたこれらのメソッドの実行`INSERT`、 `UPDATE`、および`DELETE`ステートメントと、その結果、メインのクエリによって返される列に基づいて入力パラメーターを受け入れます。 [のファイルのアップロード](uploading-files-vb.md)を補強するチュートリアル、`CategoriesTableAdapter`メイン クエリを使用する、`BrochurePath`列。

以降、 `CategoriesTableAdapter` s メイン クエリを参照していません、`Picture`列で、新しいレコードの追加も、値を持つ既存のレコードを更新したことができます、`Picture`列。 この情報をキャプチャするにはを作成する新しいメソッドは具体的には、バイナリ データでレコードを挿入するために使用する TableAdapter のか、自動生成をカスタマイズして`INSERT`ステートメント。 問題は、自動生成をカスタマイズする`INSERT`ステートメントは、カスタマイズがウィザードによって上書きされることにリスクいます。 たとえば、カスタマイズ、`INSERT`ステートメントの使用を含むように、`Picture`列。 これは、tableadapter を更新`Insert`メソッドをカテゴリの画像のバイナリ データの追加の入力パラメーターが含まれます。 この DAL メソッドを使用して、プレゼンテーション層では、この BLL メソッドを呼び出すビジネス ロジック層のメソッドを作成する可能性がありますし、すばらしく動作させる. 次の時間まで TableAdapter 構成ウィザードを使って、TableAdapter を構成します。 ウィザードが完了したら、当社のカスタマイズとすぐに、`INSERT`ステートメントが上書きされます、`Insert`メソッドは、古い形式を元に戻すし、コードがコンパイルされなくなります。

> [!NOTE]
> この面倒な作業は、アドホック SQL ステートメントの代わりにストアド プロシージャを使用する場合以外の問題です。 今後のチュートリアルでは、データ アクセス層でのストアド プロシージャ、アドホック SQL ステートメントの代わりの使用を検証します。


この可能性を回避するために、自動生成された SQL ステートメントをカスタマイズするのではなく、頭痛の種、s、TableAdapter の新しいメソッドを代わりに作成を使用できます。 このメソッドは、名前付き`InsertWithPicture`の値を受け入れる、 `CategoryName`、 `Description`、`BrochurePath`と`Picture`列し、実行、`INSERT`ステートメントに新しいレコードをすべての 4 つの値を格納します。

型指定されたデータセットを開くし、デザイナーを右クリックし、`CategoriesTableAdapter`のヘッダー、コンテキスト メニューから追加のクエリを選択します。 これにより、TableAdapter クエリがデータベースにアクセスする方法を求めてでは、まずする TableAdapter クエリ構成ウィザードが起動します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 次の手順では、生成されるクエリの種類を要求します。 クエリを作成する新しいレコードを追加する re 経過、`Categories`テーブルを挿入を選択し、[次へ] をクリックします。


[![挿入オプションを選択します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**図 1**: 挿入オプションを選択します ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))。


ここで指定する必要があります、 `INSERT` SQL ステートメント。 ウィザードが自動的に提案を`INSERT`TableAdapter のメイン クエリに対応するステートメント。 ここで、s、`INSERT`を挿入するステートメント、 `CategoryName`、`Description`と`BrochurePath`値。 ステートメントを更新できるように、`Picture`列がスコープに含める、`@Picture`パラメーター、次のように。


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

ウィザードの最後の画面を使用して、新しい TableAdapter メソッドの名前を付けるよう求められます。 入力`InsertWithPicture`[完了] をクリックします。


[![新しい TableAdapter メソッド InsertWithPicture 名](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**図 2**: 新しい TableAdapter メソッド名前`InsertWithPicture`([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))。


## <a name="step-2-updating-the-business-logic-layer"></a>手順 2: ビジネス ロジック層を更新しています

先ほど作成した、DAL メソッドを呼び出す BLL メソッドを作成する必要がありますが、データ アクセス層に直接移動をバイパスするのではなく、プレゼンテーション層がビジネス ロジック層とやり取りする必要がありますのみため (`InsertWithPicture`)。 このチュートリアルでメソッドを作成、`CategoriesBLL`という名前のクラス`InsertWithPicture`3 つの入力として受け取る`String`s と`Byte`配列。 `String`入力パラメーターは、カテゴリの名前、説明、およびパンフレット ファイルのパス中に、`Byte`アレイがカテゴリの画像のバイナリ コンテンツ。 次のコードに示す、この BLL メソッドは、対応する DAL メソッドを呼び出します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> 追加する前に型指定されたデータセットを保存するかどうかを確認、 `InsertWithPicture` BLL にメソッド。 以降、`CategoriesTableAdapter`クラス コードが自動生成された、型指定されたデータセットに基づいて t don は、型指定されたデータセットに変更を保存する最初の`Adapter`t が勝利したプロパティが認識、`InsertWithPicture`メソッド。


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>手順 3: 既存のカテゴリとそのバイナリ データを一覧表示します。

このチュートリアルでは、ページにより、システムに新しいカテゴリを追加するには、エンドユーザーを作成し、新しいカテゴリの画像とパンフレットを提供することがいます。 [前のチュートリアル](displaying-binary-data-in-the-data-web-controls-vb.md)TemplateField ImageField と各カテゴリの名前を表示する GridView を使用説明、画像、およびそのパンフレットをダウンロードするリンク。 このチュートリアルですべての既存カテゴリ一覧が表示され、新しいものを作成するのには、両方のページを作成する機能を複製して使用できます。

開いて開始、`DisplayOrDownload.aspx`ページから、`BinaryData`フォルダー。 ソース ビューに移動し、GridView コントロールと ObjectDataSource s 宣言構文内で貼り付けることをコピー、`<asp:Content>`要素`UploadInDetailsView.aspx`します。 また、don t もご活用ください経由でコピーする、`GenerateBrochureLink`の分離コード クラスからメソッド`DisplayOrDownload.aspx`に`UploadInDetailsView.aspx`。


[![コピーして貼り付ける UploadInDetailsView.aspx に DisplayOrDownload.aspx から宣言構文](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**図 3**: をコピーしてから宣言型構文貼り付けます`DisplayOrDownload.aspx`に`UploadInDetailsView.aspx`([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))。


宣言の構文をコピーした後、`GenerateBrochureLink`経由でメソッドを`UploadInDetailsView.aspx` ページで、すべてが正しくコピーされたことを確実にブラウザーからページを表示します。 カテゴリの画像と同様に、パンフレットをダウンロードするリンクを含む 8 つのカテゴリを一覧表示する GridView が表示されます。


[![各カテゴリのバイナリ データと共に表示されます。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**図 4**: する必要がありますようになりました「各カテゴリに沿ってのバイナリ データを ([フルサイズ画像を表示する をクリックします。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>手順 4: 構成、`CategoriesDataSource`サポートを挿入するには

`CategoriesDataSource` ObjectDataSource を使用して、 `Categories` GridView が現在のデータを挿入する機能では行いません。 このデータ ソース コントロールを通じて挿入をサポートするためにマップします必要があります。 その`Insert`メソッドを、その基になるオブジェクトのメソッドに`CategoriesBLL`。 具体的にマップする、`CategoriesBLL`メソッドが戻る手順 2. で追加された`InsertWithPicture`します。

ObjectDataSource s のスマート タグからのデータ ソースの構成のリンクをクリックして開始します。 最初の画面は、データ ソースが使用すると、連携するよう構成オブジェクトを示しています。`CategoriesBLL`します。 このような設定のままに-は、高度なデータ メソッドの定義の画面を横にあるをクリックします。 [挿入] タブに移動し、選択、`InsertWithPicture`ドロップダウン リストからメソッド。 ウィザードを完了するには、[完了] をクリックします。


[![ObjectDataSource InsertWithPicture メソッドを使用して構成します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`InsertWithPicture`メソッド ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))。


> [!NOTE]
> ウィザードを完了すると、Visual Studio はキーおよびフィールドの更新する場合は、Web のデータが再生成するフィールドを制御を依頼することがあります。 [はい] を選択した、フィールドのカスタマイズが上書きされますので、いいえを選択します。


ObjectDataSource がの値を加わりましたウィザードを完了すると、その`InsertMethod`プロパティだけでなく`InsertParameters`4 つのカテゴリ列の場合、次の宣言型マークアップとして示しています。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>手順 5: 挿入のインターフェイスを作成します。

最初に説明、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)、DetailsView コントロールの挿入をサポートするデータ ソース コントロールを使用する場合に使用できる組み込み挿入インターフェイスを提供します。 S の新しいカテゴリをすばやく追加するユーザーの挿入のインターフェイスを完全に表示する GridView の上には、このページに DetailsView コントロールを追加することができます。 DetailsView で新しいカテゴリを追加すると、その下にある GridView が自動更新を新しいカテゴリが表示されます。

上記の設定、GridView、デザイナーには、ツールボックスから、DetailsView をドラッグすることにより、その`ID`プロパティを`NewCategory`を消去して、`Height`と`Width`プロパティの値。 DetailsView s のスマート タグから、既存バインド`CategoriesDataSource`し挿入を有効にする チェック ボックスを確認します。


[![DetailsView を CategoriesDataSource にバインドし、挿入を有効にします。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**図 6**: バインドに DetailsView、`CategoriesDataSource`と挿入を有効にする ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))。


挿入インターフェイスに DetailsView を完全にレンダリングする次のように設定します。 その`DefaultMode`プロパティを`Insert`します。

DetailsView は 5 つ BoundFields `CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath`ですが、`CategoryID`ため BoundField は挿入インターフェイスに表示されません、 `InsertVisible`プロパティに設定されて`False`します。 によって返される列であるために、これらの BoundFields が存在する、`GetCategories()`メソッド、ObjectDataSource がそのデータを取得する呼び出します。 挿入すると、ただし、わけで、t の値を指定できるようにする`NumberOfProducts`します。 さらに、パンフレットの PDF をアップロードするほか、新しいカテゴリの画像をアップロードすることを許可する必要があります。

削除、 `NumberOfProducts` DetailsView を全体と、更新プログラムから BoundField、`HeaderText`のプロパティ、`CategoryName`と`BrochurePath`BoundFields カテゴリとパンフレットにそれぞれします。 次に、変換、`BrochurePath`を TemplateField BoundField、この新しい TemplateField の付与、画像の新しい TemplateField を追加、`HeaderText`画像の値。 移動、 `Picture` TemplateField 間なるように、 `BrochurePath` TemplateField および CommandField します。


![DetailsView を CategoriesDataSource にバインドし、挿入を有効にします。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**図 7**: バインドに DetailsView、`CategoriesDataSource`と挿入を有効にします。


変換する場合、`BrochurePath`をフィールドの編集 ダイアログ ボックスで TemplateField BoundField、TemplateField が含まれています、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`します。 のみ、`InsertItemTemplate`は必要に応じて、ただし、これを自由に他の 2 つのテンプレートを削除します。 この時点で、DetailsView s の宣言構文は、次のようになります。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>パンフレットと画像フィールドの FileUpload コントロールを追加します。

現在、 `BrochurePath` TemplateField s `InsertItemTemplate` 、テキスト ボックスが含まれています中に、 `Picture` TemplateField にテンプレートが含まれていません。 これら 2 つの TemplateField %s を更新する必要があります`InsertItemTemplate`FileUpload コントロールを使用します。

DetailsView s のスマート タグからのテンプレートの編集] オプションを選択し、[、 `BrochurePath` TemplateField の`InsertItemTemplate`ドロップダウン リストから。 テキスト ボックスを削除し、テンプレートに、ツールボックスから FileUpload コントロールをドラッグします。 FileUpload コントロール s 設定`ID`に`BrochureUpload`します。 同様に、FileUpload コントロールを追加、 `Picture` TemplateField の`InsertItemTemplate`します。 この FileUpload コントロール s 設定`ID`に`PictureUpload`します。


[![FileUpload コントロールを後に追加します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**図 8**: FileUpload コントロールを追加、 `InsertItemTemplate` ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))。


これらの追加を行った後 TemplateField s の 2 つの宣言型構文になります。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

ユーザーが新しいカテゴリを追加するパンフレットおよび画像が正しいファイルの種類であることを確認します。 パンフレット、ユーザーは、PDF を指定する必要があります。 画像が、ユーザーがイメージ ファイルをアップロードする必要がありますが、許可*任意*ファイルまたは Gif、Jpg などの特定の種類のイメージ ファイルのみのイメージですか? 異なる種類のファイルを許容するために d を拡張しなければ、`Categories`この型を使用してクライアントに送信できるように、ファイルの種類をキャプチャする列を含めるようにスキーマ`Response.ContentType`で`DisplayCategoryPicture.aspx`します。 T たくないため、このような列があるが賢明ユーザー固有のイメージ ファイルの種類を提供するだけに制限します。 `Categories`テーブルの既存のイメージは、ビットマップが、Jpg は web 経由で提供されたイメージのより適切なファイル形式。

ユーザーが、正しくないファイルの種類をアップロードする場合は、挿入をキャンセルし、問題を示すメッセージを表示する必要があります。 DetailsView の下にあるラベル Web コントロールを追加します。 設定の`ID`プロパティを`UploadWarning`チェック ボックスをオフにその`Text`プロパティ、設定、`CssClass`警告にプロパティと`Visible`と`EnableViewState`プロパティを`False`。 `Warning`で CSS クラスが定義されている`Styles.css`大きな、赤、斜体、太字のフォントでテキストを表示します。

> [!NOTE]
> 理想的には、`CategoryName`と`Description`BoundFields TemplateFields とカスタマイズされたその挿入インターフェイスに変換されます。 `Description`インターフェイスなどを挿入する可能性がありますにより適した複数行テキスト ボックスでします。 いるので、`CategoryName`列は受け入れません`NULL`値、ユーザーは、新しいカテゴリの名前の値を提供することを確認する、RequiredFieldValidator を追加する必要があります。 次の手順は、リーダーを演習として残されます。 再び参照[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)詳細については、データ変更インターフェイスの拡張にします。


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>手順 6: アップロードされたパンフレットを Web サーバーのファイル システムに保存します。

ユーザーは、新しいカテゴリの値を入力、[挿入] ボタンをクリックすると、ポストバックが発生して挿入のワークフロー。 まず、DetailsView s [ `ItemInserting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx)が起動します。 次に、ObjectDataSource s`Insert()`メソッドが呼び出される、その結果に追加される新しいレコード、`Categories`テーブル。 その後、DetailsView s [ `ItemInserted`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx)が起動します。

ObjectDataSource s 前に`Insert()`メソッドが呼び出されると、まず適切なファイルの種類がユーザーによってアップロードされたことを確認し、パンフレット PDF を web サーバーのファイル システムに保存する必要があります。 DetailsView s のイベント ハンドラーを作成`ItemInserting`イベントと、次のコードを追加します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

イベント ハンドラーを参照することで開始、 `BrochureUpload` DetailsView s のテンプレートから FileUpload コントロール。 次に、カタログがアップロードされた場合、アップロードされたファイルの拡張機能が検査されます。 場合は、拡張子はありません。PDF、し、警告が表示されます、取り消された場合は、insert、およびイベント ハンドラーの実行が終了します。

> [!NOTE]
> アップロードされたファイルの拡張機能の証明書利用者は、アップロードされたファイルが PDF ドキュメントであることを確認するいく手法ではありません。 ユーザーが拡張子を持つ有効な PDF ドキュメントがある可能性があります`.Brochure`、または非 PDF ドキュメントを取得、その可能性があります、`.pdf`拡張機能。 ファイル %s のバイナリ コンテンツより増やしたファイルの種類を確認するためにプログラムで調査する必要があります。 このような徹底的なアプローチでは、ただしは大げさすぎる場合です。拡張機能のチェックは、ほとんどのシナリオに対して十分です。


説明したように、[のファイルのアップロード](uploading-files-vb.md)チュートリアルでは、1 人のユーザーのアップロードにもう 1 つの s が上書きされないように、ファイル システムにファイルの保存時に、注意を取得する必要があります。 このチュートリアルでは、アップロードされたファイルと同じ名前を使用することを試みます。 内のファイルが既に存在する場合、`~/Brochures`ディレクトリに同じファイル名をただしが追加され、末尾に数値一意の名前が見つかるまでです。 たとえば、パンフレットという名前のファイルをユーザーがアップロード`Meats.pdf`、という名前のファイルが既に存在するが`Meats.pdf`で、`~/Brochures`フォルダーに保存されているファイルの名前変更します`Meats-1.pdf`。 存在する場合を見ていきます`Meats-2.pdf`で、一意のファイル名が見つかるまでです。

次のコードでは、 [ `File.Exists(path)`メソッド](https://msdn.microsoft.com/library/system.io.file.exists.aspx)に指定したファイル名のファイルが既に存在するかどうかは確認します。 そうである場合、競合が検出されないまでパンフレットの新しいファイル名を続行します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

ファイルをファイル システムと ObjectDataSource s を保存する必要が有効なファイル名が見つかった後、`brochurePath``InsertParameter`値は、このファイル名がデータベースに書き込まれるように更新する必要があります。 説明したように、*のファイルのアップロード*チュートリアルでは、ファイルを保存 s FileUpload コントロールを使用して`SaveAs(path)`メソッド。 ObjectDataSource %s を更新する`brochurePath`パラメーターを使用して、`e.Values`コレクション。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>手順 7: データベースにアップロードされた画像を保存しています

新しいにアップロードされた画像を格納する`Categories`レコード、必要がありますを ObjectDataSource にアップロードしたバイナリ コンテンツを割り当てる`picture`DetailsView s パラメーター`ItemInserting`イベント。 ただし、この割り当てを行った前に、最初にアップロードされた画像は、JPG としないその他のイメージの種類を確認する必要があります。 手順 6 のようにその型を確認するために、アップロードされた画像のファイル拡張子を使用して、s ことができます。

中に、`Categories`テーブルでは、`NULL`の値を`Picture`画像がある列で、現在のすべてのカテゴリ。 S が、ユーザー、このページを通じて新しいカテゴリを追加するときに、画像を提供することができます。 次のコードは、画像がアップロードされていると、適切な拡張機能を使用していることを確認することを確認します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

このコードを配置する必要があります*する前に*手順 6 のコード パンフレット ファイルは、ファイル システムに保存する前に画像のアップロードで問題がある場合、イベント ハンドラーは終了できるようにします。

適切なファイルがアップロードされたと仮定は、次のコード行で画像パラメーターの値にアップロードしたバイナリ コンテンツを割り当てます。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>完全な`ItemInserting`イベント ハンドラー

完全を期すため、ここでは、`ItemInserting`全体のイベント ハンドラー。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>手順 8: 修正、`DisplayCategoryPicture.aspx`ページ

Let s は、挿入のインターフェイスをテストする少しと`ItemInserting`いくつかの手順が最後に作成されたイベント ハンドラー。 参照してください、`UploadInDetailsView.aspx`しようと、カテゴリを追加するが、画像を省略すると、ブラウザー内でページまたは非 JPG 画像または PDF 以外パンフレットを指定します。 このような場合も、エラー メッセージが表示されます、insert のワークフローが取り消されました。


[![警告メッセージが表示される場合、無効な種類のファイルのアップロード](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**図 9**: A 警告メッセージが表示される場合、無効な種類のファイルのアップロード ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))。


1 回、確認すること、ページが画像をアップロードする必要があります受注 t PDF 以外または非 JPG ファイルを受け入れる、有効な JPG 画像の新しいカテゴリを追加パンフレット フィールドを空のまま。 [挿入] ボタンをクリックした後、ページがポストバックをし、新しいレコードに追加されます、`Categories`データベースに直接格納されているアップロードされた画像のバイナリ コンテンツを含むテーブル。 GridView が更新され、新しく追加されたカテゴリの行を示していますが、図 10 に示す、新しいカテゴリの画像が正しくレンダリングされません。


[![新しいカテゴリの画像は表示されません。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**図 10**: 画像が表示されていない、新しいカテゴリ s ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))。


新しい画像が表示されない理由は、`DisplayCategoryPicture.aspx`ページを指定したカテゴリの画像を返す、OLE ヘッダーを持つビットマップを処理するように構成します。 このヘッダーの 78 バイトがから取り除かれます、`Picture`クライアントに送信される前に、列のバイナリ コンテンツをバックアップします。 アップロードした新しいカテゴリの JPG ファイルには、この OLE ヘッダーはありません。そのため、画像のバイナリ データから必要な有効なバイトを削除しています。

今すぐ OLE ヘッダーと Jpg で両方のビットマップがあるので、`Categories`テーブルを更新する必要があります`DisplayCategoryPicture.aspx`OLE ヘッダーは、元の 8 つのカテゴリを削除し、この新しいカテゴリのレコードの削除をバイパスするようにします。 次のチュートリアルで、既存のレコードのイメージを更新する方法を説明し、Jpg ように古いカテゴリ画像がすべて更新します。 ここでは、次のコードを使用して、`DisplayCategoryPicture.aspx`その元の 8 つのカテゴリに対してのみ OLE ヘッダーを削除します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

この変更により、JPG イメージにレンダリングされます正しく GridView。


[![新しいカテゴリの JPG イメージは正しくレンダリング](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**図 11**: の新しいカテゴリの JPG イメージは正しくレンダリング ([フルサイズの画像を表示する をクリックします](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))。


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>手順 9: 例外が発生した場合、パンフレットを削除します。

バイナリ データを格納する web サーバーのファイル システム上の課題の 1 つは、データ モデルとそのバイナリ データの間の切断が発生します。 そのため、レコードが削除されたときに、ファイル システムに対応するバイナリ データも削除にする必要があります。 これも挿入するときに取得できます。 次のシナリオを検討してください: ユーザーが有効な画像とパンフレットを指定するという新しいカテゴリを追加します。 ポストバックの発生に [挿入] ボタンをクリックすると DetailsView の`ItemInserting`イベントが起動され、パンフレットを web サーバーのファイル システムに保存します。 次に、ObjectDataSource s`Insert()`を呼び出し、メソッドが呼び出される、`CategoriesBLL`クラス s`InsertWithPicture`メソッドを呼び出す、 `CategoriesTableAdapter` s`InsertWithPicture`メソッド。

ここでは、データベースがオフラインの場合の動作やでエラーがあるかどうか、 `INSERT` SQL ステートメントか? 明らかに、挿入は失敗、新しいカテゴリの行をデータベースに追加しないようにします。 Web サーバーのファイル システム上に座ってパンフレットのアップロードされたファイル、まだしました! このファイルは、挿入のワークフロー中に例外が発生した場合に削除する必要があります。

既に説明したとおり、[処理 BLL - と DAL レベルの例外で、ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアルでは、さまざまな階層を通じてバブルアップされ、そのアーキテクチャの内から例外がスローされたときにします。 プレゼンテーション層に DetailsView s から、例外が発生したかどうかを判断できます`ItemInserted`イベント。 このイベント ハンドラーは、ObjectDataSource 秒の値も用意されています。`InsertParameters`します。 そのため、イベント ハンドラーを作成できます、`ItemInserted`そうである場合、例外が発生した場合にチェックするイベントは、ObjectDataSource 秒で指定されたファイルを削除します。`brochurePath`パラメーター。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>まとめ

バイナリ データを含むレコードを追加するための web ベースのインターフェイスを提供するために必要な手順を数多くあります。 バイナリ データが直接データベースに格納される場合は、バイナリ データが挿入されているケースを処理する特定のメソッドを追加する、アーキテクチャを更新する必要あります可能性があります。 アーキテクチャが更新されたら、次の手順は、バイナリ データの各フィールドの FileUpload コントロールに含めるカスタマイズされた DetailsView を使用して実現可能挿入インターフェイスを作成します。 アップロードされたデータは、web サーバーのファイル システムに保存または DetailsView s でデータ ソースのパラメーターに割り当てられた`ItemInserting`イベント ハンドラー。

ファイル システムにバイナリ データを保存するには、直接データベースにデータを保存するよりも詳細な計画が必要です。 名前付けスキームは、もう 1 つの s を上書きする 1 つのユーザーのアップロードを回避するために選択する必要があります。 また、余分な手順を実行して、データベースの挿入が失敗した場合は、アップロードされたファイルを削除する必要があります。

あるパンフレットと画像、ですがシステムに新しいカテゴリを追加する機能を既存のカテゴリのバイナリ データを更新する方法または削除されたカテゴリのバイナリ データを正しく削除する方法を確認するには、まだ ve します。 次のチュートリアルではこれら 2 つのトピックについて説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dave Gardner、Teresa Murphy、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](displaying-binary-data-in-the-data-web-controls-vb.md)
> [次へ](updating-and-deleting-existing-binary-data-vb.md)
