---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "コント ローラーから、モデルのデータにアクセス |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 82b4521279dcd9b9dc5a8e81b3a0d87ab26d46ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーから、モデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


このセクションで作成、新しい`MoviesController`クラスし、ムービー データが取得され、ビュー テンプレートを使用してブラウザーに表示するコードを記述します。

**アプリケーションをビルドする**次の手順に進む前にします。

右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。 次のオプションは、アプリケーションをビルドするまでは表示されません。 次のオプションを選択します。

- コント ローラー名: **MoviesController**です。 (これは、既定値です。 )
- テンプレート: **Entity Framework を使用して読み取り/書き込みアクションと、ビュー、MVC コント ローラー**です。
- モデル クラス:**ムービー (MvcMovie.Models)**です。
- データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)**です。
- ビュー: **Razor (CSHTML)**です。 (既定値です。)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

**[追加]**をクリックします。 Visual Studio Express には、次のファイルとフォルダーを作成します。

- *MoviesController.cs*ファイルはプロジェクトの*コント ローラー*フォルダーです。
- A*映画*プロジェクトのフォルダーに*ビュー*フォルダーです。
- *Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*Views\Movies*フォルダーです。

ASP.NET MVC 4 に自動的に作成された、CRUD (作成、読み取り、更新、および削除) のアクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビューです。 作成、一覧表示、編集、およびムービーのエントリを削除することができますを完全に機能の web アプリケーションがあるようになりました。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して*/Movies*お使いのブラウザーのアドレス バーの URL にします。 既定のルーティングにアプリケーションが依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`は既定値にルーティング`Index`のアクション メソッド、`Movies`コント ローラー。 ブラウザー要求言い換えれば、`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`です。 いずれかがまだ追加されているため、ムービーの空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。

![](accessing-your-models-data-from-a-controller/_static/image3.png)

クリックすると、**作成**と、ムービー情報がデータベースに保存されているサーバーにポストするフォームのボタンをクリックします。 リダイレクトしている、 */Movies* URL、一覧に新しく作成したムービーを確認できます。

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを確認します。

開く、 *Controllers\MoviesController.cs*ファイルし、生成された確認`Index`メソッドです。 ムービーのコント ローラーの一部、`Index`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

次の行から、`MoviesController`クラスは、前述のようにムービーのデータベース コンテキストをインスタンス化します。 ムービーのデータベース コンテキストを使用して、クエリ、編集、および映画を削除することができます。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

要求を`Movies`コント ローラーのすべてのエントリが返されます、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。

## <a name="strongly-typed-models-and-the-model-keyword"></a>モデルを厳密に型指定と@modelキーワード

このチュートリアルで既に説明しましたコント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートに、`ViewBag`オブジェクト。 `ViewBag`ビューに情報を渡す便利な遅延バインディングされた方法を提供する動的オブジェクトです。

ASP.NET MVC では、厳密に渡せるように入力データまたはオブジェクトをビュー テンプレートも用意されています。 これには、アプローチにより向上コンパイル時のコードと、Visual Studio エディターでは、豊富な IntelliSense のチェック厳密型指定されました。 Visual Studio でスキャフォールディング メカニズムには、使用してこのアプローチが使用されている、`MoviesController`メソッドとビューが作成されたとき、クラスとビューのテンプレートです。

*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッドです。 ムービーのコント ローラーの一部、`Details`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

場合、`Movie`が見つかるのインスタンス、`Movie`モデルは、詳細ビューに渡されます。 内容の確認、 *Views\Movies\Details.cshtml*ファイル。

含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが期待しているオブジェクトの種類を指定することができます。 ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、 *Details.cshtml*テンプレート コードを渡します各ムービーのフィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で、厳密に型指定された HTML ヘルパー`Model`オブジェクト。 作成および編集する方法とテンプレートの表示もムービー モデル オブジェクトを渡します。

確認、 *Index.cshtml*ビュー テンプレートと`Index`メソッドで、 *MoviesController.cs*ファイル。 コードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx)オブジェクトを呼び出すとき、`View`内のヘルパー メソッド、`Index`アクション メソッド。 コードが、これを渡します`Movies`表示するリストのコント ローラーから。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

ビデオ コント ローラーを作成したときに Visual Studio Express に自動的に含まれている次`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

これは、`@model`ディレクティブでは、コント ローラーを使用してビューに渡されるムービーの一覧にアクセスすることができます、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.cshtml*テンプレート、ループ、映画を深める、 `foreach` 、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`です。 その他の利点にはコードのコンパイル時のチェックを取得して、完全なコード エディターで IntelliSense をサポートすることを意味します。

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB の使用

指定されたデータベース接続文字列が指す、entity Framework Code First の検出、`Movies`データベースをまだ、コードの最初にデータベースを自動的に作成するために存在しませんでした。 作成されたことを確認することができます、*アプリ\_データ*フォルダーです。 表示されない場合、 *Movies.mdf*ファイルをクリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダーです。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

ダブルクリックして*Movies.mdf*を開くには**データベース エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダーです。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> データベース エクスプ ローラーが表示されない場合から、**ツール**メニューの **データベースへの接続**、キャンセルして、**データ ソースの選択**ダイアログ。 これにより、開いているデータベース エクスプ ローラー。


> [!NOTE]
> VWD または Visual Studio 2010 を使用するし、次の次のいずれかのようなエラーを表示します。
> 
> - データベース ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES です。MDF' 706 のバージョンになっているために開くことができません。 このサーバーは、655 およびそれ以前のバージョンをサポートします。 ダウン グレード パスはサポートされていません。
> - &quot;これらの列は、ユーザー コードによってハンドルされませんでした。&quot;渡された SqlConnection が初期カタログが指定されていません。
> 
> インストールする必要があります、 [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)と[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)です。 確認、`MovieDBContext`前のページで指定された接続文字列。


右クリックし、`Movies`テーブルを選択して**テーブル データの表示**作成したデータを表示します。

![](accessing-your-models-data-from-a-controller/_static/image8.png)

右クリックし、`Movies`テーブルを選択して**テーブル定義を開く**に構造体を Entity Framework Code First が自動的に作成するテーブルを参照してください。

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`先ほど作成したクラスです。 Entity Framework Code First 自動的に作成されたこのスキーマに基づく、`Movie`クラスです。

完了したらを右クリックして、接続を閉じる*MovieDBContext*を選択して**閉じる接続**です。 (接続を終了しないで場合がありますエラーが発生した、次回プロジェクトを実行する)。

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。 チュートリアルでは、[次へ]、うまくスキャフォールディング コードの残りの部分を確認し、追加、`SearchIndex`メソッドおよび`SearchIndex`ビューをこのデータベースで映画を検索できます。

>[!div class="step-by-step"]
[前へ](adding-a-model.md)
[次へ](examining-the-edit-methods-and-edit-view.md)
