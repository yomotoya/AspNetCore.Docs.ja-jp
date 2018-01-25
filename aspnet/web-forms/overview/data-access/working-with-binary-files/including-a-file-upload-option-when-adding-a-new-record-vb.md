---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: "新しいレコード (VB) を追加するときにオプションをアップロードするファイルを含む |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、両方のテキスト データを入力し、バイナリ ファイルをアップロードするユーザーができる Web インターフェイスを作成する方法を示します。 オプションの使用可能な t について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: eb462a0e8ce88037855ea12d00c1afc0419fa04e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>新しいレコード (VB) を追加するときに、ファイルのアップロード オプションを含む
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe)または[PDF のダウンロード](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> このチュートリアルでは、両方のテキスト データを入力し、バイナリ ファイルをアップロードするユーザーができる Web インターフェイスを作成する方法を示します。 バイナリ データの格納に使用できるオプションを示すためはファイル システムに格納されている場合は、他に 1 つのファイル、データベースに保存されます。


## <a name="introduction"></a>はじめに

前の 2 つのチュートリアルではため、探索ファイルアップロード コントロールを使用して、クライアントから web サーバーにファイルを送信および W のデータにこのバイナリ データを表示する方法を説明する方法を見て、アプリケーションのデータ モデルに関連付けられているバイナリ データを格納するための手法eb コントロールです。 おただし、データ モデルにアップロードされたデータを関連付ける方法について説明するには、まだしました。

このチュートリアルでは、新しいカテゴリを追加する web ページを作成します。 だけでなく、カテゴリの名前と説明のテキスト ボックスは、このページは、新しいカテゴリの図用の 1 つの 2 つのファイルアップロード コントロールとパンフレットのいずれかを含める必要があります。 アップロードされた画像は、新しいレコード s に直接格納されます`Picture`列、パンフレットに保存されますが、 `~/Brochures` s の新しいレコードに保存されているファイルへのパスを持つフォルダー`BrochurePath`列です。

この新しい web ページを作成する前に、アーキテクチャを更新する必要になります。 `CategoriesTableAdapter` S メイン クエリを取得することはありません、`Picture`列です。 その結果の自動生成された`Insert`メソッドは入力のみが、 `CategoryName`、 `Description`、および`BrochurePath`フィールドです。 したがって、4 つすべての入力を求める TableAdapter に追加のメソッドを作成する必要があります`Categories`フィールドです。 `CategoriesBLL`ビジネス ロジック層内のクラスを更新する必要があります。

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>手順 1: 追加、`InsertWithPicture`メソッドを`CategoriesTableAdapter`

作成したときに、`CategoriesTableAdapter`に戻り、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)チュートリアルでは、構成が自動的に生成`INSERT`、 `UPDATE`、および`DELETE`ステートメントは、メインのクエリに基づきます。 DB の直接的な方法は、メソッドの作成を採用する TableAdapter の指示はさらに、 `Insert`、 `Update`、および`Delete`です。 これらのメソッドを自動生成された実行`INSERT`、 `UPDATE`、および`DELETE`ステートメントと、その結果、メインのクエリによって返される列に基づいて入力パラメーターを受け入れます。 [ファイルのアップロード](uploading-files-vb.md)補強したチュートリアル、`CategoriesTableAdapter`メイン クエリを使用する、`BrochurePath`列です。

以降、 `CategoriesTableAdapter` s メイン クエリを参照していません、`Picture`列、おできる新しいレコードを追加でも値を持つ既存のレコードを更新、`Picture`列です。 この情報をキャプチャするのにを作成する新しいメソッドはバイナリ データを持つレコードを挿入するために使用する TableAdapter のかカスタマイズして、自動生成された`INSERT`ステートメントです。 自動生成のカスタマイズは、問題`INSERT`ステートメントがあるおリスクが、カスタマイズ、ウィザードによって上書きされます。 たとえば、カスタマイズして、`INSERT`ステートメントを使用している、`Picture`列です。 これは、tableadapter を更新`Insert`カテゴリ s s の画像のバイナリ データの追加の入力パラメーターを含める方法です。 この DAL メソッドを使用して、プレゼンテーション層を介してこの BLL メソッドを呼び出すビジネス ロジック層でメソッドを作成おでしたし、すばらしく動作させます。 次の時間まで TableAdapter 構成ウィザードを使って、TableAdapter を構成します。 ウィザードが完了したら、当社のカスタマイズとすぐに、`INSERT`ステートメントが上書きされます、`Insert`メソッドは、以前の形式を元に戻すし、コードがコンパイルされなくなります。

