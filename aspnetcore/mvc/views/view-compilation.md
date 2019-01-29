---
title: ASP.NET Core での Razor ファイルのコンパイルとプリコンパイル
author: rick-anderson
description: Razor ファイルをプリコンパイルする利点と、ASP.NET Core アプリケーションで Razor ファイルのプリコンパイルを実行する方法について説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889757"
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
> このプリコンパイル ツールは、ASP.NET Core 3.0 で削除されます。 [Razor Sdk](xref:razor-pages/sdk) に移行することをお勧めします。
>
> Razor SDK は、プロジェクト ファイルにプリコンパイル固有のプロパティが設定されていない場合のみ有効です。 たとえば、*.csproj* ファイルの `MvcRazorCompileOnPublish` プロパティを `true` に設定して、Razor SDK を無効にします。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。

ASP.NET Core 2.x プロジェクト テンプレートは既定で、暗黙的に `MvcRazorCompileOnPublish` プロパティを `true` に設定します。 そのため、この要素は *.csproj* ファイルから安全に削除できます。

> [!IMPORTANT]
> このプリコンパイル ツールは、ASP.NET Core 3.0 で削除されます。 [Razor Sdk](xref:razor-pages/sdk) に移行することをお勧めします。
>
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

## <a name="recompile-razor-files-on-change"></a>変更時に Razor ファイルを再コンパイルする

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` では、ディスク上で Razor ファイル (Razor ビューと Razor Pages) が変更された場合に、ファイルが再コンパイルおよび更新されるかどうかを判断する値が取得または設定されます。

`true` に設定した場合、[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) によって、構成済みの <xref:Microsoft.Extensions.FileProviders.IFileProvider> インスタンス内の Razor ファイルに対する変更が監視されます。

次の場合、規定値は `true` です。

* ASP.NET Core 2.1 またはそれ以前のアプリ。
* 開発環境での ASP.NET Core 2.2 以降のアプリ。

`AllowRecompilingViewsOnFileChange` は互換性スイッチに関連付けられていて、アプリの構成済みの互換バージョンに応じて異なる動作を提供することができます。 `AllowRecompilingViewsOnFileChange` を設定することによるアプリの構成は、アプリの互換バージョンで指定される値よりも優先されます。

アプリの互換バージョンが <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> またはそれ以前に設定されている場合、明示的に構成しない限り `AllowRecompilingViewsOnFileChange` は `true` に設定されます。

アプリの互換バージョンが `CompatibilityVersion.Version_2_2` またはそれ以降に設定されている場合、環境が開発であるか値を明示的に構成しない限り、`AllowRecompilingViewsOnFileChange` は `false` に設定されます。

アプリの互換バージョンの設定に関するガイダンスと例については、<xref:mvc/compatibility-version> をご覧ください。

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
