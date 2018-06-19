---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: (VB) のファイルのアップロード |Microsoft ドキュメント
author: rick-anderson
description: サーバーのファイル システムで格納は、Web サイトにユーザー (Word、PDF ドキュメント) などのバイナリ ファイルをアップロードできるようにする方法を説明してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fbc4aaf80ac7e0f960e140b492055fe35cd2b6ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30888098"
---
<a name="uploading-files-vb"></a>ファイルのアップロード (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe)または[PDF のダウンロード](uploading-files-vb/_static/datatutorial54vb1.pdf)

> サーバーのファイル システムまたはデータベースのいずれかに格納は、Web サイトにユーザー (Word、PDF ドキュメント) などのバイナリ ファイルをアップロードできるようにする方法を説明します。


## <a name="introduction"></a>はじめに

すべてのチュートリアルでは、おテキスト データだけを解析してきた ve 操作していた。 ただし、多くのアプリケーションでは、テキストとバイナリ データの両方をキャプチャするデータ モデルがあります。 オンライン dating サイト、ユーザーは自分のプロファイルに関連付ける画像をアップロードします。 採用の web サイトができます。 Microsoft Word や PDF ドキュメントとしてその復帰をアップロードできます。

バイナリ データの操作は、新しい課題のセットを追加します。 アプリケーションのバイナリ データを格納する方法を決定お必要があります。 新しいレコードを挿入するために使用されるインターフェイスが各自のコンピューターからファイルをアップロードするアクセス許可を更新する必要があり、表示または関連付けられているバイナリ データをレコード s をダウンロードするための手段を提供する追加手順を実行する必要があります。 このチュートリアルで、次の 3 つが、これらの課題を hurdle する方法について説明します。 これらのチュートリアルの最後を作成した画像と PDF ドキュメントをカテゴリごとに関連付けます完全に機能するアプリケーション。 この特定のチュートリアルではおをバイナリ データを格納するためのさまざまな手法を見て各自のコンピューターからファイルをアップロードするユーザーを有効にする方法を調査および web サーバーのファイル システム上に保存しました。

> [!NOTE]
> アプリケーションのデータ モデルの一部であるバイナリ データとも呼ば、 [BLOB](http://en.wikipedia.org/wiki/Binary_large_object)、バイナリ ラージ オブジェクトの頭字語です。 これらのチュートリアルでことにしました用語のバイナリ データを使用するという用語は、BLOB は同じ意味です。


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>手順 1: Web ページのバイナリ データで作業を作成します。

バイナリ データのサポートの追加に関連する課題の調査を始める前に、まず、このチュートリアルと、次の 3 つの必要がありますを web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`BinaryData`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![バイナリ データに関連するチュートリアルについては、ASP.NET ページを追加します。](uploading-files-vb/_static/image1.gif)

**図 1**: バイナリ データに関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`BinaryData`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image2.png))


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、向上した後、GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、バイナリ データのチュートリアルを使用した作業の項目が含まれています。


![サイト マップではバイナリ データのチュートリアルを使用した作業のエントリが含まれるようになりました](uploading-files-vb/_static/image3.gif)

**図 3**: サイト マップではバイナリ データのチュートリアルを使用した作業のエントリが含まれるようになりました


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>手順 2: バイナリ データを格納する場所を決定します。

アプリケーションのデータ モデルに関連付けられているバイナリ データは、2 つの場所に格納できますデータベースに格納されているファイルへの参照で web サーバーのファイル システム上。または、データベース自体内に直接 (図 4 を参照してください)。 それぞれのアプローチには長所と短所の独自セットがあり、最もメリットのある詳細な説明。


[![ファイル システム上またはデータベース内で直接、バイナリ データを格納することができます。](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**図 4**: ファイル システム上またはデータベース内で直接、バイナリ データを格納することができます ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image4.png))


