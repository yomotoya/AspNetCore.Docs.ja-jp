---
title: Blazor 用のリンカーを構成する
author: guardrex
description: Blazor アプリを構築するときに、中間言語 (IL) リンカーを制御する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668065"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="5d7e5-103">Blazor 用のリンカーを構成する</span><span class="sxs-lookup"><span data-stu-id="5d7e5-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="5d7e5-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5d7e5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="5d7e5-105">Blazor では、各リリース モードのビルド中に[中間言語 (IL)](/dotnet/standard/managed-code#intermediate-language--execution) のリンク設定を実行して、出力アセンブリから不要な IL を削除します。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="5d7e5-106">次の方法のいずれかを使って、アセンブリのリンクを制御できます。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="5d7e5-107">MSBuild プロパティを使ってリンクをグローバルに無効にする。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="5d7e5-108">構成ファイルを使ってアセンブリごとにリンクを制御する。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="5d7e5-109">MSBuild プロパティを使ってリンクを無効にする</span><span class="sxs-lookup"><span data-stu-id="5d7e5-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="5d7e5-110">リリース モードでは、アプリをビルドするときに既定でリンクが有効になります。これには公開が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="5d7e5-111">すべてのアセンブリに対してリンクを無効にするには、プロジェクト ファイルで MSBuild プロパティ `<BlazorLinkOnBuild>` を `false` に設定します。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="5d7e5-112">構成ファイルを使ってリンクを制御する</span><span class="sxs-lookup"><span data-stu-id="5d7e5-112">Control linking with a configuration file</span></span>

<span data-ttu-id="5d7e5-113">XML の構成ファイルを用意してそのファイルをプロジェクト ファイル内で MSBuild 項目として指定することで、アセンブリごとにリンクを制御できます。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="5d7e5-114">構成ファイルの例 (*Linker.xml*) を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-114">The following is an example configuration file (*Linker.xml*):</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/aspnet/Blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="5d7e5-115">構成ファイルのファイル形式について詳しくは、[IL リンカー:xml 記述子の構文](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="5d7e5-116">`BlazorLinkerDescriptor` 項目を使ってプロジェクト ファイル内で構成ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="5d7e5-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
