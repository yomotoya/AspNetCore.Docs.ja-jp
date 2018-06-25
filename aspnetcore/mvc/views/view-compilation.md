---
title: ASP.NET Core での Razor ファイルのコンパイルとプリコンパイル
author: rick-anderson
description: Razor ファイルをプリコンパイルする利点と、ASP.NET Core アプリケーションで Razor ファイルのプリコンパイルを実行する方法について説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 6ef450a24f57c721021f77f6df5088574caa2645
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274041"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core での Razor ファイルのコンパイル

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
関連する MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。 ビルド時の Razor ファイルの公開はサポートされていません。 Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。
::: moniker-end
::: moniker range="= aspnetcore-2.0"
関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。 ビルド時の Razor ファイルの公開はサポートされていません。 Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。 Razor ファイルは、[Razor SDK](xref:razor-pages/sdk) を使用してビルド時と公開時にコンパイルされます。
::: moniker-end

## <a name="precompilation-considerations"></a>プリコンパイルに関する考慮事項

Razor ファイルのプリコンパイルの副作用を次に示します。

* 公開されるバンドルが小さくなります
* 起動時間が短縮されます
* Razor ファイルを編集することはできません。公開されるバンドルには関連付けられたコンテンツがありません。

## <a name="deploy-precompiled-files"></a>プリコンパイル済みファイルの展開

::: moniker range=">= aspnetcore-2.1"
Razor ファイルのビルドおよび発行時のコンパイルは、既定で Razor SDK によって有効になっています。 更新後の Razor ファイルの編集は、ビルド時にサポートされています。 既定では、*.cshtml* ファイルなしのコンパイルされた *Views.dll* のみがアプリケーションと展開されます。

> [!IMPORTANT]
> Razor SDK は、プロジェクト ファイルにプリコンパイル固有のプロパティが設定されていない場合のみ有効です。 たとえば、*.csproj* ファイルの `MvcRazorCompileOnPublish` プロパティを `true` に設定して、Razor SDK を無効にします。
::: moniker-end

::: moniker range="= aspnetcore-2.0"
プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。

ASP.NET Core 2.x プロジェクト テンプレートは既定で、暗黙的に `MvcRazorCompileOnPublish` プロパティを `true` に設定します。 そのため、この要素は *.csproj* ファイルから安全に削除できます。

> [!IMPORTANT]
> ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、Razor ファイルのプリコンパイルを使用できません。
::: moniker-end

::: moniker range="= aspnetcore-1.1"
`MvcRazorCompileOnPublish` プロパティを `true` に設定して、[Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。 次の *.csproj* サンプルでは、これらの設定が強調表示されています。

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
[.NET Core CLI publish コマンド](/dotnet/core/tools/dotnet-publish)を使用して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。 たとえば、プロジェクト ルートで、次のコマンドを実行します。

```console
dotnet publish -c Release
```

コンパイル済みの Razor ファイルを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。 たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end