---
title: "Visual Studio for Mac を使用する Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: f4fb4fc3402c866fa9f956341c06be34ca9f4763
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>データ モデルの追加

* ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。
* *Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。
* **[新しいファイル]** ダイアログで次を実行します。

  * 左側のウィンドウで **[全般]** を選択します。
  * 中央ウィンドウで **[空のクラス]** を選択します。
  * クラスに **Movie** という名前を付け、**[新規]** を選択します。

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

たとえば、`services.AddDbContext<MovieContext>(options =>` 行の `MovieContext` など、赤の波線を右クリックします。 **[Quick Fix]\(クイック フィックス\)、[using RazorPagesMovie.Models;](\RazorPagesMovie.Models; を使用\)** の順に選択します。 Visual studio によって using ステートメントが追加されます。

プロジェクトをビルドして、エラーがないことを確認します。

![[作成] ページ](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>移行用の Entity Framework Core NuGet パッケージ

コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。 [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) リンクをクリックして、使用するバージョン番号を取得します。 このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。 **注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。

*.csproj* ファイルを編集するには、次を実行します。

* **[ファイル]**、**[開く]** を選択して、*.csproj* ファイルを選択します。
* **[オプション]** を選択します。
* **[Open with](\次で開く\)** を **[ソース コード エディター]** に変更します。

![csproj ファイルの編集](model/csproj.png)

`Microsoft.EntityFrameworkCore.Tools.DotNet` ツール参照の 2 つ目の **\<ItemGroup>** への追加

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

執筆時点では、次のコードに示されているバージョン番号は正確です。

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>プロジェクトへの Pages/Movies ファイルの追加

* Visual Studio で *Pages* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。
* *Movies* フォルダーを選択します。
* *[Chosse files to include in the project](\プロジェクトに含めるファイルを選択する\)* ダイアログで、**[Include All](\すべて含める\)** を選択します。

次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。

>[!div class="step-by-step"]
[前: はじめに](xref:tutorials/razor-pages-mac/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)
