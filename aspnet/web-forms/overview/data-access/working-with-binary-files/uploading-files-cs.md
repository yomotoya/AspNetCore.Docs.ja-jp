---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: アップロード ファイル (c#) |Microsoft Docs
author: rick-anderson
description: サーバーのファイル システムで格納は、Web サイトに (Word や PDF ドキュメントなど) のバイナリ ファイルをアップロードするユーザーを許可する方法を説明してください.
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: f420797b7c06b9063b70b784a5b61c7d02162c1d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820076"
---
<a name="uploading-files-c"></a>ファイルのアップロード (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe)または[PDF のダウンロード](uploading-files-cs/_static/datatutorial54cs1.pdf)

> サーバーのファイル システムまたはデータベースのいずれかで保存が Web サイト (Word や PDF ドキュメントなど) のバイナリ ファイルをアップロードするユーザーを許可する方法について説明します。


## <a name="introduction"></a>はじめに

すべてのチュートリアルでは、私たちだけテキスト データを解析してきた ve してきました。 ただし、多くのアプリケーションでは、テキストとバイナリ データの両方をキャプチャするデータ モデルがあります。 オンライン dating サイトは、ユーザーが自分のプロファイルに関連付ける画像をアップロードを許可する可能性があります。 マイクロソフトの人材募集の web サイトには、ユーザーが、再開として Microsoft Word や PDF ドキュメントのアップロードができるように可能性があります。

バイナリ データの操作は、新しい課題のセットを追加します。 アプリケーションのバイナリ データを格納する方法決める必要があります。 新しいレコードを挿入するために使用されるインターフェイスは、ユーザーが自分のコンピューターからファイルをアップロードできるようにする更新するには、表示または関連付けられているバイナリ データをレコード %s をダウンロードするための手段を提供する追加の手順を実行する必要があります。 このチュートリアルで、次の 3 つに、これらの課題を増やせる方法を見ていきます。 これらのチュートリアルの最後を組み込みました画像と PDF ドキュメントを各カテゴリに関連付けられるアプリケーションを完全に機能します。 この特定のチュートリアルではありますバイナリ データを格納するためのさまざまな手法を見てを自分のコンピューターからファイルをアップロードするユーザーを有効にする方法について説明し、これが web サーバーのファイル システムに保存します。

> [!NOTE]
> アプリケーションのデータ モデルの一部であるバイナリ データとして呼ば、 [BLOB](http://en.wikipedia.org/wiki/Binary_large_object)、バイナリ ラージ オブジェクトの略語です。 これらのチュートリアルで選択しました用語のバイナリ データを使用する用語 BLOB は同義です。


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>手順 1: バイナリ データの Web ページで、作業を作成します。

バイナリ データのサポートの追加に関連する課題を始める前に、このチュートリアルと、次の 3 つの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`BinaryData`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![バイナリ データに関連するチュートリアルについては、ASP.NET ページを追加します。](uploading-files-cs/_static/image1.gif)

**図 1**: バイナリ データに関連するチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`BinaryData`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image2.png))。


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、向上した後、GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューで、バイナリ データのチュートリアルを使用した作業の項目できるようになりました。


![サイト マップ データのバイナリのチュートリアルを使用した作業のエントリになりました](uploading-files-cs/_static/image3.gif)

**図 3**: サイト マップ データのバイナリのチュートリアルを使用した作業のエントリになりました


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>手順 2: は、バイナリ データを格納する場所を決定します。

2 つの場所のいずれかで、アプリケーションのデータ モデルに関連付けられているバイナリ データを格納できますデータベースに格納されているファイルへの参照で web サーバーのファイル システム上。または、データベース自体内で直接 (図 4 参照)。 それぞれのアプローチは、独自の長所と短所のセットを備え、効率的に詳細な議論について。


[![ファイル システム上またはデータベースで直接、バイナリ データを格納することができます。](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**図 4**: ファイル システム上またはデータベースで直接、バイナリ データを格納することができます ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image4.png))。


