---
uid: whitepapers/aspnet-data-access-content-map
title: "ASP.NET データ アクセス - 推奨リソース |Microsoft ドキュメント"
author: rick-anderson
description: "このトピックでは、Entity Framework および SQL Se を使用して、主に ASP.NET web アプリケーションのデータにアクセスする方法に関するドキュメント リソースへのリンクを提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: bf94b3dd6a1c450bf534f93747f217808ed3a30c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET データ アクセス - リソースを推奨
====================
> このトピックでは、Entity Framework および SQL Server を使用して、主に ASP.NET web アプリケーションのデータにアクセスする方法に関するドキュメント リソースへのリンクを提供します。
> 
> Post、優れたブログがわかっている場合[stackoverflow](http://stackoverflow.com)スレッド、またはその他のリンクが、役に立つを[電子メールの送信](mailto:aspnetue@microsoft.com?subject=Data Access Content Map)リンクを使用します。
> 
> 最後の更新された 4/3/2014


このトピックは、次のセクションで構成されています。

- [ASP.NET でのデータ アクセスの概要](#gettingstarted)
- [Entity Framework を使用](#ef)

    - [最初に Entity Framework のコードを使用](#cf)
    - [Entity Framework Code First Migrations を使用します。](#efcfmigrations)
    - [最初に Entity Framework データベースを使用またはモデル優先 (EF Designer)](#efdbf)
    - [Entity Framework (遅延読み込みは、一括読み込み、および明示的な読み込み) に関連するデータの読み込み](#efrelateddata)
    - [Entity Framework パフォーマンスの最適化](#optimizingef)
    - [Entity Framework アプリケーションで同時実行の処理](#efconcurrency)
    - [Entity Framework に関する書籍](#efbooks)
    - [Entity Framework の他のリソース](#otherefresources)
- [データ バインド ASP.NET web フォーム アプリケーション](#wfdatabinding)

    - [フォームのモデル バインディングの Web の使用](#wfmodelbinding)
    - [データ ソース コントロールをフォームの Web の使用](#wfdsc)
    - [Web を使用してフォームのデータ バインド コントロールとデータ バインディング式](#wfdbc)
- [SQL Server データベースの操作](#sqlserver)

    - [SQL Server Express LocalDB データベースの操作](#sslocaldb)
    - [SQL Server Express データベースの操作](#sse)
    - [Windows Azure SQL データベースでの処理](#ssdb)
    - [SQL Server と Windows Azure SQL データベースの選択](#ssdbchoosing)
- [NoSQL データベース管理システムでの作業](#nosql)
- [ASP.NET アプリケーションでの LINQ クエリの使用](#linq)
- [動的データ スキャフォールディングを使用します。](#dd)
- [データ アクセスを保護します。](#securing)
- [データ アクセス パフォーマンスを最適化します。](#optimizingdataaccess)
- [データベースの配置](#deploying)
- [Web サービスを介してデータにアクセスします。](#webservice)
- [その他のリソース](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>ASP.NET でのデータ アクセスの概要

- [データ ストレージ オプション (Windows Azure と実際のクラウド アプリのビルド)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)です。 クラウドの開発に関する電子書籍の章します。 リレーショナル データベースに慣れている多くの開発者が見落とされる可能性がある別の方法としては、NoSQL データベースが導入されています。 新機能についての考え方を選択するときにリレーショナルまたは NoSQL、または特定のプラットフォームを選択するガイドラインを示します。
- [ASP.NET データ アクセス オプション](https://msdn.microsoft.com/en-us/library/ms178359.aspx)(MSDN)。 データの概要については、ASP.NET 用のリレーショナル データベースのオプションとプラットフォームを選択し、シナリオに合った適切なメソッドにアクセスする方法についてのガイダンスにアクセスします。
- [リレーショナル データベース](http://en.wikipedia.org/wiki/Relational_database)です。 Wikipedia)。 、リレーショナル データベースで作業していない場合は、リレーショナル データベース用語と概念の概要については、このページを参照してください。 SQL Server の概要を参照してください特に[SQL Server データベースの操作](#sqlserver)このトピックで後述します。

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Entity Framework を使用

- [Entity Framework 開発方法](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf)(MSDN)。 Entity Framework 開発アプローチ Database First Model First または Code First を選択する方法に関するガイダンスです。

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>最初に Entity Framework のコードを使用
  

次のチュートリアルでは、ダウンロード可能なサンプル アプリケーションを提供します。

- [MVC 5 を使用する EF 6 にあたって](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 接続の回復、コマンドの途中受信、および非同期など移行および EF 6 など、Entity Framework Code First のシナリオのさまざまな機能をカバーします。 これは、更新されたバージョンの[EF 5/MVC 4 シリーズ](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 以前の系列には、リポジトリと新しい系列に含まれていない作業単位のパターンのチュートリアルが含まれています。
- [Introduction to ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md)です。 幅の狭い Entity Framework コードの範囲の最初シナリオをカバーが MVC 機能を導入する際のより包括的なジョブでは。
- [モデル バインディング機能と Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)です。 Web フォーム アプリケーションで Code First を使用します。
- [ASP.NET 4.5 Web フォームの概要](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)です。 概要 Web フォームをいくつかの範囲の Code First です。 モデル バインディングを使用します。
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md)です。 メンバーシップおよび承認も実装している e コマース MVC 3 アプリケーションで Code First を使用します。 MVC のバージョンと ASP.NET メンバーシップ (認証と承認) システムここでは使用が期限切れです。ASP.NET メンバーシップを最新の状態の詳細については、次を参照してください。 [https://asp.net/identity](https://asp.net/identity)です。

その他のリソース:

- [Entity Framework の既存のデータベースの最初にコード](https://msdn.microsoft.com/en-us/data/jj200620)です。 MSDN です。 ビデオとチュートリアルを既存のデータベースで Code First を使用する方法を示しています。
- [データ プラットフォーム デベロッパー センターの Entity Framework](https://msdn.microsoft.com/data/ef)です。 MSDN です。 作成および Entity Framework チームによって管理されている Entity Framework ドキュメントのガイドを参照してください、[開始](https://msdn.microsoft.com/en-us/data/ee712907)リンクします。

参照[Entity Framework に関する書籍](#efbooks)と[Entity Framework の他のリソース](#otherefresources)このトピックで後述します。

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Entity Framework Code First Migrations を使用します。
  

ほとんどのカバー移行上に示したコードの最初のチュートリアル。 次のリソースを参照してください。

- [Visual Studio を使用して ASP.NET Web 配置](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)です。 Code First Migrations を使用してデータベースを展開する方法を示すチュートリアル シリーズの 2 部構成です。
- [Windows Azure Web サイトにメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 Microsoft Azure の場合)。 移行を使用して、メンバーシップとアプリケーション データを Azure にデプロイする方法です。
- [Visual Studio と ASP.NET の web 配置の概要](https://msdn.microsoft.com/en-us/library/dd394698.aspx#dbdeployment)です。 参照してください、 **Visual Studio でデータベースの配置を構成する**Code First Migrations を Visual Studio web 配置機能に統合する方法の詳細についてのセクションでします。
- [データ プラットフォーム デベロッパー センターの Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) (MSDN)。 Entity Framework チームの移行のドキュメントです。
- [移行スクリーン キャスト系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)です。 EF ブログ)。 Code First Migrations で高度なトピックの次の 3 つのビデオ。
- [Code First Migrations ASP.NET Web Pages サイトで](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites)です。 Mikesdotnetting ブログ)。 Visual Studio クラス ライブラリ プロジェクトでデータ コンテキストを設定することによって、ASP.NET Web Pages サイトで Code First migrations を使用する方法を示します。

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>最初に Entity Framework データベースを使用またはモデル優先 (EF Designer)

- [Entity Framework 6 Database First MVC 5 を使用すると作業の開始](../mvc/overview/getting-started/database-first-development/setting-up-database.md)です。 サーバー エクスプ ローラーで、データベースを作成するスクリプトを実行し、Entity Framework デザイナーを使用して、データ モデルを作成します。 ページを単純な CRUD web を作成する方法と処理関数を行うことができるコードの最初のチュートリアルでは、いずれかの EF のすべてのワークフローが、同じ DbContext API を使用するので、その他のデータを示します。

次のリソースが古いです。 これらは、Entity Framework のバージョン 4.0 を使用して、Web フォーム アプリケーションでのデータ バインディングのデータ ソース コントロールを使用する場合に便利です。

- [Entity Framework 4.0 の概要](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)です。 使用する方法を示します、 **EntityDataSource**コントロール。
- [Entity Framework を続行](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(を使用する方法を示します、 **ObjectDataSource**コントロール。 同時実行処理、EF パフォーマンスに関するチュートリアルおよび EF 4.0 の新機能のチュートリアルに関するチュートリアルが含まれます。

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>処理に関連データが Entity Framework (遅延読み込みは、一括読み込み、および明示的な読み込み)

- [ASP.NET MVC アプリケーションに Entity Framework と関連するデータを読み取る](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。 最初に、MVC サンプル アプリケーションをコードします。 示すメソッドは、Web フォーム モデルのバインディングとデータベースの最初のワークフローにも適用されます。
- [データ プラットフォーム デベロッパー センターの関連エンティティを読み込む](https://msdn.microsoft.com/en-us/data/jj574232)(MSDN)。 読み込みに関する、Entity Framework チームのドキュメントに関連するデータ。

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Entity Framework パフォーマンスの最適化

- [ASP.NET アプリケーション用の Entity Framework のシナリオを高度な](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。 独自のストアド プロシージャの呼び出しまたは独自の SQL ステートメントを実行する方法、変更の検出を無効にする方法と変更を保存するときに検証を無効にする方法を示します。
- [Entity Framework 5 のパフォーマンスに関する考慮事項](https://msdn.microsoft.com/en-us/data/hh949853)(MSDN)。
- [パフォーマンスに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc853327) (MSDN)。
- [ASP.NET Web アプリケーションに Entity Framework でのパフォーマンスを最大化](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)です。 Entity Framework 4.0 に適用されます。
- 関連項目[最適化の ASP.NET データ アクセス](#optimizingdataaccess)このトピックで後述します。

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Entity Framework アプリケーションで同時実行の処理

- [Entity Framework、ASP.NET MVC アプリケーションでの同時実行の処理](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。 コードの最初に、MVC サンプル アプリケーションを使用して DbContext API です。
- [データ プラットフォーム デベロッパー センター – オプティミスティック同時実行パターン](https://msdn.microsoft.com/en-us/data/jj592904)(MSDN)。 Entity Framework チームの同時実行のドキュメントです。
- [ASP.NET Web アプリケーションに Entity Framework での同時実行の処理](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)です。 Entity Framework 4.0 に適用されます。 データベースの最初に、ObjectContext API、Web フォーム サンプル アプリケーションを使用します。

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Entity Framework に関する書籍

- [Entity Framework のプログラミング: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman して Rowan Miller です。
- [Entity Framework のプログラミング: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman して Rowan Miller です。

これらの書籍は現在推奨されている手法で最新の状態です。 インターネット上よりも使用可能な Entity Framework の包括的なまだ追跡容易な概要を提供します。 別のブック[Entity Framework のプログラミング](http://shop.oreilly.com/product/9780596807252.do)Julie Lerman によってはよりも大規模かつですより包括的なが古いするテクニックの多くに対応されがなくなると、Entity Framework を使用することをお勧めします。 Entity Framework チームで推奨されるブックの一覧も参照してください。[データ プラットフォーム デベロッパー センターのブック](https://msdn.microsoft.com/en-us/data/aa937716)MSDN サイトです。

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>その他の Entity Framework のリソース

- [Entity Framework (ADO.NET) チーム ブログ](https://blogs.msdn.com/b/adonet/)です。 最新の情報と新しい拡張機能のお知らせする最適なリソースの 1 つ。 他の EF に関連するブログを参照してくださいでブログロール[Entity Framework の概要](https://msdn.microsoft.com/en-us/data/ee712907)です。
- [MSDN マガジン](https://msdn.microsoft.com/en-us/magazine/default.aspx)です。 参照してください、**データ ポイント**Entity Framework に関連するトピックに関する多くの場合は、その列です。

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>データ バインド ASP.NET web フォーム アプリケーション

- [ASP.NET Web フォームのデータ アクセス オプション](https://msdn.microsoft.com/en-us/library/jj822927.aspx)(MSDN)<a id="wfmodelbinding"></a>です。

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>フォームのモデル バインディングの Web の使用

- [モデル バインディング機能と Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)です。 チュートリアル シリーズ EF Code First を使用します。
- [Web フォーム モデル バインディング パート 1: データの選択](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。 これらの以前のブログ投稿ではという現在 ItemType プロパティでした ModelType が含まれている情報が有効ではそれ以外の場合。
- [Web フォーム モデル バインディング パート 2: データのフィルター処理](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。
- [Web フォーム モデル バインディング パート 3: 更新と検証](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx)(Scott Guthrie のブログ)。
- [ASP.NET 4.5 Web フォーム モデル バインディング](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md)です。 (ビデオ)。
- [モデル バインディング Part 1 - データの選択](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md)(ビデオ)。
- [モデル バインディング パート 2 - フィルタ リング](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md)(ビデオ)。
- [データを表示 ASP.NET 4.5 Web フォームの作業項目と詳細](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)です。

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>データ ソース コントロールをフォームの Web の使用

- [データ ソースの Web サーバー コントロール](https://msdn.microsoft.com/en-us/library/ms247258.aspx)(MSDN)。
- [動的なデータ プロバイダーと EntityDataSource コントロールの Entity Framework 6 のリリースを発表](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)(Microsoft の Web 開発のブログ)。

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Web を使用してフォームのデータ バインド コントロールとデータ バインディング式

- [モデル バインディング機能と Web フォーム](https://go.microsoft.com/fwlink/?LinkId=286117)です。 一連のチュートリアルについて EF Code First を使用します。
- [データを表示 ASP.NET 4.5 Web フォームの作業項目と詳細](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)です。
- [データ コントロールを厳密に型指定された](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx)(Scott Guthrie のブログ)。
- [データ コントロールを厳密に型指定された](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)(ビデオ)。
- [ASP.NET 4.5 Web フォーム厳密に型指定されたデータ コントロール](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)(ビデオ)。
- [Web サーバー コントロールをデータ バインドされた](https://msdn.microsoft.com/en-us/library/ms228214.aspx)(MSDN)。
- [データ バインディング式の概要](https://msdn.microsoft.com/en-us/library/ms178366.aspx)(MSDN)。 これは、ページのみカバー **Eval**と**バインド**; に含める更新されていない**項目**と**BindItem**です。

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>SQL Server データベースの操作

- [SQL Server データベース機能](https://msdn.microsoft.com/en-us/library/hh230827.aspx)(MSDN)。 さまざまな SQL Server のトピックに一般的な概要については、目次にこの 1 つ下のエントリを参照してください。
- [SQL Server のエディション](https://msdn.microsoft.com/en-us/library/ms178359.aspx#sqlserver)(MSDN)。 概要の詳細については、1 つずつへのリンクを使用可能な SQL Server エディションです。)
- [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/en-us/library/jj653752.aspx)(MSDN)。
- [ASP.NET Web アプリケーション用 SQL Server Compact を使用して](https://msdn.microsoft.com/en-us/library/ms247257.aspx)(MSDN)。
- [Microsoft SQL Server: データベースの製品サンプル](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)です。 AdventureWorks のサンプル データベース。
- [サンプル データベースのインストール](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)です。 メソッド以外に、次に示すダウンロードすることも、サンプルの .mdf ファイルのいずれかのアプリに\_web プロジェクトのデータ フォルダーが LocalDB は、データベースに変換し、LocalDB 接続文字列を作成します。 実行する方法については、次を参照してください。[する方法: LocalDB にアップグレードする](https://msdn.microsoft.com/en-us/library/hh873188.aspx)です。

SQL Server Express LocalDB は、作業を行い、SQL Server と SQL データベース間では、次のセクションも参照してください。

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>SQL Server Express LocalDB データベースの操作

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/en-us/library/hh510202(v=sql.110).aspx) (MSDN)。 LocalDB への公式の MSDN 概要です。
- [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/en-us/library/jj653752.aspx)(MSDN)。
- [方法: LocalDB にアップグレードする](https://msdn.microsoft.com/en-us/library/hh873188.aspx)(MSDN)。 以前のバージョンの SQL Server Express LocalDB に .mdf ファイルを移行する方法。 またのいずれかをダウンロードする場合、このプロセスを完了する必要がある、[サンプル データベースの SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483)です。
- [強化された SQL Express LocalDB を導入](https://go.microsoft.com/fwlink/?LinkId=234375)(SQL Server Express のブログ)。 MSDN に含まれるよりも LocalDB が作成された理由の背景情報があります。
- [LocalDB: マイ データベースとは?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express のブログ)。 LocalDB のデータベース ファイルが作成される場所に関する情報です。
- [LocalDB を使用して、完全な IIS と、パート 1: ユーザー プロファイル](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx)(SQL Server Express のブログ)。 LocalDB は、IIS を使用するものではありません。 この一連のブログの投稿では、問題といくつかの回避策について説明します。

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>SQL Server Express データベースの操作

- [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/en-us/library/jj653752.aspx)(MSDN)。 SQL Server Express で AttachDBFileName 接続文字列の設定を使用する場合は、特に、ユーザー インスタンスのセクションのこのページを参照してください。
- [ローカル SQL Server Express 2008 の所有権を取得する方法](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx)(SQL Server Express のブログ)。 一般的な問題はない SQL Server Express インスタンスの管理者であるために、SQL Server Express データベースで操作するされていません。 既定では、SQL Server Express をインストールしたユーザーだけは、管理者です。 このブログでは、自分でに、コンピューターの管理者である場合、SQL Server Express の管理者を作成する方法について説明します。
- [ASP.NET web アプリケーションは実稼働環境で SQL Server Express データベースを使用できますか。](https://msdn.microsoft.com/en-us/library/jj653753.aspx#sql_express_in_production) (MSDN)。

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Windows Azure SQL データベースでの処理

- [Windows Azure Web サイトへのメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)(Microsoft Azure サイト)。
- [SQL データベース](https://docs.microsoft.com/azure/sql-database/)(Microsoft Azure サイト)。 チュートリアルと操作方法のガイドを取得します。
- [Windows Azure SQL データベース](https://msdn.microsoft.com/en-us/library/windowsazure/ee336279.aspx)(MSDN)。 MSDN の SQL データベースのコンテンツのテーブルの最上位ノード。
- [Windows Azure SQL データベース TechNet Wiki の記事インデックス](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx)(Microsoft TechNet サイト)。
- [Transient Fault Handling Application Block](https://msdn.microsoft.com/en-us/library/hh680934(v=PandP.50).aspx)です。 フレームワークを調整によって発生する一時的なネットワーク障害や接続エラーを処理することができます。 NuGet パッケージで利用できる: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling)です。
- [SQL データベースおよび Entity Framework の概要](https://msdn.microsoft.com/en-us/data/jj556244)(MSDN)。
- [Windows Azure のトレーニング キット](https://www.microsoft.com/en-us/download/details.aspx?id=8396)(Microsoft ダウンロード センター)。 SQL データベースにはハンズオン ラボが含まれています。
- [Windows Azure SQL データベース コミュニティ フォーラム](https://social.msdn.microsoft.com/Forums/en-US/ssdsgetstarted/threads)です。
- [Windows Azure SQL データベースへの移動](https://msdn.microsoft.com/en-us/library/ff803375.aspx)(MSDN)。 Microsoft Patterns and Practices チームによって包括的なエンド ツー エンド シナリオの 1 つの章します。 移行する理由について説明し、SQL データベースに SQL Server から移行する方法です。
- [Windows Azure SQL データベースに SQL Server データベースを移行する](https://msdn.microsoft.com/en-us/library/windowsazure/jj156160.aspx)(MSDN)。
- [SQL データベース移行ウィザード](http://sqlazuremw.codeplex.com/)です。 データベースと SQL データベースの間を移行するためのオープン ソース ツールです。

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server と Windows Azure SQL データベースの選択

- [Windows Azure SQL データベースの SQL Server の比較](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx)(Microsoft TechNet サイト)。
- [Windows Azure SQL データベースへのデータ移行: ツールや技法](https://msdn.microsoft.com/en-us/library/windowsazure/hh694043.aspx)(MSDN)。 SQL データベースに SQL Server を比較し、SQL Server から SQL データベースに移行する場合のガイダンスを提供するセクションが含まれます。
- [Windows Azure SQL データベース提供ガイド](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx)(Microsoft TechNet サイト)。
- [SQL Server 機能の制限 (Windows Azure SQL データベース)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394115.aspx) (MSDN)。
- [Windows Azure テーブル ストレージと Windows Azure SQL Database の比較し、対照的](https://msdn.microsoft.com/en-us/library/jj553018.aspx)(MSDN)。 Windows Azure にデプロイするアプリケーション、Windows Azure テーブル ストレージの代わりに Windows Azure SQL データベース可能性があります。 このトピックでは、これらの方法の間で決定するのに役立ちます。
- [Windows Azure SQL データベース](https://msdn.microsoft.com/en-us/library/windowsazure/ee336279)(MSDN)。
- [ガイドラインと制限事項 (Windows Azure SQL データベース)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>NoSQL データベース管理システムでの作業

- [Windows Azure データ サービス](https://www.windowsazure.com/en-us/develop/net/data/)(Microsoft Azure サイト)。 参照してください、[テーブル サービス機能のガイド](https://docs.microsoft.com/azure/)と**ビッグ データ**ページのセクションです。
- [ASP.NET 多層アプリケーションを使用してストレージ テーブル、キュー、および Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure サイト)。 Windows Azure ストレージの NoSQL テーブルを使用してダウンロードできるサンプル アプリケーションでエンド ツー エンド チュートリアルです。

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>ASP.NET アプリケーションでの LINQ クエリの使用

- [ASP.NET データ アクセス オプション](https://msdn.microsoft.com/en-us/library/ms178359.aspx#linq)(MSDN)。 LINQ の概要が含まれます。
- [LINQ のトレーニング ビデオ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/)(行えるのブログ)。
- [動的な LINQ リソースへのリンクを持つ ASP.NET フォーラム スレッド](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)です。

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>動的データ スキャフォールディングを使用します。

- [動的なデータ プロジェクト テンプレート](https://msdn.microsoft.com/en-us/library/jj822927.aspx#dynamicdata)(MSDN)。 動的なデータ プロジェクトを使用する場合のガイダンスです。
- [ASP.NET 動的データ](https://msdn.microsoft.com/en-us/library/ee845452.aspx)(MSDN)。

<a id="securing"></a>

## <a name="securing-data-access"></a>データ アクセスを保護します。

- [ASP.NET でのデータ アクセスをセキュリティで保護する](https://msdn.microsoft.com/en-us/library/ms178375.aspx)(MSDN)。
- [セキュリティに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc716760.aspx) (MSDN)。
- [方法: は、データ ソース コントロールを使用する場合、接続文字列をセキュリティで保護](https://msdn.microsoft.com/en-us/library/ms178372.aspx)(MSDN)。

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>データ アクセス パフォーマンスを最適化します。

- [ASP.NET のパフォーマンス概要](https://msdn.microsoft.com/en-us/library/cc668225.aspx)(MSDN)。
- [ASP.NET キャッシュ](https://msdn.microsoft.com/en-us/library/xsbfdd8c.aspx)(MSDN)。
- [ASP.NET のパフォーマンスを向上させる](https://msdn.microsoft.com/en-us/library/ff647787)(MSDN)。 このページの上部にある終了「コンテンツ」の警告が、情報の大部分は引き続き関連し、比較可能な更新済みのリソースはありません。
- [SQL Server のパフォーマンスを向上させる](https://msdn.microsoft.com/en-us/library/ff647793)(MSDN)。 前のリンクとして同じコメントです。

関連項目[を最適化する Entity Framework パフォーマンス](#optimizingef)このトピックで前述しました。

<a id="deploying"></a>

## <a name="deploying-a-database"></a>データベースの配置

- [ASP.NET Web 展開 - 推奨リソース](aspnet-web-deployment-content-map.md)(MSDN)。

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Web サービスを介してデータにアクセスします。

- [Web サービスを介してデータにアクセスする](https://msdn.microsoft.com/en-us/library/ms178359.aspx#webservice)(MSDN)。 WCF と Web API を使用する場合のガイダンスです。
- [ASP.NET Web API の概要](../web-api/index.md)です。
- [WCF Data Services](https://msdn.microsoft.com/en-us/data/bb931106) (MSDN)。

<a id="additional"></a>

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET データ アクセスに関する FAQ](https://msdn.microsoft.com/en-us/library/jj653753.aspx) (MSDN)。
- [ASP.NET Web フォームのデータのチュートリアル](../web-forms/overview/data-access/index.md)です。 これらのチュートリアルのほとんどは比較的古いです。参照してください[ASP.NET データ アクセス オプション](https://msdn.microsoft.com/en-us/library/ms178359.aspx)と[データ ストレージ オプション (実際のクラウド アプリのビルドと Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)最初に正しくないデータのアクセス方法を取得しないようにシナリオに合ったです。
- [ASP.NET MVC のコンテンツ マップ](../mvc/overview/getting-started/recommended-resources-for-mvc.md)です。
- [ASP.NET Web Pages のチュートリアル - データ](../web-pages/overview/data/index.md)です。
- [Visual Studio でのデータにアクセスする](https://msdn.microsoft.com/en-us/library/wzabh8c4.aspx)(MSDN)。 ようなリンクの一覧を示しますこのコンテンツ マップには、ASP.NET ではなく、Visual Studio に重点を置いています。
