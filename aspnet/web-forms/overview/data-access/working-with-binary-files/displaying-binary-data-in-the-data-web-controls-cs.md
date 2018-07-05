---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: データ Web でのバイナリ データの表示を制御する (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、Web ページで、イメージ ファイルの表示、[ダウンロード] リンク f のプロビジョニングなどをバイナリ データを表示するオプションに注目しています.
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 62cc931b670931677b4e9632dccd6634715b3c71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814855"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>データ Web コントロール (c#) でバイナリ データを表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe)または[PDF のダウンロード](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> このチュートリアルでは、Web ページで、イメージ ファイルの表示、PDF ファイルの [ダウンロード] リンクのプロビジョニングなどをバイナリ データを表示するオプションに注目します。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、アプリケーションの基になるデータ モデルをバイナリ データを関連付けるための 2 つの手法を調査を FileUpload コントロールをブラウザーから web サーバーのファイル システムにファイルをアップロードするために使用します。 ここにアップロードしたバイナリ データをデータ モデルに関連付ける方法を確認するには、まだきました。 つまり、ファイルがアップロードおよびファイル システムに保存された後、適切なデータベースのレコードにファイルへのパスを格納する必要があります。 データベースで直接データが格納される場合、アップロードしたバイナリ データはファイル システムに保存できません必要がありますが、データベースに挿入する必要があります。

データ モデルとデータの関連付けを見ると、前に、最初に、エンドユーザー、バイナリ データを提供する方法を見て s を使用できます。 テキスト データを表示することが単純で、しかしのバイナリ データ表示方法でしょうか。 これによって異なります、もちろん、バイナリ データの種類。 イメージの場合する可能性が高く、イメージを表示pdf で Microsoft Word 文書、ZIP ファイル、およびその他の種類のバイナリ データをダウンロード リンクを提供する方が適切です。

データを使用して、関連付けられているテキスト データと共にバイナリ データを表示する方法に注目するはこのチュートリアルでは GridView や DetailsView など Web を制御します。 次のチュートリアルで、データベースと、アップロードされたファイルの関連付けを有効にします。

## <a name="step-1-providingbrochurepathvalues"></a>手順 1: 提供`BrochurePath`値

`Picture`内の列、`Categories`テーブルに既にさまざまなカテゴリの画像のバイナリ データが含まれています。 具体的には、`Picture`レコードごとに列が粗く、低品質、16 色のビットマップ イメージのバイナリ コンテンツを保持します。 各カテゴリの画像は 172 ピクセル、幅と 120 ピクセルの高さは、し、約 11 サポート技術情報を使用します。 複数 s はどのようなバイナリ コンテンツで、`Picture`列には 78 バイトが含まれています[OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding)ヘッダーが、イメージを表示する前に削除する必要があります。 Northwind データベースは Microsoft Access でそのルートを持つために、このヘッダーの情報は存在します。 Access では、このヘッダーに追加する OLE オブジェクトのデータ型を使用してバイナリ データが格納されます。 ここでは、私たちは、画像を表示するためにこれらの低品質のイメージからのヘッダーを削除する方法で表示されます。 今後のチュートリアルでは、s のカテゴリを更新するインターフェイスを作成します`Picture`列 OLE ヘッダーを使用して、不要な OLE ヘッダーのない同等の JPG イメージでこれらのビットマップ イメージに置き換えます。

前のチュートリアルでは、FileUpload コントロールを使用する方法を説明しました。 そのため、パンフレット ファイルを web サーバーのファイル システムに追加してくださいすることができます。 これにより、ただしは更新されません、`BrochurePath`内の列、`Categories`テーブル。 次のチュートリアルでこれを実現する方法を見ていきますが、ここでは手動でこの列の値を指定します。

このチュートリアルのダウンロードで 7 つの PDF パンフレット ファイルがあります、`~/Brochures`フォルダー、シーフード以外のカテゴリのそれぞれに 1 つ。 意図しないすべてのレコードにバイナリ データが関連付けられているシナリオを処理する方法を説明するシーフード パンフレットの追加を省略するとは。 更新する、`Categories`これらの値を持つテーブルを右クリックします、`Categories`ノード サーバー エクスプ ローラーからテーブル データの表示を選択します。 図 1 に示すよう、パンフレットのある各カテゴリのパンフレット ファイルへの仮想パスを入力します。 パンフレット シーフード カテゴリがないために、以下のままにしてその`BrochurePath`列の値として`NULL`します。


[![カテゴリ表の BrochurePath 列の値を手動で入力します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**図 1**: の値を手動で入力、`Categories`テーブル s`BrochurePath`列 ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))。


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>手順 2: GridView でパンフレットのダウンロード リンクを提供します。

`BrochurePath`に指定された値、`Categories`カテゴリのパンフレットのダウンロードへのリンクと共に、各カテゴリの一覧を表示する GridView を作成する準備ができたらテーブル。 手順 4. でも、カテゴリのイメージを表示するには、この GridView を拡張します。

GridView のデザイナーには、ツールボックスからドラッグして開始、`DisplayOrDownloadData.aspx`ページで、`BinaryData`フォルダー。 GridView s 設定`ID`に`Categories`GridView s のスマート タグを新しいデータ ソースにバインドを選択します。 具体的には、という名前を ObjectDataSource にバインド`CategoriesDataSource`を使用してデータを取得する、`CategoriesBLL`オブジェクト`GetCategories()`メソッド。


[![CategoriesDataSource という名前の新しい ObjectDataSource を作成します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))。


[![CategoriesBLL クラスを使用する ObjectDataSource を構成します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**図 3**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))。


[![GetCategories() メソッドを使用してカテゴリの一覧を取得します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**図 4**: 一覧のカテゴリを使用して、取得、`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))。


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に追加する、BoundField、`Categories`の GridView、 `CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath``DataColumn`秒。 さあ、削除、`NumberOfProducts`以降 BoundField、`GetCategories()`メソッドのクエリでは、この情報は取得されません。 削除も、 `CategoryID` BoundField の名前を変更し、`CategoryName`と`BrochurePath`BoundFields`HeaderText`プロパティをカテゴリやパンフレット、それぞれします。 これらの変更を行った後、次のよう GridView コントロールと ObjectDataSource s 宣言型マークアップになります。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

ブラウザーからこのページを表示 (図 5 を参照してください)。 8 つのカテゴリの各が一覧表示されます。 7 つのカテゴリと`BrochurePath`値が、`BrochurePath`それぞれ BoundField に表示される値。 シーフードを持つ、`NULL`値その`BrochurePath`、空のセルを表示します。


[![各カテゴリの名前、説明、および BrochurePath 値が一覧表示します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**図 5**: 各カテゴリの名前、説明、および`BrochurePath`値が表示されます ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))。


テキストを表示するのではなく、`BrochurePath`パンフレットへのリンクを作成する列。 これを行うには、削除、 `BrochurePath` BoundField 内に置き換えます。 新しい内 s 設定`HeaderText`プロパティ、パンフレットをその`Text`パンフレットを表示するプロパティとその`DataNavigateUrlFields`プロパティを`BrochurePath`。


![BrochurePath 用内を追加します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**図 6**: の内の追加 `BrochurePath`


図 7 に示すように、リンクの列を GridView これ追加されます。 ビューのパンフレットのリンクをクリックをいずれか、ブラウザーで直接、PDF を表示またはユーザー PDF reader がインストールされているかどうかによって、ファイルをダウンロードして、ブラウザーの設定を確認します。


[![カテゴリの s パンフレット ビュー パンフレットのリンクをクリックして表示できます。](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**図 7**: ビュー パンフレットのリンクをクリックしてカテゴリ s パンフレットを表示できます ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))。


[![S パンフレット PDF カテゴリが表示されます。](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**図 8**: パンフレットの PDF を表示、カテゴリ s ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))。


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>パンフレットなしのカテゴリ ビュー パンフレットのテキストを非表示

図 7 に示すよう、`BrochurePath`内に表示されます、`Text`かどうかに関係なく、すべてのレコードのプロパティの値 (パンフレットを表示) が s 以外`NULL`値`BrochurePath`。 もちろん場合、`BrochurePath`は`NULL`、テキストのみで、リンクを表示し、シーフード カテゴリは、(バックアップは、図 7 を参照してください)。 テキスト ビュー パンフレットを表示するのではなくことがなくこれらのカテゴリを用意すると良い場合があります、`BrochurePath`値は No パンフレット利用できるように、いくつかの代替テキストを表示します。

この動作を提供するためにコンテンツを持つがに基づいて、適切な出力を出力するページ メソッドへの呼び出しを使用して生成されて TemplateField に必要な`BrochurePath`値。 最初に検討しましたこの手法を書式設定に戻り、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)チュートリアル。

選択して、内を TemplateField にオン、`BrochurePath`列の編集 ダイアログ ボックス内をクリック、Convert でこのフィールドを TemplateField にリンクします。


![内を TemplateField に変換します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**図 9**: 内を TemplateField に変換します。


これを TemplateField が作成されます、`ItemTemplate`ハイパーリンク Web を格納しているいるコントロール`NavigateUrl`プロパティにバインドする、`BrochurePath`値。 このマークアップ メソッドの呼び出しを置き換えます`GenerateBrochureLink`の値を渡して、 `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

次に、作成、`protected`メソッドは、asp.net ページという名前の分離コード クラス`GenerateBrochureLink`を返す、`string`を受け入れると、`object`入力パラメーターとして。


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

場合、このメソッドが決定します、渡されたで`object`値は、データベース`NULL`し、そうである場合は、カテゴリにパンフレットがされていないことを示すメッセージを返します。 それ以外の場合、存在する場合、`BrochurePath`値がハイパーリンクに表示されました。 されている場合、`BrochurePath`値は提示に渡される、 [ `ResolveUrl(url)`メソッド](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)します。 このメソッドは、渡されたで解決*url*、置換、`~`文字と適切な仮想パス。 たとえば、アプリケーションがルートに`/Tutorial55`、`ResolveUrl("~/Brochures/Meats.pdf")`戻ります`/Tutorial55/Brochures/Meat.pdf`します。

図 10 は、これらの変更が適用された後に、ページを示します。 なおシーフード カテゴリの`BrochurePath`フィールドなしパンフレットに利用可能なテキストが表示されます。


[![これらのカテゴリせず、パンフレットのテキストなしパンフレット利用が表示されます。](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**図 10**: これらのカテゴリせず、パンフレット、テキストの No パンフレットの利用が表示されます ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))。


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>手順 3: カテゴリの画像を表示する Web ページの追加

ユーザーにアクセスする ASP.NET ページ、ASP.NET ページの HTML が表示されます。 受信した HTML では、テキストだけでは、し、バイナリ データが含まれていません。 画像、サウンド ファイルなど、Macromedia Flash アプリケーション、埋め込み Windows Media Player のビデオなどの追加のバイナリ データは、web サーバー上の別のリソースとして存在します。 HTML では、これらのファイルへの参照が含まれていますが、ファイルの実際の内容は含まれません。

たとえば、html 形式で、`<img>`要素を使用して、画像の参照を`src`属性は、イメージ ファイルを指すようになります。


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

ブラウザーでは、この HTML を受信すると、その後、ブラウザーで表示するイメージ ファイルのバイナリ コンテンツを取得する web サーバーに別の要求を行います。 同じ概念は、任意のバイナリ データに適用されます。 手順 2 では、パンフレットがいない s HTML マークアップをページの一部としてブラウザーに送信します。 はなく、レンダリングされる HTML 提供ハイパーリンクをクリックすると、PDF ドキュメントを直接要求するブラウザーの原因となった。

表示またはデータベース内に存在するバイナリ データをダウンロードできるように、データを返す別の web ページを作成する必要があります。 このアプリケーションでは、データベース カテゴリの画像に直接格納されている、s がのみ 1 つのバイナリ データ フィールド。 そのため、ページ必要がありますが、呼び出されると、特定のカテゴリの画像データを返します。

新しい ASP.NET ページの追加、`BinaryData`という名前のフォルダー`DisplayCategoryPicture.aspx`します。 これを行うときに、マスター ページの選択 チェック ボックスをオフのままにします。 このページが必要ですが、`CategoryID`値で、クエリ文字列とそのカテゴリ s のバイナリ データを返します。`Picture`列。 このページは、バイナリ データ、およびその他に何も返される、ために、HTML セクション内のマークアップに必要はありません。 したがって、左下隅の [ソース] タブをクリックし、ページのマークアップを除くのすべてを削除、`<%@ Page %>`ディレクティブ。 つまり、 `DisplayCategoryPicture.aspx` s 宣言型マークアップする必要がありますが単一行で構成されます。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

表示された場合、`MasterPageFile`属性、`<%@ Page %>`ディレクティブを削除します。

ページ分離コード クラスで次のコードを追加、`Page_Load`イベント ハンドラー。


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

このコードの先頭で読み取ることによって、`CategoryID`という名前の変数にクエリ文字列値`categoryID`します。 次に、呼び出しに画像データが取得、`CategoriesBLL`クラスの`GetCategoryWithBinaryDataByCategoryID(categoryID)`メソッド。 このデータを使用して、クライアントに返される、`Response.BinaryWrite(data)`メソッドが呼び出されると、前に、`Picture`列値の OLE ヘッダーを削除する必要があります。 作成してこの情報は、`byte`という名前の配列`strippedImageData`78 正確に保持する文字があるものよりも小さいか、`Picture`列。 [ `Array.Copy`メソッド](https://msdn.microsoft.com/library/z50k9bft.aspx)からデータをコピーするために使用`category.Picture`を上に位置 78 から`strippedImageData`します。

`Response.ContentType`プロパティを指定します、 [MIME の種類](http://en.wikipedia.org/wiki/MIME)のブラウザーのレンダリング方法を認識するように返されるコンテンツ。 以降、`Categories`テーブルの`Picture`列がビットマップ イメージ、ビットマップの MIME の種類が使用されますここ (bmp イメージ/)。 MIME の種類を省略すると、ほとんどのブラウザーでもイメージが表示されます正しくイメージ ファイルのバイナリ データの内容に基づいて、型が推論できるためです。 ただし、その MIME を含めるが賢明ですが可能な場合に入力します。 参照してください、 [Internet Assigned Numbers Authority の web サイト](http://www.iana.org/)の完全な一覧について[MIME メディア タイプ](http://www.iana.org/assignments/media-types/)します。

参照するくださいとこのページを作成すると、特定のカテゴリの画像を表示できる`DisplayCategoryPicture.aspx?CategoryID=categoryID`します。 図 11 はから表示できます、飲料カテゴリの画像`DisplayCategoryPicture.aspx?CategoryID=1`します。


[![画像が表示されます、飲料カテゴリ s](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**図 11**: 画像が表示されます、飲料カテゴリ s ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))。


アクセスすると、if `DisplayCategoryPicture.aspx?CategoryID=categoryID`、"system.byte[]' を入力するには、"System.DBNull"型のオブジェクトはキャストできません。 を読み取る例外が発生した、原因と考えられるこの 2 つの点があります。 まず、`Categories`テーブル s`Picture`列では許可`NULL`値。 `DisplayCategoryPicture.aspx`  ページで、ただし、前提としていますがない`NULL`値が存在します。 `Picture`のプロパティ、`CategoriesDataTable`がある場合に直接アクセスすることはできません、`NULL`値。 許可する場合`NULL`の値を`Picture`d する次の条件を追加する列。


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

上記のコードには、一部のイメージという名前のファイルが s が前提としています`NoPictureAvailable.gif`で、`Images`画像なしのこれらのカテゴリに対して表示するフォルダー。

場合、この例外を発生することも、 `CategoriesTableAdapter` s`GetCategoryWithBinaryDataByCategoryID`メソッドの`SELECT`アドホック SQL ステートメントを使用しているし、tableadapter のウィザードを再実行している場合に発生することがメイン クエリの列リストに戻るステートメントが元に戻るメインのクエリ。 いることを確認するチェック`GetCategoryWithBinaryDataByCategoryID`メソッド s`SELECT`ステートメントが含まれていますが、`Picture`列。

> [!NOTE]
> 毎回、`DisplayCategoryPicture.aspx`がアクセスすると、データベースにアクセスし、指定したカテゴリの画像データが返されます。 カテゴリの画像から t 変更された場合、ユーザーが最後に表示するため、これは、無駄な努力します。 HTTP では、さいわい、*条件付きの取得*します。 条件付きの GET と HTTP 要求を行うクライアントに送信に沿って、 [ `If-Modified-Since` HTTP ヘッダー](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)クライアントは、web サーバーからこのリソースを取得する最後の日時を提供します。 Web サーバーが応答がこの日付が指定されているために、コンテンツが変更されていない場合、 [Not Modified 状態コード (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)せず、要求されたリソースのコンテンツを送信するとします。 簡単に言えば、この手法により、web サーバーが変更されていない場合、クライアントが最終アクセス後にリソースのコンテンツのバックアップに送信する必要がなくなります。


この動作を実装するただし、追加する必要があります、`PictureLastModified`列を`Categories`ときにキャプチャするテーブル、`Picture`列が最後に更新されたコードを確認すると、`If-Modified-Since`ヘッダー。 詳細については、`If-Modified-Since`ヘッダーと条件付きの GET ワークフローを参照してください。 [RSS ハッカーの条件付き GET を HTTP](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)と[A 深く見て ASP.NET ページの HTTP 要求を実行する](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx)します。

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>手順 4: を GridView にカテゴリの画像を表示します。

使用して表示できる特定のカテゴリの画像を表示する web ページが作成できた、[イメージ Web コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx)または HTML`<img>`要素を指す`DisplayCategoryPicture.aspx?CategoryID=categoryID`します。 イメージの URL がデータベースのデータによって決定は、GridView、DetailsView、ImageField を使用してに表示できます。 含まれています、ImageField`DataImageUrlField`と`DataImageUrlFormatString`プロパティ内のように動作する`DataNavigateUrlFields`と`DataNavigateUrlFormatString`プロパティ。

S を強化できるように、`Categories`で GridView`DisplayOrDownloadData.aspx`各カテゴリの画像を表示する、ImageField を追加することで。 単に、ImageField を追加し、設定、`DataImageUrlField`と`DataImageUrlFormatString`プロパティ`CategoryID`と`DisplayCategoryPicture.aspx?CategoryID={0}`、それぞれします。 これを表示する GridView の列が作成されます、`<img>`要素が`src`属性参照`DisplayCategoryPicture.aspx?CategoryID={0}`ここで、{0}は GridView 行 s に置き換えられます`CategoryID`値。


![GridView に、ImageField を追加します。](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**図 12**: ImageField を GridView に追加します。


Soothe と同様に、ImageField を追加した後、GridView s の宣言型構文になります次。


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

ブラウザーからこのページを表示する時間がかかります。 各レコードが今すぐ、カテゴリの画像を含んだ方法に注意してください。


[![各の行のカテゴリの画像が表示されます。](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**図 13**: カテゴリ s の各の行の画像が表示されます ([フルサイズの画像を表示する をクリックします](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、バイナリ データを表示する方法について確認しました。 データを表示する方法は、データの種類によって異なります。 PDF パンフレット ファイルで、提供したは、ユーザー ビュー パンフレット リンクをクリックすると、PDF ファイルに直接ユーザーをかかりました。 カテゴリの画像、私たちを取得して、データベースからバイナリ データを返すページを最初に作成されたおり、そのページを使用して GridView に各カテゴリの画像を表示します。

これまできましたが挿入、更新、およびバイナリ データをデータベースに対して削除を実行する方法を確認する準備ができたら、バイナリ データを表示する方法を説明しました。 次のチュートリアルでは、アップロードされたファイルを対応するデータベース レコードに関連付ける方法を紹介します。 チュートリアルでは、その後、既存のバイナリ データを更新する方法と、関連付けられているレコードが削除されると、バイナリ データを削除する方法を見ていきます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy、Dave gardner 著でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](uploading-files-cs.md)
> [次へ](including-a-file-upload-option-when-adding-a-new-record-cs.md)
