---
title: ASP.NET Core での Razor ページ アプリへのモデルの追加
author: rick-anderson
description: Entity Framework Core (EF Core) を利用し、データベースでムービーを管理するためのクラスを追加する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 50b1b01448ad870a2889db7dfe8367ab9e661840
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729964"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>ASP.NET Core での Razor ページ アプリへのモデルの追加

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

*Models* フォルダーを右クリックします。 **[追加]**、**[クラス]** の順に選択します。 クラスに **Movie** という名前を付けて、次のプロパティを追加します。

`Movie` クラスの内容を次のコードに置き換えます。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>ムービー モデルのスキャフォールディング

このセクションでは、ムービー モデルがスキャフォールディングされます。 つまり、スキャフォールディング ツールにより、ムービー モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。

*Pages/Movies* フォルダーを作成します。

* **ソリューション エクスプローラー**で、*[Pages]* フォルダーを右クリックして、**[追加]** > **[新しいフォルダー]** の順に選択します。
* フォルダーに *Movies* という名前を付けます。

**ソリューション エクスプローラー**で、*[Pages/Movies]* フォルダーを右クリックし、**[追加]** > **[スキャフォールディングされた新しい項目]** の順に選択します。

![前の手順からのイメージ。](model/_static/sca.png)

**[スキャフォールディングを追加]** ダイアログで、**[Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用する Razor Pages (CRUD)\)** > **[追加]** の順に選択します。

![前の手順からのイメージ。](model/_static/add_scaffold.png)

**[Add Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用して Razor Pages (CRUD) を追加する\)** ダイアログを完了します。

* **[モデル クラス]** ドロップ ダウンで、**[Movie (RazorPagesMovie.Models)]** を選択します。
* **データ コンテキスト クラス**行で、**+** (プラス) 記号を選択し、生成された名前 **RazorPagesMovie.Models.RazorPagesMovieContext** を受け入れます。
* **[データ コンテキスト クラス]** ドロップ ダウンで、**[RazorPagesMovie.Models.RazorPagesMovieContext]** を選択します。
* **[追加]** を選びます。

![前の手順からのイメージ。](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>最初の移行の実行

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。

* 初期移行を追加します。
* 初期移行でデータベースを更新します。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

PMC で、次のコマンドを入力します。

```PMC
Add-Migration Initial
Update-Database
```

代わりに、次の .NET Core CLI コマンドを使用できます。

```console
dotnet ef migrations add Initial
dotnet ef database update
```

次の警告メッセージは無視してください。次のチュートリアルで修正します。

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。

エラーが発生した場合は、次のようにします。

SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.\(SqlException: ログインで要求されている "RazorPagesMovieContext-GUID" データベースを開くことができませんでした。\) The login failed.\(ログインに失敗しました。\)
Login failed for user 'User-name'.\(ユーザー 'ユーザー名' はログインできませんでした。\)

[移行手順](#pmc)を失敗しました。
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

*Models* フォルダーを右クリックします。 **[追加]**、**[クラス]** の順に選択します。 クラスに **Movie** という名前を付けて、次のプロパティを追加します。

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>データベース接続文字列の追加

*appsettings.json* ファイルに接続文字列を追加します。

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>データベース コンテキストの登録

[Startup クラスの ConfigureServices メソッド](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) で、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用してデータベース コンテキストを登録します。

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

プロジェクトをビルドして、エラーがないことを確認します。

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>スキャフォールディング ツールの追加と初期移行の実行

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。

* Visual Studio Web コード生成パッケージを追加します。 スキャフォールディング エンジンを実行するには、このパッケージが必要です。
* 初期移行を追加します。
* 初期移行でデータベースを更新します。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

PMC で、次のコマンドを入力します。

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

代わりに、次の .NET Core CLI コマンドを使用できます。

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

次のメッセージは無視してください。

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

これは次のチュートリアルで修正します。

`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。

`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>アプリのテスト

* アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。
* **[作成]** リンクをテストします。

  ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* **[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。

SQL の例外が発生した場合は、移行を実行済みであり、データベースを更新したことを確認します。

次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。

> [!div class="step-by-step"]
> [前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)    