> [!NOTE]
> この面倒な作業は、アドホック SQL ステートメントではなくストアド プロシージャを使用する場合以外の問題です。 今後のチュートリアルは、データ アクセス レイヤーでアドホック SQL ステートメントを使用せずにストアド プロシージャを使用して調査します。


この可能性を回避するには、自動生成された SQL ステートメントをカスタマイズするのではなく、頭痛の種でした、s、TableAdapter の新しいメソッドを代わりに作成を使用できます。 このメソッドは、名前付き`InsertWithPicture`の値を受け入れる、 `CategoryName`、 `Description`、`BrochurePath`と`Picture`列を実行し、`INSERT`新しいレコードに 4 つすべての値を格納するステートメント。

型指定されたデータセットを開くし、デザイナーを右クリックし、`CategoriesTableAdapter`のヘッダーとクエリの追加、コンテキスト メニューをクリックします。 これは、TableAdapter クエリがデータベースにアクセスする方法を求めてで開始する TableAdapter クエリ構成ウィザードを起動します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 次の手順は、生成されるクエリの種類に求めます。 クエリを作成する新しいレコードを追加する re お以降、`Categories`テーブルを挿入を選択し、[次へ] をクリックします。


[![挿入オプションを選択します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**図 1**: 挿入オプションを選択 ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


ここで指定する必要があります、 `INSERT` SQL ステートメント。 ウィザードが自動的に提案、 `INSERT` TableAdapter のメインのクエリに対応するステートメント。 ここで、s、`INSERT`を挿入するステートメント、 `CategoryName`、`Description`と`BrochurePath`値。 ステートメントを更新できるように、`Picture`列がと共に含まれる、`@Picture`パラメーター、次のようにします。


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

ウィザードの最終画面では、新しい TableAdapter メソッドの名前を付けることが求められます。 入力`InsertWithPicture`[完了] をクリックします。


[![新しい TableAdapter メソッド InsertWithPicture の名前](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**図 2**: 新しい TableAdapter メソッド名前`InsertWithPicture`([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>手順 2: ビジネス ロジック層を更新します。

先ほど作成した DAL メソッドを呼び出す BLL メソッドを作成する必要があります、データ アクセス層に直接移動することをバイパスするのではなく、プレゼンテーション層がビジネス ロジック層とやり取りする必要がありますのみため (`InsertWithPicture`)。 このチュートリアルでメソッドを作成、`CategoriesBLL`という名前のクラス`InsertWithPicture`3 つの入力として受け取る`String`s と`Byte`配列。 `String`入力パラメーターは、カテゴリの名前、説明、およびパンフレット ファイルのパス中に、`Byte`配列は、カテゴリの画像のバイナリ コンテンツ。 次のコードに示すこの BLL メソッドは、対応する DAL メソッドを呼び出します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> 追加する前に型指定されたデータセットを保存してあることを確認してください、 `InsertWithPicture` BLL メソッドです。 以降、`CategoriesTableAdapter`クラス コードが自動生成された、型指定されたデータセットに基づいて、表示されない最初の変更を保存、型指定されたデータセット、 `Adapter` t が勝利したプロパティは次のトピック、`InsertWithPicture`メソッドです。


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>手順 3: 既存のカテゴリとバイナリ データを一覧表示します。

このチュートリアルでは、ページにより、システムに新しいカテゴリを追加するには、エンドユーザーを作成し、新しいカテゴリの画像とパンフレットを提供することはおです。 [前のチュートリアル](displaying-binary-data-in-the-data-web-controls-vb.md)TemplateField ImageField と各カテゴリの名前を表示する GridView を使用しました説明、画像、およびそのパンフレットをダウンロードするリンクです。 すべての既存カテゴリの一覧し、新規に作成するのには、両方のページを作成するこのチュートリアルでは、その機能をレプリケート s を使用できます。

開いて開始、`DisplayOrDownload.aspx`ページから、`BinaryData`フォルダーです。 ソース ビューに移動し、GridView と ObjectDataSource s 宣言の構文をコピー、内で貼り付けることにより、`<asp:Content>`内の要素`UploadInDetailsView.aspx`です。 また、しないことを忘れがちに経由でコピー、`GenerateBrochureLink`の分離コード クラスのメソッドの`DisplayOrDownload.aspx`に`UploadInDetailsView.aspx`です。


[![コピーして貼り付ける UploadInDetailsView.aspx に DisplayOrDownload.aspx から宣言の構文](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**図 3**: コピーしてから、宣言の構文を貼り付ける`DisplayOrDownload.aspx`に`UploadInDetailsView.aspx`([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


宣言の構文をコピーした後、`GenerateBrochureLink`経由でメソッドを`UploadInDetailsView.aspx` ページで、すべてのものが正しくコピーされたことを確認するブラウザーでページを表示します。 パンフレットだけでなくカテゴリの画像をダウンロードするリンクを含む 8 つのカテゴリを一覧表示する GridView が表示されます。


[![各カテゴリのバイナリ データと共に表示されます。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**図 4**: する必要がありますようになりました「各カテゴリ沿ったのバイナリ データを ([フルサイズのイメージを表示するには、をクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>ステップ 4: 構成、`CategoriesDataSource`サポートを挿入するには

`CategoriesDataSource` ObjectDataSource を使用して、 `Categories` GridView が現在データを挿入する機能では行いません。 このデータ ソース コントロールからの挿入をサポートするためにマップするお必要があります。 その`Insert`その基になるオブジェクトのメソッドにメソッド`CategoriesBLL`です。 具体的にマップする、`CategoriesBLL`メソッドのステップ 2 で再び追加お`InsertWithPicture`です。

ObjectDataSource s のスマート タグからのデータ ソースの構成のリンクをクリックして開始します。 最初の画面は、データ ソースを構成すると、オブジェクトを示しています。`CategoriesBLL`です。 としてこの設定をそのままで、およびデータ メソッドの定義の画面に詳細の横にあるをクリックします。 [挿入] タブに移動し、選択、`InsertWithPicture`ドロップダウン リストからメソッドです。 ウィザードを完了するには、[完了] をクリックします。


[![ObjectDataSource InsertWithPicture メソッドを使用して構成します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`InsertWithPicture`メソッド ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> ウィザードを完了すると、Visual Studio を要求するフィールドの更新およびキーにする場合は、Web のデータが再生成するフィールドを制御します。 はい を選択すると、フィールドが加えたカスタマイズは上書きためいいえ を選択します。


ウィザードの完了後に、ObjectDataSource は今すぐの値を含めるその`InsertMethod`プロパティだけでなく`InsertParameters`の 4 つのカテゴリ列として、次の宣言型マークアップを示しています。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>手順 5: 挿入インターフェイスの作成

最初に、説明、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)、DetailsView コントロールの挿入をサポートするデータ ソース コントロールを操作するときに使用できる組み込みの挿入インターフェイスを提供します。 S DetailsView コントロールを簡単に新しいカテゴリを追加するユーザーを許可する、挿入するインターフェイスを完全に表示する GridView の上には、このページを追加できるようにします。 DetailsView で新しいカテゴリを追加したときにその下位にある GridView は自動的に更新し、新しいカテゴリを表示します。

DetailsView を設定、GridView 上のデザイナーには、ツールボックスからドラッグして開始その`ID`プロパティを`NewCategory`を消去して、`Height`と`Width`プロパティの値。 DetailsView s のスマート タグからにバインドして、既存の`CategoriesDataSource`し、挿入を有効にする チェック ボックスを確認します。


[![DetailsView を CategoriesDataSource にバインドし、挿入を有効にします。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**図 6**: を DetailsView のバインド、`CategoriesDataSource`と挿入を有効にする ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


挿入するインターフェイスに DetailsView を完全に表示するには、するために次のように設定します。 その`DefaultMode`プロパティを`Insert`です。

DetailsView に 5 つ BoundFields 注`CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath`ですが、`CategoryID`挿入インターフェイスでは、ため BoundField はレンダリングされませんその`InsertVisible`プロパティに設定されている`False`です。 によって返される列であるために、これらの BoundFields が存在する、`GetCategories()`メソッドを ObjectDataSource の起動をそのデータを取得します。 挿入すると、ただし、たくないユーザーが値を指定できるようにする`NumberOfProducts`です。 さらに、新しいカテゴリの画像のアップロードし、パンフレットの PDF をアップロードできるようにする必要があります。

削除、 `NumberOfProducts` DetailsView を完全とし、更新プログラムから BoundField、`HeaderText`のプロパティ、`CategoryName`と`BrochurePath`BoundFields カテゴリとパンフレットをそれぞれします。 次に、変換、`BrochurePath`を TemplateField BoundField この新しい TemplateField を与える、画像の新しい TemplateField を追加し、`HeaderText`画像の値。 移動、 `Picture` TemplateField 間ように、 `BrochurePath` TemplateField と CommandField です。


![DetailsView を CategoriesDataSource にバインドし、挿入を有効にします。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**図 7**: を DetailsView のバインド、`CategoriesDataSource`挿入を有効にし、


変換する場合、`BrochurePath`フィールドの編集 ダイアログ ボックスで TemplateField に BoundField、TemplateField が含まれています、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`です。 のみ、`InsertItemTemplate`は必要に応じて、ただし、制限はないので、他の 2 つのテンプレートを削除します。 この時点で DetailsView s の宣言構文は次のようになります。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>画像フィールドとパンフレットのファイルアップロード コントロールを追加します。

現在、 `BrochurePath` TemplateField s `InsertItemTemplate` 、テキスト ボックスを含む中に、 `Picture` TemplateField に任意のテンプレートが含まれていません。 これら 2 つの TemplateField %s を更新する必要があります`InsertItemTemplate`ファイルアップロード コントロールを使用しています。

DetailsView s のスマート タグからのテンプレートの編集 オプションを選択してから、 `BrochurePath` TemplateField の`InsertItemTemplate`ドロップダウン リストからです。 テキスト ボックスを削除し、テンプレートに、ツールボックスからファイルアップロード コントロールをドラッグします。 ファイルアップロード コントロール s 設定`ID`に`BrochureUpload`です。 同様に、ファイルアップロード コントロールを追加、 `Picture` TemplateField の`InsertItemTemplate`です。 このファイルアップロード コントロール s 設定`ID`に`PictureUpload`です。


[![後にファイルアップロード コントロールを追加します。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**図 8**: するファイルアップロード コントロールを追加、 `InsertItemTemplate` ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


これらの追加を行った後 TemplateField s の 2 つの宣言構文になります。


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

ユーザーが、新しいカテゴリを追加するパンフレットと画像が、正しいファイルの種類があることを確認します。 パンフレット、ユーザーは、PDF を指定する必要があります。 画像のユーザーが、イメージ ファイルをアップロードする必要がありますが、許可お*任意*画像のファイルまたは Gif、Jpg など、特定の種類のイメージ ファイルだけですか? 別のファイルの種類を許容するのには d 必要がありますを拡張する、`Categories`この型を使用してクライアントに送信することができるように、ファイルの種類をキャプチャする列を含めるようにスキーマ`Response.ContentType`で`DisplayCategoryPicture.aspx`です。 T たくないこのような列があるため、のみ型を提供する、特定のイメージ ファイルにユーザーを制限する賢明になります。 `Categories`テーブル %s の既存のイメージがビットマップがの Jpg イメージの場合、web 経由で提供された適切なファイル形式。

ユーザーが、正しくないファイルの種類をアップロードする場合、挿入をキャンセルし、問題を示すメッセージを表示する必要があります。 DetailsView の下にラベル Web コントロールを追加します。 設定の`ID`プロパティを`UploadWarning`をクリア、その`Text`プロパティを設定、`CssClass`が警告にプロパティと`Visible`と`EnableViewState`プロパティを`False`です。 `Warning`で CSS クラスが定義されている`Styles.css`大きな、赤、斜体、太字のフォントでテキストを表示します。

> [!NOTE]
> 理想的には、`CategoryName`と`Description`BoundFields TemplateFields とカスタマイズの挿入のインターフェイスに変換されます。 `Description`インターフェイスなどの挿入は可能性がありますより適切なを通じて複数行テキスト ボックス。 `CategoryName`列は受け入れません`NULL`値、ユーザーが、新しいカテゴリの名の値を提供することを確認する、RequiredFieldValidator を追加する必要があります。 次の手順は、演習として、リーダーに残されます。 参照[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)のデータ変更インターフェイスの拡張について詳しく説明します。


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>手順 6: アップロードされたパンフレットを Web サーバーのファイル システムに保存します。

ユーザーは、新しいカテゴリの値を入力、[挿入] ボタンをクリックすると、ポストバックが発生して挿入するワークフロー。 最初に、DetailsView s [ `ItemInserting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx)発生します。 次に、ObjectDataSource s`Insert()`メソッドが呼び出され、その結果は、新しいレコードに追加されている、`Categories`テーブル。 その後、DetailsView s [ `ItemInserted`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx)発生します。

ObjectDataSource s 前に`Insert()`メソッドが呼び出されるを適切なファイルの種類がユーザーによってアップロードされたことを最初に確認をしパンフレット PDF web サーバーのファイル システムに保存する必要があります。 DetailsView s のイベント ハンドラーを作成`ItemInserting`イベントし、次のコードを追加します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

参照することによって、イベント ハンドラーを開始、 `BrochureUpload` DetailsView のテンプレートからファイルアップロード コントロール。 次に、カタログがアップロードされた場合、アップロードされたファイルの拡張機能が検査されます。 拡張機能がない場合です。PDF、し、警告が表示されます、insert は取り消され、イベント ハンドラーの実行が終了します。

> [!NOTE]
> アップロードされたファイルの拡張機能に証明書利用者がアップロードされたファイルには、PDF ドキュメントを確保するための手法を確実です。 ユーザーが拡張子を持つ有効な PDF ドキュメントを持つでした`.Brochure`、または非 PDF ドキュメントを取得、その可能性があります、`.pdf`拡張機能です。 ファイル %s のバイナリ コンテンツは、確定複数ファイルの種類を確認するためにプログラムで調査する必要があります。 このような完全なアプローチも多くの場合、過剰です。拡張機能のチェックはほとんどのシナリオです。


説明したように、[ファイルのアップロード](uploading-files-vb.md)チュートリアルでは、注意する必要がある 1 人のユーザーのアップロードで別の s が上書きされないように、ファイル システムにファイルの保存時にします。 このチュートリアルでは、アップロードされたファイルと同じ名前を使用を試みます。 内のファイルが既に存在する場合、`~/Brochures`を同じファイル名で、ただし、ディレクトリおあります番号を追加、末尾に一意の名前が見つかるまでです。 たとえば、ユーザーが名前付きパンフレット ファイルをアップロード`Meats.pdf`、という名前のファイルが既に存在が`Meats.pdf`で、`~/Brochures`フォルダーおを保存するファイル名を変更します`Meats-1.pdf`です。 存在する場合を見ていきます`Meats-2.pdf`など、一意のファイル名が見つかるまでです。

次のコードでは、 [ `File.Exists(path)`メソッド](https://msdn.microsoft.com/library/system.io.file.exists.aspx)を指定したファイル名のファイルが既に存在するかどうかを判断します。 場合は、しようとパンフレットを新しいファイル名の競合が見つからなくなるまで続行します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

ファイルをファイル システムと ObjectDataSource s に保存する必要がある有効なファイル名が見つかった後、`brochurePath``InsertParameter`値は、このファイル名がデータベースに書き込まれるように更新する必要があります。 バックアップで示したように、*ファイルのアップロード*チュートリアルでは、ファイルを保存する s ファイルアップロード コントロールを使用して`SaveAs(path)`メソッドです。 ObjectDataSource %s を更新する`brochurePath`パラメーターを使用して、`e.Values`コレクション。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>手順 7: データベースにアップロードされた画像を保存します。

新たにアップロードされた画像を格納する`Categories`レコード、いただくために、ObjectDataSource s にアップロードされたバイナリ コンテンツを割り当てる`picture`DetailsView s パラメーター`ItemInserting`イベント。 ただし、この割り当てを行う場合、前に、最初にアップロードされた画像が、JPG およびいないその他のイメージの種類であることを確認する必要があります。 手順 6 のようにその型を確認するためにアップロードされた画像のファイル拡張子を使用して s を使用できます。

中に、`Categories`テーブルでは、`NULL`の値を`Picture`画像がある列で、現在のすべてのカテゴリ。 S 強制的にこのページで、新しいカテゴリを追加するときに、画像を提供するユーザーを使用できます。 次のコードは、画像がアップロードされていると、適切な拡張機能を使用していることを確認することを確認します。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

このコードを配置する必要があります*する前に*手順 6. からコード画像アップロード中に問題がある場合、イベント ハンドラーはパンフレット ファイルは、ファイル システムに保存する前に終了できるようにします。

を適切なファイルがアップロードされたと想定されるは、次のコード行で画像パラメーター s の値にアップロード済みのバイナリ コンテンツを割り当てます。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>完全な`ItemInserting`イベント ハンドラー

完全を期すのためここでは、`ItemInserting`全体のイベント ハンドラー。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>手順 8: 修正、`DisplayCategoryPicture.aspx`ページ

Let s をとって、挿入するインターフェイスをテストし、`ItemInserting`いくつかの手順が最後に作成されたイベント ハンドラー。 参照してください、`UploadInDetailsView.aspx`しようと、カテゴリを追加するが、画像を省略すると、ブラウザー内でページまたは非 JPG 画像または PDF 以外パンフレットを指定します。 このような場合も、エラー メッセージが表示され、挿入のワークフローが取り消されました。


[![警告メッセージが表示される場合は、無効なファイルの種類がアップロードされます。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**図 9**: A 警告メッセージが表示される場合は、無効なファイルの種類がアップロードされる ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


確認した後、ページ、画像をアップロードして受注 t PDF 以外または非 JPG ファイルを受け入れ、有効な JPG 図を使って新しいカテゴリを追加パンフレット フィールドを空のままです。 [挿入] ボタンをクリックすると、ページがポストバックをし、新しいレコードに追加されます、`Categories`データベースに直接格納されているアップロードされた画像のバイナリ コンテンツを含むテーブル。 GridView が更新され、新しく追加されたカテゴリに属している行を示していますが、図 10 では、新しいカテゴリの画像は正しく表示されません。


[![新しいカテゴリの画像が表示されていない s](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**図 10**: 画像が表示されていない新しいカテゴリ s ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


新しい画像が表示されないためにです、`DisplayCategoryPicture.aspx`いるビットマップを OLE ヘッダーを処理する、指定したカテゴリの画像を返すページが構成されています。 この 78 バイトのヘッダーはから削除されて、`Picture`送信される前に、の column のバイナリ コンテンツがクライアントにバックアップします。 新しいカテゴリの JPG ファイル アップロードしたには、この OLE ヘッダーはありません。そのため、画像のバイナリ データから必要な有効なバイトを削除しています。

今すぐ OLE ヘッダーとで Jpg を両方のビットマップがあるため、`Categories`テーブルを更新する必要があります`DisplayCategoryPicture.aspx`OLE ヘッダーは、元の 8 つのカテゴリを削除し、この新しいカテゴリ レコードの削除をバイパスできるようにします。 次のチュートリアルについて確認を既存のレコードのイメージを更新する方法とは、Jpg ようにすべての古いカテゴリ画像が更新されます。 ここでは、次のコードを使用して、`DisplayCategoryPicture.aspx`をその元の 8 つのカテゴリに対してのみ OLE ヘッダーを取り除きます。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

この変更により、JPG イメージは、今すぐ正しくレンダリング GridView でします。


[![新しいカテゴリの JPG イメージは正しくレンダリングされます。](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**図 11**: 新しいカテゴリの JPG イメージは正しくレンダリングされます ([フルサイズのイメージを表示するをクリックして](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>手順 9: 削除例外発生した場合にパンフレット

バイナリ データを格納する web サーバーのファイル システム上の課題の 1 つは、データ モデルとそのバイナリ データの間での切断が発生します。 そのため、レコードが削除されると、ファイル システムに対応するバイナリ データも削除してください。 これは、ことができますになるように挿入すると、同様にします。 次のシナリオを検討してください: ユーザーは有効な画像とパンフレットを指定する、新しいカテゴリを追加します。 ポストバックが発生した挿入 ボタンをクリックし、DetailsView s に`ItemInserting`パンフレットを web サーバーのファイル システムに保存のイベントが発生します。 次に、ObjectDataSource s`Insert()`メソッドが呼び出され、これを呼び出す、`CategoriesBLL`クラス s`InsertWithPicture`メソッドを呼び出して、 `CategoriesTableAdapter` s`InsertWithPicture`メソッドです。

ここで、データベースがオフラインの場合の動作またはでエラーがあるかどうか、 `INSERT` SQL ステートメントですか? 明確に、挿入は失敗し、ため、データベースに新しいカテゴリの行は追加されません。 Web サーバーのファイル システム上にアップロードされたパンフレット ファイルがまだある! このファイルは、挿入するワークフローの間に例外の発生時に削除する必要があります。

既に説明したとおり、[処理 BLL-DAL レベルでの例外および ASP.NET ページ](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)チュートリアルでは、さまざまな層を通じたをバブル イベント アーキテクチャの深さから例外がスローされます。 プレゼンテーション層で DetailsView s から、例外が発生したかどうかを判断できます`ItemInserted`イベント。 このイベント ハンドラーは、ObjectDataSource 秒の値も用意されています。`InsertParameters`です。 そのため、イベント ハンドラーを生み出すことができます、 `ItemInserted` ObjectDataSource 秒で指定されたファイルを削除する場合と、例外が発生した場合にチェックするイベント`brochurePath`パラメーター。


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>まとめ

バイナリ データが含まれるレコードを追加するための web ベースのインターフェイスを提供するために必要なステップ数があります。 バイナリ データは、データベースに直接格納される場合は、バイナリ データが挿入されるケースを処理する特定のメソッドを追加する、アーキテクチャを更新する必要ありますが高くなります。 アーキテクチャを更新すると、次の手順はバイナリ データの各フィールドのファイルアップロード コントロールに含めるカスタマイズされた DetailsView を実現できます挿入のインターフェイスを作成しています。 アップロードされたデータは、web サーバーのファイル システムに保存または DetailsView s でデータ ソースのパラメーターに割り当てられた`ItemInserting`イベント ハンドラー。

ファイル システムにバイナリ データを保存するには、データベースに直接データを保存するよりも詳細な計画が必要です。 もう 1 つの s を上書きする 1 つのユーザーのアップロードを回避するためには、名前付けスキームを選択する必要があります。 また、余分な手順を実行して、データベースの挿入が失敗した場合、アップロードされたファイルを削除する必要があります。

ようになりましたがあるパンフレットし、画像、ですがを使用してシステムに新しいカテゴリを追加する機能を既存のカテゴリのバイナリ データを更新する方法または削除済みのカテゴリのバイナリ データを正しく削除する方法を確認するには、まだ ve です。 次のチュートリアルでは、これら 2 つのトピックを検討しましょう。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Dave ガードナー、Teresa マーフィー、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](displaying-binary-data-in-the-data-web-controls-vb.md)
[次へ](updating-and-deleting-existing-binary-data-vb.md)
