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
# <a name="configure-the-linker-for-blazor"></a>Blazor 用のリンカーを構成する

作成者: [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor では、各リリース モードのビルド中に[中間言語 (IL)](/dotnet/standard/managed-code#intermediate-language--execution) のリンク設定を実行して、出力アセンブリから不要な IL を削除します。

次の方法のいずれかを使って、アセンブリのリンクを制御できます。

* MSBuild プロパティを使ってリンクをグローバルに無効にする。
* 構成ファイルを使ってアセンブリごとにリンクを制御する。

## <a name="disable-linking-with-an-msbuild-property"></a>MSBuild プロパティを使ってリンクを無効にする

リリース モードでは、アプリをビルドするときに既定でリンクが有効になります。これには公開が含まれます。 すべてのアセンブリに対してリンクを無効にするには、プロジェクト ファイルで MSBuild プロパティ `<BlazorLinkOnBuild>` を `false` に設定します。

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>構成ファイルを使ってリンクを制御する

XML の構成ファイルを用意してそのファイルをプロジェクト ファイル内で MSBuild 項目として指定することで、アセンブリごとにリンクを制御できます。

構成ファイルの例 (*Linker.xml*) を次に示します。

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

構成ファイルのファイル形式について詳しくは、[IL リンカー:xml 記述子の構文](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor)に関するページをご覧ください。

`BlazorLinkerDescriptor` 項目を使ってプロジェクト ファイル内で構成ファイルを指定します。

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
