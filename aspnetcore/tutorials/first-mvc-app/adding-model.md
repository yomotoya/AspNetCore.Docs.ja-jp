---
title: "ASP.NET Core MVC アプリへのモデルの追加"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: c2cd3cc81221c146dec70e487a17b33360eb6112
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

注: ASP.NET Core 2.0 テンプレートには、*Models* フォルダーが含まれています。

*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。 クラスに **Movie** という名前を付けて、次のプロパティを追加します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` フィールドは、データベースで主キー用に必要です。 

プロジェクトをビルドして、エラーがないことを確認します。 これで、**M**VC アプリに、**モ**デルがあることになります。

## <a name="scaffolding-a-controller"></a>コントローラーのスキャフォールディング

**ソリューション エクスプローラー**で、*Controllers* フォルダーを右クリックし、**[追加]、[コントローラー]** の順に選択します。

![前述の手順を参照](adding-model/_static/add_controller.png)

**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。

* [Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。 15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。
* 更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。

**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold2.png)

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
* 作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/&ast;.cshtml*)

データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。 ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。

アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。

```
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

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC メニュー](adding-model/_static/pmc.png)

PMC で、次のコマンドを入力します。

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**注:** `Install-Package` コマンドでエラーが発生した場合は、NuGet パッケージ マネージャーを開き、`Microsoft.EntityFrameworkCore.Tools` パッケージを検索してください。 これにより、パッケージをインストールしたり、パッケージが既にインストールされているかどうかを確認したりすることができます。 または、PMC で問題がある場合、[CLI を使用したアプローチ](#cli)を参照してください。

`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_Initial.cs* ファイルの `Up` メソッドを実行します。

<a name="cli"></a> 前述の手順は、PMC ではなく、コマンド ライン インターフェイス (CLI) を使用して実行できます。

* [EF Core ツール](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)を *.csproj* ファイルに追加します。
* (プロジェクト ディレクトリの) コンソールから、次のコマンドを実行します。

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Intellisense のコンテキスト メニュー (Model アイテムの ID、価格、リリース日、およびタイトル プロパティ)](adding-model/_static/ints.png)

## <a name="additional-resources"></a>その他の技術情報

* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
* [グローバライズとローカライズ](xref:fundamentals/localization)

>[!div class="step-by-step"]
[前のチュートリアル ビューの追加](adding-view.md)
[次のチュートリアル SQL の使用](working-with-sql.md)  
