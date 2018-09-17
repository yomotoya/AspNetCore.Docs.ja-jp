---
title: ASP.NET Core 2.1 以降用 Microsoft.AspNetCore.App メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538454"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>ASP.NET Core 2.1 用の Microsoft.AspNetCore.App メタパッケージ

この機能では、.NET Core 2.1 以降を対象とする ASP.NET Core 2.1 以降が必要です。

ASP.NET Core 用の [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [メタパッケージ](/dotnet/core/packages#metapackages):

* [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/)、[IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) を除き、サードパーティの依存関係は含まれません。 含まれるサードパーティの依存関係は、主要なフレームワークの機能を保証するために必要なものです。
* サードパーティの依存関係 (上で示したもの以外) を含むものを除き、ASP.NET Core チームによってサポートされているすべてのパッケージが含まれます。
* サードパーティの依存関係 (上で示したもの以外) を含むものを除き、Entity Framework Core チームによってサポートされているすべてのパッケージが含まれます。

`Microsoft.AspNetCore.App` パッケージには、ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降のすべての機能が含まれます。 ASP.NET Core 2.1 以降を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。 ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降を対象とするアプリケーションは、`Microsoft.AspNetCore.App` パッケージを使うことをお勧めします。

`Microsoft.AspNetCore.App` メタパッケージのバージョン番号は、ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。

`Microsoft.AspNetCore.App` メタパッケージを使うと、アプリを保護するバージョンの制限が提供されます。

* `Microsoft.AspNetCore.App` 内のパッケージに対して (直接的ではなく) 推移的な依存関係を持つパッケージが含まれ、それらのバージョン番号が異なる場合、NuGet はエラーを生成します。
* アプリに追加される他のパッケージは、`Microsoft.AspNetCore.App` に含まれるパッケージのバージョンを変更できません。
* バージョンの整合性により信頼性の高いエクスペリエンスが保証されます。 `Microsoft.AspNetCore.App` は、関連するビットのテストされていないバージョンの組み合わせが同じアプリ内で一緒に使われるのを防ぐように設計されました。

`Microsoft.AspNetCore.App` メタパッケージを使うアプリケーションでは、ASP.NET Core 共有フレームワークが自動的に利用されます。 `Microsoft.AspNetCore.App` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**&mdash;ASP.NET Core 共有フレームワークにはこれらの資産が含まれています。 共有フレームワーク内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。 詳しくは、「[.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)」で "共有フレームワーク" に関する説明をご覧ください。

次のプロジェクト ファイルは ASP.NET Core の `Microsoft.AspNetCore.App` メタパッケージを参照し、一般的な ASP.NET Core 2.1 のテンプレートを表します。

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

`Microsoft.AspNetCore.App` 参照のバージョン番号では、共有フレームワークのバージョンが選択されることが保証**されません**。 たとえば、バージョン `2.1.1` が指定されているのに、インストールされているのは `2.1.3` であるものとします。 この場合、アプリは `2.1.3` を使います。 お勧めしませんが、ロールフォワードの動作 (パッチとマイナーの両方または一方) を無効にすることができます。 パッケージ バージョンのロールフォワード動作の詳細については、[dotnet ホストのロールフォワード](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)に関するページを参照してください。

## <a name="update-aspnet-core"></a>ASP.NET Core の更新

`Microsoft.AspNetCore.App` [メタパッケージ](/dotnet/core/packages#metapackages)は、NuGet から更新される従来型のパッケージではありません。 `Microsoft.NETCore.App` に似て、`Microsoft.AspNetCore.App` は共有ランタイムを表しており、NuGet の外部で処理される特殊なバージョン管理セマンティクスを持ちます。 詳しくは、「[パッケージ、メタパッケージ、フレームワーク](/dotnet/core/packages)」をご覧ください。

ASP.NET Core を更新するには:

* 開発用コンピューターやビルド サーバー上: [.NET Core SDK](https://www.microsoft.com/net/download) をダウンロードしてインストールします。
* 配置サーバー上: [.NET Core ランタイム](https://www.microsoft.com/net/download)をダウンロードしてインストールします。

 アプリケーションは、アプリケーションの再起動時にインストールされている最新バージョンにロールフォワードされます。 プロジェクト ファイル内で `Microsoft.AspNetCore.App` バージョン番号を更新する必要はありません。 詳細については、「[フレームワーク依存のアプリをロールフォワードする](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)」を参照してください。

アプリケーションで以前に `Microsoft.AspNetCore.All` を使っていた場合は、「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](xref:fundamentals/metapackage#migrate)」をご覧ください。
