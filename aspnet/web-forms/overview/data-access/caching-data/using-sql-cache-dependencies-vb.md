---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: SQL キャッシュ依存関係 (VB) を使用して |Microsoft Docs
author: rick-anderson
description: 最も単純なキャッシュ戦略では、キャッシュされたデータを一定の時間後に期限切れを許可です。 この単純な方法はことにキャッシュされたデータ maintai を意味しています.
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: f47cc7c1fd4fd0d1e41bef31a2e68dd34393d52e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829968"
---
<a name="using-sql-cache-dependencies-vb"></a>SQL キャッシュ依存関係 (VB) を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip)または[PDF のダウンロード](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> 最も単純なキャッシュ戦略では、キャッシュされたデータを一定の時間後に期限切れを許可です。 ただし、この単純なアプローチでは、キャッシュされたデータには長時間保持されている古いデータや最新のデータが短時間に期限が切れていますが、基になるデータ ソースとの関連付けが保持されません。 SQL database には、基になるデータが変更されましたまでキャッシュされたデータが保持されるように SqlCacheDependency クラスを使用することをお勧めします。 方法は、このチュートリアルで説明します。


## <a name="introduction"></a>はじめに

キャッシュの技術確認、 [ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)と[アーキテクチャでデータをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアルは、指定した後、キャッシュからデータを削除する時間ベースの有効期限を使用期間。 この方法は、データ界整合性制約に対してキャッシュのパフォーマンスの向上を分散する最も簡単な方法です。 時間の有効期限を選択して*x*秒、ページ開発者 concedes のみのキャッシュのパフォーマンスの利点をご利用いただくに*x*秒しますが、自分のデータが決してなる古いを超えて最大に簡単な rest できます*x*秒。 もちろん、静的なデータの*x*で検査された、web アプリケーションの有効期間を拡張できます、[アプリケーションの起動時にデータをキャッシュ](caching-data-at-application-startup-vb.md)チュートリアル。

データベースのデータをキャッシュする場合、時間ベースの有効期限はその使いやすさに多くの場合、選択されているが、不適切なソリューションでは頻繁に。 理想的には、データベースのデータがデータベースでは、基になるデータが変更されたまでキャッシュされたままその後は、キャッシュも削除します。 この方法では、キャッシュのパフォーマンスの利点を最大化し、古いデータの継続時間を最小限に抑えられます。 ただし、これらの利点がありますを活用するためには、基になるデータベースのデータが変更されており、キャッシュから対応する項目の削除を認識している場所にいくつかのシステムをある必要があります。 ASP.NET 2.0 では、前にページの開発者がこのシステムを実装する責任を負います。

ASP.NET 2.0 には、 [ `SqlCacheDependency`クラス](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx)変更がデータベースで発生したキャッシュされたアイテムを対応するように場合を判断するために必要なインフラストラクチャを削除できます。 基になるデータが変更されたときを決定するための 2 つの手法があります。 通知とポーリングします。 通知とポーリング間の違いを説明するには、後に作成しますインフラストラクチャ ポーリングをサポートし、使用する方法に必要な`SqlCacheDependency`宣言型のクラスとシナリオのプログラムを使用します。

## <a name="understanding-notification-and-polling"></a>についての通知とポーリング

データベースのデータが変更されたときの判断に使用できる 2 つの手法があります。 通知とポーリングします。 通知では、データベースが自動的にクエリが最後に実行されるため、特定のクエリの結果が変更されたときに、ASP.NET ランタイムがエラーでは、この時点で、クエリに関連付けられているキャッシュされたアイテムが削除されます。 ポーリングでは、データベース サーバーは、特定のテーブルが前回更新されたときに関する情報を保持します。 ASP.NET ランタイムは、テーブルが変更内容を確認するデータベースを定期的にポーリングをキャッシュに入力されたためです。 データが変更されたこれらのテーブルでは、削除された、関連するキャッシュ項目があります。

通知オプション ポーリングよりも少ないセットアップが必要ですより詳細なので、テーブル レベルではなく、クエリ レベルでの変更を追跡します。 残念ながら、通知では、Microsoft SQL Server 2005 (つまり、Express 以外のエディション) の全エディションで使用できるのみです。 ただし、ポーリング オプションは、すべてのバージョンの Microsoft SQL Server 2005 に 7.0 から使用できます。 これらのチュートリアルでは、SQL Server 2005 Express edition を使用するための設定およびポーリング オプションを使用して焦点を当てます。 リソースは、SQL Server 2005 通知機能をさらには、このチュートリアルの最後には、「さらに資料」セクションを参照してください。

という名前のテーブルを含める、ポーリングとデータベースを構成する必要があります`AspNet_SqlCacheTablesForChangeNotification`- 3 つの列を持つ`tableName`、 `notificationCreated`、および`changeId`します。 このテーブルには、web アプリケーションでの SQL キャッシュ依存関係で使用する必要があるデータを持つ各テーブルの行が含まれています。 `tableName`列中にテーブルの名前を指定する`notificationCreated`テーブルに行が追加された日時を示します。 `changeId`型の列は、 `int` 0 の最初の値を持つとします。 その値は、テーブルを変更するたびにインクリメントされます。

加え、`AspNet_SqlCacheTablesForChangeNotification`テーブル、データベースも含める必要がありますが表示されるテーブルの各トリガー SQL キャッシュ依存関係。 これらのトリガーが行の挿入、更新、または削除されるたびに実行され、表 s をインクリメント`changeId`値`AspNet_SqlCacheTablesForChangeNotification`します。

ASP.NET ランタイムは、現在を追跡`changeId`テーブルを使用してデータをキャッシュする場合の`SqlCacheDependency`オブジェクト。 データベースが定期的に確認と、`SqlCacheDependency`オブジェクト`changeId`、データベース内の値とは異なります、異なる以降が削除されます`changeId`値のことがありますが変更されたテーブルにデータがキャッシュされてからことを示します。

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>手順 1: 調査、`aspnet_regsql.exe`コマンド ライン プログラム

ポーリング アプローチではデータベースは上記で説明したインフラストラクチャを含むをセットアップする必要があります: 定義済みのテーブル (`AspNet_SqlCacheTablesForChangeNotification`)、いくつかのストアド プロシージャ、および web での SQL キャッシュ依存関係で使用できるテーブルの各トリガーアプリケーション。 これらのテーブル、ストアド プロシージャ、およびトリガーは、コマンド ライン プログラムで作成できます`aspnet_regsql.exe`は、`$WINDOWS$\Microsoft.NET\Framework\version`フォルダー。 作成する、`AspNet_SqlCacheTablesForChangeNotification`テーブルと関連付けられているストアド プロシージャ、コマンドラインから次を実行します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> 指定されたデータベースのログインがあります。 これらのコマンドを実行する、 [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx)と[ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx)ロール。 T-SQL では、データベースに送信を確認する、`aspnet_regsql.exe`コマンド ライン プログラムを参照してください[このブログ エントリ](http://scottonwriting.net/sowblog/posts/10709.aspx)します。


たとえば、Microsoft SQL Server データベースにポーリングするためのインフラストラクチャを追加するという`pubs`という名前のデータベース サーバー `ScottsServer` Windows 認証を使用して、適切なディレクトリに移動し、コマンドラインから入力します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

データベース レベルのインフラストラクチャが追加されると、SQL キャッシュ依存関係で使用されるこれらのテーブルにトリガーを追加する必要があります。 使用して、`aspnet_regsql.exe`コマンド ライン プログラムをもう一度が使用してテーブル名を指定、`-t`スイッチを使用してではなく、`-ed`使用を切り替える`-et`、次のように。


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

トリガーを追加する、`authors`と`titles`でテーブル、`pubs`上のデータベース`ScottsServer`を使用します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

このチュートリアルでは、トリガーを追加、 `Products`、 `Categories`、および`Suppliers`テーブル。 手順 3. で特定のコマンドラインの構文に注目します。

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>手順 2: で Microsoft SQL Server 2005 Express Edition データベースを参照します。`App_Data`

`aspnet_regsql.exe`コマンド ライン プログラムでは、ポーリングのために必要なインフラストラクチャを追加するには、データベースとサーバー名が必要です。 新機能に存在する Microsoft SQL Server 2005 Express データベースのデータベースとサーバーの名前が、`App_Data`フォルダーですか? データベースとサーバーの名前とは何かを検出するのではなくは行っていませんが、データベースをアタッチする最も簡単な方法が、`localhost\SQLExpress`データベース インスタンスとデータを使用して、名前を変更[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)します。 コンピューターにインストールされている SQL Server 2005 の完全なバージョンのいずれかがある場合、可能性がありますが既にある SQL Server Management Studio がコンピューターにインストールします。 無料ダウンロードすることができます、Express edition のみの場合[Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)します。

Visual Studio を閉じることで開始します。 次に接続する SQL Server Management Studio を開き、`localhost\SQLExpress`サーバーが Windows 認証を使用します。


![Localhost \sqlexpress サーバーにアタッチします。](using-sql-cache-dependencies-vb/_static/image1.gif)

**図 1**: にアタッチ、`localhost\SQLExpress`サーバー


サーバーに接続したら、Management Studio は、サーバーを表示して、データベース、セキュリティ、およびその他のサブフォルダーがあります。 データベース フォルダーを右クリックし、アタッチ オプションを選択します。 データベースのアタッチ ダイアログが表示されます (図 2 を参照してください)。 追加ボタンをクリックし、 `NORTHWND.MDF` database フォルダーで、web アプリケーションの s`App_Data`フォルダー。


[![NORTHWND をアタッチします。App_Data フォルダーから MDF データベース](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**図 2**: アタッチ、`NORTHWND.MDF`からデータベース、`App_Data`フォルダー ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image2.png))。


これにより、データベース フォルダーに、データベースが追加されます。 データベース名、データベース ファイルへの完全パスであるか、完全なパスの先頭に、 [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)します。 Aspnet を使用する場合に、この時間のかかるデータベース名を入力しなくてもすむようにする\_regsql.exe コマンド ライン ツール、名前の変更だけデータベースを右クリックしてよりわかりやすい名前にデータベースをアタッチおよび選択する名前を変更します。 Ve DataTutorials に自分のデータベースの名前を変更します。


![わかりやすい名前に、アタッチされたデータベースの名前を変更します。](using-sql-cache-dependencies-vb/_static/image3.gif)

**図 3**: やすい名前に、アタッチされたデータベースの名前を変更


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>手順 3: Northwind データベースへのポーリングのインフラストラクチャの追加

これで、私たちがアタッチされている、`NORTHWND.MDF`からデータベース、`App_Data`ポーリング インフラストラクチャを追加する準備ができたらフォルダー、します。 DataTutorials にデータベースの名前を変更している、仮定は、次の 4 つのコマンドを実行します。


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

これら 4 つのコマンドを実行した後は、Management Studio でデータベース名を右クリックしてのタスク サブメニューに移動し、デタッチを選択します。 Management Studio をいったん閉じ、Visual Studio。

Visual Studio に開き直すと後、は、サーバー エクスプ ローラーを使用して、データベースにドリルダウンします。 新しいテーブルに注意してください (`AspNet_SqlCacheTablesForChangeNotification`)、新しいストアド プロシージャ、およびトリガーで、 `Products`、 `Categories`、および`Suppliers`テーブル。


![データベースが、ポーリングのために必要なインフラストラクチャが含まれています](using-sql-cache-dependencies-vb/_static/image4.gif)

**図 4**: データベースが、ポーリングのために必要なインフラストラクチャが含まれています


## <a name="step-4-configuring-the-polling-service"></a>手順 4: ポーリングのサービスを構成します。

必要なテーブル、トリガー、およびストアド プロシージャを作成した後には、データベースを最後の手順はこれを行うポーリングのサービスを構成する`Web.config`(ミリ秒単位) およびポーリング間隔を使用するデータベースを指定することで。 次のマークアップは、1 秒ごとに、Northwind データベースをポーリングします。


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name`値、`<add>`要素 (NorthwindDB) が、特定のデータベースで人間が判読できる名前を関連付けます。 SQL キャッシュ依存関係を使用する場合に、キャッシュされたデータに基づいているテーブルとここで定義されているデータベース名を参照する必要があります。 使用する方法を見て、`SqlCacheDependency`キャッシュ手順 6. でデータをプログラムでの SQL キャッシュ依存関係を関連付けるクラス。

ポーリング システムがで定義されているデータベースに接続される SQL キャッシュ依存関係が確立されると、`<databases>`要素すべて`pollTime`(ミリ秒) を実行し、`AspNet_SqlCachePollingStoredProcedure`ストアド プロシージャ。 手順 3 を使用してバックアップをこのストアド プロシージャに追加された、`aspnet_regsql.exe`コマンド ライン ツールの取得、`tableName`と`changeId`内の各レコードの値`AspNet_SqlCacheTablesForChangeNotification`します。 古い SQL キャッシュ依存関係は、キャッシュから削除されます。

`pollTime`設定にはパフォーマンスと staleness のデータ間にトレードオフが導入されています。 小さな`pollTime`値には、データベースへの要求の数が増加しますが、キャッシュから古いデータをすばやく削除の詳細は。 大きな`pollTime`値は、データベース要求の数を減らしますが、バックエンド データが変更されたときと、関連するキャッシュ項目が削除されるときの間の遅延が増加します。 幸いにも、データベース要求が単純なストアド プロシージャの実行 s 単純で軽量なテーブルから少数の行を返します。 別の実験の操作を行いますが、`pollTime`最適なバランスを検索する値データベース、アプリケーションのアクセスとデータの期限を延長します。 最小`pollTime`指定できる値は 500 です。

> [!NOTE]
> 上記の例では、1 つは、`pollTime`値、`<sqlCacheDependency>`が要素を指定できます必要に応じて、`pollTime`値、`<add>`要素。 これは、指定されたデータベースが複数存在し、データベースごとのポーリング間隔をカスタマイズする場合に便利です。


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>手順 5: は、宣言によって SQL キャッシュ依存関係の操作

手順 1 ~ 4 では、必要なデータベース インフラストラクチャをセットアップし、ポーリング システムを構成する方法について説明しました。 このインフラストラクチャを導入するには、プログラムまたは宣言型のいずれかの手法を使用して関連の SQL キャッシュ依存項目データ キャッシュに追加したことようになりましたできます。 この手順で SQL キャッシュ依存関係を宣言して使用する方法を考察します。 手順 6 では、プログラムによる方法も取り上げます。

[ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)チュートリアルは、ObjectDataSource の宣言型のキャッシュ機能を説明します。 設定するだけで、`EnableCaching`プロパティを`True`と`CacheDuration`プロパティをいくつかの時間間隔は、ObjectDataSource は、指定した期間に、その基になるオブジェクトから返されるデータをキャッシュして、自動的にします。 ObjectDataSource は、1 つまたは複数の SQL キャッシュ依存関係にも使用できます。

SQL キャッシュ依存関係を宣言して使用を示すため、開く、`SqlCacheDependencies.aspx`ページで、`Caching`フォルダーとツールボックスからデザイナーにドラッグする GridView。 GridView s 設定`ID`に`ProductsDeclarative`という名前の新しい ObjectDataSource にバインドするを選択して、スマート タグからとは、`ProductsDataSourceDeclarative`します。


[![ProductsDataSourceDeclarative という名前の新しい ObjectDataSource を作成します。](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`ProductsDataSourceDeclarative`([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image4.png))。


構成を使用する ObjectDataSource、`ProductsBLL`クラスを選択します タブをドロップダウン リストを設定`GetProducts()`します。 更新プログラム タブで、選択、 `UpdateProduct` - 次の 3 つの入力パラメーターを持つオーバー ロード`productName`、`unitPrice`と`productID`します。 INSERT および DELETE の各タブで (なし) ドロップダウン リストを設定します。


[![次の 3 つの入力パラメーターを持つ UpdateProduct オーバー ロードを使用します。](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**図 6**: UpdateProduct のオーバー ロードを使用して、3 つの入力パラメーター ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image6.png))。


[![(なし) を挿入および削除のタブのドロップダウン リストを設定します。](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**図 7**: (None) にドロップダウン リストを挿入および削除のタブの設定 ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image8.png))。


データ ソース構成ウィザードを完了すると、Visual Studio と作成されます BoundFields CheckBoxFields GridView で各データ フィールド。 すべてのフィールドの削除が`ProductName`、 `CategoryName`、および`UnitPrice`、し、必要に応じて、これらのフィールドを書式設定します。 GridView のスマート タグからは、ページングを有効にする、並べ替えを有効にするには、および編集を有効にするチェック ボックスを確認します。 Visual Studio で ObjectDataSource s は設定`OldValuesParameterFormatString`プロパティを`original_{0}`します。 適切に機能するには、GridView の編集機能のためには、宣言型構文またはその既定値に設定から完全このプロパティを削除するか`{0}`します。

最後に、GridView とセットの上のラベルの Web コントロールを追加、`ID`プロパティを`ODSEvents`とその`EnableViewState`プロパティを`False`します。 これらの変更を行った後、ページ s 宣言型マークアップは、次のようになります。 注がさまざまな SQL キャッシュ依存関係の機能を検証する必要のない GridView のフィールドに見た目のカスタマイズを加えました。


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

ObjectDataSource s のイベント ハンドラーを次に、作成`Selecting`イベントでするのに対して、次のコードを追加します。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

いることを思い出してください ObjectDataSource の`Selecting`その基になるオブジェクトからデータを取得するときにのみイベントが発生します。 独自のキャッシュからデータが、ObjectDataSource にアクセスする場合は、このイベントは起動しません。

次に、ブラウザーからこのページを参照してください。 以降任意のキャッシュ、ページ、並べ替え、またはグリッド ページを編集するたびに実装するには、まだ ve する必要がありますテキスト、表示するイベントが発生すると、図 8 に示すよう。


[![ObjectDataSource のするイベントは、各時間、編集、GridView はページングまたは並べ替えが発生しました。](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**図 8**:、ObjectDataSource s`Selecting`イベント発生の各時刻が GridView はページング、編集、または並べ替え ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image10.png))。


説明したように、 [ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-vb.md)チュートリアルでは、設定、`EnableCaching`プロパティを`True`ObjectDataSource で指定した期間のデータをキャッシュすると、その`CacheDuration`プロパティ。 ObjectDataSource があります、 [ `SqlCacheDependency`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)パターンを使用して、キャッシュされたデータに 1 つまたは複数の SQL キャッシュ依存関係を追加します。


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

場所*databaseName*で指定されているデータベースの名前を指定します、`name`の属性、`<add>`内の要素`Web.config`、および*tableName*データベース テーブルの名前を指定します。 たとえば、ObjectDataSource、データを無期限にキャッシュを作成する Northwind s に対して SQL キャッシュ依存関係に基づいて`Products`テーブルで、設定 ObjectDataSource s`EnableCaching`プロパティを`True`とその`SqlCacheDependency`プロパティをNorthwindDB:Products します。

> [!NOTE]
> SQL キャッシュ依存関係を使用する*と*時間に基づいて有効期限を設定して`EnableCaching`に`True`、`CacheDuration`の時間間隔と`SqlCacheDependency`データベースとテーブル名にします。 時間ベースの有効期限に達したとき、またはポーリング システムは、基になるデータベースのデータが変更されたことを最初に発生した方をノート、ObjectDataSource はそのデータを削除します。


GridView `SqlCacheDependencies.aspx` - 2 つのテーブルからデータを表示します。`Products`と`Categories`(製品 s`CategoryName`を使用してフィールドを取得、`JOIN`で`Categories`)。 このため、2 つの SQL キャッシュ依存関係を指定すると: NorthwindDB:Products;NorthwindDB:Categories します。


[![SQL キャッシュ依存関係を製品と分類を使用して、キャッシュをサポートするために、ObjectDataSource を構成します。](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**図 9**: でのサポートのキャッシュを使用して SQL キャッシュ依存関係を ObjectDataSource を構成`Products`と`Categories`([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image12.png))。


キャッシュをサポートする ObjectDataSource を構成した後、ブラウザーを使用してページを再検討します。 テキスト選択イベントが発生した最初のページ アクセス時に表示する必要がありますが、ページング、並べ替え、または編集 または キャンセル ボタンをクリックするとすぐ移動する必要があります。 これは、ObjectDataSource のキャッシュにデータを読み込んだ後に備えてメモリにまでため、`Products`または`Categories`テーブルが変更されるものや、データが GridView により更新されました。

テキストが新しいブラウザー ウィンドウを開き、編集、挿入、および削除のセクションで基本チュートリアルに移動、グリッドのページングとするイベントがないことに注意が呼び出された後 (`~/EditInsertDelete/Basics.aspx`)。 名前または製品の価格を更新します。 次からの最初のブラウザー ウィンドウに表示データの別のページ、グリッドの並べ替えや行の編集 ボタンをクリックします。 データされた基になるデータベースの変更 (図 10 参照)、この時点では、発生するイベントが再び表示されます。 テキストが表示されない場合は、しばらく待ってからもう一度やり直してください。 変更のポーリングのサービスをチェックすることに注意してください、`Products`テーブルすべて`pollTime`ミリ秒、基になるデータが更新されたときと、キャッシュされたデータが削除されるときの遅延があるようにします。


[![製品のキャッシュされたデータを削除する、Products テーブルを変更します。](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**図 10**: 製品のキャッシュ データを削除して、Products テーブルの変更 ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image14.png))。


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>手順 6: をプログラムで使用、`SqlCacheDependency`クラス

[アーキテクチャでデータをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアル ObjectDataSource でキャッシュを密結合ではなく、アーキテクチャの別のキャッシュ レイヤーを使用する利点について説明しました。 そのチュートリアルで作成した、`ProductsCL`クラスをプログラムでデータ キャッシュの操作を示します。 キャッシュ層で SQL キャッシュ依存関係を利用するには、使用、`SqlCacheDependency`クラス。

ポーリング システムと、`SqlCacheDependency`オブジェクトは、特定のデータベースとテーブルのペアを関連付ける必要があります。 たとえば、次のコードを作成、`SqlCacheDependency`オブジェクトは、Northwind データベース %s に基づく`Products`テーブル。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

2 つの入力パラメーターを`SqlCacheDependency`コンス トラクターは、データベースとテーブル名では、それぞれします。 ObjectDataSource s 使用するような`SqlCacheDependency`プロパティは、使用するデータベース名で指定された値と同じです、`name`の属性、`<add>`要素`Web.config`します。 テーブル名は、データベース テーブルの実際の名前です。

関連付ける、`SqlCacheDependency`でデータ キャッシュに追加された項目のいずれかの操作を使用して、`Insert`依存関係を受け取るメソッド オーバー ロードします。 次のコードを追加*値*に無期限の期間のデータ キャッシュに関連付けます、`SqlCacheDependency`上、`Products`テーブル。 つまり、*値*をポーリング システムが検出されたため、またはメモリ制約によってそれが削除されるまで、キャッシュに残ります、`Products`キャッシュ以降にそのテーブルが変更されています。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

キャッシュ レイヤー s`ProductsCL`クラスが現在のデータをキャッシュ、 `Products` 60 秒間の時間ベースの有効期限を使用してテーブルします。 S SQL キャッシュ依存関係を代わりに使用するように、このクラスを更新できるようにします。 `ProductsCL`クラスの`AddCacheItem`メソッドで、データをキャッシュに追加する責任を負いますが、現在、次のコードが含まれています。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

使用するには、このコードを更新、`SqlCacheDependency`オブジェクトの代わりに、`MasterCacheKeyArray`依存関係をキャッシュします。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

この機能をテストするには、既存の下に、GridView を追加します。 `ProductsDeclarative` GridView。 この新しい GridView s 設定`ID`に`ProductsProgrammatic`、スマート タグをという名前の新しい ObjectDataSource にバインドし、`ProductsDataSourceProgrammatic`します。 構成を使用する ObjectDataSource、`ProductsCL`ドロップダウン リストの選択とに更新 タブの設定クラス`GetProducts`と`UpdateProduct`、それぞれします。


[![ProductsCL クラスを使用する ObjectDataSource を構成します。](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**図 11**: 構成に使用する ObjectDataSource、`ProductsCL`クラス ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image16.png))。


[![GetProducts メソッドをタブのドロップダウン一覧から選択します](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**図 12**: 選択、 `GetProducts`  タブを選択してドロップダウン リストからメソッド ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image18.png))。


[![UpdateProduct メソッドを更新プログラム タブのドロップダウン一覧から選択します。](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**図 13**: UpdateProduct メソッドを更新 タブのドロップダウン リストから選択 ([フルサイズの画像を表示する をクリックします](using-sql-cache-dependencies-vb/_static/image20.png))。


データ ソース構成ウィザードを完了すると、Visual Studio と作成されます BoundFields CheckBoxFields GridView で各データ フィールド。 このページに追加する最初の GridView ですべてのフィールドを削除するようが`ProductName`、 `CategoryName`、および`UnitPrice`、し、必要に応じて、これらのフィールドを書式設定します。 GridView のスマート タグからは、ページングを有効にする、並べ替えを有効にするには、および編集を有効にするチェック ボックスを確認します。 同様、 `ProductsDataSourceDeclarative` ObjectDataSource、Visual Studio の設定は、 `ProductsDataSourceProgrammatic` ObjectDataSource s`OldValuesParameterFormatString`プロパティを`original_{0}`します。 GridView s の編集機能を適切に機能は、このプロパティを設定するためにバックアップ`{0}`(または宣言型構文とプロパティの割り当てを完全に削除) します。

これらのタスクを完了すると、結果として得られる GridView コントロールと ObjectDataSource 宣言型マークアップは、次のようになります。


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

キャッシュ層でキャッシュの依存関係にブレークポイントを設定、SQL をテストする、`ProductCL`クラスの`AddCacheItem`メソッドと、デバッグを開始します。 アクセスと`SqlCacheDependencies.aspx`データが最初に要求され、キャッシュに格納、ブレークポイントをヒットする必要があります。 次に、GridView で別のページに移動またはいずれかの列を並べ替えます。 これにより、データを再クエリする GridView が以降のキャッシュにデータを検出する必要があります、`Products`データベース テーブルが変更されていません。 データは、繰り返しが見つからない場合、キャッシュに十分なメモリが使用可能なコンピューターにいるかどうかを確認してからやり直してください。

GridView のいくつかのページのページングの後に 2 番目のブラウザー ウィンドウを開き、編集、挿入、および削除のセクションで基本のチュートリアルに移動 (`~/EditInsertDelete/Basics.aspx`)。 Products テーブルからレコードを更新し、最初のブラウザー ウィンドウで、新しいページを表示や並べ替えのヘッダーのいずれかをクリックします。

このシナリオでは、次の 2 つのいずれかが表示されますデータベースに変更したため、キャッシュされたデータが削除されたことを示す、いずれかのブレークポイントにヒットする。または、ブレークポイントがヒットしない、つまり`SqlCacheDependencies.aspx`古いデータが表示されています。 ブレークポイントがヒットしないデータの変更によって、ポーリングのサービスがまだ起動されないので、可能性があります。 変更のポーリングのサービスをチェックすることに注意してください、`Products`テーブルすべて`pollTime`ミリ秒、基になるデータが更新されたときと、キャッシュされたデータが削除されるときの遅延があるようにします。

> [!NOTE]
> この遅延はで GridView を使用して、製品の 1 つを編集するときに表示される可能性が高く、`SqlCacheDependencies.aspx`します。 [アーキテクチャでデータをキャッシュ](caching-data-in-the-architecture-vb.md)チュートリアルが追加されました、`MasterCacheKeyArray`キャッシュすることで編集されているデータを確認する依存関係、`ProductsCL`クラスの`UpdateProduct`メソッドは、キャッシュから削除されました。 このキャッシュの依存関係を置き換えただし、変更するときに、`AddCacheItem`この手順で前にメソッドとそのため、`ProductsCL`クラスは引き続きポーリング システム ノートへの変更まで、キャッシュされたデータを表示、`Products`テーブル。 再導入する方法を見て、`MasterCacheKeyArray`手順 7. で依存関係をキャッシュします。


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>手順 7: 複数の依存関係をキャッシュされた項目に関連付ける

いることを思い出してください、`MasterCacheKeyArray`キャッシュの依存関係がいることを確認するために使用が*すべて*内に関連付けられている任意の 1 つの項目が更新されたときに、製品関連のデータをキャッシュから削除します。 たとえば、`GetProductsByCategoryID(categoryID)`メソッド キャッシュ`ProductsDataTables`インスタンスごとに一意*categoryID*値。 これらのオブジェクトのいずれかが削除される場合、`MasterCacheKeyArray`キャッシュの依存関係により、他のユーザーがも削除されるようになります。 このキャッシュの依存関係を持たないキャッシュされたデータが変更されたときにされるその他の製品のキャッシュされたデータが最新でない可能性がありますに発生する可能性が存在します。 その結果、そのことが重要に維持するということ、 `MasterCacheKeyArray` SQL キャッシュ依存関係を使用する場合は、依存関係をキャッシュします。 ただし、データをキャッシュ s`Insert`メソッドは、1 つの依存関係オブジェクトのみが許可されます。

さらに、SQL キャッシュ依存関係を使用する場合に依存関係として複数のデータベース テーブルを関連付けることがあります。 たとえば、`ProductsDataTable`にキャッシュされている、`ProductsCL`クラスには、各製品カテゴリと仕入先名が含まれていますが、`AddCacheItem`メソッドでは、依存関係のみが使用`Products`します。 このような状況で、ユーザーは、カテゴリまたは仕入先の名前を更新する場合にキャッシュされている製品のデータがキャッシュに残りますが最新でいます。 そのため、製品のキャッシュされたデータに依存するようにするだけでなく、`Products`テーブルが、`Categories`と`Suppliers`テーブルも。

[ `AggregateCacheDependency`クラス](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx)キャッシュ項目を複数の依存関係を関連付けるための手段を提供します。 まず、作成、`AggregateCacheDependency`インスタンス。 使用して依存関係のセットを次に、追加、 `AggregateCacheDependency` s`Add`メソッド。 その後、データ キャッシュに項目を挿入すると、を渡す、`AggregateCacheDependency`インスタンス。 ときに*任意*の`AggregateCacheDependency`のインスタンスの依存関係が変更、キャッシュされた項目は削除されます。

更新されたコードを次に示します、`ProductsCL`クラスの`AddCacheItem`メソッド。 メソッドを作成、`MasterCacheKeyArray`キャッシュ依存関係と共に`SqlCacheDependency`オブジェクトを`Products`、 `Categories`、および`Suppliers`テーブル。 これらすべてを 1 つに結合されます`AggregateCacheDependency`という名前のオブジェクト`aggregateDependencies`に渡されたし、`Insert`メソッド。


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

この新しいコードをテストします。内容に変わって、 `Products`、 `Categories`、または`Suppliers`テーブルが削除されるキャッシュされたデータが発生します。 さらに、`ProductsCL`クラス s`UpdateProduct`メソッドは、GridView を使用して製品を編集するときに呼び出される、削除、`MasterCacheKeyArray`これにより、キャッシュされた依存関係をキャッシュ`ProductsDataTable`が削除されると、次の再取得するデータ要求。

> [!NOTE]
> SQL キャッシュ依存関係はでも使用できます[出力キャッシュ](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)します。 この機能のデモについてを参照してください: [SQL Server で ASP.NET 出力キャッシュを使用して](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)します。


## <a name="summary"></a>まとめ

データベースのデータをキャッシュする場合、データベースでデータが変更されたことになるまで、データはキャッシュに理想的には残ります。 ASP.NET 2.0 で SQL キャッシュ依存関係を作成され、宣言とプログラムの両方のシナリオで使用することができます。 この方法での課題の 1 つは、検出データが変更されたときです。 Microsoft SQL Server 2005 の完全なバージョンでは、クエリの結果が変更されたときにアプリケーションをアラート通知機能を提供します。 Express エディションの SQL Server 2005 と SQL Server の以前のバージョンでは、ポーリング システムを代わりに使用する必要があります。 さいわい、ポーリングのために必要なインフラストラクチャを設定することはとても簡単です。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Microsoft SQL Server 2005 でのクエリ通知の使用](https://msdn.microsoft.com/library/ms175110.aspx)
- [クエリ通知を作成します。](https://msdn.microsoft.com/library/ms188669.aspx)
- [使用した ASP.NET のキャッシュ、`SqlCacheDependency`クラス](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server の登録ツール (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [概要 `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者はでした Marko Rangel、Teresa Murphy、および Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](caching-data-at-application-startup-vb.md)
