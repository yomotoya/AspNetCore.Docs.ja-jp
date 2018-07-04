---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: コント ローラーからモデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: fb052b85d033f2c60f1fab6f5d5a1773aad22d35
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368613"
---
<a name="accessing-your-models-data-from-a-controller"></a>コント ローラーからモデルのデータにアクセスします。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。

**アプリケーションをビルド**次の手順に進む前にします。

右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。 アプリケーションをビルドするまでは、以下のオプションは表示されません。 次のオプションを選択します。

- コント ローラー名: **MoviesController**します。 (これは、既定値です。 )
- テンプレート: **Entity Framework を使用して、読み取り/書き込み操作とビューがある MVC コント ローラー**します。
- モデル クラス: **Movie (MvcMovie.Models)** します。
- データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** します。
- ビュー: **Razor (CSHTML)** します。 (既定値です。)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

**[追加]** をクリックします。 Visual Studio Express には、次のファイルとフォルダーが作成されます。

- *MoviesController.cs*プロジェクトのファイル*コント ローラー*フォルダー。
- A*映画*プロジェクトのフォルダー*ビュー*フォルダー。
- *Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*views \movies*フォルダー。

ASP.NET MVC 4 では自動的に作成、CRUD (作成、読み取り、更新、および削除) アクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビュー。 完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。 アプリケーションが既定のルーティングに依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクション メソッド、`Movies`コント ローラー。 つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。 まだ追加していないため、映画の空のリストになります。

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>ムービーを作成します。

**[Create New]\(新規作成\)** リンクを選択します。 ムービーの詳細を入力し、**作成**ボタンをクリックします。

![](accessing-your-models-data-from-a-controller/_static/image3.png)

クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。 リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

いくつかのムービー エントリを作成します。 **[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。

## <a name="examining-the-generated-code"></a>生成されたコードを調べる

開く、 *Controllers\MoviesController.cs*ファイルを開き、生成された確認`Index`メソッド。 ムービー コント ローラーの一部、`Index`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。 ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。

## <a name="strongly-typed-models-and-the-model-keyword"></a>厳密に型指定されたモデルと@modelキーワード

このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。 `ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。

ASP.NET MVC では、ビュー テンプレートにオブジェクトやデータに型指定を厳密に渡す機能も提供します。 これにより、アプローチにより優れたコンパイル時のコードと Visual Studio エディターでの高度な IntelliSense のチェックが厳密に型指定します。 Visual Studio でスキャフォールディング メカニズムでは、このアプローチを使用する、`MoviesController`メソッドとビューの作成時に、クラスとビューのテンプレート。

*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッド。 ムービー コント ローラーの一部、`Details`メソッドを次に示します。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

場合、`Movie`が見つかるのインスタンス、`Movie`モデルは、詳細ビューに渡されます。 内容を確認、 *Views\Movies\Details.cshtml*ファイル。

含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。 ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、 *Details.cshtml*テンプレートでは、このコードでは各ムービー フィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で厳密に型指定された HTML ヘルパー`Model`オブジェクト。 Create と Edit メソッドとビュー テンプレートもムービー モデル オブジェクトを渡します。

確認、 *Index.cshtml*テンプレートの表示と`Index`メソッドで、 *MoviesController.cs*ファイル。 このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。 このコードで、これは`Movies`コント ローラーからビューの一覧。

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

ムービー コント ローラーを作成したときに Visual Studio Express に自動的に含まれている、次`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

これは、`@model`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。 たとえば、 *Index.cshtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。 その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB の使用

Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。 作成されたことを確認することができます、*アプリ\_データ*フォルダー。 表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。

![](accessing-your-models-data-from-a-controller/_static/image6.png)

ダブルクリック*Movies.mdf*を開く**データベース エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダー。

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> データベース エクスプ ローラーが表示されない場合から、**ツール**メニューの **データベースへの接続**、キャンセルして、**データ ソースの選択**ダイアログ。 これにより、開いているデータベース エクスプ ローラー。


> [!NOTE]
> VWD または Visual Studio 2010 を使用して次の次のいずれかのようなエラーを表示します。
> 
> - データベース ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES します。MDF' 706 のバージョンであるため、開くことができません。 このサーバーは、655 およびそれ以前のバージョンをサポートします。 ダウン グレード パスがサポートされていません。
> - &quot;これらの列は、ユーザー コードによって処理されなかった&quot;渡された SqlConnection では、初期カタログが指定されていません。
> 
> インストールする必要がある、 [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)と[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)します。 確認、`MovieDBContext`前のページで指定された接続文字列。


右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。

![](accessing-your-models-data-from-a-controller/_static/image8.png)

右クリックし、`Movies`テーブルを選択**テーブル定義を開く**に構造を Entity Framework Code First が自動的に作成するテーブルを参照してください。

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。 Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。

完了したらを右クリックして接続を閉じる*MovieDBContext*選択**閉じる接続**します。 (接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。 次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。

> [!div class="step-by-step"]
> [前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)
