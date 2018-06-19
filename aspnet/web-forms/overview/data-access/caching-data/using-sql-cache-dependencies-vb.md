---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: SQL キャッシュ依存関係 (VB) を使用して |Microsoft ドキュメント
author: rick-anderson
description: 最も単純なキャッシュ方法では、キャッシュされたデータの一定の時間後に期限切れです。 しかし、この簡単な方法をキャッシュされたデータ maintai.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 452d856fe352ef2eb7dfcc3f3acd6aa5bcb5ae41
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878374"
---
<a name="using-sql-cache-dependencies-vb"></a>SQL キャッシュ依存関係 (VB) を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip)または[PDF のダウンロード](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> 最も単純なキャッシュ方法では、キャッシュされたデータの一定の時間後に期限切れです。 ただし、この簡単な方法では、キャッシュされたデータでは、古いデータを長時間保持されているや最新のデータが短時間に期限が切れていますが、基になるデータ ソースとの関連付けは保持されません。 その基になるデータが、SQL データベースに変更されたまでキャッシュされたデータが保持されるように、SqlCacheDependency クラスを使用することをお勧めします。 方法は、このチュートリアルで説明します。


## <a name="introduction"></a>はじめに

キャッシュの技術確認、 [、ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)と[アーキテクチャでは、データをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアルでは、時間ベースの有効期限を使用を指定した後、キャッシュからデータを削除するには期間。 この方法は、データの陳腐化に対してキャッシュのパフォーマンスの向上を分散する最も簡単な方法です。 時間の有効期限を選択して*x* 、ページの開発者 concedes 秒までの間のキャッシュ機能のみのパフォーマンスの利点をご利用いただけます*x*秒しますが、安心です自分のデータを最大値よりも長く古いできなくなります*x* (秒)。 もちろん、静的なデータの*x*で確認されたに応じて、web アプリケーションの有効期間に拡張することができます、[アプリケーションの起動時にデータをキャッシュ](caching-data-at-application-startup-vb.md)チュートリアルです。

データベースのデータをキャッシュするには、時間ベースの有効期限は、使いやすさに多くの場合、選択されているが、不適切なソリューションでは頻繁にします。 理想的には、データベースのデータは、基になるデータがデータベースに変更されてまでキャッシュされたまましかし、キャッシュを削除するとします。 この方法は、キャッシュのパフォーマンス メリットを最大化され、古いデータの時間を最小限に抑えられます。 ただし、これらの利点がありますを活用するためには、基になるデータベースのデータが変更されており、キャッシュから対応する項目を削除を知っている場所にいくつかのシステムをする必要があります。 ASP.NET 2.0 では、前にページの開発者がこのシステムの実装を担当します。

ASP.NET 2.0 には、 [ `SqlCacheDependency`クラス](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx)し、変更が発生すると、データベースにキャッシュされたアイテムを対応することを決定するために必要なインフラストラクチャを削除することができます。 基になるデータが変更されたときを判断するための 2 つの方法があります: 通知とポーリングします。 通知とポーリングの違いを紹介した後を作成、インフラストラクチャ ポーリングをサポートし、使用する方法を調査するために必要な`SqlCacheDependency`宣言内のクラスをプログラムでのシナリオです。

## <a name="understanding-notification-and-polling"></a>Understanding 通知とポーリング

データベース内のデータが変更されたときの決定に使用できる 2 つの方法があります: 通知とポーリングします。 通知には、データベースが自動的にクエリが最後に実行されるため、特定のクエリの結果が変更されたときに、ASP.NET ランタイムがエラー、この時点で、クエリに関連付けられているキャッシュされたアイテムが削除されます。 、ポーリングでは、データベース サーバーは、特定のテーブルが前回更新されたときに関する情報を保持します。 ASP.NET ランタイムが変更されているテーブルをチェックするデータベースを定期的にポーリングをキャッシュに入力されたためです。 データが変更されたこれらのテーブルがある、関連するキャッシュ アイテムを削除します。

通知オプション ポーリングよりも少ないセットアップを必要より詳細なクエリ レベルではなく、テーブル レベルでの変更を追跡するためです。 残念ながら、通知では、Microsoft SQL Server 2005 (つまり、Express 以外のエディション) の全エディションで使用できるのみです。 ただし、すべてのバージョンの Microsoft SQL Server 2005 を 7.0 からのポーリング オプションを使用できます。 これらのチュートリアルでは、SQL Server 2005 Express edition を使用するための設定およびポーリング オプションを使用してに焦点を当てます。 SQL Server 2005 の通知機能上のリソースには、このチュートリアルの最後に、さらに閲覧のセクションを参照してください。

、ポーリングでという名前のテーブルを含めるにデータベースを構成する必要があります`AspNet_SqlCacheTablesForChangeNotification`- 3 つの列を持つ`tableName`、 `notificationCreated`、および`changeId`です。 このテーブルには、web アプリケーションで、SQL キャッシュ依存関係で使用する必要があるデータを持つ各テーブルの行が含まれています。 `tableName`列中にテーブルの名前を指定する`notificationCreated`行がテーブルに追加された日時を示します。 `changeId`型の列は、 `int` 0 の最初の値を保持しています。 その値は、テーブルを変更するたびにインクリメントされます。

加え、`AspNet_SqlCacheTablesForChangeNotification`テーブル、データベースもする必要があります SQL キャッシュ依存関係に表示されるテーブルの各トリガーを含めます。 これらのトリガーは行が挿入、更新、または削除されるたびに実行され、表 s をインクリメント`changeId`値`AspNet_SqlCacheTablesForChangeNotification`です。

ASP.NET ランタイムが、現在は追跡`changeId`を使用してデータをキャッシュすると、テーブルの`SqlCacheDependency`オブジェクト。 データベースが定期的にチェックと任意`SqlCacheDependency`オブジェクト`changeId`、データベース内の値とは異なりますが、異なるされてから解放される`changeId`ことがありますが変更されたテーブルにデータがキャッシュされるため値を示します。

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>手順 1: 探索、`aspnet_regsql.exe`コマンド ライン プログラム

ポーリング アプローチでは、データベースが上記で説明したインフラストラクチャを格納するセットアップをする必要があります定義済みのテーブル (`AspNet_SqlCacheTablesForChangeNotification`)、いくつかのストアド プロシージャ、および各 SQL キャッシュ依存関係が、web での使用可能性があるテーブルのトリガー。アプリケーション。 これらのテーブル、ストアド プロシージャ、およびトリガーは、コマンド ライン プログラムで作成できます`aspnet_regsql.exe`に存在する、`$WINDOWS$\Microsoft.NET\Framework\version`フォルダーです。 作成する、`AspNet_SqlCacheTablesForChangeNotification`テーブルと関連するストアド プロシージャ、コマンドラインから、次を実行します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> 指定されたデータベース ログインがあります。 これらのコマンドを実行する、 [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx)と[ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx)ロール。 データベースに送信する T-SQL を検査する、`aspnet_regsql.exe`コマンド ライン プログラムを参照してください[このブログ記事](http://scottonwriting.net/sowblog/posts/10709.aspx)です。


たとえば、Microsoft SQL Server データベースをポーリングするためのインフラストラクチャを追加するという名前の`pubs`という名前のデータベース サーバーで`ScottsServer`Windows 認証を使用して、適切なディレクトリに移動し、コマンド ライン コマンドを入力します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

データベース レベルのインフラストラクチャが追加した後、SQL キャッシュ依存関係で使用されるこれらのテーブルにトリガーを追加する必要があります。 使用して、`aspnet_regsql.exe`コマンド ライン プログラムをもう一度が、テーブルを使用して名前を指定して、`-t`スイッチおよび使用する代わりに、`-ed`使用を切り替える`-et`、次のように。


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

トリガーを追加する、`authors`と`titles`でテーブル、`pubs`上のデータベース`ScottsServer`を使用します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

このチュートリアルでは追加、トリガーを`Products`、 `Categories`、および`Suppliers`テーブル。 手順 3. で特定のコマンドライン構文に紹介します。

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>手順 2: で Microsoft SQL Server 2005 Express Edition データベースを参照します。`App_Data`

`aspnet_regsql.exe`コマンド ライン プログラムでは、ポーリングのために必要なインフラストラクチャを追加するために、データベースとサーバー名が必要です。 存在する Microsoft SQL Server 2005 Express のデータベースのデータベースとサーバーの名前は何ですが、`App_Data`フォルダーですか? データベースとサーバーの名前とは何かを検出するのではなくすればにデータベースをアタッチする最も簡単な方法を見つけたら、`localhost\SQLExpress`データベース インスタンスとデータを使用して、名前の変更[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)です。 場合は、コンピューターにインストールされている SQL Server 2005 の完全なバージョンの 1 つある場合は、し、可能性がありますが既にある SQL Server Management Studio がコンピューターにインストールされています。 無料をダウンロードすることができる場合は、Express edition がある場合のみ、 [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)です。

Visual Studio を終了して開始します。 次に、SQL Server Management Studio を開くしに接続する、`localhost\SQLExpress`サーバーが Windows 認証を使用します。


![Localhost \sqlexpress サーバーにアタッチします。](using-sql-cache-dependencies-vb/_static/image1.gif)

**図 1**: にアタッチ、`localhost\SQLExpress`サーバー


サーバーに接続すると、Management Studio は、サーバーを表示して、データベース、セキュリティ、およびなどのサブフォルダーがあります。 データベース フォルダーを右クリックし、アタッチ オプションを選択します。 データベースのアタッチ ダイアログが表示されます (図 2 を参照してください)。 [追加] ボタンをクリックし、選択、`NORTHWND.MDF`データベース フォルダーで、web アプリケーションの`App_Data`フォルダーです。


[![NORTHWND をアタッチします。App_Data フォルダーから MDF データベース](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**図 2**: アタッチ、`NORTHWND.MDF`からデータベース、`App_Data`フォルダー ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image2.png))


これは、データベース フォルダーにデータベースを追加します。 データベース名、データベース ファイルへの完全パスであるか、完全なパスの先頭に、 [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)です。 Aspnet を使用する場合に、この時間のかかるデータベース名を入力しなくて済むようにする\_regsql.exe コマンド ライン ツールだけデータベースを右クリックしてよりわかりやすい名前に、データベースに接続されている名前を変更して、名前を変更します。 I ve DataTutorials にデータベースの名前を変更します。


![アタッチされたデータベースをよりわかりやすい名前に変更します。](using-sql-cache-dependencies-vb/_static/image3.gif)

**図 3**: アタッチされたデータベースをよりわかりやすい名前に変更


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>手順 3: Northwind データベースへポーリング インフラストラクチャを追加します。

添付したこと、`NORTHWND.MDF`からデータベース、`App_Data`ポーリング インフラストラクチャを追加する準備ができたらフォルダーです。 DataTutorials にデータベースの名前を変更していると仮定すると、次の 4 つのコマンドを実行します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

これら 4 つのコマンドを実行すると、Management Studio でデータベース名を右クリックし、タスク サブメニューに移動およびデタッチを選択します。 Management Studio を終了し、Visual Studio を閉じて再度開きます。

Visual Studio が再度開くと後、サーバー エクスプ ローラーを使用してデータベースにドリルダウンします。 新しいテーブルに注意してください (`AspNet_SqlCacheTablesForChangeNotification`)、新しいストアド プロシージャ、およびトリガーで、 `Products`、 `Categories`、および`Suppliers`テーブル。


![データベースが含まれています、ポーリングのために必要なインフラストラクチャには](using-sql-cache-dependencies-vb/_static/image4.gif)

**図 4**: データベースが含まれています、ポーリングのために必要なインフラストラクチャには


## <a name="step-4-configuring-the-polling-service"></a>手順 4: ポーリング サービスの構成

データベースに必要なテーブル、トリガー、およびストアド プロシージャを作成した後、最後はを通じて実行されるポーリング サービスを構成する`Web.config`ミリ秒単位でデータベースを使用してポーリング間隔を指定することによってです。 次のマークアップは、1 秒ごとに、Northwind データベースをポーリングします。


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name`値で、`<add>`要素 (NorthwindDB) 人間が判読できる名前を特定のデータベースに関連付けます。 SQL キャッシュ依存関係を使用する場合は、ここで定義された、キャッシュされたデータを基になっているテーブルだけでなく、データベース名を参照してくださいお必要があります。 使用する方法を見てみましょう、 `SqlCacheDependency` SQL キャッシュ依存関係をプログラムによって関連付けるクラスは手順 6. でデータをキャッシュします。

ポーリング システムで定義されているデータベースに接続、SQL キャッシュ依存関係が確立されると、`<databases>`要素すべて`pollTime`(ミリ秒) を実行し、`AspNet_SqlCachePollingStoredProcedure`ストアド プロシージャです。 手順 3 を使用してこのストアド プロシージャで追加されたバックアップを作成、`aspnet_regsql.exe`コマンド ライン ツールに返されます、`tableName`と`changeId`内の各レコード値`AspNet_SqlCacheTablesForChangeNotification`です。 古い SQL キャッシュ依存関係は、キャッシュから削除されます。

`pollTime`設定パフォーマンスとデータの陳腐化の間のトレードオフが導入されています。 小さな`pollTime`値には、データベースへの要求の数が増加しますより迅速にキャッシュから古いデータを削除します。 大規模な`pollTime`値は、データベース要求の数を削減しますが、バックエンドのデータが変更されたときと、関連するキャッシュ アイテムを削除する時期の間の遅延が増加します。 幸いにも、データベースの要求は単純なストアド プロシージャを実行単純で軽量なテーブルから少数の行を返す s。 別のテスト操作を行いますが、`pollTime`データベース アプリケーションのアクセスとデータの陳腐化の最適なバランスを検索する値。 最小`pollTime`指定できる値は 500 です。

> [!NOTE]
> 上記の例は、1 つ`pollTime`値で、`<sqlCacheDependency>`要素が必要に応じてを指定できます、`pollTime`値で、`<add>`要素。 これは、機能は、指定されたデータベースが複数存在し、データベースごとのポーリング間隔をカスタマイズする場合に便利です。


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>手順 5: は、宣言によって SQL キャッシュ依存関係の操作

手順 1 ~ 4 では、必要なデータベース インフラストラクチャをセットアップして、ポーリング システムを構成する方法について説明しました。 このインフラストラクチャを導入するには、プログラムまたは宣言型のいずれかの手法を使用して、関連の SQL キャッシュ依存と共に項目データ キャッシュに追加おことようになりましたできます。 この手順では SQL キャッシュ依存関係を宣言して使用する方法について確認します。 手順 6. でプログラムによる方法について見ていきます。

[、ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)チュートリアルは、ObjectDataSource の宣言のキャッシュ機能を探索します。 設定するだけで、`EnableCaching`プロパティを`True`と`CacheDuration`プロパティがある時間間隔を、ObjectDataSource を指定した期間その基になるオブジェクトから返されるデータが自動的にはキャッシュされます。 ObjectDataSource では、1 つまたは複数の SQL キャッシュ依存関係も使用できます。

SQL キャッシュ依存関係を宣言して使用を示すためには、開く、 `SqlCacheDependencies.aspx`  ページで、`Caching`フォルダーと、ツールボックスからデザイナーにドラッグする GridView。 集合 GridView s`ID`に`ProductsDeclarative`し、そのスマート タグからという名前の新しい ObjectDataSource にバインドする選択`ProductsDataSourceDeclarative`です。


[![ProductsDataSourceDeclarative をという名前の新しい ObjectDataSource を作成します。](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`ProductsDataSourceDeclarative`([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image4.png))


構成を使用する ObjectDataSource、`ProductsBLL`クラスし、ドロップダウン リストの選択 タブで設定を`GetProducts()`です。 更新プログラム タブで、選択、 `UpdateProduct` - 3 つの入力パラメーターを持つオーバー ロード`productName`、 `unitPrice`、および`productID`です。 挿入および削除の各タブで (なし) ドロップダウン リストを設定します。


[![次の 3 つの入力パラメーターを持つ UpdateProduct オーバー ロードを使用します。](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**図 6**: UpdateProduct のオーバー ロードを使用して、次の 3 つの入力パラメーターを持つ ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image6.png))


[![挿入および削除のタブの (なし) ドロップダウン リストを設定します。](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**図 7**: (None) にドロップ ダウン リストを挿入および削除のタブの設定 ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image8.png))


データ ソース構成ウィザードを完了すると、Visual Studio と作成されます BoundFields CheckBoxFields gridview の各データ フィールドの。 すべてのフィールドを削除するが、 `ProductName`、`CategoryName`と`UnitPrice`、し、必要に応じて、これらのフィールドを書式設定します。 GridView s のスマート タグからページングを有効にする、並べ替えを有効にするには、および編集を有効にするチェック ボックスを確認します。 Visual Studio には、ObjectDataSource s は設定`OldValuesParameterFormatString`プロパティを`original_{0}`です。 GridView s の編集機能適切に機能するためには、宣言構文またはその既定値に戻すセット全体からこのプロパティを削除するか`{0}`です。

最後に、GridView とセット上のラベルの Web コントロールを追加、`ID`プロパティを`ODSEvents`とその`EnableViewState`プロパティを`False`です。 これらの変更を加えたら、ページ s の宣言型マークアップは、次のようになります。 注されるさまざまな見た目のカスタマイズは不要な SQL キャッシュ依存関係の機能を示す GridView フィールドを作成しています。


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

次に、ObjectDataSource s のイベント ハンドラーを作成`Selecting`イベントで、次のコードを追加、および。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

注意してください、ObjectDataSource の`Selecting`イベントは、その基になるオブジェクトからデータを取得する場合にのみ発生します。 場合は、ObjectDataSource 独自のキャッシュからデータにアクセスする、このイベントは起動しません。

ここで、ブラウザーからこのページを参照してください。 以降お ve まだキャッシュを実装する任意、ページ、並べ替え、または、グリッド ページを編集するたびにする必要があります、テキスト、表示するイベントが発生した場合、図 8 に示すよう。


[![GridView をページングすると、編集、または Sorted、ObjectDataSource s を選択するとイベントが発生させる](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**図 8**:、ObjectDataSource s`Selecting`イベント発生の各時間が GridView でページングが、編集、または Sorted ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image10.png))


説明したとおり、 [、ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)チュートリアルでは、設定、`EnableCaching`プロパティを`True`ObjectDataSource によって指定された期間に対して、データをキャッシュすると、その`CacheDuration`プロパティです。 ObjectDataSource をも、 [ `SqlCacheDependency`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)パターンを使用して、キャッシュされたデータに 1 つまたは複数の SQL キャッシュ依存関係を追加します。


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

ここで*databaseName*で指定されているデータベースの名前を指定します、`name`の属性、`<add>`内の要素`Web.config`、および*tableName*データベース テーブルの名前を指定します。 たとえば、データを無期限にキャッシュする ObjectDataSource を作成するに基づいて Northwind s に対して、SQL キャッシュ依存関係`Products`テーブルで、設定、ObjectDataSource s`EnableCaching`プロパティを`True`とその`SqlCacheDependency`プロパティをNorthwindDB:Products です。

> [!NOTE]
> SQL キャッシュ依存関係を使用することができます*と*を設定して時間ベースの有効期限`EnableCaching`に`True`、`CacheDuration`時間間隔と`SqlCacheDependency`データベースとテーブル名にします。 時間ベースの有効期限に達したときに、またはポーリング システム ノート、基になるデータベースのデータが変更されたこと、どちらかになるときに、ObjectDataSource はそのデータを削除します。


GridView `SqlCacheDependencies.aspx` - 2 つのテーブルからデータが表示されます`Products`と`Categories`(製品 s`CategoryName`を介して取得されたフィールドは、`JOIN`で`Categories`)。 そのため、2 つの SQL キャッシュ依存関係を指定する: NorthwindDB:Products です。NorthwindDB:Categories です。


[![製品およびカテゴリを SQL キャッシュ依存関係を使用するキャッシュをサポートするために、ObjectDataSource を構成します。](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**図 9**: 上の構成サポートのキャッシュを使用して SQL キャッシュ依存関係を ObjectDataSource`Products`と`Categories`([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image12.png))


キャッシュをサポートする ObjectDataSource を構成すると、ブラウザーを使ってページを再表示します。 テキストを選択するとイベントが起動される最初のページ アクセスで表示する必要がありますが、必要がありますと解消ページング、並べ替え、または編集 または キャンセル ボタンをクリックします。 これは、データがキャッシュに読み込む、ObjectDataSource s、後に残っているのでがあるまで、`Products`または`Categories`テーブルが変更されるものや、GridView 経由でデータを更新します。

テキストが新しいブラウザー ウィンドウを開き、編集、挿入、および削除のセクションで基本チュートリアルに移動、グリッドのページングとするイベントがないことに注意が呼び出された後 (`~/EditInsertDelete/Basics.aspx`)。 名前または製品の価格を更新します。 次からの最初のブラウザーのウィンドウに表示データの別のページ、グリッドの並べ替えまたは行の編集 をクリックします。 データされた基になるデータベースの変更 (図 10 を参照してください)、今回は、発生するイベントは再び表示されます。 テキストが表示されない場合、しばらく待ってからもう一度やり直してください。 ポーリング サービスがへの変更をチェックすることに注意してください、`Products`テーブルすべて`pollTime`ミリ秒、基になるデータが更新されたときと、キャッシュされたデータを削除する時期の遅延があるようにします。


[![Products テーブルの変更にキャッシュされている製品のデータを削除します](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**図 10**: Products テーブルを変更する製品のキャッシュ データを削除します ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>手順 6: をプログラムで使用、`SqlCacheDependency`クラス

[アーキテクチャでは、データをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアルを指定したキャッシュは、ObjectDataSource を密に結合ではなく、アーキテクチャの別のキャッシュ レイヤーを使用する利点を調べる。 そのチュートリアルで作成しました、`ProductsCL`クラスをプログラムでのデータ キャッシュ機能を示すです。 キャッシュ レイヤーでの SQL キャッシュ依存関係を利用するを使用して、`SqlCacheDependency`クラスです。

ポーリング システムで、`SqlCacheDependency`オブジェクトは、特定のデータベースとテーブルのペアを関連付ける必要があります。 たとえば、次のコードを作成、`SqlCacheDependency`オブジェクトは、Northwind データベース %s に基づく`Products`テーブル。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

2 つの入力パラメーター、 `SqlCacheDependency` s のコンス トラクターは、それぞれデータベースとテーブルの名前。 ObjectDataSource s 使用するような`SqlCacheDependency`プロパティは、使用するデータベース名で指定された値と同じです、`name`の属性、`<add>`内の要素`Web.config`です。 テーブル名は、データベース テーブルの実際の名前です。

関連付けるには、`SqlCacheDependency`データ キャッシュに追加された項目を含むのいずれかの操作を使用して、`Insert`依存関係を受け入れるメソッドのオーバー ロードします。 次のコードを追加*値*に無期限の期間のデータ キャッシュに関連付けます、`SqlCacheDependency`上、`Products`テーブル。 要するに、*値*メモリ制約によってそれが削除されるまで、またはをポーリング システムが検出されたため、キャッシュに残りますが、`Products`テーブルがキャッシュされてから変更します。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

キャッシュ レイヤー s`ProductsCL`クラスが現在のデータをキャッシュ、 `Products` 60 秒間の時間ベースの有効期限を使用するテーブルします。 S SQL キャッシュ依存関係を代わりに使用するように、このクラスを更新できるようにします。 `ProductsCL`クラスの`AddCacheItem`データをキャッシュに追加するのには、メソッドには現在、次のコードが含まれています。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

使用するには、このコードを更新、`SqlCacheDependency`オブジェクトの代わりに、`MasterCacheKeyArray`依存関係をキャッシュします。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

この機能をテストするには、GridView、ページを追加、既存の下に`ProductsDeclarative`GridView です。 この新しい GridView s 設定`ID`に`ProductsProgrammatic`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`ProductsDataSourceProgrammatic`です。 構成を使用する ObjectDataSource、`ProductsCL`クラス、ドロップダウン リストの選択とに更新 タブの設定`GetProducts`と`UpdateProduct`、それぞれします。


[![構成、ObjectDataSource ProductsCL クラスを使用するには](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**図 11**: 構成を使用する ObjectDataSource、`ProductsCL`クラス ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image16.png))


[![GetProducts メソッドを選択 タブのドロップダウン リストから選択します。](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**図 12**: 選択、 `GetProducts`  タブの選択のドロップダウン リストからメソッド ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image18.png))


[![更新プログラム タブのドロップダウン リストから UpdateProduct 方法を選択します。](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**図 13**: 更新 タブのドロップダウン リストから UpdateProduct 方法を選択 ([フルサイズのイメージを表示するをクリックして](using-sql-cache-dependencies-vb/_static/image20.png))


データ ソース構成ウィザードを完了すると、Visual Studio と作成されます BoundFields CheckBoxFields gridview の各データ フィールドの。 このページに追加された最初の GridView にすべてのフィールドを削除するようが`ProductName`、 `CategoryName`、および`UnitPrice`、し、必要に応じて、これらのフィールドを書式設定します。 GridView s のスマート タグからページングを有効にする、並べ替えを有効にするには、および編集を有効にするチェック ボックスを確認します。 同様、 `ProductsDataSourceDeclarative` ObjectDataSource、Visual Studio は設定、 `ProductsDataSourceProgrammatic` ObjectDataSource s`OldValuesParameterFormatString`プロパティを`original_{0}`です。 GridView s の編集機能を適切に機能は、このプロパティを設定するためにバックアップ`{0}`(または宣言構文から、プロパティの割り当てを完全に削除) します。

これらのタスクを完了すると、結果として得られる GridView と ObjectDataSource 宣言型マークアップは、次のようになります。


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

SQL をテストするキャッシュ レイヤーでキャッシュの依存関係にブレークポイントを設定、`ProductCL`クラスの`AddCacheItem`メソッドと、デバッグを開始します。 アクセスするときに`SqlCacheDependencies.aspx`データは最初に要求され、キャッシュに格納と、ブレークポイントをヒットする必要があります。 次に、GridView で別のページに移動するか、並べ替える列のいずれか。 これが原因で、そのデータを再クエリする GridView がされてから、キャッシュにデータを検出する必要があります、`Products`データベース テーブルが変更されていません。 データが繰り返しが見つからない場合、キャッシュのための十分なメモリが使用可能なコンピューターのことを確認してからやり直してください。

ページング GridView のいくつかのページを後に 2 番目のブラウザー ウィンドウを開き、編集、挿入、および削除のセクションで基本チュートリアルへ移動 (`~/EditInsertDelete/Basics.aspx`)。 Products テーブルからレコードを更新し、次に、最初のブラウザー ウィンドウから、新しいページを表示または並べ替えのヘッダーのいずれかをクリックします。

このシナリオでは、次の 2 つのいずれかが表示されます。 キャッシュされたデータがデータベース内の変更により削除されることを示す、いずれかのブレークポイントにヒットします。または、ブレークポイントがヒットしない、つまり`SqlCacheDependencies.aspx`が古いデータを表示されているようになりました。 ブレークポイントはヒットしませんが、データの変更によって、ポーリング サービスがまだ起動されないので、可能性があります。 ポーリング サービスがへの変更をチェックすることに注意してください、`Products`テーブルすべて`pollTime`ミリ秒、基になるデータが更新されたときと、キャッシュされたデータを削除する時期の遅延があるようにします。

> [!NOTE]
> この遅延が GridView でを使用して、製品のいずれかの編集時に表示される可能性が高く`SqlCacheDependencies.aspx`です。 [アーキテクチャでは、データをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアルが追加されました、`MasterCacheKeyArray`キャッシュ データから編集されていることを確認する依存関係、`ProductsCL`クラスの`UpdateProduct`メソッドは、キャッシュから削除されました。 ただし、置き換えましたこのキャッシュの依存関係を変更する場合、`AddCacheItem`この手順の前半のメソッドとそのため、`ProductsCL`ポーリング システム ノートへの変更になるまでにキャッシュされたデータを表示するクラスは引き続き、`Products`テーブル。 再導入する方法をお見せ、`MasterCacheKeyArray`手順 7. で依存関係をキャッシュします。


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>手順 7: キャッシュされたアイテムと複数の依存関係の関連付け

注意してください、`MasterCacheKeyArray`キャッシュの依存関係がいることを確認するために使用*すべて*内に関連付けられている任意の 1 つの項目が更新されたときに、製品関連のデータをキャッシュから削除します。 たとえば、`GetProductsByCategoryID(categoryID)`メソッド キャッシュ`ProductsDataTables`インスタンスごとに一意な*categoryID*値。 これらのオブジェクトが削除される場合、`MasterCacheKeyArray`キャッシュの依存関係、他のユーザーがも削除されることを確認します。 このキャッシュの依存関係なく、キャッシュされたデータが変更されたときに、可能性がその他の製品のキャッシュされたデータが古くなる可能性があります。 その結果に維持するということが重要、 `MasterCacheKeyArray` SQL キャッシュ依存関係を使用する場合は、依存関係をキャッシュします。 ただし、データはキャッシュ s`Insert`メソッドだけでは、単一の依存関係オブジェクト。

さらに、SQL キャッシュ依存関係を操作するときに依存関係として複数のデータベース テーブルを関連付けることがあります。 たとえば、`ProductsDataTable`でキャッシュされた、`ProductsCL`クラスには、各製品カテゴリと仕入先名が含まれていますが、`AddCacheItem`メソッドのみを使用して、依存関係の`Products`します。 このような状況で、ユーザーがカテゴリや供給業者の名前を更新する場合にキャッシュされている製品のデータはキャッシュに残りとが最新でいます。 キャッシュされている製品のデータに依存するようにするため、だけでなく、`Products`テーブルが、`Categories`と`Suppliers`テーブルもします。

[ `AggregateCacheDependency`クラス](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx)キャッシュ項目に複数の依存関係を関連付けるための手段を提供します。 まずを作成して、`AggregateCacheDependency`インスタンス。 使用して依存関係のセットを次に、追加、 `AggregateCacheDependency` s`Add`メソッドです。 その後、データ キャッシュにアイテムを挿入するときに渡す、`AggregateCacheDependency`インスタンス。 ときに*任意*の`AggregateCacheDependency`インスタンスの依存関係が変更、キャッシュされた項目は削除されます。

更新されたコードを次に示します、`ProductsCL`クラスの`AddCacheItem`メソッドです。 メソッドを作成、`MasterCacheKeyArray`キャッシュ依存関係と共に`SqlCacheDependency`オブジェクトに対する、 `Products`、 `Categories`、および`Suppliers`テーブル。 これらはすべてを組み合わせて 1 つ`AggregateCacheDependency`という名前のオブジェクト`aggregateDependencies`に渡されたし、`Insert`メソッドです。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

この新しいコードをテストします。ように変わります、 `Products`、 `Categories`、または`Suppliers`テーブルが削除するキャッシュされたデータに発生します。 さらに、`ProductsCL`クラス s `UpdateProduct` GridView を使用して製品を編集するときに呼び出される、メソッドが削除、`MasterCacheKeyArray`これにより、キャッシュされた依存関係をキャッシュ`ProductsDataTable`削除して、次回の再取得するデータ要求。

> [!NOTE]
> SQL キャッシュ依存関係はでも使用できます[出力キャッシュ](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)です。 この機能のデモについてを参照してください: [SQL Server で ASP.NET 出力キャッシュを使用して](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)です。


## <a name="summary"></a>まとめ

データベースのデータをキャッシュする場合、データは理想的にはまでキャッシュに、データベースで変更されているです。 ASP.NET 2.0 では、SQL キャッシュ依存関係を作成して宣言とプログラムの両方のシナリオで使用します。 このアプローチの課題の 1 つは、データが変更されたときを検出するのにです。 Microsoft SQL Server 2005 の完全なバージョンでは、クエリ結果が変更されたときに、アプリケーションに警告できます通知機能を提供します。 Express エディションの SQL Server 2005 と SQL Server の以前のバージョンでは、ポーリング システムを代わりに使用する必要があります。 さいわい、ポーリングのために必要なインフラストラクチャを設定することは簡単です。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Microsoft SQL Server 2005 のクエリ通知の使用](https://msdn.microsoft.com/library/ms175110.aspx)
- [クエリ通知を作成します。](https://msdn.microsoft.com/library/ms188669.aspx)
- [ASP.NET でのキャッシュ、`SqlCacheDependency`クラス](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server の登録ツール (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [概要 `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Marko Rangel、Teresa マーフィー Hilton Giesenow がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](caching-data-at-application-startup-vb.md)
