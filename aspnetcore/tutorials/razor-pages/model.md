---
title: ASP.NET Core での Razor ページ アプリへのモデルの追加
author: rick-anderson
description: Entity Framework Core (EF Core) を利用し、データベースでムービーを管理するためのクラスを追加する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: a288b454ac1b418ef0deacb3643be22d440cb938
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>ASP.NET Core での Razor ページ アプリへのモデルの追加

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

*Models* フォルダーを右クリックします。 **[追加]**、**[クラス]** の順に選択します。 クラスに **Movie** という名前を付けて、次のプロパティを追加します。

[!INCLUDE [model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>データベース接続文字列の追加

*appsettings.json* ファイルに接続文字列を追加します。

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>データベース コンテキストの登録

*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

プロジェクトをビルドして、エラーがないことを確認します。

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>スキャフォールディング ツールの追加と初期移行の実行

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。

* Visual Studio Web コード生成パッケージを追加します。 スキャフォールディング エンジンを実行するには、このパッケージが必要です。
* 初期移行を追加します。
* 初期移行でデータベースを更新します。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

PMC で、次のコマンドを入力します。

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

代わりに、次の .NET Core CLI コマンドを使用できます。

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。

`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。 このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。 `Initial` 引数は移行の命名に使用されます。 任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。 詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。

`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。

[!INCLUDE [model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE [model 4](../../includes/RP/model4tbl.md)]

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
