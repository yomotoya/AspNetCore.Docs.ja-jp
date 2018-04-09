---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: (VB) をコントロール データ Web でのバイナリ データの表示 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、イメージ ファイルの表示や [ダウンロード] リンク f のプロビジョニングなど、Web ページ上にバイナリ データを表示するオプションを見てお.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 006a4d014b610f3079d7f25e9420f687447a1b26
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Web コントロール (VB) の データのバイナリ データを表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe)または[PDF のダウンロード](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> このチュートリアルではイメージ ファイルの表示や PDF ファイルの [ダウンロード] リンクのプロビジョニングなど、Web ページ上にバイナリ データを表示するオプションについて説明します。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、アプリケーションを基になるデータ モデルをバイナリ データの関連付けの 2 つの方法を検討をファイルアップロード コントロール、ブラウザーから web サーバーのファイル システムにファイルをアップロードするために使用します。 ここにアップロードされたバイナリ データをデータ モデルに関連付ける方法を確認するには、まだ ve です。 つまり、ファイルをアップロードおよびファイル システムに保存された後、適切なデータベースのレコードにファイルへのパスを格納する必要があります。 データは、データベースに直接格納されているが、アップロードされたバイナリ データは、ファイル システムに保存できません必要がありますが、データベースに挿入する必要があります。

データ モデルとデータの関連付けを見ると、前に、まず、バイナリ データをエンドユーザーに提供する方法について見て s を使用できます。 テキスト データの表示は単純なのでが、バイナリ データ表示方法でしょうか。 これによって異なります、もちろん、バイナリ データの種類。 イメージは、おする可能性が高く、イメージを表示pdf で Microsoft Word 文書、ZIP ファイル、および他のバイナリ データ、ダウンロード リンクを提供する型が適切です。

データを使用して、関連付けられたテキスト データと共にバイナリ データを表示する方法を紹介はこのチュートリアルでは、GridView と DetailsView のように Web を制御します。 次のチュートリアルでおありますに注目データベースと、アップロードされたファイルの関連付け。

## <a name="step-1-providingbrochurepathvalues"></a>手順 1: 提供`BrochurePath`値

`Picture`内の列、`Categories`テーブルに既にさまざまなカテゴリの画像のバイナリ データが含まれています。 具体的には、`Picture`レコードごとに列が粗く、低品質、16 色のビットマップ イメージのバイナリ コンテンツを保持します。 各カテゴリの画像は 172 ピクセル、幅の広いから 120 ピクセルの高さは、し、ほぼ 11 サポート技術情報を使用します。 どのような秒以上、バイナリ コンテンツで、 `Picture` 78 バイトが列に含まれている[OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding)ヘッダー、イメージを表示する前に削除する必要があります。 Northwind データベースは、Microsoft Access 内でそのルートを持つために、このヘッダー情報が存在します。 Access では、バイナリ データはこのヘッダーに付け加える OLE オブジェクト型を使用して格納されます。 ここでは、おについては、画像を表示するためにこれらの低品質イメージからヘッダーを取り除きます。 今後のチュートリアルでは、カテゴリ s を更新するためのインターフェイスをビルドします`Picture`列と不要な OLE ヘッダーなしの同等の JPG イメージと OLE ヘッダーを使用してこれらのビットマップ イメージを置換します。

前のチュートリアルでは、ファイルアップロード コントロールを使用する方法を説明しました。 そのため、さあ、パンフレット ファイルを web サーバーのファイル システムに追加することができます。 これにより、ただしは更新されません、`BrochurePath`内の列、`Categories`テーブル。 次のチュートリアルでは、これを実現する方法を会いしましょう今のところを行う必要が手動でこの列の値を指定します。

このチュートリアルのダウンロードにできたら、7 つの PDF パンフレット ファイルで、`~/Brochures`フォルダー、Seafood 以外のカテゴリのそれぞれに 1 つです。 シナリオでないすべてのレコードが関連付けられているバイナリ データを処理する方法を説明するために Seafood パンフレットを追加することは意図的に除外します。 更新する、`Categories`これらの値を持つテーブルを右クリックして、`Categories`ノード サーバー エクスプ ローラーからテーブル データの表示を選択します。 図 1 に示すように、パンフレットを持つ各カテゴリ用のパンフレット ファイルへの仮想パスを入力します。 Seafood カテゴリのパンフレットがないために、以下のままにしてその`BrochurePath`列の値`NULL`です。


[![テーブルの BrochurePath のカテゴリ 列の値を手動で入力します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**図 1**: の値を手動で入力、`Categories`表 s`BrochurePath`列 ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>手順 2: GridView でパンフレットのダウンロード リンクを提供します。

`BrochurePath`に指定された値、`Categories`カテゴリのパンフレットをダウンロードするリンクと共に、各カテゴリを一覧表示する GridView を作成する準備ができたらテーブルです。 手順 4. でもカテゴリ s の画像を表示するには、この GridView を拡張します。

GridView のデザイナーには、ツールボックスからドラッグして開始、 `DisplayOrDownloadData.aspx`  ページで、`BinaryData`フォルダーです。 GridView s を設定`ID`に`Categories`GridView s のスマート タグから新しいデータ ソースにバインドするを選択します。 具体的には、名前付き、ObjectDataSource へのバインド`CategoriesDataSource`を使用してデータを取得する、`CategoriesBLL`オブジェクトの`GetCategories()`メソッドです。


[![CategoriesDataSource をという名前の新しい ObjectDataSource を作成します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![構成、ObjectDataSource CategoriesBLL クラスを使用するには](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**図 3**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラス ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![GetCategories() メソッドを使用してカテゴリの一覧を取得します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**図 4**: リストのカテゴリを使用して、取得、`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に追加する BoundField、`Categories`の GridView、 `CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath` `DataColumn` s。 さあ、削除、`NumberOfProducts`以降 BoundField、`GetCategories()`のメソッドのクエリでは、この情報は取得されません。 削除も、 `CategoryID` BoundField の名前を変更し、`CategoryName`と`BrochurePath`BoundFields`HeaderText`プロパティ カテゴリとパンフレットをそれぞれします。 これらの変更を加えたら、GridView と ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

ブラウザーからこのページを表示 (図 5 を参照してください)。 8 つのカテゴリの各が一覧表示されます。 7 つのカテゴリと`BrochurePath`値がある、`BrochurePath`それぞれ BoundField に表示される値。 持つ seafood、`NULL`値をその`BrochurePath`、空のセルが表示されます。


[![各カテゴリの名前、説明、および BrochurePath 値が表示されています。](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**図 5**: 各カテゴリの名前、説明、および`BrochurePath`値が表示されている ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


テキストを表示するのではなく、`BrochurePath`パンフレットへのリンクを作成する列。 これを実現する、削除、 `BrochurePath` BoundField 内に置き換えます。 新しい内 s を設定`HeaderText`プロパティ パンフレットをその`Text`パンフレットを表示するプロパティとその`DataNavigateUrlFields`プロパティを`BrochurePath`です。


![BrochurePath の内を追加します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**図 6**: の内の追加 `BrochurePath`


図 7 に示す GridView にこれリンクの列が追加されます。 ビュー パンフレット リンクをクリックするが、ブラウザーに直接 PDF を表示または PDF reader がインストールされているかどうかによって、ファイルをダウンロードするユーザーと、ブラウザーの設定を確認します。


[![カテゴリのパンフレット ビュー パンフレット リンクをクリックして表示できます。](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**図 7**: ビュー パンフレット リンクをクリックして、カテゴリのパンフレットを見なすことができます ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![S パンフレット PDF カテゴリが表示されます。](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**図 8**: パンフレット PDF を表示、カテゴリ s ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>パンフレットせずカテゴリのビュー パンフレット テキストを非表示にします。

図 7 に示す、`BrochurePath`内に表示されます、`Text`プロパティの値 (パンフレットを表示) かどうかに関係なく、すべてのレコードが s 以外`NULL`値を`BrochurePath`です。 もちろん場合、`BrochurePath`は`NULL`、テキストのみでのリンクを表示し、Seafood カテゴリの場合と同様 (戻るは図 7 を参照してください)。 テキスト ビュー パンフレットを表示するではなくせず、それらのカテゴリを用意すると良い場合があります、`BrochurePath`値は No パンフレット利用できるように、いくつかの代替テキストを表示します。

この動作を提供するためにいただくためにコンテンツを持つがに基づいて、適切な出力を出力するページのメソッドの呼び出しを経由して生成される TemplateField を使用して、`BrochurePath`値。 お最初に調査この手法を書式設定に戻り、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)チュートリアルです。

選択して、内を TemplateField にオン、`BrochurePath`内クリックし、変換でこのフィールドを TemplateField に列の編集 ダイアログ ボックスでリンクします。


![内を TemplateField に変換します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**図 9**: 内を TemplateField に変換


これで TemplateField が作成されます、`ItemTemplate`ハイパーリンク Web を格納しているいるコントロール`NavigateUrl`プロパティにバインドされる、`BrochurePath`値。 このマークアップをメソッドの呼び出しに置き換える`GenerateBrochureLink`の値を渡して、 `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

次に、作成、`Protected`メソッドで、ASP.NET ページという名前の s 分離コード クラス`GenerateBrochureLink`を返す、`String`を受け入れると、`Object`入力パラメーターとして。


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

場合、このメソッドを決定渡された`Object`値は、データベース`NULL`し、必要な場合は、分類がカタログされていないことを示すメッセージを返します。 それ以外の場合は、`BrochurePath`値、そのハイパーリンクで表示します。 されている場合、`BrochurePath`値は提示に渡される、 [ `ResolveUrl(url)`メソッド](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)です。 このメソッドは渡された解決*url*、置換、`~`文字と適切な仮想パス。 たとえば、アプリケーションがルートと`/Tutorial55`、`ResolveUrl("~/Brochures/Meats.pdf")`戻ります`/Tutorial55/Brochures/Meat.pdf`です。

図 10 は、これらの変更が適用された後に、ページを示します。 なお Seafood カテゴリの`BrochurePath`フィールドには、今すぐいいえパンフレット使用可能なテキストが表示されます。


[![テキストなしパンフレット使用できるものカテゴリせず、パンフレットに対して表示されます。](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**図 10**: これらのカテゴリせず、パンフレットのテキストの No パンフレットの使用が表示されます ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>手順 3: カテゴリの画像を表示する Web ページを追加します。

ユーザーは、ASP.NET ページを訪問した、ASP.NET ページの HTML が表示されます。 受信した HTML は単なるテキスト、バイナリ データが含まれていません 画像、音声ファイル、Macromedia Flash アプリケーション、埋め込みの Windows Media Player ビデオ、およびなど、追加バイナリ データは、web サーバー上の別のリソースとして存在します。 HTML では、これらのファイルへの参照が含まれていますが、実際のファイルの内容は含まれません。

たとえば、HTML では、`<img>`要素を使用で、図を参照して、`src`属性イメージ ファイルを指す次のように。


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

ブラウザーでは、この HTML を受信すると、次に、ブラウザーで表示するイメージ ファイルのバイナリ コンテンツを取得する web サーバーに別の要求を行います。 バイナリ データに、同じ概念が適用されます。 手順 2. でパンフレットに送信されませんでしたページの HTML マークアップの一部としてブラウザー。 代わりに、レンダリングされる HTML 提供されるハイパーリンク、クリックすると、PDF ドキュメントを直接要求するブラウザーの原因となったときにします。

表示や、データベース内にあります。 バイナリ データをダウンロードできるように、データを返す別の web ページを作成する必要があります。 アプリケーションでは、データベース カテゴリの画像に直接格納されている、そこ s 1 つだけのバイナリ データ フィールドです。 そのため、ページが必要、呼び出されると、特定のカテゴリの画像データを返します。

新しい ASP.NET ページの追加、`BinaryData`という名前のフォルダー`DisplayCategoryPicture.aspx`です。 これを行うときに、マスター ページを選択 チェック ボックスをオフのままにします。 このページが必要ですが、`CategoryID`値はクエリ文字列を返します。 そのカテゴリ s のバイナリ データの`Picture`列です。 バイナリ データとそれ以外のものをこのページを返すので、HTML セクションに、すべてのマークアップその必要はありません。 したがって、左下隅の [ソース] タブをクリックし、ページのマークアップを除くすべて削除、`<%@ Page %>`ディレクティブです。 つまり、 `DisplayCategoryPicture.aspx` s 宣言型マークアップは単一行で構成します。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

表示される場合、`MasterPageFile`属性、`<%@ Page %>`ディレクティブを削除します。

ページの分離コード クラスに次のコードを追加、`Page_Load`イベントのハンドラー。


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

このコードの先頭で読み取ることによって、`CategoryID`という名前の変数にクエリ文字列値`categoryID`です。 次に、画像データは、取得への呼び出しを使用して、`CategoriesBLL`クラスの`GetCategoryWithBinaryDataByCategoryID(categoryID)`メソッドです。 このデータを使用して、クライアントに返される、`Response.BinaryWrite(data)`メソッドが呼び出されると、前に、`Picture`列値の OLE ヘッダーを削除する必要があります。 作成することでこれを行う、`Byte`という名前の配列`strippedImageData`78 正確に保持する文字にはどのようなよりも小さいか、`Picture`列です。 [ `Array.Copy`メソッド](https://msdn.microsoft.com/library/z50k9bft.aspx)からデータをコピーするために使用`category.Picture`位置以降にある 78 上に`strippedImageData`です。

`Response.ContentType`プロパティを指定します、 [MIME の種類](http://en.wikipedia.org/wiki/MIME)のブラウザーでレンダリングする方法を認識するように返されるコンテンツ。 以降、`Categories`表の`Picture`列がビットマップ イメージ、ビットマップの MIME の種類が使用ここ (イメージ/bmp)。 MIME の種類を省略すると、ほとんどのブラウザーはまだイメージのため正しく表示イメージ ファイルのバイナリ データの内容に基づいて型を推論することができます。 ただし、その MIME を含めるが賢明ですが可能な場合に入力します。 参照してください、 [Internet Assigned Numbers Authority の web サイト](http://www.iana.org/)の完全な一覧については[MIME メディアの種類](http://www.iana.org/assignments/media-types/)です。

このページを作成するにアクセスして、特定のカテゴリの画像を表示できる`DisplayCategoryPicture.aspx?CategoryID=categoryID`です。 図 11 はから表示できます、飲料カテゴリの画像`DisplayCategoryPicture.aspx?CategoryID=1`します。


[![画像が表示されます、飲料カテゴリ s](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**図 11**: 画像が表示されます、飲料カテゴリ s ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


アクセスする際に`DisplayCategoryPicture.aspx?CategoryID=categoryID`System.DBNull"'system.byte[]' を型に型のオブジェクトはキャストできません。 を読み取る例外が発生した、2 つの原因と考えられるこのです。 最初に、`Categories`表 s`Picture`列で許容`NULL`値。 `DisplayCategoryPicture.aspx`  ページで、ただし、前提としていますが、非`NULL`値が存在します。 `Picture`のプロパティ、`CategoriesDataTable`がある場合に直接アクセスできることはできません、`NULL`値。 許可する場合`NULL`の値を`Picture`列 d に含める場合、次の条件。


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

上記のコードには、という名前のファイル 画像の一部が s が前提としています`NoPictureAvailable.gif`で、`Images`画像せず、それらのカテゴリを表示するフォルダーです。

この例外が発生することも、 `CategoriesTableAdapter` s`GetCategoryWithBinaryDataByCategoryID`メソッドの`SELECT`アドホック SQL ステートメントを使用しているし、している、tableadapter のウィザードを再実行する場合に発生することができますが、メイン クエリの列一覧に戻るステートメントは元に戻すメイン クエリです。 確認`GetCategoryWithBinaryDataByCategoryID`メソッド s`SELECT`ステートメントがまだ含まれています、`Picture`列です。

> [!NOTE]
> たびに、`DisplayCategoryPicture.aspx`がアクセスすると、データベースにアクセスして、指定したカテゴリの画像データが返されます。 ユーザーが最後に表示するためにカテゴリの画像いない t が変更された場合は、これは無駄な作業です。 さいわい、HTTP では、*条件付き取得*です。 条件付きの GET HTTP 要求を行っているクライアントはに沿って送信、 [ `If-Modified-Since` HTTP ヘッダー](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)クライアントは、web サーバーからこのリソースを取得する最終日時を提供します。 この日付を指定するために、コンテンツが変更されていない場合、web サーバーが応答として返さ、 [Not Modified 状態コード (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)せず、要求されたリソースのコンテンツを送信するとします。 つまり、この手法は、これが変更されていない場合、クライアントが最終アクセス後リソースのコンテンツを送信することから、web サーバーを緩和します。


この動作を実装するただし、追加することが必要、`PictureLastModified`列を`Categories`ときにキャプチャするテーブル、`Picture`列が最後に更新されたコードを確認するだけでなく、`If-Modified-Since`ヘッダー。 詳細については、`If-Modified-Since`ヘッダーと条件付きの GET ワークフローを参照してください。 [RSS ハッカーの HTTP 条件付きの GET](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)と[A より深いを見て、ASP.NET ページ内の HTTP 要求を実行する](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx)です。

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>手順 4: GridView のカテゴリの画像を表示します。

これでが特定のカテゴリの画像を表示する web ページ、して表示を使用して、[イメージ Web コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx)または HTML`<img>`要素を指す`DisplayCategoryPicture.aspx?CategoryID=categoryID`です。 イメージの URL は、データベースのデータによって決まります GridView または使用して、ImageField DetailsView で表示できます。 ImageField を含む`DataImageUrlField`と`DataImageUrlFormatString`プロパティ内のように動作する`DataNavigateUrlFields`と`DataNavigateUrlFormatString`プロパティです。

S を強化できるように、`Categories`で GridView`DisplayOrDownloadData.aspx`各カテゴリの画像を表示する、ImageField を追加することによりします。 単に、ImageField を追加し、設定、`DataImageUrlField`と`DataImageUrlFormatString`プロパティ`CategoryID`と`DisplayCategoryPicture.aspx?CategoryID={0}`、それぞれします。 これを表示する GridView 列が作成されます、`<img>`要素が`src`属性参照`DisplayCategoryPicture.aspx?CategoryID={0}`{0} が GridView 行 s を置き換え、`CategoryID`値。


![GridView に、ImageField を追加します。](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**図 12**: GridView に、ImageField を追加


追加した後、ImageField、GridView s の宣言構文はように見えます。 soothe 以下。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

ブラウザーからこのページを表示するのにはしばらくを実行します。 各レコードが今すぐカテゴリの画像を含んだ方法に注意してください。


[![各の行のカテゴリの画像が表示されます。](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**図 13**: カテゴリ s 各の行の画像が表示されます ([フルサイズのイメージを表示するをクリックして](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>まとめ

このチュートリアルでは、バイナリ データを表示する方法を確認します。 データがどのように表示されるかは、データの種類によって異なります。 PDF パンフレット ファイルの提供したは、ユーザー ビュー パンフレット リンクをクリックすると、PDF ファイルに直接ユーザーをかかりました。 カテゴリの画像お最初を取得し、データベースからバイナリ データを返すページを作成し、そのページを使用して GridView に各カテゴリの画像を表示します。

これまで、挿入、更新、およびバイナリ データをデータベースに対して削除を実行する方法を確認する準備ができたらバイナリ データを表示する方法を見てきました。 次のチュートリアルにアップロードされたファイルを対応するデータベース レコードに関連付ける方法を紹介します。 チュートリアルでは、その後、既存のバイナリ データを更新する方法と、関連付けられているレコードが削除されたときに、バイナリ データを削除する方法を会いしましょう。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよび Dave ガードナーがいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](uploading-files-vb.md)
> [次へ](including-a-file-upload-option-when-adding-a-new-record-vb.md)
