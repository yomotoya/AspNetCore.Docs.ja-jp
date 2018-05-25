---
title: ASP.NET Core の Razor SDK
author: Rick-Anderson
description: ASP.NET Core の Razor ページを使用して、ページのコーディングに重点を置いたシナリオをより簡略化して、MVC を使用する場合よりも生産性を高める方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: acc049a69574968d1e304d6c504cb89243387d6c
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core の Razor SDK

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)] には、`Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK) が含まれています。 Razor SDK とは、次のとおりです。

* ASP.NET Core MVC ベース プロジェクト用の [Razor](xref:mvc/views/razor) ファイルを含むプロジェクトのビルド、パッケージ、および発行に関わる表示と操作を標準化できます。
* Razor ファイルのコンパイルのカスタマイズを可能にする事前定義されたターゲット、プロパティ、項目のセットを含みます。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Razor SDK を使用する

多くの Web アプリは、Razor SDK を明示的に参照する必要はありません。 

Razor SDK を使用して Razor ビューまたは Razor ページを含むクラス ライブラリを構築する方法は次のとおりです。

* `Microsoft.NET.Sdk.Razor` の代わりに `Microsoft.NET.Sdk` を使用します。
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Razor ページや Razor ビューをビルドおよびコンパイルするために必要な追加の依存関係を組み込むには、通常 `Microsoft.AspNetCore.Mvc` へのパッケージ参照が必要です。 少なくとも、プロジェクトで次へのパッケージ参照を追加する必要があります。

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 前述のパッケージは、`Microsoft.AspNetCore.Mvc` に組み込まれています。 次のマークアップは、Razor SDK で、ASP.NET Core Razor ページ アプリ用の Razor ファイルがビルドされた基本の *.csproj* ファイルです。
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>プロパティ

Razor の SDK の動作は、プロジェクトをビルドする際に次のプロパティにより制御されます。

* `RazorCompileOnBuild` : `true` の場合、プロジェクトをビルドする際に、Razor アセンブリがコンパイルおよび生成されます。 既定値は `true` です。
* `RazorCompileOnPublish` : `true` の場合、プロジェクトを発行する際に、Razor アセンブリがコンパイルおよび生成されます。 既定値は `true` です。

Razor SDK への入力および出力は、次のプロパティおよび項目を使用して構成されます。

| 項目                                         | 説明                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | コードの生成対象に入力する項目要素 (*.cshtml* ファイル) です。 |
| RazorCompile                                  | Razor コンパイル対象への入力である項目要素 (.cs ファイル) です。 Razor アセンブリに追加でコンパイルするファイルを指定するには、この ItemGroup を使用します。 |
| RazorTargetAssemblyAttribute                  | Razor アセンブリ用の属性をコード生成するために使用する項目要素です。 例:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | 生成された Razor アセンブリに埋め込みのリソースとして追加される項目要素です。 |

| プロパティ                                      | 説明                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Razor によって生成されたアセンブリの (拡張子なしの) ファイル名です。 | 
| RazorOutputPath                               | Razor の出力ディレクトリです。                                      |
| RazorCompileToolset                           | Razor アセンブリをビルドするために使用するツールセットを決定するために使用します。 有効な値は、`Implicit` および `PrecompilationTool` です。 |
| EnableDefaultContentItems                     | `true` の場合、*.cshtml* ファイルなどの特定の種類のファイルがプロジェクトのコンテンツとして含まれます。 Microsoft.NET.Sdk.Web を介して参照する場合、*wwwroot* 下のすべてのファイルおよび構成ファイルも含まれます。         |
| EnableDefaultRazorGenerateItems               | `true` の場合、`RazorGenerate` 項目の `Content` 項目の *.cshtml* ファイルが含まれます。 |
| GenerateRazorTargetAssemblyInfo               | `true` の場合、`RazorAssemblyAttribute` で指定された属性を含む *.cs* ファイルが生成され、それがコンパイルの出力が含まれます。 |
| EnableDefaultRazorTargetAssemblyInfoAttributes | `true` の場合、`RazorAssemblyAttribute` にアセンブリ属性の既定のセットが追加されます。 |
| CopyRazorGenerateFilesToPublishDirectory       | `true` の場合、RazorGenerate 項目 (*.cshtml* ファイル) が発行ディレクトリにコピーされます。 ビルド時または発行時にコンパイルに含まれる場合、通常発行済みアプリケーションには Razor ファイルは不要です。 既定値は `false` です。 |
| CopyRefAssembliesToPublishDirectory            | `true` の場合、発行ディレクトリに参照アセンブリ項目がコピーされます。 通常、Razor のコンパイルがビルド時または発行時に発生する場合、発行済みアプリケーションには参照アセンブリは不要です。 これが `true` に設定されている場合、発行済みのアプリケーションで実行時のコンパイルが必要な場合、実行時に cshtml ファイルが変更されたり、埋め込みのビューが使用されます。 既定値は `false` です。 |
| IncludeRazorContentInPack                      | `true` の場合、すべての Razor コンテンツ アイテム (*.cshtml* ファイル) は、生成された NuGet パッケージに含まれるものとしてマークされます。 既定値は `false` です。 |
| EmbedRazorGenerateSources | `true` の場合、生成された Razor アセンブリに、埋め込みファイルとして RazorGenerate (*.cshtml*) 項目が追加されます。 既定値は `false` です。 |
| UseRazorBuildServer                           | `true` の場合、コードの生成作業をオフロードするために、永続的なビルド サーバーが使用されます。 既定値は、`UseSharedCompilation` の値です。 |

### <a name="targets"></a>ターゲット
Razor SDK では、次の 2 つの主要なターゲットが定義されています。

* `RazorGenerate`: コードにより RazorGenerate 項目要素から *.cs* ファイルが生成されます。 このターゲットの前後に実行する追加のターゲットを指定するには、`RazorGenerateDependsOn` プロパティを使用します。
* `RazorCompile`: Razor アセンブリに生成された *.cs* ファイルをコンパイルします。 このターゲットの前後に実行する追加のターゲットを指定するには、`RazorCompileDependsOn` を使用します。
