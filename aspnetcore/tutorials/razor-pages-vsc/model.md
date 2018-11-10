---
title: Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio Code を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244724"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

* *Models* という名前のフォルダーを追加します。
* *Movie.cs* という名前の *Models* フォルダーにクラスを追加します。
* *Models/Movie.cs* ファイルに次のコードを追加します。

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>SQLite 用の Entity Framework Core NuGet パッケージ

コマンド ラインから、次の .NET Core CLI コマンドを実行します。

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>データベース コンテキストの登録

*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

*Startup.cs* の先頭に次の `using` ステートメントを追加します。

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

プロジェクトをビルドして、エラーがないことを確認します。

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>ムービー モデルのスキャフォールディング

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* **Windows の場合**: 次のコマンドを実行します。

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **macOS および Linux の場合**: 次のコマンドを実行します。

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。

> [!div class="step-by-step"]
> [前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-vsc/page)
