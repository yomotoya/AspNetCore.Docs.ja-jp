---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: コント ローラー (VB) から、モデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 0c0cdb7214b99908e4d64583e880fb03f69252c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369303"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>コント ローラー (VB) から、モデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/accessing-your-models-data-from-a-controller.md)このチュートリアルの。


このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。 続行する前に、アプリケーションを構築することを確認します。

右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。 次のオプションを選択します。

- コント ローラー名: **MoviesController**します。 (これは、既定値です)。
- テンプレート: **Entity Framework を使用して、読み取り/書き込み操作とビューとコント ローラー**します。
- モデル クラス: **Movie (MvcMovie.Models)** します。
- データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** します。
- ビュー: **Razor (CSHTML)** します。 (既定値です。)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

**[追加]** をクリックします。 Visual Web Developer には、次のファイルとフォルダーが作成されます。

- *MoviesController.vb*プロジェクトのファイル*コント ローラー*フォルダー。
- A*映画*プロジェクトのフォルダー*ビュー*フォルダー。
- *Create.vbhtml、Delete.vbhtml、Details.vbhtml、Edit.vbhtml*、および*Index.vbhtml*新しい*views \movies*フォルダー。

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 のスキャフォールディング機能は自動的に作成、CRUD (作成、読み取り、更新、および削除) アクション メソッドとビューの。 完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。 アプリケーションが既定のルーティングに依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクション メソッド、`Movies`コント ローラー。 つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。 まだ追加していないため、映画の空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。 リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを調べる

開く、 *Controllers\MoviesController.vb*ファイルを開き、生成された確認`Index`メソッド。 ムービー コント ローラーの一部、`Index`メソッドを次に示します。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。 ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。

## <a name="strongly-typed-models-and-the-model-keyword"></a>厳密に型指定されたモデルと@modelキーワード

このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。 `ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。

ASP.NET MVC では、ビュー テンプレートにオブジェクトやデータに型指定を厳密に渡す機能も提供します。 これにより、アプローチにより優れたコンパイル時のコードと Visual Web Developer のエディターでの高度な IntelliSense のチェックが厳密に型指定します。 このアプローチを使用しています、`MoviesController`クラスと*Index.vbhtml*テンプレートの表示。

このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。 このコードで、これは`Movies`コント ローラーからビューの一覧。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

含めることによって、`@ModelType`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。 Visual Web Developer で、次が自動的に含めムービー コント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.vbhtml*ファイル。

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

これは、`@ModelType`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.vbhtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。 その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>SQL Server Compact の使用

Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。 作成されたことを確認することができます、*アプリ\_データ*フォルダー。 表示されない場合、 *Movies.sdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

ダブルクリック*Movies.sdf*を開く**サーバー エクスプ ローラー**します。 展開し、**テーブル**フォルダーに、データベースで作成されたテーブルを参照してください。

> [!NOTE]
> ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**します。 (ソフトウェアへのリンク、このチュートリアル シリーズのパート 1 での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合を閉じて Visual Web Developer を再び開く必要があります。


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットをクリックし、`EdmMetadata`テーブル。 `EdmMetadata`テーブルは、モデルとデータベースの同期を確認する Entity Framework によって使用されます。

右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

右クリックし、`Movies`テーブルを選択**テーブル スキーマの編集**します。

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。 Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。

完了したら、接続を閉じます。 (接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。 次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。

> [!div class="step-by-step"]
> [前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)
