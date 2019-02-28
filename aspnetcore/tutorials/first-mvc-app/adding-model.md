---
title: ASP.NET Core MVC アプリへのモデルの追加
author: rick-anderson
description: 単純な ASP.NET Core アプリケーションにモデルを追加します。
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: ccdb7b920517c94b9154fe73b4ef1633f4ad0157
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833554"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC アプリへのモデルの追加

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Tom Dykstra](https://github.com/tdykstra)

このセクションでは、データベースのムービーを管理するクラスを追加します。 これらのクラスは、**M**VC アプリの "**モ**デル" 部分です。

[Entity Framework Core](/ef/core) (EF Core) でこれらのクラスを使用して、データベースを操作します。 EF Core は、記述する必要があるデータ アクセス コードを簡略化するオブジェクト リレーショナル マッピング (ORM) フレームワークです。

作成するモデル クラスは、EF Core に対する依存関係がないために、POCO クラス (**P**lain-**O**ld **C**LR **O**bjects から) と呼ばれます。 これらは単に、データベースに格納されるデータのプロパティを定義します。

このチュートリアルでは、まずモデル クラスを記述し、EF コアによってデータベースが作成されます。 ここで取り上げていない別の方法は、既存のデータベースからモデル クラスを生成する方法です。 その方法については、[ASP.NET Core の既存のデータベース](/ef/core/get-started/aspnetcore/existing-db)に関するページを参照してください。

## <a name="add-a-data-model-class"></a>データ モデル クラスの追加

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。 クラスに **Movie** と名前を付けます。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* *Movie.cs* という名前の *Models* フォルダーにクラスを追加します。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a>ムービー モデルのスキャフォールディング

このセクションでは、ムービー モデルがスキャフォールディングされます。 つまり、スキャフォールディング ツールにより、ムービー モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

*ソリューション エクスプローラー*で、**Controllers** フォルダーを右クリックし、**[追加]、[スキャフォールディングされた新しい項目]** の順に選択します。

![前述の手順を参照](adding-model/_static/add_controller21.png)

**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順に選択します。

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold21.png)

**[コントローラーの追加]** ダイアログ ボックスを完了します。

* **モデル クラス:** *Movie (MvcMovie.Models)*
* **データ コンテキスト クラス:** **+** アイコンを選択して、既定の **MvcMovie.Models.MvcMovieContext** を追加します。

![[データの追加] コンテキスト](adding-model/_static/dc.png)

* **ビュー:** 各オプションの既定値をオンにします。
* **コントローラー名:** 既定の *MoviesController* のままにします。
* **[追加]** を選択します。

![[コントローラーの追加] ダイアログ](adding-model/_static/add_controller2.png)

Visual Studio では、次が作成されます。

* Entity Framework Core の[データベース コンテキスト クラス](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)
* ムービー コントローラー (*Controllers/MoviesController.cs*)
* 作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (<em>Views/Movies/&ast;.cshtml</em>)

データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* スキャフォールディング ツールをインストールします。

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 次のコマンドを実行します。

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* スキャフォールディング ツールをインストールします。

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 次のコマンドを実行します。

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

データベースを作成する必要があり、それには EF Core [移行](xref:data/ef-mvc/migrations)機能を使用します。 移行では、データ モデルに一致するデータベースを作成し、データ モデルの変更時にデータベース スキーマを更新することができます。

<a name="pmc"></a>

## <a name="initial-migration"></a>最初の移行

このセクションでは、次のタスクを完了しました。

