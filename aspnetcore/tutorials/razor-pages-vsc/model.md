---
title: Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio Code を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38152972"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

* *Models* という名前のフォルダーを追加します。
* *Movie.cs* という名前の *Models* フォルダーにクラスを追加します。
* *Models/Movie.cs* ファイルに次のコードを追加します。

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

*Startup.cs* の先頭に次の `using` ステートメントを追加します。

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

プロジェクトをビルドして、エラーがないことを確認します。

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。 このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。 **注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。

以下のように、*RazorPagesMovie.csproj* ファイルを編集します。

* **[ファイル]**、 > **[ファイルを開く]** の順に選択してから、*RazorPagesMovie.csproj* ファイルを選択します。
* `Microsoft.EntityFrameworkCore.Tools.DotNet` 用のツール参照の 2 つ目の **\<ItemGroup >** への追加

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>ムービー モデルのスキャフォールディング

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

**注: Windows で次のコマンドを実行します。MacOS および Linux の場合は、次のコマンドを参照してください。**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* MacOS および Linux では、次のコマンドを実行します。

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

エラーが発生した場合は、次のようにします。
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio を終了し、コマンドを再度実行します。

[!INCLUDE [model 4](../../includes/RP/model4.md)]

次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。

> [!div class="step-by-step"]
> [前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-vsc/page)
