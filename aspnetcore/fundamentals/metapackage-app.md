---
title: ASP.NET Core 2.1 以降に対応した Microsoft.AspNetCore.App メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/21/2019
uid: fundamentals/metapackage-app
ms.openlocfilehash: 913e3d83fbf1af7ea995a88202f86c60b359a7e2
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085650"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21-or-later"></a>ASP.NET Core 2.1 以降に対応した Microsoft.AspNetCore.App メタパッケージ

この機能には、.NET Core 2.1 以降を対象とする ASP.NET Core 2.1 以降が必要です。

ASP.NET Core 用の [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [メタパッケージ](/dotnet/core/packages#metapackages):

* [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/)、[IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) を除き、サードパーティの依存関係は含まれません。 含まれるサードパーティの依存関係は、主要なフレームワークの機能を保証するために必要なものです。
* サードパーティの依存関係 (上で示したもの以外) を含むものを除き、ASP.NET Core チームによってサポートされているすべてのパッケージが含まれます。
* サードパーティの依存関係 (上で示したもの以外) を含むものを除き、Entity Framework Core チームによってサポートされているすべてのパッケージが含まれます。

`Microsoft.AspNetCore.App` パッケージには、ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降のすべての機能が含まれます。 ASP.NET Core 2.1 以降を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。 ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降を対象とするアプリケーションは、`Microsoft.AspNetCore.App` パッケージを使うことをお勧めします。

`Microsoft.AspNetCore.App` メタパッケージのバージョン番号は、最小の ASP.NET Core バージョンと Entity Framework Core バージョンを表します。

`Microsoft.AspNetCore.App` メタパッケージを使うと、アプリを保護するバージョンの制限が提供されます。

* `Microsoft.AspNetCore.App` 内のパッケージに対して (直接的ではなく) 推移的な依存関係を持つパッケージが含まれ、それらのバージョン番号が異なる場合、NuGet はエラーを生成します。
* アプリに追加される他のパッケージは、`Microsoft.AspNetCore.App` に含まれるパッケージのバージョンを変更できません。
* バージョンの整合性により信頼性の高いエクスペリエンスが保証されます。 `Microsoft.AspNetCore.App` は、関連するビットのテストされていないバージョンの組み合わせが同じアプリ内で一緒に使われるのを防ぐように設計されました。

`Microsoft.AspNetCore.App` メタパッケージを使うアプリケーションでは、ASP.NET Core 共有フレームワークが自動的に利用されます。 `Microsoft.AspNetCore.App` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**&mdash;ASP.NET Core 共有フレームワークにはこれらの資産が含まれています。 共有フレームワーク内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。 詳しくは、[共有フレームワーク](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/)に関するページをご覧ください。

次のプロジェクト ファイルは ASP.NET Core の `Microsoft.AspNetCore.App` メタパッケージを参照し、一般的な ASP.NET Core 2.1 のテンプレートを表します。

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

上記のマークアップは、一般的な ASP.NET Core 2.1 以降のテンプレートを表します。 `Microsoft.AspNetCore.App` パッケージ参照のバージョン番号は指定されていません。 バージョンが指定されていない場合は、[暗黙的な](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)バージョンが SDK によって指定されます (つまり `Microsoft.NET.Sdk.Web`)。 SDK によって指定される暗黙的なバージョンを利用し、パッケージ参照ではバージョン番号を明示的に設定しないことをお勧めします。 この方法に関して質問がある場合は、[Microsoft.AspNetCore.App の暗黙的なバージョンについてのディスカッション](https://github.com/aspnet/AspNetCore.Docs/issues/6430)で GitHub にコメントしてください。

ポータブル アプリの場合、暗黙的なバージョンは `major.minor.0` に設定されます。 共有フレームワークのロールフォワード メカニズムは、インストールされている共有フレームワークの中で最新の互換性のあるバージョンを使ってアプリを実行します。 開発、テスト、運用で確実に同じバージョンが使われるようにするため、すべての環境に同じバージョンの共有フレームワークをインストールしてください。 自己完結型アプリの場合は、暗黙的なバージョン番号は、インストールされている SDK にバンドルされている共有フレームワークの `major.minor.patch` に設定されます。

`Microsoft.AspNetCore.App` 参照でバージョン番号を指定しても、共有フレームワークのバージョンが選択されることは保証**されません**。 たとえば、バージョン "2.1.1" が指定されているのに、インストールされているのは "2.1.3" であるものとします。 この場合、アプリは "2.1.3" を使います。 お勧めしませんが、ロールフォワード (パッチとマイナーの両方または一方) を無効にすることができます。 .NET ホストのロールフォワードに関する詳細、およびその動作を構成する方法については、[.NET ホストのロールフォワード](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)に関するページをご覧ください。

::: moniker range="= aspnetcore-2.1"

`<Project Sdk` は、暗黙的バージョン `Microsoft.AspNetCore.App` を使用するように `Microsoft.NET.Sdk.Web` に設定する必要があります。 `<Project Sdk="Microsoft.NET.Sdk">` (末尾の `.Web` なし) を使用すると、次のようになります。

* 次の警告が生成されます。

  *Warning NU1604:Project dependency Microsoft.AspNetCore.App does not contain an inclusive lower bound.Include a lower bound in the dependency version to ensure consistent restore results.* (警告 NU1604: プロジェクト依存関係 Microsoft.AspNetCore.App には下限が含まれていません。復元結果に一貫性が与えられるように、依存関係バージョンに下限を追加してください。)

* これは、.NET Core 2.1 SDK に関する既知の問題です。

::: moniker-end

<a name="update"></a>

## <a name="update-aspnet-core"></a>ASP.NET Core の更新

`Microsoft.AspNetCore.App` [メタパッケージ](/dotnet/core/packages#metapackages)は、NuGet から更新される従来型のパッケージではありません。 `Microsoft.NETCore.App` に似て、`Microsoft.AspNetCore.App` は共有ランタイムを表しており、NuGet の外部で処理される特殊なバージョン管理セマンティクスを持ちます。 詳しくは、「[パッケージ、メタパッケージ、フレームワーク](/dotnet/core/packages)」をご覧ください。

ASP.NET Core を更新するには:

* 開発用コンピューターおよびビルド サーバーの場合:[.NET Core SDK](https://www.microsoft.com/net/download) をダウンロードしてインストールします。
* 配置サーバーの場合:[.NET Core ランタイム](https://www.microsoft.com/net/download)をダウンロードしてインストールします。

 アプリケーションは、アプリケーションの再起動時にインストールされている最新バージョンにロールフォワードされます。 プロジェクト ファイル内で `Microsoft.AspNetCore.App` バージョン番号を更新する必要はありません。 詳細については、「[フレームワーク依存のアプリをロールフォワードする](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)」を参照してください。

アプリケーションで以前に `Microsoft.AspNetCore.All` を使っていた場合は、「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](xref:fundamentals/metapackage#migrate)」をご覧ください。