* 初期移行を追加します。
* 初期移行でデータベースを更新します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** (PMC) の順に選択します。

   ![PMC メニュー](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. PMC で、次のコマンドを入力します。

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。

   データベース スキーマは、`MvcMovieContext` クラスで指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。 `Initial` 引数は、移行の名前です。 任意の名前を使用できますが、慣例により、移行を説明する名前が使用されます。 詳細については、「<xref:data/ef-mvc/migrations>」を参照してください。

   `Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

`ef migrations add InitialCreate` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。

データベース スキーマは、`MvcMovieContext` クラスで指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。 `InitialCreate` 引数は、移行の名前です。 任意の名前を使用できますが、慣例により、移行を説明する名前が選択されます。

---

## <a name="examine-the-context-registered-with-dependency-injection"></a>依存関係挿入に登録されるコンテキストを調べる

ASP.NET Core には、[依存関係挿入 (DI)](xref:fundamentals/dependency-injection) が組み込まれています。 サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に DI に登録されます。 これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。 DB コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

スキャフォールディング ツールが自動的に DB コンテキストを作成し、それを DI コンテナーに登録します。

次の `Startup.ConfigureServices` メソッドを調べます。 強調表示された行は、スキャフォールダーによって追加されました。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`MvcMovieContext` では、`Movie` モデルのために EF Core 機能 (作成、読み取り、更新、削除など) が調整されます。 データ コンテキスト (`MvcMovieContext`) は [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) から派生されます。 データ コンテキストによって、データ モデルに含めるエンティティが指定されます: 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

上記のコードによって、エンティティ セットの [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) プロパティが作成されます。 Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応します。 エンティティはテーブル内の行に対応します。

[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。 ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

DB コンテキストを作成し、それを DI コンテナーに登録しました。

---

<a name="test"></a>

### <a name="test-the-app"></a>アプリのテスト

* アプリを実行し、ブラウザーで URL に `/Movies` を追加します ( `http://localhost:port/movies` )。

次のようなデータベースの例外が表示された場合: 

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

[移行手順](#pmc)を失敗しました。

* **[作成]** リンクをテストします。

  > [!NOTE]
  > `Price` フィールドに小数点のコンマを入力できない場合があります。 小数点にコンマ (",") を使う英語以外のロケール、および英語 (米国) 以外の日付形式で、[jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する必要があります。 グローバル化の手順については、[この GitHub の記事](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420)をご覧ください。

* **[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。

次の `Startup` クラスを調べます。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

上のコードで強調表示されている部分は、[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに追加されているムービー データベース コンテキストを示します。

* `services.AddDbContext<MvcMovieContext>(options =>` では、使用するデータベースと接続文字列を指定します。
* `=>` は[ラムダ演算子](/dotnet/articles/csharp/language-reference/operators/lambda-operator)です。

*Controllers/MoviesController.cs* ファイルを開いて、コンストラクターを調べます。

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`MvcMovieContext `) がコントローラーに挿入されています。 データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>厳密に型指定されたモデルと @model キーワード

コントローラーで `ViewData` ディクショナリを使ってビューにデータまたはオブジェクトを渡す方法を前に示しました。 `ViewData` ディクショナリは動的オブジェクトであり、ビューに情報を渡すための便利な遅延バインディングの方法を提供します。

MVC にも、厳密に型指定されたモデル オブジェクトをビューに渡す機能があります。 この厳密に型指定された方法を使うと、コンパイル時のコードのチェックが向上します。 スキャフォールディング メカニズムは、メソッドとビューを作成するときに、`MoviesController` クラスとビューでこの方法 (つまり、厳密に型指定されたモデルを渡すこと) を使いました。

*Controllers/MoviesController.cs* ファイルで生成された `Details` メソッドを調べてください。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

通常、`id` パラメーターはルート データとして渡されます。 たとえば、`https://localhost:5001/movies/details/1` は次のように設定します。

* コントローラーを `movies` コントローラーに (最初の URL セグメント)。
* アクションを `details` に (2 番目の URL セグメント)。
* ID を 1 に (最後の URL セグメント)。

次のようにクエリ文字列で `id` を渡すこともできます。

`https://localhost:5001/movies/details?id=1`

ID 値が指定されない場合、`id` パラメーターは [null 許容型](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) として定義されます。

ルート データまたはクエリ文字列の値と一致するムービー エンティティを選択するため、[ラムダ式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)が `FirstOrDefaultAsync` に渡されます。

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

ムービーが見つかった場合、`Movie` モデルのインスタンスが `Details` ビューに渡されます。

```csharp
return View(movie);
   ```

*Views/Movies/Details.cshtml* ファイルの内容を確認してください。

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

ビュー ファイルの先頭に `@model` ステートメントを含めることで、ビューが期待するオブジェクトの型を指定することができます。 ムービー コントローラーを作成したとき、*Details.cshtml* ファイルの先頭に次の `@model` ステートメントが自動的に追加されています。

```HTML
@model MvcMovie.Models.Movie
   ```

この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。 たとえば、*Details.cshtml* ビューでは、コードで厳密に型指定された `Model` オブジェクトを使って、`DisplayNameFor` および `DisplayFor` HTML ヘルパーに各ムービー フィールドを渡しています。 `Create` および `Edit` のメソッドとビューも、`Movie` モデル オブジェクトを渡します。

Movies コントローラーの *Index.cshtml* ビューと `Index` メソッドを確認してください。 コードで `View` メソッドを呼び出すときの `List` オブジェクトの作成方法に注意してください。 コードでは、この `Movies` リストを `Index` アクション メソッドからビューに渡しています。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

ムービー コントローラーを作成したとき、スキャフォールディングによって *Index.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーのリストにアクセスできます。 たとえば、*Index.cshtml* ビューのコードでは、`foreach` ステートメントを使って厳密に型指定された `Model` オブジェクトのムービーをループ処理しています。

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

`Model` オブジェクトは厳密に型指定されているので (`IEnumerable<Movie>` オブジェクトとして)、ループ内の各項目は `Movie` として型指定されます。 それ以外の利点としては、コンパイル時にコードのチェックが行われます。

## <a name="additional-resources"></a>その他の技術情報

* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
* [グローバライズとローカライズ](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [前のチュートリアル ビューの追加](adding-view.md)
> [次のチュートリアル SQL の使用](working-with-sql.md)  