Northwind データベースに関連付ける各製品の画像を拡張したいことを想像してください。 1 つのオプションは、web サーバーのファイル システムでこれらのイメージ ファイルを格納および内のパスを記録すること、`Products`テーブル。 この方法により、d を追加、`ImagePath`列を`Products`型のテーブル`varchar(200)`、おそらくします。 Web サーバーのファイル システムにその画像を格納する可能性があります Chai のユーザーが画像をアップロードすると、`~/Images/Tea.jpg`ここで、`~`アプリケーションの物理パスを表します。 つまり、物理パスにある web サイトが root 化されている場合`C:\Websites\Northwind\`、`~/Images/Tea.jpg`と等価になります`C:\Websites\Northwind\Images\Tea.jpg`します。 Chai レコードを更新するイメージ ファイルをアップロードした後には、私たちの d、`Products`テーブルようにその`ImagePath`列には、新しいイメージのパスが参照されています。 使用して`~/Images/Tea.jpg`単`Tea.jpg`すべての製品イメージは、アプリケーション内に配置することを決定した場合`Images`フォルダー。

バイナリ データを格納するファイル システム上の主な利点は次のとおりです。

- **容易な実装**を格納して、データベース内に直接格納されるバイナリ データを取得するときに、ファイル システムを介してデータを扱うよりも少しのコードは、間もなく表示されるよう、します。 さらに、ユーザーを表示またはバイナリ データをダウンロードするために、必要がありますが表示データへの URL。 Web サーバーのファイル システム上のデータが存在する場合、URL は簡単です。 データがデータベースに格納されている場合、web を取得し、データベースからデータを返すを作成する必要がをページします。
- **バイナリ データを広いアクセス**バイナリ データを他のサービスまたはアプリケーションでは、データベースからデータをプルできないものにアクセスできる必要があります。 たとえば、各製品に関連付けられているイメージでユーザーに提供する必要があるも[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)、ファイル システムにバイナリ データを格納する場合 d。
- **パフォーマンス**バイナリ データがファイル システムに格納されている場合、データベース サーバーと web サーバー間の要求とネットワークの輻輳よりも少なくなります、データベース内で直接、バイナリ データが格納されている場合。

バイナリ データを格納するファイル システム上の主な欠点は、データベースからデータを切り離すことです。 レコードが削除された場合、`Products`テーブル、web サーバーのファイル システムに関連付けられているファイルが自動的に削除されません。 余分なコード ファイルまたはファイル システムを削除するのには使用されていない、孤立したファイルを乱雑記述する必要があります。 さらに、データベース、バックアップ時にする必要があることを確認してもファイル システムに関連付けられているバイナリ データのバックアップを作成します。 データベースを移動すると、別のサイトまたはサーバーのような問題が発生します。

型の列を作成してバイナリ データを Microsoft SQL Server 2005 データベースに直接格納する代わりに、`varbinary`します。 その他の可変長データ型を持つなどにこのコラムで保持できるバイナリ データの最大長を指定できます。 たとえば、最大で予約する 5,000 バイト使用`varbinary(5000)`;`varbinary(MAX)`ストレージの最大サイズを約 2 GB を使用します。

データベースに直接バイナリ データを保存する主な利点は、バイナリ データとデータベースのレコード間の密結合です。 これには、バックアップまたは別のサイトまたはサーバーにデータベースの移動などのデータベース管理作業が大幅に簡略化します。 また、レコードを自動的に削除すると、対応するバイナリ データを削除します。 バイナリ データを格納するデータベース内の複数の微妙な利点もあります。 参照してください[を格納するバイナリ ファイルを直接データベースを使用して ASP.NET 2.0 で](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx)のについて、詳しく検討します。

> [!NOTE]
> Microsoft SQL Server 2000 およびそれ以前のバージョンで、`varbinary`データ型には、最大 8,000 バイトの制限が必要があります。 最大 2 GB のバイナリ データを格納する、 [ `image`データ型](https://msdn.microsoft.com/library/ms187993.aspx)代わりに使用する必要があります。 追加`MAX`SQL Server 2005、ただしで、`image`データ型が非推奨とされました。 これは、秒でもサポートされますの下位互換性のためを Microsoft が発表した、`image`データ型は、SQL Server の将来のバージョンで削除される予定です。


表示が、以前のデータ モデルを使用する場合、`image`データ型。 Northwind データベースの s`Categories`テーブルには、`Picture`カテゴリのイメージ ファイルのバイナリ データの格納に使用できる列です。 型のこの列は、Northwind データベースには、Microsoft Access や SQL Server の以前のバージョンでは、そのルートがあるので`image`します。

このチュートリアルと、次の 3 つの場合は、両方のアプローチを使用します。 `Categories`テーブルが既に、`Picture`カテゴリの画像のバイナリ コンテンツを格納するための列。 追加の列を追加します`BrochurePath`印刷品質、光沢のあるカテゴリの概要を提供するために使用する web サーバーのファイル システム上の PDF へのパスを格納するため、します。

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>手順 3: 追加、`BrochurePath`列を`Categories`テーブル

現在、Categories テーブルが 4 つのみの列: `CategoryID`、 `CategoryName`、 `Description`、および`Picture`します。 これらのフィールドだけでなく (存在する場合は、カテゴリのパンフレットを指しますが、新しいものを追加する必要があります。 この列を追加する移動をサーバー エクスプ ローラーには、テーブルにドリルダウンを右クリックし、`Categories`テーブルし、テーブル定義を開く (図 5 を参照してください)。 サーバー エクスプ ローラーが表示されない場合は、表示 メニューから、サーバー エクスプ ローラーのオプションを選択して起動または Ctrl + Alt + S をヒットします。

新しい追加`varchar(200)`列を`Categories`という名前のテーブル`BrochurePath`でき、 `NULL` s と保存 アイコンをクリックします (または Ctrl + S をヒット)。


[![Categories テーブルに BrochurePath 列を追加します。](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**図 5**: 追加、`BrochurePath`列を`Categories`テーブル ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image6.png))。


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>手順 4: アーキテクチャを使用する更新、`Picture`と`BrochurePath`列

`CategoriesDataTable`データ アクセス層 (DAL) の 4 つが現在`DataColumn`定義 s: `CategoryID`、 `CategoryName`、 `Description`、および`NumberOfProducts`します。 最初にこの DataTable を設計したときに、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、`CategoriesDataTable`済みましたが、最初の 3 つの列、`NumberOfProducts`で列が追加されました、[マスター/詳細は、箇条書きを使用してリストと詳細 DataList マスター レコードの](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)チュートリアル。

説明したよう*データ アクセス層を作成する*、型指定されたデータセットのデータ テーブルは、ビジネス オブジェクトを構成します。 データベースとの通信をクエリ結果をビジネス オブジェクトを設定するため、Tableadapter が担当します。 `CategoriesDataTable`によって設定されますが、 `CategoriesTableAdapter`、3 つのデータの取得方法のあります。

- `GetCategories()` TableAdapter のメイン クエリを実行し、返します、 `CategoryID`、 `CategoryName`、および`Description`内のすべてのレコードのフィールド、`Categories`テーブル。 メインのクエリが自動生成されたによって使用されるもの`Insert`と`Update`メソッド。
- `GetCategoryByCategoryID(categoryID)` 返します、 `CategoryID`、`CategoryName`と`Description`カテゴリのフィールドが`CategoryID`equals *categoryID*します。
- `GetCategoriesAndNumberOfProducts()` -を返します、 `CategoryID`、 `CategoryName`、および`Description`内のすべてのレコードのフィールドを`Categories`テーブル。 サブクエリを使用して、各カテゴリに関連付けられている製品の数を返します。

これらのクエリからことに注意してください、`Categories`テーブル s`Picture`または`BrochurePath`列もできません、`CategoriesDataTable`提供`DataColumn`秒であり、これらのフィールド。 画像を使用するにと`BrochurePath`プロパティでは、最初に追加する必要があります、`CategoriesDataTable`し、更新、`CategoriesTableAdapter`をこれらの列を返すクラス。

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>追加、`Picture`と`BrochurePath``DataColumn`s

これら 2 つの列を追加して、開始、`CategoriesDataTable`します。 右クリックし、`CategoriesDataTable`ヘッダー、コンテキスト メニューの 追加とし、列のオプションを選択します。 これは、新しい作成`DataColumn`という名前の DataTable に`Column1`します。 この列の名前を変更`Picture`します。 [プロパティ] ウィンドウから次のように設定します。、 `DataColumn` s`DataType`プロパティを`System.Byte[]`(これは、ドロップダウン リストで、オプションではありません) も入力する必要があります。


[![データ型を持つが system.byte[] DataColumn という名前の画像を作成します。](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**図 6**: 作成、`DataColumn`名前付き`Picture`が`DataType`は`System.Byte[]`([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image8.png))。


もう 1 つ追加`DataColumn`DataTable には、とします`BrochurePath`既定値を使用して`DataType`値 (`System.String`)。

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>返す、`Picture`と`BrochurePath`TableAdapter の値

これらの 2 つの`DataColumn`に追加される、 `CategoriesDataTable`、私たちを更新する準備ができたら、 `CategoriesTableAdapter`。 両方のメインの TableAdapter クエリで返される列値があるが、これはデータを戻す、バイナリ毎回、`GetCategories()`メソッドが呼び出されました。 Let s がメインの TableAdapter クエリを元に戻しますを更新する代わりに、`BrochurePath`を返す特定のカテゴリの追加のデータ取得メソッドを作成および`Picture`列。

TableAdapter のメインのクエリを更新するを右クリックし、`CategoriesTableAdapter`のヘッダー、コンテキスト メニューから、構成オプションを選択します。 私たちのテーブル アダプター構成ウィザードが起動さまざまな過去のチュートリアルで確認したところです。 更新に戻すクエリ、 `BrochurePath` [完了] をクリックします。


[![更新プログラムも BrochurePath を返す SELECT ステートメントで列リスト](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**図 7**: で列リストを更新、`SELECT`ステートメントを返すことも`BrochurePath`([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image10.png))。


Tableadapter をアドホック SQL ステートメントを使用する場合は、すべての列リストを更新、メイン クエリで列リストを更新、`SELECT`クエリは TableAdapter のメソッド。 つまり、`GetCategoryByCategoryID(categoryID)`を返すメソッドが更新されました、`BrochurePath`列は、意図したとする可能性があります。 ただし、それも更新で列リスト、`GetCategoriesAndNumberOfProducts()`メソッドは、各カテゴリの製品の数を返すサブクエリを削除します。 そのため、このメソッドの s を更新する必要があります`SELECT`クエリ。 右クリックし、`GetCategoriesAndNumberOfProducts()`メソッドは、の構成を選択し、元に戻す、`SELECT`元の値に戻すクエリ。


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

特定のカテゴリ s を返す新しい TableAdapter メソッドを次に、作成`Picture`列の値。 右クリックし、`CategoriesTableAdapter`のヘッダー、TableAdapter クエリ構成ウィザードを起動するクエリの追加オプションを選択します。 このウィザードの最初の手順は、アドホック SQL ステートメントを使用してデータをクエリする場合、新しいストアド プロシージャ、または、既存ことを確認します。 SQL ステートメントを使用を選択し、[次へ] をクリックします。 以降の行を返すことを 2 番目の手順からオプションの行を返す SELECT を選択します。


[![SQL ステートメントのオプションを選択します。](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**図 8**: SQL ステートメントのオプションを選択します ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image12.png))。


[![行を返すので、クエリでは、Categories テーブルからレコードを返すは、選択を選択します](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**図 9**: ので、クエリは、複数行を返す選択選択、Categories テーブルからレコードを返します ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image14.png))。


3 番目の手順では、次の SQL クエリを入力し、[次へ] をクリックします。


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

最後の手順では、新しいメソッドの名前を選択します。 使用`FillCategoryWithBinaryDataByCategoryID`と`GetCategoryWithBinaryDataByCategoryID`塗りつぶし DataTable 戻って DataTable パターン、それぞれします。 ウィザードを完了するには、[完了] をクリックします。


[![TableAdapter のメソッドの名前を選択します。](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**図 10**: tableadapter のメソッドの名前を選択 ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image16.png))。


> [!NOTE]
> テーブル アダプター クエリ構成ウィザードの完了後に、新しいコマンド テキスト データを返すことスキーマと、メイン クエリのスキーマと異なることを通知 ダイアログ ボックスを参照してください可能性があります。 つまり、ウィザードが注目を TableAdapter のメイン クエリ`GetCategories()`先ほど作成した 1 つは異なるスキーマを返します。 これは、ここで必要なため、このメッセージを無視することができます。


また、留意場合は、アドホック SQL ステートメントを使用しているし、後で TableAdapter のメイン クエリでの変更ウィザードを使用して、それが変更、`GetCategoryWithBinaryDataByCategoryID`メソッドの`SELECT`ステートメントの列リストからそれらの列を含める、メインのクエリ (つまり、削除は、`Picture`クエリからの列)。 返される列のリストを手動で更新する必要があります、`Picture`で行ったような列、`GetCategoriesAndNumberOfProducts()`この手順で前にメソッド。

2 つを追加した後`DataColumn`s、`CategoriesDataTable`と`GetCategoryWithBinaryDataByCategoryID`メソッドを`CategoriesTableAdapter`、型指定されたデータセット デザイナーでこれらのクラス図 11 のスクリーン ショットのようになります。


![データセット デザイナーには、メソッド、新しい列が含まれます。](uploading-files-cs/_static/image11.gif)

**図 11**: データセット デザイナーには、メソッド、新しい列が含まれます。


## <a name="updating-the-business-logic-layer-bll"></a>ビジネス ロジック層 (BLL) の更新

DAL を更新すると、残っているは、新しいメソッドを追加するビジネス ロジック層 (BLL) を強化する`CategoriesTableAdapter`メソッド。 次のメソッドを追加、`CategoriesBLL`クラス。


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>手順 5: Web サーバーにクライアントからファイルのアップロード

バイナリ データを収集するときに多くの場合このデータは、エンドユーザーによって提供されます。 この情報をキャプチャするには、ユーザーを web サーバーのコンピューターからファイルをアップロードできる必要があります。 アップロードされたデータは、web サーバーのファイル システムと、データベース内のファイルへのパスを追加または直接データベースにバイナリの内容の書き込みにファイルの保存を意味する可能性がありますデータ モデルと統合する必要があります。 この手順では、ユーザーが自分のコンピューターからファイルをサーバーにアップロードできるようにする方法を紹介します。 次のチュートリアルでデータ モデルと、アップロードされたファイルの統合を有効にします。

ASP.NET 2.0 の新[FileUpload Web コントロール](https://msdn.microsoft.com/library/ms227677(VS.80).aspx)ユーザーが自分のコンピューターからファイルを web サーバーに送信するためのメカニズムを提供します。 FileUpload コントロールとして表示、`<input>`要素が`type`ファイルで、ブラウザーの [参照] ボタンを含むテキスト ボックスとして表示する属性を設定します。 参照 ボタンをクリックすると、ユーザーがファイルを選択 ダイアログ ボックスが表示されます。 フォームがポストバック時に、選択したファイルの内容がポストバックと共に送信されます。 サーバー側では、アップロードされたファイルに関する情報は、FileUpload コントロール プロパティからアクセスできます。

ファイルのアップロードを示すためには、開く、`FileUpload.aspx`ページで、`BinaryData`フォルダー、FileUpload コントロールをツールボックスからデザイナーにドラッグおよび s コントロール設定`ID`プロパティを`UploadTest`します。 設定ボタン Web コントロールを次に、追加の`ID`と`Text`プロパティを`UploadButton`し、それぞれ、選択したファイルをアップロードします。 最後に、クリアします、ボタンの下にラベル Web コントロールを配置、`Text`プロパティとその`ID`プロパティを`UploadDetails`します。


[![FileUpload コントロールを ASP.NET ページに追加します。](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**図 12**: FileUpload コントロールを ASP.NET ページに追加 ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image18.png))。


図 13 は、ブラウザーで表示した場合は、このページを示します。 ユーザーが自分のコンピューターからファイルを選択できるファイルの選択 ダイアログ ボックスを表示、参照ボタンをクリックするとに注意してください。 ファイルを選択すると、選択したファイルのアップロード ボタンをクリックしてにより選択したファイルのバイナリ コンテンツを web サーバーに送信するポストバックが発生します。


[![ユーザーが自分のコンピューターからサーバーにアップロードするファイルを選択できます。](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**図 13**: ユーザーは自分のコンピューターをサーバーからアップロードするファイルを選択することができます ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image20.png))。


ポストバックでアップロードされたファイルをファイル システムに保存できます。 またはバイナリ データは、Stream を使用して直接協力できます。 この例では、%s を作成できるように、`~/Brochures`フォルダーし、アップロードされたファイルを保存します。 追加することで開始、`Brochures`フォルダーのルート ディレクトリのサブフォルダーとしてサイトにします。 イベント ハンドラーを次に、作成、 `UploadButton` s`Click`イベントと、次のコードを追加します。


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

FileUpload コントロールは、さまざまなアップロードされたデータを操作するためのプロパティを提供します。 たとえば、 [ `HasFile`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx)ファイルが、ユーザーによってアップロードされたかどうかを示す中に、 [ `FileBytes`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx)バイトの配列としてアップロードしたバイナリ データへのアクセスを提供します。 `Click`ファイルがアップロードされていることを確認してイベント ハンドラーを起動します。 ファイルがアップロードされた場合、ラベルには、アップロードされたファイルのサイズ (バイト単位)、そのコンテンツの種類の名前が表示されます。

> [!NOTE]
> 確認するファイルをアップロードすることを確認する、`HasFile`プロパティ場合に警告を表示、s`false`を使用することや、 [RequiredFieldValidator コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx)代わりにします。


FileUpload s`SaveAs(filePath)`を指定したアップロードされたファイルを保存します。 *filePath*します。 *filePath*必要があります、*物理パス*(`C:\Websites\Brochures\SomeFile.pdf`) ではなく*仮想**パス*(`/Brochures/SomeFile.pdf`)。 [ `Server.MapPath(virtPath)`メソッド](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx)仮想パスを受け取り、その対応する物理パスを返します。 仮想パスは、ここでは、`~/Brochures/fileName`ここで、 *fileName*アップロードされたファイルの名前を指定します。 参照してください[を使用して Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml)仮想および物理パスと使用の詳細については`Server.MapPath`します。

完了した後、`Click`イベント ハンドラーでは、ブラウザーでページをテストする少し。 [参照] ボタンをクリックして、ハード ドライブからファイルを選択し、選択したファイルのアップロード ボタンをクリックします。 ポストバックに保存する前に、ファイルに関する情報が表示される web サーバーに、選択したファイルの内容を送り、`~/Brochures`フォルダー。 ファイルをアップロードした後は、Visual Studio に戻り、ソリューション エクスプ ローラーで [更新] ボタンをクリックします。 ~/Brochures フォルダーにアップロードしたファイルが表示する必要があります。


[![Web サーバーにアップロードされたファイル EvolutionValley.jpg](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**図 14**: ファイル`EvolutionValley.jpg`Web サーバーにアップロードされました ([フルサイズの画像を表示する をクリックします](uploading-files-cs/_static/image22.png))。


![EvolutionValley.jpg が ~/Brochures フォルダーに保存されました](uploading-files-cs/_static/image15.gif)

**図 15**:`EvolutionValley.jpg`に保存された、`~/Brochures`フォルダー


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>アップロードされたファイルをファイル システムに保存する際に細部について

これは、いくつかの微妙な web サーバーのファイル システムにファイルをアップロードして保存するときに対処する必要があります。 セキュリティの最初に、そこの問題。 ファイル システムにファイルの保存、ASP.NET ページが実行されているセキュリティ コンテキストに書き込みアクセス許可が必要です。 ASP.NET 開発 Web サーバーは、現在のユーザー アカウントのコンテキストで実行されます。 マイクロソフトのインターネット インフォメーション サービス (IIS) web サーバーとしてを使用している場合、セキュリティ コンテキストは、IIS とその構成のバージョンに依存します。

ファイル システムに保存する場合の課題の 1 つが、ファイルの名前付け中心とするとします。 現時点では、すべてのアップロードされたファイルを保存、ページ、`~/Brochures`ディレクトリ クライアント コンピューター上のファイルと同じ名前を使用します。 ユーザー A は、名前を持つパンフレットをアップロードする場合`Brochure.pdf`、ファイルとして保存されます`~/Brochure/Brochure.pdf`します。 しばらく後でユーザー B が同じファイル名を異なるパンフレット ファイルをアップロードする場合は (`Brochure.pdf`) でしょうか。 コードである、ユーザーのファイルは、ユーザー B のアップロードで上書きされます。

ファイル名の競合を解決するための手法を数多くあります。 1 つは、既に存在する場合と同じ名前の 1 つのファイルのアップロードを禁止するためです。 ユーザー B がという名前のファイルをアップロードしようとした場合、この方法で`Brochure.pdf`システムはそのファイルを保存されず、代わりに、ファイルの名前を変更し、もう一度お試しに、ユーザー B に通知するメッセージが表示されます。 別の方法がある可能性がある一意のファイル名を使用してファイルを保存するには、[グローバル一意識別子 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)またはレコードの主キー列をデータベースから、対応する値 (アップロードが関連付けられていると仮定します。特定の行のデータ モデル)。 次のチュートリアルではこれらのオプションの詳細について説明します。

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>バイナリ データの膨大な量に関連する課題

これらのチュートリアルでは、キャプチャされたバイナリ データがサイズで中程度のサイズであると仮定します。 メガ バイトのバイナリ データ ファイルの膨大な量の操作またはより大きなチュートリアルの範囲を超えている新たな課題が導入されています。 たとえば、既定では ASP.NET が拒否されます 4 MB 以上のアップロードでこれを構成することができます、 [ `<httpRuntime>`要素](https://msdn.microsoft.com/library/e1f13641.aspx)で`Web.config`します。 IIS は、独自のファイル アップロード サイズの制限、法律すぎるです。 参照してください[ファイル アップロード サイズを IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html)詳細についてはします。 さらに、大きなファイルのアップロードにかかる時間超える可能性がある既定の ASP.NET が要求を待つ 110 秒。 大きなファイルを使用する場合に発生するメモリとパフォーマンスの問題もあります。

FileUpload コントロールは、大きなファイルのアップロードの実用的ではありません。 ファイルの内容は、サーバーにポストされると、エンド ユーザーは、アップロードの進行状況を確認する必要があります待機します。 これは、数秒でアップロードすることができますが、アップロードする分を受け取る可能性のある大きいファイルを扱うときに問題があるサイズの小さなファイルを扱うときほど、問題ではありません。 大規模なアップロードを処理するが適しているアップロード コントロールのさまざまなサード パーティ製のファイルがあるし、これらのベンダーの多くは、進行状況の指標、および ActiveX のアップロードより洗練されたユーザー エクスペリエンスを提供するマネージャーを提供します。

が大きなファイルを処理するために、アプリケーションが必要な場合は、慎重に課題を調査し、特定のニーズに適した解決策を探す必要があります。

## <a name="summary"></a>まとめ

バイナリ データをキャプチャする必要があるアプリケーションを構築すると、さまざまな課題が導入されています。 このチュートリアルでは最初の 2 つ検討しました。 バイナリ データを格納する場所を決定すると、ユーザーが web ページからのバイナリ コンテンツをアップロードすることができます。 次の 3 つのチュートリアルでは、上にアップロードされたデータをデータベース内のレコードに関連付ける方法と、データのフィールドをテキストと共にバイナリ データを表示する方法を見ていきます。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [大きな値データ型の使用](https://msdn.microsoft.com/library/ms178158.aspx)
- [FileUpload コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 の FileUpload サーバー コントロール](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [ファイルをアップロードします。](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy、「社長補佐 Leigh でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](displaying-binary-data-in-the-data-web-controls-cs.md)
