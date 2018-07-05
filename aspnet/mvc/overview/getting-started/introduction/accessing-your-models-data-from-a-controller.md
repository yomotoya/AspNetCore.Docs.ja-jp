---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: コント ローラーからモデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817552"
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーからモデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。

**アプリケーションをビルド**次の手順に進む前にします。 アプリケーションをビルドしない場合は、コント ローラーを追加中にエラーが表示されます。

ソリューション エクスプ ローラーで右クリックし、*コント ローラー*フォルダーをクリック**追加**、し**コント ローラー**します。

![](accessing-your-models-data-from-a-controller/_static/image1.png)

**スキャフォールディングの追加**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラーとビュー、Entity Framework を使用して**、順にクリックします**追加**します。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- 選択**Movie (MvcMovie.Models)** モデル クラス。
- 選択**MovieDBContext (MvcMovie.Models)** のデータ コンテキスト クラス。
- コント ローラー名を入力**MoviesController**します。

  次の図には、完了のダイアログが表示されます。  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

**[追加]** をクリックします。 (エラーが発生した場合おそらくしていないアプリケーションを構築するコント ローラーの追加を開始する前にします。)Visual Studio には、次のファイルとフォルダーが作成されます。

- *MoviesController.cs*ファイル、*コント ローラー*フォルダー。
- A *views \movies*フォルダー。
- *Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*views \movies*フォルダー。

Visual Studio が自動的に作成、 [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビュー。 完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。

アプリケーションを実行し、をクリックして、 **MVC ムービー**リンク (するか、参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL に)。 アプリケーションが既定のルーティングに依存するため (で定義されている、*アプリ\_\routeconfig.cs*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクションメソッド`Movies`コント ローラー。 つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。 まだ追加していないため、映画の空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> [Price] フィールドに小数点またはコンマを入力することはできません。 コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (&quot;、&quot;) する必要があります、小数点と英語 (米国) 以外の日付形式は、 *globalize.js*と特定*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`します。 次のチュートリアルでこれを行う方法を紹介します。 ここでは、単に 10 のような整数を入力します。


クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。 リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを調べる

開く、 *Controllers\MoviesController.cs*ファイルを開き、生成された確認`Index`メソッド。 ムービー コント ローラーの一部、`Index`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`テーブルし、結果を渡す、`Index`ビュー。 次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。 ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>厳密に型指定されたモデルと@modelキーワード

このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。 `ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。

MVC を渡す機能も用意されています。*強く*型指定されたビュー テンプレートのオブジェクト。 この厳密に型指定のアプローチによりより優れたコンパイル時、コードのチェックで豊富な[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio エディターでします。 Visual Studio でスキャフォールディング メカニズムは、このアプローチを使用 (は、渡す、*強く*型指定されたモデル) で、`MoviesController`メソッドとビューが作成されたときに、クラスとビュー テンプレート。

*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッド。 `Details`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id`一般パラメーター、ルート データとしてたとえば`http://localhost:1234/movies/details/1`ムービー コント ローラー、アクション、コント ローラーを設定`details`と`id`を 1 にします。 渡すこともできますクエリ文字列を含む id で、次のように。

`http://localhost:1234/movies/details?id=1`

場合、`Movie`が見つかるのインスタンス、`Movie`にモデルが渡される、`Details`ビュー。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

内容を確認、 *Views\Movies\Details.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。 ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、 *Details.cshtml*テンプレートでは、このコードでは各ムービー フィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で厳密に型指定された HTML ヘルパー`Model`オブジェクト。 `Create`と`Edit`メソッドとビュー テンプレート ムービー モデル オブジェクトを渡すこともできます。

確認、 *Index.cshtml*テンプレートの表示と`Index`メソッドで、 *MoviesController.cs*ファイル。 このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。 このコードで、これは`Movies`一覧は、`Index`ビューにアクション メソッド。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Visual Studio が、次を自動的に含まれているムービー コント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

これは、`@model`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.cshtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。 その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB の使用

Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。 作成されたことを確認することができます、*アプリ\_データ*フォルダー。 表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。

![](accessing-your-models-data-from-a-controller/_static/image9.png)

ダブルクリック*Movies.mdf*を開く**サーバー エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダー。 Id。 横にあるキー アイコンに注意してください。 既定では、主キーの ID をという名前のプロパティが EF になります。 EF と MVC の詳細についてを参照してください Tom Dykstra の優れたチュートリアル[MVC と EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

右クリックし、`Movies`テーブルを選択**テーブル定義を開く**に構造を Entity Framework Code First が自動的に作成するテーブルを参照してください。

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。 Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。

完了したらを右クリックして接続を閉じる*MovieDBContext*選択**閉じる接続**します。 (接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

これで、データを表示、編集、更新および削除できるデータベースができました。 次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。 MVC を Entity Framework を使用する方法の詳細については、次を参照してください。 [、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。

> [!div class="step-by-step"]
> [前へ](creating-a-connection-string.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)
