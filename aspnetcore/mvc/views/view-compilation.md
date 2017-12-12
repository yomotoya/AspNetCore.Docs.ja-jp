---
title: "Razor ビューのコンパイルとプリコンパイル"
author: rick-anderson
description: "MVC Razor ビューのコンパイル、および ASP.NET Core アプリケーションでのプリコンパイルを有効にする方法を説明するリファレンス ドキュメント。"
keywords: "ASP.NET Core、Razor ビューのコンパイル、Razor 事前コンパイル、Razor プリコンパイル"
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Razor ビューのコンパイル、および ASP.NET Core でのプリコンパイル

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビューは、ビューが呼び出されたときに、実行時にコンパイルされます。 ASP.NET 1.1.0 のコアし以降が必要に応じて Razor ビューをコンパイルし、アプリの配置&mdash;プリコンパイルと呼ばれるプロセスです。 ASP.NET Core 2.x プロジェクト テンプレートは、既定では、プリコンパイルを有効にします。

> [!NOTE]
> Razor ビュー プリコンパイルを実行するときに現在使用できません、[自己完結型の配置 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0。 2.1 を離したときに、機能は Scd の使用可能になります。 詳細については、次を参照してください。 [Windows 上の Linux でのクロス コンパイルするときに、ビューのコンパイルが失敗した](https://github.com/aspnet/MvcPrecompilation/issues/102)です。

プリコンパイルの考慮事項:

* ビューをプリコンパイルするは、パブリッシュされたバンドル小さいと起動時間が高速になります。
* ビューをプリコンパイルした後、Razor ファイルを編集することはできません。 編集済みのビューは、パブリッシュされたバンドル内に存在することはできません。 

プリコンパイル済みのビューを展開します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

参照をパッケージを含める場合は、プロジェクトは、.NET Framework を対象、 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

プロジェクトは .NET Core をターゲットの変更の必要はありません。

ASP.NET Core 2.x プロジェクト テンプレートが暗黙的に設定`MvcRazorCompileOnPublish`に`true`既定では、つまりこのノードはから安全に削除できる、 *.csproj*ファイル。 明示的に指定する場合は、設定で害はありません、`MvcRazorCompileOnPublish`プロパティを`true`です。 次*.csproj*サンプルには、この設定が強調表示します。

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

設定`MvcRazorCompileOnPublish`に`true`、パッケージの参照を含めて、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`です。 次*.csproj*サンプルにはこれらの設定が強調表示されます。

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

A *< project_name >。PrecompiledViews.dll*プリコンパイルが成功した場合、コンパイル済みの Razor ビューを含むファイルが生成されます。 たとえば、次のスクリーン ショットの内容を示しています*Index.cshtml*内の*WebApplication1.PrecompiledViews.dll*:。

![DLL 内の razor ビュー](view-compilation/_static/razor-views-in-dll.png)
