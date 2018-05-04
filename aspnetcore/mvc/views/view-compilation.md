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
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>ASP.NET Core での Razor ビューのコンパイルとプリコンパイル

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビューは、ビューが呼び出されるときに、実行時にコンパイルされます。 ASP.NET Core 1.1.0 以降では、必要に応じて Razor ビューをコンパイルし、それらをアプリを使用して展開します (プリコンパイルというプロセス)。 ASP.NET Core 2.x プロジェクトのテンプレートでは、既定でプリコンパイルが有効になります。

> [!IMPORTANT]
> ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、現時点では Razor ビューのプリコンパイルを使用できません。 この機能は 2.1 のリリース時に SCD で使用可能になります。 詳細については、「[View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102)」 (Windows の Linux のクロス コンパイル時にビューをコンパイルできない) を参照してください。

プリコンパイルに関する考慮事項:

* ビューをプリコンパイルすると、公開されるバンドルが小さくなり、起動時間が短縮されます。
* ビューをプリコンパイルした後に Razor ファイルを編集することはできません。 公開されたバンドルには編集されたビューは存在しません。 

プリコンパイル済みのビューを展開するには、次のようにします。

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) へのパッケージ参照を含めます。

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。

ASP.NET Core 2.x プロジェクトのテンプレートでは既定で暗黙的に `MvcRazorCompileOnPublish` が `true` に設定されます。これは、このノードを *.csproj* ファイルから安全に削除できることを意味します。 明示的に `MvcRazorCompileOnPublish` プロパティを `true` に設定しても問題はありません。 次の *.csproj* サンプルでは、この設定が強調表示されています。

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
`MvcRazorCompileOnPublish` を `true` に設定し、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` へのパッケージ参照を含めます。 次の *.csproj* サンプルでは、これらの設定が強調表示されています。

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

[.NET Core CLI publish コマンド](/dotnet/core/tools/dotnet-publish)を使用して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。 たとえば、プロジェクト ルートで、次のコマンドを実行します。

```console
dotnet publish -c Release
```

コンパイル済みの Razor ビューを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。 たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)