想像に各 product に画像を関連付けるには、Northwind データベースを拡張します。 1 つのオプションは web サーバーのファイル システムにこれらのイメージ ファイルを格納およびパスを記録すること、`Products`テーブル。 この方法で d を追加、`ImagePath`列を`Products`型のテーブル`varchar(200)`、おそらくです。 Web サーバーのファイル システムにその画像を格納する可能性がありますユーザーは、Chai の画像をアップロード、`~/Images/Tea.jpg`ここで、`~`アプリケーション s の物理パスを表します。 つまり、物理パスにある web サイトがルート化されている場合`C:\Websites\Northwind\`、`~/Images/Tea.jpg`と同じ`C:\Websites\Northwind\Images\Tea.jpg`です。 Chai レコードの更新 d イメージ ファイルをアップロードした後、`Products`テーブルようにその`ImagePath`列は、新しいイメージのパスを参照します。 使用して`~/Images/Tea.jpg`または`Tea.jpg`場合は、すべての製品の画像はアプリケーション s 内に配置することをにしました`Images`フォルダーです。

バイナリ データを格納するファイル システム上の主な利点は次のとおりです。

- **実装の容易性**を格納して、データベース内に直接格納されるバイナリ データを取得するときに、ファイル システムを介してデータを扱うビット以上のコードと思います間もなく、します。 さらに、ユーザーを表示またはバイナリ データをダウンロードするために、必要がありますが表示されたデータへの URL。 Web サーバーのファイル システムにデータがある場合、URL は単純です。 データがデータベースに格納されている場合、web ページを取得し、データベースからデータを返すを作成する必要します。
- **バイナリ データへのアクセスをより広い**バイナリ データは、その他のサービスまたはアプリケーションでは、データベースからデータをプルできませんしたものにアクセスする必要があります。 たとえば、各 product に関連付けられているイメージを使用してユーザーに使用する必要がありますも[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)、後者 d するファイル システムにバイナリ データを格納します。
- **パフォーマンス**バイナリ データがファイル システムに格納されている場合、データベース サーバーと web サーバー間の要求とネットワークの輻輳よりも少なくなります、データベース内で直接、バイナリ データが格納されている場合。

バイナリ データを格納するファイル システム上の主な欠点は、データベースからデータを分離ことです。 レコードが削除された場合、`Products`テーブル、web サーバーのファイル システムに関連付けられているファイルが自動的に削除されません。 ファイルまたはファイル システムを削除する追加のコードが散在し、使用されていない、孤立したファイルを記述する必要があります。 さらに、データベースをバックアップする場合もファイル システムに関連付けられているバイナリ データのバックアップを作成することを確認してようにする必要があります。 別のサイトまたはサーバーの問題のような課題にデータベースを移動します。

型の列を作成することでバイナリ データを Microsoft SQL Server 2005 データベースに直接保存する代わりに、`varbinary`です。 ほかの可変長データ型と同様にこの列でのバイナリ データを保持できる最大長を指定できます。 たとえば、多くてを予約する 5,000 バイト使用`varbinary(5000)`です。`varbinary(MAX)`では、ストレージの最大サイズを約 2 GB です。

データベース内で直接バイナリ データを格納する主な利点は、バイナリ データおよびデータベースのレコード間の密結合です。 これには、バックアップまたは別のサイトまたはサーバーにデータベースの移動など、データベース管理タスクが大幅に簡略化します。 また、レコードを自動的に削除すると、対応するバイナリ データが削除されます。 バイナリ データを格納するデータベース内の複数の微妙な利点もあります。 参照してください[を格納するバイナリ ファイルを直接データベースを使用して ASP.NET 2.0 の](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx)の詳細情報についてはします。

> [!NOTE]
> Microsoft SQL Server 2000 以前のバージョンでのみで、`varbinary`データ型が 8,000 バイトの最大数。 最大 2 GB のバイナリ データを格納する、 [ `image`データ型](https://msdn.microsoft.com/library/ms187993.aspx)代わりに使用する必要があります。 追加すると、 `MAX` SQL Server 2005、ただしで、`image`データ型は廃止されました。 これは、s ために引き続きサポート後方互換性のためが、Microsoft はことを発表しましたが、`image`データ型は、SQL Server の将来のバージョンで削除される予定です。


表示古いデータ モデルを使用して作業している場合、`image`データ型。 Northwind データベース s`Categories`テーブルには、`Picture`カテゴリのイメージ ファイルのバイナリ データの格納に使用できる列です。 型のこの列は、Northwind データベースには、Microsoft Access や SQL Server の以前のバージョンでは、そのルートがあるので`image`です。

このチュートリアルと、次の 3 つの場合は、どちらの方法でもを使用されます。 `Categories`テーブルが既に、`Picture`カテゴリの画像のバイナリ コンテンツを格納するための列です。 追加の列の追加`BrochurePath`、印刷品質、洗練されたカテゴリの概要を提供するために使用する web サーバーのファイル システムに PDF へのパスを格納します。

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>手順 3: 追加、`BrochurePath`列を`Categories`テーブル

現在、Categories テーブルが 4 つだけの列: `CategoryID`、 `CategoryName`、 `Description`、および`Picture`です。 これらのフィールドだけでなく (存在する場合は、カテゴリのパンフレットをポイントするか、新しいを追加する必要があります。 この列を追加するには、サーバー エクスプ ローラーにドリル ダウンするにはテーブルを右クリックを参照してください、`Categories`テーブルし、テーブル定義を開く (図 5 を参照してください)。 サーバー エクスプ ローラーが表示されない場合は、[表示] メニューからサーバー エクスプ ローラーのオプションを選択して表示または Ctrl + Alt + S をヒットします。

新しい`varchar(200)`列を`Categories`という名前のテーブル`BrochurePath`でき、 `NULL` s と保存] アイコンをクリックして (または Ctrl キーを押しながら S キーを押して)。


[![Categories テーブルに BrochurePath 列を追加します。](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**図 5**: 追加、`BrochurePath`列を`Categories`テーブル ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>手順 4: 使用するアーキテクチャを更新する、`Picture`と`BrochurePath`列

`CategoriesDataTable`データ アクセス層 (DAL) では現在は 4 つ`DataColumn`定義 s: `CategoryID`、 `CategoryName`、 `Description`、および`NumberOfProducts`です。 おもともと設計されている場合にこの DataTable、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)チュートリアルでは、`CategoriesDataTable`だけが最初の 3 つの列以外の場合は、`NumberOfProducts`列が追加されました、[マスター/詳細、箇条書きを使用します。マスター詳細 DataList レコードの一覧](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)チュートリアルです。

説明したよう*データ アクセス レイヤーを作成する*、型指定されたデータセットのデータ テーブルは、ビジネス オブジェクトを構成します。 Tableadapter は、データベースとの通信をクエリ結果と共にビジネス オブジェクトを設定するためです。 `CategoriesDataTable`によって設定されますが、 `CategoriesTableAdapter`、3 つのデータ取得方法は。

- `GetCategories()` TableAdapter のメインのクエリを実行し、返します、 `CategoryID`、 `CategoryName`、および`Description`内のすべてのレコードのフィールド、`Categories`テーブル。 メインのクエリが自動生成されたによって使用されるもの`Insert`と`Update`メソッドです。
- `GetCategoryByCategoryID(categoryID)` 返します、 `CategoryID`、 `CategoryName`、および`Description`カテゴリのフィールドが`CategoryID`equals *categoryID*です。
- `GetCategoriesAndNumberOfProducts()` -を返します、 `CategoryID`、 `CategoryName`、および`Description`内のすべてのレコードのフィールドを`Categories`テーブル。 また、サブクエリを使用して、各カテゴリに関連付けられている製品の数を返します。

これらのクエリの戻り値はする通知、`Categories`表 s`Picture`または`BrochurePath`列もできません、`CategoriesDataTable`提供`DataColumn`秒であり、これらのフィールドです。 画像を操作するためと`BrochurePath`プロパティ、最初に追加する必要があります、`CategoriesDataTable`し、更新、`CategoriesTableAdapter`これらの列を返すためにします。

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>追加する、`Picture`と`BrochurePath``DataColumn`s

これら 2 つの列を追加して、開始、`CategoriesDataTable`です。 右クリックし、`CategoriesDataTable`のヘッダーのコンテキスト メニューの [追加と Column オプションを選択します。 これは、新しい作成`DataColumn`という名前の DataTable に`Column1`です。 この列の名前を変更`Picture`です。 [プロパティ] ウィンドウから次のように設定します。、 `DataColumn` s`DataType`プロパティを`System.Byte[]`(これは、ドロップダウン リストのオプションではありません; に入力する必要があります)。


[![データ型を持つは system.byte[] DataColumn という名前の画像を作成します。](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**図 6**: 作成、`DataColumn`名前付き`Picture`が`DataType`は`System.Byte[]`([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image8.png))


もう 1 つ追加`DataColumn`命名、DataTable に`BrochurePath`既定値を使用して`DataType`値 (`System.String`)。

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>返す、`Picture`と`BrochurePath`TableAdapter の値

これらの 2 つの`DataColumn`に追加される、 `CategoriesDataTable`、私たちを更新する準備ができたら、`CategoriesTableAdapter`です。 両方のメインの TableAdapter クエリで返される列値があるが、これは元に戻す、バイナリ データたびに、`GetCategories()`メソッドが呼び出されました。 Let s が元に戻すにメインの TableAdapter クエリを更新する代わりに、 `BrochurePath` s 特定のカテゴリを表す追加のデータの取得メソッドを作成および`Picture`列です。

TableAdapter のメインのクエリを更新するを右クリックし、`CategoriesTableAdapter`のヘッダーし、コンテキスト メニューから構成オプションを選択します。 私たちテーブル アダプターの構成ウィザードが表示にはさまざまな過去のチュートリアルを見てきました。 元に戻すにクエリを更新、 `BrochurePath` [完了] をクリックします。


[![更新プログラムも BrochurePath を返す SELECT ステートメントで列の一覧](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**図 7**: で列リストを更新、`SELECT`ステートメントを返すことも`BrochurePath`([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image10.png))


Tableadapter をアドホック SQL ステートメントを使用する場合は、すべての列リストを更新、メインのクエリで列リストを更新、`SELECT`クエリは TableAdapter のメソッドです。 つまり、`GetCategoryByCategoryID(categoryID)`を返すメソッドが更新されて、`BrochurePath`列で、意図したと場合があります。 ただし、これも更新で列リスト、`GetCategoriesAndNumberOfProducts()`を各カテゴリの製品の数を返すサブクエリを削除するメソッドです。 そのため、このメソッドを更新する必要があります`SELECT`クエリ。 右クリックし、`GetCategoriesAndNumberOfProducts()`メソッドを構成] を選択し、元に戻す、`SELECT`元の値に戻すクエリ。


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

特定のカテゴリ s を返す新しい TableAdapter メソッドを次に、作成`Picture`列の値。 右クリックし、`CategoriesTableAdapter`のヘッダー TableAdapter クエリの構成ウィザードを起動するクエリの追加オプションを選択します。 このウィザードの最初の手順は、アドホック SQL ステートメントを使用してデータをクエリする場合、新しいストアド プロシージャ、または既存 us を要求します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 行を返すおのでを 2 番目のステップからの行のオプションを返す SELECT を選択します。


[![使用する SQL ステートメント オプションを選択します。](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**図 8**: 使用する SQL ステートメント オプションを選択 ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image12.png))


[![複数行を返すため、クエリでは、Categories テーブルからレコードを返しますの選択が選択します。](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**図 9**: クエリが、選択をオンに行が返されます、Categories テーブルからレコードを返すため ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image14.png))


3 番目の手順で次の SQL クエリを入力し、[次へ] をクリックします。


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

最後の手順では、新しいメソッドの名前を選択します。 使用して`FillCategoryWithBinaryDataByCategoryID`と`GetCategoryWithBinaryDataByCategoryID`塗りつぶしの DataTable と戻り値、データ テーブルのパターンをそれぞれします。 ウィザードを完了するには、[完了] をクリックします。


[![TableAdapter のメソッドの名前を選択します。](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**図 10**: tableadapter のメソッドの名前を選択 ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image16.png))


> [!NOTE]
> テーブル アダプター クエリの構成ウィザードを完了した後は、新しいコマンド テキストを返すことでデータをメイン クエリのスキーマと異なることを通知するダイアログ ボックスを参照してください可能性があります。 つまり、ウィザードは、注意してくださいを TableAdapter のメイン クエリ`GetCategories()`先ほど作成したものとは異なるスキーマを返します。 これは、ここで必要なため、このメッセージを無視することができます。


また、注意してくださいこと、アドホック SQL ステートメントを使用して、使用して、ウィザードに後で TableAdapter のメインのクエリを変更する場合は変更されること、`GetCategoryWithBinaryDataByCategoryID`メソッドの`SELECT`ステートメント列リストからそれらの列を含める、メインのクエリ (つまり、削除は、`Picture`クエリから列)。 返される列のリストを手動で更新する必要が、`Picture`で行ったのような列、`GetCategoriesAndNumberOfProducts()`この手順の前半のメソッドです。

2 つの追加後に`DataColumn`s、`CategoriesDataTable`と`GetCategoryWithBinaryDataByCategoryID`メソッドを`CategoriesTableAdapter`、型指定されたデータセット デザイナーでこれらのクラス図 11 にスクリーン ショットのようになります。


![データセット デザイナーには、新しい列とメソッドが含まれています](uploading-files-vb/_static/image11.gif)

**図 11**: データセット デザイナーには、新しい列とメソッドが含まれています


## <a name="updating-the-business-logic-layer-bll"></a>ビジネス ロジック層 (BLL) の更新

更新、DAL では、新しいメソッドを追加するビジネス ロジック層 (BLL) を拡張するために`CategoriesTableAdapter`メソッドです。 次のメソッドを追加、`CategoriesBLL`クラス。


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>手順 5: クライアントから Web サーバーへのファイルをアップロードします。

バイナリ データを収集するときに多くの場合このデータは、エンドユーザーが提供されます。 この情報をキャプチャするには、ユーザーを自分のコンピューターから web サーバーへのファイルをアップロードできる必要があります。 アップロードされたデータは、web サーバーのファイル システムと、データベース内のファイルへのパスの追加またはデータベースに直接バイナリの内容の書き込みにファイルの保存を意味する可能性がありますデータ モデルと統合する必要があります。 このステップでは、ユーザーが各自のコンピューターからサーバーへのファイルをアップロードできるようにする方法を紹介します。 次のチュートリアルでアップロードされたファイルをデータ モデルと統合することに注目有効にします。

ASP.NET 2.0 s 新しい[ファイルアップロード Web コントロール](https://msdn.microsoft.com/library/ms227677(VS.80).aspx)ユーザーは各自のコンピューターから web サーバーにファイルを送信するためのメカニズムを提供します。 ファイルアップロード コントロールとして表示、`<input>`要素が`type`ファイルで、ブラウザーの [参照] ボタンとテキスト ボックスとして表示する属性を設定します。 [参照] ボタンをクリックすると、ユーザーがファイルを選択] ダイアログ ボックスが表示されます。 フォームがポストバック時に、選択したファイルの内容がポストバックと共に送信されます。 サーバー側でアップロードされたファイルに関する情報はファイルアップロード コントロールのプロパティを使用してアクセスします。

ファイルのアップロードを示すためには、開く、 `FileUpload.aspx` ] ページで、`BinaryData`フォルダーがファイルアップロード コントロールをツールボックスからデザイナーにドラッグし、制御 s を設定`ID`プロパティを`UploadTest`です。 Button Web コントロールの設定を次に、追加の`ID`と`Text`プロパティ`UploadButton`とそれぞれに選択されたファイルをアップロードします。 最後に、クリア、ボタンの下にラベル Web コントロールを配置、`Text`プロパティとその`ID`プロパティを`UploadDetails`です。


[![ASP.NET ページにファイルアップロード コントロールを追加します。](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**図 12**: ファイルアップロード コントロール、ASP.NET ページを追加 ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image18.png))


図 13 では、ブラウザーで表示したときに、このページを示します。 ファイルの選択] ダイアログ ボックスを表示、参照ボタンをクリックすると、ユーザーが各自のコンピューターからファイルを選択できるようにするに注意してください。 ファイルを選択するは、web サーバーに、選択したファイル %s のバイナリ コンテンツを送信するポストバックを発生させる、選択したファイルのアップロード] ボタンをクリックします。


[![ユーザーが自分のコンピューターからサーバーにアップロードするファイルを選択できます。](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**図 13**: ユーザーは自分のコンピューターをサーバーからアップロードするファイルを選択することができます ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image20.png))


ポストバックでアップロードされたファイルは、ファイル システムに保存できます。 またはストリームを使用して直接、バイナリ データで使用できます。 この例では、%s を作成できるように、`~/Brochures`フォルダーし、アップロードされたファイルを保存します。 追加することで開始、`Brochures`フォルダーのルート ディレクトリのサブフォルダーとしてサイトをします。 イベント ハンドラーを次に、作成、 `UploadButton` s`Click`イベントし、次のコードを追加します。


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

ファイルアップロード コントロールでは、さまざまなアップロードされたデータを操作するためのプロパティを提供します。 インスタンス、 [ `HasFile`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx)ファイルが、ユーザーによってアップロードされたかどうかを示すときに、 [ `FileBytes`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx)バイトの配列としてアップロードされたバイナリ データへのアクセスを提供します。 `Click`ファイルがアップロードされていることを確認してイベント ハンドラーを起動します。 ファイルがアップロードされた場合、ラベルがアップロードされたファイルのサイズ (バイト単位)、そのコンテンツの種類の名前を表示します。

> [!NOTE]
> ユーザーが確認できますファイルをアップロードすることを確認する、`HasFile`プロパティ場合に警告を表示、s `False`、または使用することは、 [RequiredFieldValidator コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx)代わりにします。


ファイルアップロード s`SaveAs(filePath)`を指定されたアップロードされたファイルを保存*filePath*です。 *filePath*する必要があります、*物理パス*(`C:\Websites\Brochures\SomeFile.pdf`) ではなく、*仮想**パス*(`/Brochures/SomeFile.pdf`)。 [ `Server.MapPath(virtPath)`メソッド](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx)仮想パスを受け取り、対応する物理パスを返します。 仮想パスは、ここでは、`~/Brochures/fileName`ここで、 *fileName*アップロードされたファイルの名前を指定します。 参照してください[を使用して Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml)仮想および物理パスと使用に関する詳細について`Server.MapPath`です。

完了した後、`Click`イベント ハンドラーでは、ブラウザーでページをテストします。 [参照] ボタンをクリックして、ハード ドライブからファイルを選択し、選択したファイルのアップロード] ボタンをクリックします。 ポストバックは、web サーバーに保存する前に、ファイルの情報が表示されますを選択したファイルの内容を送信、`~/Brochures`フォルダーです。 アップロードした後、ファイル、Visual Studio に戻り、およびソリューション エクスプ ローラーで [更新] ボタンをクリックします。 ~/Brochures フォルダーにアップロードしたファイルを参照してください!


[![Web サーバーにアップロードされたファイル EvolutionValley.jpg](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**図 14**: ファイル`EvolutionValley.jpg`Web サーバーにアップロードされている ([フルサイズのイメージを表示するをクリックして](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg が ~/Brochures フォルダーに保存されました](uploading-files-vb/_static/image15.gif)

**図 15**:`EvolutionValley.jpg`に保存された、`~/Brochures`フォルダー


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>ファイル システムにアップロードされたファイルを保存する際に微妙な

いくつかの微妙な web サーバーのファイル システムにファイルをアップロードして保存するときに対処が必要があります。 最初に、セキュリティのないの問題です。 ファイル システムにファイルを保存するには、書き込みのアクセス許可が ASP.NET ページを実行しているセキュリティ コンテキストに必要です。 ASP.NET 開発 Web サーバーは、現在のユーザー アカウントのコンテキストで実行されます。 マイクロソフトのインターネット インフォメーション サービス (IIS) は、web サーバーとして使用されている場合に、セキュリティ コンテキストは、IIS とその構成のバージョンによって異なります。

ファイル システムに保存する場合の課題の 1 つのファイルの名前付け中心にします。 現時点では、すべてのアップロードされたファイルを保存、ページ、`~/Brochures`ディレクトリ クライアント コンピューター上のファイルと同じ名前を使用します。 ユーザー A はアップロード名前を持つパンフレット場合`Brochure.pdf`、ファイルとして保存されます`~/Brochure/Brochure.pdf`です。 しばらく後ユーザー B が同じファイル名が含まれている異なるパンフレット ファイルをアップロードする場合は (`Brochure.pdf`)? コードでおユーザーがあるようになりましたが、ユーザー B のアップロードで s ファイルが上書きされます。

ファイル名の競合を解決する方法の数があります。 1 つは、同じ名前のいずれかが既に存在する場合、ファイルのアップロードを禁止します。 ユーザー B がという名前のファイルをアップロードしようとしたときに、この方法で`Brochure.pdf`システムはそのファイルを保存されず、代わりに、ファイルの名前を変更し、もう一度やり直してくださいユーザー B を知らせるメッセージを表示します。 別のアプローチが生じる可能性がある一意のファイル名を使用してファイルを保存するには、[グローバル一意識別子 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)またはレコードの主キー列をデータベースから、対応する値 (アップロードに関連付けられていると仮定すると、特定の行のデータ モデル)。 次のチュートリアルではこれらのオプションの詳細についてをについて説明します。

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>非常に大量のバイナリ データに関連する課題

これらのチュートリアルでは、キャプチャされたバイナリ データがサイズで適切なであると仮定します。 いくつかのメガバイト数であるバイナリ データ ファイルの量が非常に大きい場合に動作しているか大きいはこれらのチュートリアルの範囲を超える新しい問題が発生します。 たとえば、既定では ASP.NET は拒否 4 MB を超えるのアップロードでこれを構成することができます、 [ `<httpRuntime>`要素](https://msdn.microsoft.com/library/e1f13641.aspx)で`Web.config`です。 IIS では、独自のファイル アップロード サイズの制限はすぎますが適用されます。 参照してください[ファイル アップロード サイズを IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html)詳細についてはします。 さらに、サイズの大きなファイルのアップロードにかかる時間超える可能性がある既定の ASP.NET が要求の待機 110 秒です。 大きなファイルを操作するときに発生するメモリとパフォーマンスの問題もあります。

ファイルアップロード コントロールは大きなファイルのアップロードの実用的ではありません。 ように、ファイル コンテンツがサーバーにポストされて、エンドユーザーは、アップロードの進行状況を確認する必要があります待機します。 これより小さい、数秒でアップロードすることができますが、ファイルをアップロードする分かかる場合がありますよりも大きなファイルを処理する場合、問題になることができますを処理する場合ほど問題ではありません。 さまざまなサード パーティ製のファイル アップロード コントロールを大規模なアップロードを処理するが適してし、これらのベンダーの多くは、進捗状況と ActiveX アップロードより光沢のあるユーザー エクスペリエンスを提供するマネージャーを提供します。

アプリケーションは、大きなファイルを処理する必要がある場合、は、慎重に問題を調査し、特定のニーズに適した解決策を検索する必要があります。

## <a name="summary"></a>まとめ

バイナリ データをキャプチャする必要があるアプリケーションを構築すると、さまざまな課題が導入されています。 このチュートリアルでは最初の 2 つに調査お: バイナリ データを格納する場所を決定する、ユーザーが web ページによってバイナリ コンテンツをアップロードするを許可します。 次の 3 つのチュートリアルでは、経由では、データベース内のレコードにアップロードされたデータを関連付ける方法と、データのフィールドをテキストと共にバイナリ データを表示する方法が表示されます。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [大きな値データ型の使用](https://msdn.microsoft.com/library/ms178158.aspx)
- [ファイルアップロード コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 ファイルアップロード サーバー コントロール](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [ファイルをアップロードします。](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよび「社長補佐 Leigh がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](updating-and-deleting-existing-binary-data-cs.md)
> [次へ](displaying-binary-data-in-the-data-web-controls-vb.md)
