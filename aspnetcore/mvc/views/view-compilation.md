---
title: ASP.NET Core での Razor ビューのコンパイルとプリコンパイル
author: rick-anderson
description: ASP.NET Core アプリで Razor ビューのコンパイルとプリコンパイルを有効にする方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>ASP.NET Core での Razor ファイル (.cshtml) のコンパイル

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビューは、ビューが呼び出されるときに、実行時にコンパイルされます。 ASP.NET Core 2.1.0 以降では、[Razor Sdk](/aspnetcore/mvc/razor-pages/sdk) を使用してビューをビルド時または発行時にコンパイルします。 ASP.NET Core 1.1 および ASP.NET Core 2.0 では、ビューはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。 



プリコンパイルに関する考慮事項:

* ビューをプリコンパイルすると、公開されるバンドルが小さくなり、起動時間が短縮されます。
* ビューをプリコンパイルした後に Razor ファイルを編集することはできません。 公開されたバンドルには編集されたビューは存在しません。 

プリコンパイル済みのビューを展開するには、次のようにします。

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
Razor ファイルのビルドおよび発行時のコンパイルは、既定で Razor Sdk によって有効になっています。 更新後の Razor ファイルの編集は、ビルド時にサポートされています。 既定では、cshtml ファイルなしのコンパイルされた *Views.dll* のみがアプリケーションと展開されます。 
    
> [!IMPORTANT]
> Razor Sdk は、プロジェクト ファイルにプリコンパイル固有のプロパティが設定されていない場合のみ有効です。 たとえば、*.csproj* ファイルで `MvcRazorCompileOnPublish` を設定すると、Razor Sdk が無効になります。

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) へのパッケージ参照を含めます。

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。

ASP.NET Core 2.x プロジェクトのテンプレートでは既定で暗黙的に `MvcRazorCompileOnPublish` が `true` に設定されます。これは、このノードを *.csproj* ファイルから安全に削除できることを意味します。
    
> [!IMPORTANT]
> ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、Razor ビューのプリコンパイルを使用できません。 

[.NET Core CLI publish コマンド](/dotnet/core/tools/dotnet-publish)を使用して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。 たとえば、プロジェクト ルートで、次のコマンドを実行します。

```console
dotnet publish -c Release
```

コンパイル済みの Razor ビューを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。 たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

`MvcRazorCompileOnPublish` を `true` に設定し、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` へのパッケージ参照を含めます。 次の *.csproj* サンプルでは、これらの設定が強調表示されています。

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

