---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: コント ローラー (VB) から、モデルのデータにアクセス |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 030e7cc966078b76b5f96229d437c9990f17656a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873145"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>コント ローラー (VB) から、モデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/accessing-your-models-data-from-a-controller.md)このチュートリアルのです。


このセクションで作成、新しい`MoviesController`クラスし、ムービー データが取得され、ビュー テンプレートを使用してブラウザーに表示するコードを記述します。 続行する前にアプリケーションをビルドすることを確認します。

右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。 次のオプションを選択します。

- コント ローラー名: **MoviesController**です。 (これは、既定値です)。
- テンプレート: **Entity Framework を使用して読み取り/書き込みアクションと、ビュー、コント ローラー**です。
- モデル クラス:**ムービー (MvcMovie.Models)** です。
- データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** です。
- ビュー: **Razor (CSHTML)** です。 (既定値です。)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

**[追加]** をクリックします。 Visual Web Developer では、次のファイルとフォルダーを作成します。

- *MoviesController.vb*ファイルはプロジェクトの*コント ローラー*フォルダーです。
- A*映画*プロジェクトのフォルダーに*ビュー*フォルダーです。
- *Create.vbhtml、Delete.vbhtml、Details.vbhtml、Edit.vbhtml*、および*Index.vbhtml*新しい*Views\Movies*フォルダーです。

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 のスキャフォールディング機構自動的に作成された、CRUD (作成、読み取り、更新、および削除) のアクション メソッドとするためのビューです。 作成、一覧表示、編集、およびムービーのエントリを削除することができますを完全に機能の web アプリケーションがあるようになりました。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーの URL にします。 既定のルーティングにアプリケーションが依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`は既定値にルーティング`Index`のアクション メソッド、`Movies`コント ローラー。 ブラウザー要求言い換えれば、`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`です。 いずれかがまだ追加されているため、ムービーの空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

クリックすると、**作成**と、ムービー情報がデータベースに保存されているサーバーにポストするフォームのボタンをクリックします。 リダイレクトしている、 */Movies* URL、一覧に新しく作成したムービーを確認できます。

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを確認します。

開く、 *Controllers\MoviesController.vb*ファイルし、生成された確認`Index`メソッドです。 ムービーのコント ローラーの一部、`Index`メソッドを次に示します。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

次の行から、`MoviesController`クラスは、前述のようにムービーのデータベース コンテキストをインスタンス化します。 ムービーのデータベース コンテキストを使用して、クエリ、編集、および映画を削除することができます。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

要求を`Movies`コント ローラーのすべてのエントリが返されます、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。

## <a name="strongly-typed-models-and-the-model-keyword"></a>モデルを厳密に型指定と@modelキーワード

このチュートリアルで既に説明しましたコント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートに、`ViewBag`オブジェクト。 `ViewBag`ビューに情報を渡す便利な遅延バインディングされた方法を提供する動的オブジェクトです。

ASP.NET MVC では、厳密に渡せるように入力データまたはオブジェクトをビュー テンプレートも用意されています。 これには、アプローチにより向上コンパイル時のコードと、Visual Web Developer エディターでは、豊富な IntelliSense のチェック厳密型指定されました。 このアプローチでを使用して、`MoviesController`クラスと*Index.vbhtml*ビュー テンプレート。

コードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すとき、`View`内のヘルパー メソッド、`Index`アクション メソッド。 コードが、これを渡します`Movies`表示するリストのコント ローラーから。

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

含めることによって、`@ModelType`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが期待しているオブジェクトの種類を指定することができます。 Visual Web Developer が自動的に、次を含め、ムービーのコント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.vbhtml*ファイル。

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

これは、`@ModelType`ディレクティブでは、コント ローラーを使用してビューに渡されるムービーの一覧にアクセスすることができます、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.vbhtml*テンプレート、ループ、映画を深める、 `foreach` 、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`です。 その他の利点にはコードのコンパイル時のチェックを取得して、完全なコード エディターで IntelliSense をサポートすることを意味します。

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>SQL Server Compact の使用

指定されたデータベース接続文字列が指す、entity Framework Code First の検出、`Movies`データベースをまだ、コードの最初にデータベースを自動的に作成するために存在しませんでした。 作成されたことを確認することができます、*アプリ\_データ*フォルダーです。 表示されない場合、 *Movies.sdf*ファイルをクリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダーです。

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

ダブルクリックして*Movies.sdf*を開くには**サーバー エクスプ ローラー**です。 展開し、**テーブル**フォルダーをデータベースに作成されているテーブルを参照してください。

> [!NOTE]
> ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**です。 (ソフトウェアへのリンク、このチュートリアル シリーズの第 1 部での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合は、閉じてから再度開いて Visual Web Developer する必要があります。


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットし、`EdmMetadata`テーブル。 `EdmMetadata` Entity Framework では、モデルとデータベースの同期を決定するテーブルが使用されます。

右クリックし、`Movies`テーブルを選択して**テーブル データの表示**作成したデータを表示します。

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

右クリックし、`Movies`テーブルを選択して**テーブル スキーマの編集**です。

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`先ほど作成したクラスです。 Entity Framework Code First 自動的に作成されたこのスキーマに基づく、`Movie`クラスです。

完了したら、接続を閉じます。 (接続を終了しないで場合がありますエラーが発生した、次回プロジェクトを実行する)。

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。 チュートリアルでは、[次へ]、うまくスキャフォールディング コードの残りの部分を確認し、追加、`SearchIndex`メソッドおよび`SearchIndex`ビューをこのデータベースで映画を検索できます。

> [!div class="step-by-step"]
> [前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)
