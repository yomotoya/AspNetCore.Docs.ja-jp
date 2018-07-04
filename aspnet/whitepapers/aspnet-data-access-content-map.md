---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET データ アクセス - 推奨リソース |Microsoft Docs
author: rick-anderson
description: このトピックでは、Entity Framework および SQL Se を使用して主に ASP.NET web アプリケーションでデータにアクセスする方法に関するドキュメント リソースへのリンクを提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 3a8d7de622fd2d0b229dba3af31557a172b90df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396365"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET データ アクセス - 推奨リソース
====================
> このトピックでは、Entity Framework および SQL Server を使用して、主に ASP.NET web アプリケーションでデータにアクセスする方法に関するドキュメント リソースへのリンクを提供します。
> 
> 優れたブログ記事「わかっている場合[stackoverflow](http://stackoverflow.com)スレッド、または他リンクに役立つ、[電子メールの送信](mailto:aspnetue@microsoft.com?subject=Data Access Content Map)リンクを使用します。
> 
> 最後の更新された 4/3/2014


このトピックは、次のセクションで構成されています。

- [ASP.NET でのデータ アクセスの概要](#gettingstarted)
- [Entity Framework を使用](#ef)

    - [最初に Entity Framework Code を使用](#cf)
    - [Entity Framework Code First Migrations を使用します。](#efcfmigrations)
    - [まず、Entity Framework Database を使用してモデル ファースト (EF Designer) または](#efdbf)
    - [Entity Framework (Lazy Loading、Eager Loading、および明示的な読み込み) で関連データの読み込み](#efrelateddata)
    - [Entity Framework パフォーマンスの最適化](#optimizingef)
    - [Entity Framework アプリケーションで同時実行の処理](#efconcurrency)
    - [Entity Framework に関する書籍](#efbooks)
    - [Entity Framework の他のリソース](#otherefresources)
- [データ バインド ASP.NET Web フォーム アプリケーション](#wfdatabinding)

    - [Web を使用してフォームのモデル バインド](#wfmodelbinding)
    - [データ ソース コントロールをフォーム Web の使用](#wfdsc)
    - [Web を使用してフォームのデータ バインド コントロールとデータ バインディング式](#wfdbc)
- [SQL Server データベースの操作](#sqlserver)

    - [SQL Server Express LocalDB データベースの操作](#sslocaldb)
    - [SQL Server Express データベースの操作](#sse)
    - [Windows Azure SQL データベースでの処理](#ssdb)
    - [SQL Server と Windows Azure SQL データベースの選択](#ssdbchoosing)
- [NoSQL データベース管理システムでの作業](#nosql)
- [ASP.NET アプリケーションで LINQ クエリの使用](#linq)
- [動的データ スキャフォールディングを使用します。](#dd)
- [データ アクセスをセキュリティで保護します。](#securing)
- [データ アクセス パフォーマンスの最適化](#optimizingdataaccess)
- [データベースを展開します。](#deploying)
- [Web サービスを介してデータにアクセスします。](#webservice)
- [その他のリソース](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>ASP.NET でのデータ アクセスの概要

- [データ ストレージ オプション (Windows Azure で現実世界のクラウド アプリの構築)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)します。 クラウドの開発に関する電子書籍の章。 リレーショナル データベースについて熟知する多くの開発者が見落とされがちに傾向がある別の方法としては、NoSQL データベースが導入されています。 何について考える選択するときにリレーショナルまたは NoSQL、または特定のプラットフォームの選択に関するガイドラインを示します。
- [ASP.NET データ アクセス オプション](https://msdn.microsoft.com/library/ms178359.aspx)(MSDN)。 ASP.NET のリレーショナル データベースのデータ アクセス オプションとプラットフォームを選択し、自分のシナリオに適切なメソッドにアクセスする方法についての紹介です。
- [リレーショナル データベース](http://en.wikipedia.org/wiki/Relational_database)します。 Wikipedia)。 リレーショナル データベース、作業していない場合は、リレーショナル データベース用語と概念の概要については、このページを参照してください。 SQL Server の概要を参照してください特に[SQL Server データベースでの作業](#sqlserver)このトピックで後述します。

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Entity Framework を使用

- [Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)(MSDN)。 Entity Framework 開発アプローチ Database First では、最初のモデル、または Code First を選択する方法に関するガイダンスです。

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>最初に Entity Framework Code を使用
  

次のチュートリアルでは、ダウンロード可能なサンプル アプリケーションを提供します。

- [MVC 5 を使用して EF 6 の概要](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 接続の回復性、コマンドの途中受信、および非同期などさまざまな移行と EF 6 を含む、Entity Framework Code First のシナリオの機能について説明します。 これは、更新されたバージョンの[EF 5 と MVC 4 シリーズ](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 以前のシリーズには、リポジトリと unit of work パターンは、新しい系列には含まれていない、チュートリアルが含まれています。
- [ASP.NET MVC 5 の概要](../mvc/overview/getting-started/introduction/getting-started.md)します。 狭い Entity Framework コードの範囲の最初シナリオについて説明しますより包括的なジョブの MVC 機能を紹介します。
- [モデル バインディングと Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)します。 Web フォーム アプリケーションで Code First を使用します。
- [ASP.NET 4.5 Web フォームの概要](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。 紹介 Web フォームをいくつかの範囲の Code First です。 モデル バインドを使用します。
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md)します。 メンバーシップと承認も実装する e コマース MVC 3 アプリケーションで Code First を使用します。 MVC のバージョンと ASP.NET メンバーシップ (認証と承認) システムここでは使用が期限切れです。ASP.NET メンバーシップを最新の状態の詳細については、次を参照してください。 [ https://asp.net/identity](https://asp.net/identity)します。

その他のリソース:

- [Entity Framework - 既存のデータベースの Code First](https://msdn.microsoft.com/data/jj200620)します。 MSDN です。 ビデオと既存のデータベースを Code First を使用する方法を示すチュートリアルです。
- [データ デベロッパー センター - Entity Framework](https://msdn.microsoft.com/data/ef)します。 MSDN です。 作成および Entity Framework チームによって管理された Entity Framework ドキュメントのガイドを参照してください、[開始](https://msdn.microsoft.com/data/ee712907)リンク。

参照してください[Entity Framework に関する書籍](#efbooks)と[Entity Framework の他のリソース](#otherefresources)このトピックで後述します。

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Entity Framework Code First Migrations を使用します。
  

ほとんどのカバー移行上に示したコードの最初のチュートリアル。 次のリソースを参照してください。

- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)します。 Code First Migrations を使用してデータベースをデプロイする方法を示すチュートリアル シリーズの 2 部です。
- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Windows Azure の Web サイトにデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 Microsoft Azure の場合)。 移行を使用して、メンバーシップとアプリケーションのデータを Azure にデプロイする方法。
- [Visual Studio および ASP.NET の web 配置の概要](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)します。 参照してください、 **Visual Studio でデータベースの配置を構成する**Code First Migrations を Visual Studio の web 展開の機能に統合する方法の詳細については、セクション。
- [データ デベロッパー センター - Code First Migrations](https://msdn.microsoft.com/data/jj591621) (MSDN)。 Entity Framework チームの移行のドキュメントです。
- [移行のスクリーン キャスト シリーズ](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)します。 EF のブログ)。 Code First Migrations で高度なトピックの次の 3 つのビデオ。
- [Code First Migrations で ASP.NET Web Pages サイト](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites)します。 Mikesdotnetting ブログ)。 Visual Studio クラス ライブラリ プロジェクトでデータ コンテキストを設定することによって、ASP.NET Web Pages サイトで Code First migrations を使用する方法を示します。

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>まず、Entity Framework Database を使用してモデル ファースト (EF Designer) または

- [Entity Framework 6 Database First と MVC 5 の使用の概要](../mvc/overview/getting-started/database-first-development/setting-up-database.md)します。 データベースを作成するサーバー エクスプ ローラーでスクリプトを実行し、Entity Framework デザイナーを使用して、データ モデルを作成します。 ページを単純な CRUD web を作成する方法と処理関数を利用できるコードの最初のチュートリアルのいずれかの EF のすべてのワークフローが同じ DbContext API を使用しているため、その他のデータを示します。

次のリソースが古いです。 これらは、Entity Framework のバージョン 4.0 を使用して、Web フォーム アプリケーションでのデータ バインディングのデータ ソース コントロールを使用する場合に便利です。

- [Entity Framework 4.0 の概要](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)します。 使用する方法を示しています、 **EntityDataSource**コントロール。
- [Entity Framework で続行](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(を使用する方法を示しています、 **ObjectDataSource**コントロール。 同時実行処理、EF のパフォーマンスに関するチュートリアルと EF 4.0 の新機能新機能に関するチュートリアルのチュートリアルが含まれています。

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>関連する Entity Framework (Lazy Loading、Eager Loading、および明示的な読み込み) でデータの処理

- [ASP.NET MVC アプリケーションで Entity Framework での関連データの読み取り](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。 最初に、MVC のサンプル アプリケーションをコードします。 示されているメソッドは、Web フォーム モデル バインドと Database First ワークフローにも適用されます。
- [データ デベロッパー センター - 関連エンティティを読み込む](https://msdn.microsoft.com/data/jj574232)(MSDN)。 読み込みに関する、Entity Framework チームのドキュメントに関連するデータを指定します。

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Entity Framework パフォーマンスの最適化

- [Entity Framework シナリオを ASP.NET アプリケーションの高度な](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。 ストアド プロシージャを呼び出したり、独自の SQL ステートメントを実行する方法、変更の検出を無効にする方法と変更を保存するときに検証を無効にする方法を示します。
- [Entity Framework 5 のパフォーマンスに関する考慮事項](https://msdn.microsoft.com/data/hh949853)(MSDN)。
- [パフォーマンスに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN)。
- [ASP.NET Web アプリケーションで Entity Framework でのパフォーマンスを最大化](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)します。 Entity Framework 4.0 に適用されます。
- 参照してください[最適化の ASP.NET データ アクセス](#optimizingdataaccess)このトピックで後述します。

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Entity Framework アプリケーションで同時実行の処理

- [ASP.NET MVC アプリケーションで Entity Framework での同時実行の処理](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。 最初に、MVC サンプル アプリケーションを使用して、DbContext API のコードします。
- [データ デベロッパー センター-オプティミスティック同時実行制御パターン](https://msdn.microsoft.cus/data/jj592904)(MSDN)。 Entity Framework チームの同時実行のドキュメントです。
- [ASP.NET Web アプリケーションで Entity Framework での同時実行の処理](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)します。 Entity Framework 4.0 に適用されます。 データベースの最初に、ObjectContext API を Web フォームのサンプル アプリケーションを使用します。

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Entity Framework に関する書籍

- [プログラミングの Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do)を Julie Lerman が Rowan Miller が。
- [Entity Framework のプログラミング: コード ファースト](http://shop.oreilly.com/product/0636920022220.do)を Julie Lerman が Rowan Miller が。

これらの書籍のどちらも機能は、現在の推奨される手法の最新の状態です。 インターネットで使用可能なものよりも、Entity Framework より包括的なまだわかりやすい概要を提供します。 別のブック[Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do)で Julie Lerman、ですより包括的な拡大が古い、Entity Framework を使用することをお勧めの方法では不要になった多くのテクニックについて説明します。 Entity Framework チームが推奨書籍の一覧も参照してください。[データ デベロッパー センター - ブックの「](https://msdn.microsoft.com/data/aa937716) MSDN サイト。

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>その他の Entity Framework のリソース

- [Entity Framework (ADO.NET) チーム ブログ](https://blogs.msdn.com/b/adonet/)します。 最新の情報と新しい拡張機能のお知らせの最適なリソースの 1 つ。 他の EF 関連のブログのブログを参照してください。 [Entity Framework の概要](https://msdn.microsoft.com/data/ee712907)します。
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)します。 参照してください、**データ ポイント**列は、Entity Framework に関連するトピックについては、頻繁にします。

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>データ バインド ASP.NET Web フォーム アプリケーション

- [ASP.NET Web フォームのデータ アクセス オプション](https://msdn.microsoft.com/library/jj822927.aspx)(MSDN)<a id="wfmodelbinding"></a>します。

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Web を使用してフォームのモデル バインド

- [モデル バインディングと Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)します。 チュートリアルのシリーズが EF Code First を使用します。
- [Web フォーム モデル バインド パート 1: データを選択する](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。 これらの以前のブログ投稿では、ModelType、という名前の ItemType という現在プロパティしますが、含まれている情報が有効ではそれ以外の場合。
- [Web フォーム モデル バインド パート 2: データのフィルター処理](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。
- [Web フォーム モデル バインド パート 3: 更新と検証](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx)(Scott Guthrie のブログ)。
- [ASP.NET 4.5 Web フォーム モデル バインド](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md)します。 (ビデオ)。
- [モデル バインドのパート 1 - データの選択](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md)(ビデオ)。
- [モデルのバインディング第 2 部 - フィルタ リング](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md)(ビデオ)。
- [データを表示して ASP.NET 4.5 Web フォームの使用を開始を取得する項目と詳細](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)します。

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>データ ソース コントロールをフォーム Web の使用

- [データ ソースの Web サーバー コントロール](https://msdn.microsoft.com/library/ms247258.aspx)(MSDN)。
- [動的なデータ プロバイダーと EntityDataSource コントロールの Entity Framework 6 のリリースの発表](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)(Microsoft の Web 開発のブログ)。

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Web を使用してフォームのデータ バインド コントロールとデータ バインディング式

- [モデル バインディングと Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)します。 EF Code First を使用するチュートリアル シリーズです。
- [データを表示して ASP.NET 4.5 Web フォームの使用を開始を取得する項目と詳細](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)します。
- [データ コントロールを厳密に型指定された](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。
- [データ コントロールを厳密に型指定された](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)(ビデオ)。
- [ASP.NET 4.5 Web フォーム厳密な型指定されたデータ コントロール](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)(ビデオ)。
- [Web サーバー コントロールをデータ バインドされた](https://msdn.microsoft.com/library/ms228214.aspx)(MSDN)。
- [データ バインディング式の概要](https://msdn.microsoft.com/library/ms178366.aspx)(MSDN)。 これは、ページのみカバー **Eval**と**バインド**; に含める更新されていない**項目**と**BindItem**します。

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>SQL Server データベースの操作

- [SQL Server のデータベース機能](https://msdn.microsoft.com/library/hh230827.aspx)(MSDN)。 SQL Server のトピックのさまざまな一般的な概要については、下にある目次内にあるこのエントリを参照してください。
- [SQL Server のエディション](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver)(MSDN)。 SQL Server の使用可能なエディションで、それぞれに関する詳細情報へのリンクの概要です。)
- [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [ASP.NET Web アプリケーションの SQL Server Compact を使用して](https://msdn.microsoft.com/library/ms247257.aspx)(MSDN)。
- [Microsoft SQL Server: データベースの製品サンプル](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)します。 サンプルの AdventureWorks データベース。
- [サンプル データベースのインストール](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)します。 に加えて、ここに示したメソッドは、ダウンロードすることも、サンプルの .mdf ファイルのいずれかをアプリに\_web プロジェクトのデータ フォルダー、LocalDB データベースに変換し、LocalDB 接続文字列を作成します。 その方法については、次を参照してください。[方法: LocalDB にアップグレードする](https://msdn.microsoft.com/library/hh873188.aspx)します。

SQL Server Express、LocalDB とを使用すると、SQL Server と SQL データベースの選択は、次のセクションも参照してください。

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>SQL Server Express LocalDB データベースの操作

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN)。 LocalDB を公式の MSDN 紹介します。
- [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [方法: LocalDB にアップグレードする](https://msdn.microsoft.com/library/hh873188.aspx)(MSDN)。 以前のバージョンの SQL Server Express LocalDB に .mdf ファイルを移行する方法。 いずれかをダウンロードする場合、このプロセスを経由しなければも、[サンプル データベースの SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483)します。
- [強化された SQL Express LocalDB の概要](https://go.microsoft.com/fwlink/?LinkId=234375)(SQL Server Express ブログ)。 MSDN に含まれているよりも LocalDB が作成された理由の背景情報があります。
- [LocalDB: My Database とは?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express ブログ)。 LocalDB のデータベース ファイルを作成する場所について説明します。
- [第 1 部の完全な IIS で LocalDB を使用します。 ユーザー プロファイル](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx)(SQL Server Express ブログ)。 LocalDB では、IIS を使用するものはありません。 このシリーズのブログの投稿には、問題といくつかの回避策について説明します。

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>SQL Server Express データベースの操作

- [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。 SQL Server Express で AttachDBFileName 接続文字列の設定を使用する場合は、このページのユーザー インスタンス セクションでは特にを参照してください。
- [ローカル SQL Server Express 2008 の所有権を取得する方法](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx)(SQL Server Express ブログ)。 一般的な問題は、SQL Server Express インスタンスの管理者がわからないため、SQL Server Express データベースを操作できません。 既定では、SQL Server Express をインストールしたユーザーのみには、管理者です。 このブログでは、コンピューターの管理者である場合に、SQL Server Express 管理者に自分で作成する方法について説明します。
- [ASP.NET web アプリケーションは実稼働環境で SQL Server Express データベースを使用できますか。](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN)。

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Windows Azure SQL データベースでの処理

- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Windows Azure の Web サイトにデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)(Microsoft Azure サイト)。
- [SQL データベース](https://docs.microsoft.com/azure/sql-database/)(Microsoft Azure サイト)。 チュートリアルとハウツー ガイドを取得します。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN)。 MSDN の SQL Database の目次の最上位ノードです。
- [Windows Azure SQL データベース TechNet Wiki の記事インデックス](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx)(Microsoft TechNet サイト)。
- [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx)します。 このフレームワークは調整によって発生する一時的なネットワーク障害や接続エラーを処理することができますです。 NuGet パッケージで利用できる: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling)します。
- [SQL Database と Entity Framework の概要](https://msdn.microsoft.com/data/jj556244)(MSDN)。
- [Windows Azure トレーニング キット](https://www.microsoft.com/download/details.aspx?id=8396)(Microsoft ダウンロード センター)。 SQL Database のハンズオン ラボが含まれています。
- [Windows Azure SQL データベースのコミュニティ フォーラム](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads)します。
- [Windows Azure SQL Database への移動](https://msdn.microsoft.com/library/ff803375.aspx)(MSDN)。 Microsoft Patterns and Practices チームによって包括的なエンド ツー エンド シナリオの 1 つの章。 実際に移行する理由は、SQL Server から SQL Database に移行する方法。
- [Windows Azure SQL データベースに SQL Server データベースを移行する](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx)(MSDN)。
- [SQL Database 移行ウィザード](http://sqlazuremw.codeplex.com/)します。 データベースと SQL Database の間を移行するためのオープン ソース ツール。

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server と Windows Azure SQL データベースの選択

- [Windows Azure SQL Database と SQL Server を比較](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx)(Microsoft TechNet サイト)。
- [Windows Azure SQL データベースへのデータ移行: ツールと手法](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx)(MSDN)。 SQL Database に SQL Server を比較し、SQL Server から SQL Database に移行するタイミングに関するガイダンスを提供するセクションが含まれています。
- [Windows Azure SQL データベース提供ガイド](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx)(Microsoft TechNet サイト)。
- [SQL Server 機能の制限 (Windows Azure SQL データベース)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN)。
- [Windows Azure Table Storage と Windows Azure SQL Database の比較し、は対照的](https://msdn.microsoft.com/library/jj553018.aspx)(MSDN)。 Windows Azure にデプロイするアプリケーションは、Windows Azure Table storage に Windows Azure SQL Database への代替ことがあります。 このトピックでは、これらの方法を決定するのに役立ちます。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN)。
- [ガイドラインと制限事項 (Windows Azure SQL データベース)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>NoSQL データベース管理システムでの作業

- [Windows Azure データ サービス](https://www.windowsazure.com/develop/net/data/)(Microsoft Azure サイト)。 参照してください、 [Table Service の機能ガイド](https://docs.microsoft.com/azure/)と**ビッグ データ**ページのセクション。
- [ASP.NET 多層アプリケーションを使用してストレージ テーブル、キュー、および Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure サイト)。 Windows Azure ストレージの NoSQL テーブルを使用するダウンロード可能なサンプル アプリケーションでエンド ツー エンド チュートリアル。

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>ASP.NET アプリケーションで LINQ クエリの使用

- [ASP.NET データ アクセス オプション](https://msdn.microsoft.com/library/ms178359.aspx#linq)(MSDN)。 LINQ の概要が含まれています。
- [LINQ のトレーニング ビデオ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/)(Joe Stagner のブログ)。
- [動的な LINQ リソースへのリンクを持つ ASP.NET フォーラム スレッド](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)します。

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>動的データ スキャフォールディングを使用します。

- [プロジェクト テンプレートの動的データ](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata)(MSDN)。 動的なデータ プロジェクトを使用するタイミングに関するガイダンス。
- [ASP.NET 動的データ](https://msdn.microsoft.com/library/ee845452.aspx)(MSDN)。

<a id="securing"></a>

## <a name="securing-data-access"></a>データ アクセスをセキュリティで保護します。

- [ASP.NET でのデータ アクセスをセキュリティで保護する](https://msdn.microsoft.com/library/ms178375.aspx)(MSDN)。
- [セキュリティに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN)。
- [方法: データ ソース コントロールを使用する場合の接続文字列をセキュリティで保護](https://msdn.microsoft.com/library/ms178372.aspx)(MSDN)。

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>データ アクセス パフォーマンスの最適化

- [ASP.NET のパフォーマンス概要](https://msdn.microsoft.com/library/cc668225.aspx)(MSDN)。
- [ASP.NET キャッシュ](https://msdn.microsoft.com/library/xsbfdd8c.aspx)(MSDN)。
- [ASP.NET のパフォーマンスを向上させる](https://msdn.microsoft.com/library/ff647787)(MSDN)。 このページの上部にある"Retired Content"の警告が発生が、情報の大部分はも引き続き該当と同等の更新されたリソースはありません。
- [SQL Server のパフォーマンスを向上させる](https://msdn.microsoft.com/library/ff647793)(MSDN)。 前述のリンクと同じコメントです。

参照してください[を最適化する Entity Framework パフォーマンス](#optimizingef)このトピックで前述しました。

<a id="deploying"></a>

## <a name="deploying-a-database"></a>データベースを展開します。

- [ASP.NET Web 展開 - 推奨リソース](aspnet-web-deployment-content-map.md)(MSDN)。

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Web サービスを介してデータにアクセスします。

- [Web サービスを介してデータにアクセスする](https://msdn.microsoft.com/library/ms178359.aspx#webservice)(MSDN)。 WCF と Web API を使用するタイミングに関するガイダンス。
- [ASP.NET Web API の概要](../web-api/index.md)します。
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN)。

<a id="additional"></a>

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET データ アクセスに関する FAQ](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN)。
- [ASP.NET Web フォームのチュートリアル - データ](../web-forms/overview/data-access/index.md)します。 これらのチュートリアルのほとんどは比較的古い;お読みください[ASP.NET データ アクセス オプション](https://msdn.microsoft.com/library/ms178359.aspx)と[データ ストレージ オプション (Windows Azure で構築実際のクラウド アプリケーション)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)最初は適切なデータ アクセス メソッドに入るが届かないように自分のシナリオ。
- [ASP.NET MVC のコンテンツ マップ](../mvc/overview/getting-started/recommended-resources-for-mvc.md)します。
- [ASP.NET Web Pages のチュートリアル - データ](../web-pages/overview/data/index.md)します。
- [Visual Studio でのデータにアクセスする](https://msdn.microsoft.com/library/wzabh8c4.aspx)(MSDN)。 ようなリンクの一覧を示しますこのコンテンツ マップですが ASP.NET ではなく、Visual Studio に重点を置いています。
