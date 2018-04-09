---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: コント ローラーから、モデルのデータにアクセス |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーから、モデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションで作成、新しい`MoviesController`クラスし、ムービー データが取得され、ビュー テンプレートを使用してブラウザーに表示するコードを記述します。

**アプリケーションをビルドする**次の手順に進む前にします。 アプリケーションをビルドしない場合は、コント ローラーを追加中にエラーが表示されます。

ソリューション エクスプ ローラーで右クリックし、*コント ローラー*フォルダーをクリックして**追加**、し**コント ローラー**です。

![](accessing-your-models-data-from-a-controller/_static/image1.png)

**追加 Scaffold**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラー、ビューがある Entity Framework を使用**、順にクリック**追加**です。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- 選択**ムービー (MvcMovie.Models)**モデル クラス。
- 選択**MovieDBContext (MvcMovie.Models)**データ コンテキスト クラスです。
- コント ローラー名を入力してください。 **MoviesController**です。

  次の図では、完了したダイアログを表示します。  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

**[追加]** をクリックします。 (エラーが発生した場合可能性がありますいないアプリケーションを構築するコント ローラーの追加を開始する前にします。)Visual Studio では、次のファイルとフォルダーを作成します。

- *MoviesController.cs*ファイルで、*コント ローラー*フォルダーです。
- A *Views\Movies*フォルダーです。
- *Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*Views\Movies*フォルダーです。

Visual Studio に自動的に作成された、 [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) のアクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビューです。 作成、一覧表示、編集、およびムービーのエントリを削除することができますを完全に機能の web アプリケーションがあるようになりました。

アプリケーションを実行し、をクリックして、 **MVC ムービー**リンク (を参照または、`Movies`コント ローラーを追加して*/Movies*お使いのブラウザーのアドレス バーの URL に)。 既定のルーティングにアプリケーションが依存するため (で定義されている、*アプリ\_Start\RouteConfig.cs*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`は既定値にルーティング`Index`のアクションメソッド`Movies`コント ローラー。 ブラウザー要求言い換えれば、`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`です。 いずれかがまだ追加されているため、ムービーの空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> 価格 フィールドに小数点またはコンマを入力することはできません。 コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (&quot;、&quot;) する必要があります、小数点と日付の形式を英語 (米国) 以外の場合は、 *globalize.js*と特定の*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`です。 次のチュートリアルでこれを行う方法を示します。 ここでは、単に 10 のような整数を入力します。


クリックすると、**作成**と、ムービー情報がデータベースに保存されているサーバーにポストするフォームのボタンをクリックします。 リダイレクトしている、 */Movies* URL、一覧に新しく作成したムービーを確認できます。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを確認します。

開く、 *Controllers\MoviesController.cs*ファイルし、生成された確認`Index`メソッドです。 ムービーのコント ローラーの一部、`Index`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

要求を`Movies`コント ローラーのすべてのエントリが返されます、`Movies`テーブルが表示され、結果を渡します、`Index`ビュー。 次の行から、`MoviesController`クラスは、前述のようにムービーのデータベース コンテキストをインスタンス化します。 ムービーのデータベース コンテキストを使用して、クエリ、編集、および映画を削除することができます。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>モデルを厳密に型指定と@modelキーワード

このチュートリアルで既に説明しましたコント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートに、`ViewBag`オブジェクト。 `ViewBag`ビューに情報を渡す便利な遅延バインディングされた方法を提供する動的オブジェクトです。

MVC を渡す機能も用意されています。*強く*ビュー テンプレートにオブジェクトを入力します。 この厳密な型のアプローチによりより優れたコンパイル時、コードのチェックと豊富な[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio エディターでします。 Visual Studio でスキャフォールディング メカニズムには、このアプローチが使用されている (は、渡す、*厳密*に型指定されたモデル) で、`MoviesController`メソッドとビューが作成されたとき、クラスとビューのテンプレートです。

*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッドです。 `Details`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id`パラメーターはたとえば、ルート データとして渡されます`http://localhost:1234/movies/details/1`ムービー コント ローラー、アクション、コント ローラーを設定`details`と`id`を 1 にします。 渡すこともできますクエリ文字列を含む id で次のようにします。

`http://localhost:1234/movies/details?id=1`

場合、`Movie`が見つかるのインスタンス、`Movie`にモデルが渡される、`Details`ビュー。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

内容の確認、 *Views\Movies\Details.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが期待しているオブジェクトの種類を指定することができます。 ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、 *Details.cshtml*テンプレート コードを渡します各ムービーのフィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で、厳密に型指定された HTML ヘルパー`Model`オブジェクト。 `Create`と`Edit`ムービー モデル オブジェクトも渡すメソッドとテンプレートを表示します。

確認、 *Index.cshtml*ビュー テンプレートと`Index`メソッドで、 *MoviesController.cs*ファイル。 コードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すとき、`View`内のヘルパー メソッド、`Index`アクション メソッド。 コードが、これを渡します`Movies`一覧は、`Index`ビューにアクション メソッド。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Visual Studio が、次を自動的に含め、ムービーのコント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

これは、`@model`ディレクティブでは、コント ローラーを使用してビューに渡されるムービーの一覧にアクセスすることができます、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.cshtml*テンプレート、ループ、映画を深める、 `foreach` 、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`です。 その他の利点にはコードのコンパイル時のチェックを取得して、完全なコード エディターで IntelliSense をサポートすることを意味します。

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB の使用

指定されたデータベース接続文字列が指す、entity Framework Code First の検出、`Movies`データベースをまだ、コードの最初にデータベースを自動的に作成するために存在しませんでした。 作成されたことを確認することができます、*アプリ\_データ*フォルダーです。 表示されない場合、 *Movies.mdf*ファイルをクリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダーです。

![](accessing-your-models-data-from-a-controller/_static/image9.png)

ダブルクリックして*Movies.mdf*を開くには**サーバー エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダーです。 Id。 横にある鍵のアイコンに注意してください。 既定では、EF には、主キーの ID をという名前のプロパティとなります。 EF と MVC の詳細についてを参照してください Tom Dykstra の優れたチュートリアル[MVC および EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

右クリックし、`Movies`テーブルを選択して**テーブル データの表示**作成したデータを表示します。

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

右クリックし、`Movies`テーブルを選択して**テーブル定義を開く**に構造体を Entity Framework Code First が自動的に作成するテーブルを参照してください。

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`先ほど作成したクラスです。 Entity Framework Code First 自動的に作成されたこのスキーマに基づく、`Movie`クラスです。

完了したらを右クリックして、接続を閉じる*MovieDBContext*を選択して**閉じる接続**です。 (接続を終了しないで場合がありますエラーが発生した、次回プロジェクトを実行する)。

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

これで、データを表示、編集、更新および削除できるデータベースができました。 チュートリアルでは、[次へ]、うまくスキャフォールディング コードの残りの部分を確認し、追加、`SearchIndex`メソッドおよび`SearchIndex`ビューをこのデータベースで映画を検索できます。 MVC で Entity Framework の使用の詳細については、次を参照してください。 [、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。

> [!div class="step-by-step"]
> [前へ](creating-a-connection-string.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)
