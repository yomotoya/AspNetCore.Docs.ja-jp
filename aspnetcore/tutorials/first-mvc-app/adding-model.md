---
title: .ASP.NET Core MVC アプリへのモデルの追加
author: rick-anderson
description: 単純な ASP.NET Core アプリケーションにモデルを追加します。
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960669"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。 クラスに **Movie** という名前を付けて、次のプロパティを追加します。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` フィールドは、データベースで主キー用に必要です。 

プロジェクトをビルドして、エラーがないことを確認します。 これで、**M**VC アプリに、**モ**デルがあることになります。

## <a name="scaffolding-a-controller"></a>コントローラーのスキャフォールディング

::: moniker range=">= aspnetcore-2.1"

*ソリューション エクスプローラー*で、**Controllers** フォルダーを右クリックし、**[追加]、[スキャフォールディングされた新しい項目]** の順に選択します。

![前述の手順を参照](adding-model/_static/add_controller21.png)

**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

**ソリューション エクスプローラー**で、*Controllers* フォルダーを右クリックし、**[追加]、[コントローラー]** の順に選択します。

![前述の手順を参照](adding-model/_static/add_controller.png)

**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。

* [Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。 15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。
* 更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。

**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold2.png)

::: moniker-end

**[コントローラーの追加]** ダイアログ ボックスを完了します。

* **[Model class]\(モデル クラス\):** *Movie (MvcMovie.Models)*
* **[Data context class]\(データ コンテキスト クラス\):****+** アイコンを選択し、既定の **MvcMovie.Models.MvcMovieContext** を追加します。

![[データの追加] コンテキスト](adding-model/_static/dc.png)

* **ビュー:** 各オプションの既定値をオンにします。
* **コントローラー名:** 既定の *MoviesController* のままにします。
* **[追加]** をタップします。

![[コントローラーの追加] ダイアログ](adding-model/_static/add_controller2.png)

Visual Studio では、次が作成されます。

* Entity Framework Core の[データベース コンテキスト クラス](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)
* ムービー コントローラー (*Controllers/MoviesController.cs*)
* 作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (<em>Views/Movies/&ast;.cshtml</em>)

データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。 ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。

アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

データベースを作成する必要があり、それには EF Core [移行](xref:data/ef-mvc/migrations)機能を使用します。 移行では、データ モデルに一致するデータベースを作成し、データ モデルの変更時にデータベース スキーマを更新することができます。

## <a name="add-ef-tooling-and-perform-initial-migration"></a>EF ツールの追加と初期移行の実行

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。

* Entity Framework Core のツール パッケージを追加します。 このパッケージは移行を追加し、データベースを更新するために必要です。
* 初期移行を追加します。
* 初期移行でデータベースを更新します。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。

<!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC メニュー](adding-model/_static/pmc.png)

PMC で、次のコマンドを入力します。

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

次のエラー メッセージは無視してください。次のチュートリアルで修正しました。

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *エンティティ型 'Movie' の decimal 列 'Price' に型が指定されていません。これにより、値が既定の有効桁数と小数点以下桁数に収まらない場合、自動的に切り捨てられます。'ForHasColumnType()' を使用してすべての値に適合する SQL server 列の型を明示的に指定します。*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**注:** `Install-Package` コマンドでエラーが発生した場合は、NuGet パッケージ マネージャーを開き、`Microsoft.EntityFrameworkCore.Tools` パッケージを検索してください。 これにより、パッケージをインストールしたり、パッケージが既にインストールされているかどうかを確認したりすることができます。 または、PMC で問題がある場合、[CLI を使用したアプローチ](#cli)を参照してください。

::: moniker-end

`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_Initial.cs* ファイルの `Up` メソッドを実行します。

<a name="cli"></a> 前述の手順は、PMC ではなく、コマンド ライン インターフェイス (CLI) を使用して実行できます。

* [EF Core ツール](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)を *.csproj* ファイルに追加します。
* (プロジェクト ディレクトリの) コンソールから、次のコマンドを実行します。

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  アプリを実行してエラーが表示される場合:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

`dotnet ef database update` を実行していない可能性があります。

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Intellisense のコンテキスト メニュー (Model アイテムの ID、価格、リリース日、およびタイトル プロパティ)](adding-model/_static/ints.png)

## <a name="additional-resources"></a>その他の技術情報

* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
* [グローバライズとローカライズ](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [前のチュートリアル ビューの追加](adding-view.md)
> [次のチュートリアル SQL の使用](working-with-sql.md)  
