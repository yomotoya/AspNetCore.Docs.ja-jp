---
title: ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージは、ASP.NET Core 2.1 以降では推奨されていません。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148851"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ

> [!NOTE]
> ASP.NET Core 2.1 以降を対象とするアプリケーションは、このパッケージではなく [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) を使うことをお勧めします。 この記事の「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](#migrate)」をご覧ください。

この機能では、.NET Core 2.x を対象とする ASP.NET Core 2.x が必要です。

ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。

* ASP.NET Core チームでサポートされるすべてのパッケージ。
* Entity Framework Core でサポートされるすべてのパッケージ。
* ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。

`Microsoft.AspNetCore.All` パッケージには、ASP.NET Core 2.x および Entity Framework Core 2.x のすべての機能が含まれます。 ASP.NET Core 2.0 を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。

`Microsoft.AspNetCore.All` メタパッケージのバージョン番号は、ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。

`Microsoft.AspNetCore.All` メタパッケージを使用するアプリケーションでは、[.NET Core ランタイム ストア](/dotnet/core/deploying/runtime-store)が自動的に利用されます。 このランタイム ストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。 `Microsoft.AspNetCore.All` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**。 .NET Core ランタイム ストアにこれらの資産が含まれています。 ランタイム ストア内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。

使用しないパッケージは、パッケージのトリミング処理を使用して削除することができます。 トリミング処理されたパッケージは、公開済みのアプリケーション出力から除外されます。

次の *.csproj* ファイルは、ASP.NET Core の `Microsoft.AspNetCore.All` メタパッケージを参照しています。

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>暗黙的なバージョン管理

ASP.NET Core 2.1 以降では、バージョンなしで `Microsoft.AspNetCore.All` パッケージ参照を指定することができます。 バージョンが指定されていない場合は、暗黙的なバージョンが SDK によって指定されます (`Microsoft.NET.Sdk.Web`)。 SDK によって指定される暗黙的なバージョンを利用し、パッケージ参照ではバージョン番号を明示的に設定しないことをお勧めします。 この方法に関して質問がある場合は、[Microsoft.AspNetCore.App の暗黙的なバージョンについてのディスカッション](https://github.com/aspnet/Docs/issues/6430)で GitHub にコメントしてください。

ポータブル アプリの場合、暗黙的なバージョンは `major.minor.0` に設定されます。 共有フレームワークのロールフォワード メカニズムは、インストールされている共有フレームワークの中で最新の互換性のあるバージョンを使ってアプリを実行します。 開発、テスト、運用で確実に同じバージョンが使われるようにするため、すべての環境に同じバージョンの共有フレームワークをインストールしてください。 自己完結型アプリの場合は、暗黙的なバージョン番号は、インストールされている SDK にバンドルされている共有フレームワークの `major.minor.patch` に設定されます。

`Microsoft.AspNetCore.All` パッケージ参照でバージョン番号を指定しても、共有フレームワークのバージョンが選択されることは保証**されません**。 たとえば、バージョン "2.1.1" が指定されているのに、インストールされているのは "2.1.3" であるものとします。 この場合、アプリは "2.1.3" を使います。 お勧めしませんが、ロールフォワード (パッチとマイナーの両方または一方) を無効にすることができます。 .NET ホストのロールフォワードに関する詳細、およびその動作を構成する方法については、[.NET ホストのロールフォワード](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)に関するページをご覧ください。

暗黙的なバージョンの `Microsoft.AspNetCore.All` を使用するには、プロジェクト ファイルでプロジェクトの SDK を `Microsoft.NET.Sdk.Web` に設定する必要があります。 `Microsoft.NET.Sdk` SDK が (プロジェクト ファイル上部の `<Project Sdk="Microsoft.NET.Sdk">` で) 指定されている場合、次の警告が生成されます。

*Warning NU1604: Project dependency Microsoft.AspNetCore.App does not contain an inclusive lower bound.Include a lower bound in the dependency version to ensure consistent restore results.* (警告 NU1604: プロジェクト依存関係 Microsoft.AspNetCore.App には下限が含まれていません。復元結果に一貫性が与えられるように、依存関係バージョンに下限を追加してください。)

これは .NET Core 2.1 SDK に関する既知の問題であり、.NET Core 2.2 SDK で修正されます。

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行

以下のパッケージは、`Microsoft.AspNetCore.All` には含まれますが `Microsoft.AspNetCore.App` パッケージには含まれません。

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

アプリで上記のパッケージまたは上記のパッケージによって取り込まれるパッケージに含まれるいずれかの API を使っている場合、`Microsoft.AspNetCore.All` から `Microsoft.AspNetCore.App` に移行するには、これらのパッケージへの参照をプロジェクトに追加します。

上記のパッケージの依存関係のうち、他の部分で `Microsoft.AspNetCore.App` の依存関係になっていないものは、暗黙的に含まれることはありません。 例:

* `Microsoft.Extensions.Caching.Redis` の依存関係としての `StackExchange.Redis`
* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup` の依存関係としての `Microsoft.ApplicationInsights`

## <a name="update-aspnet-core-21"></a>ASP.NET Core 2.1 を更新する

2.1 以降用の `Microsoft.AspNetCore.App` メタパッケージに移行することをお勧めします。 `Microsoft.AspNetCore.All` メタパッケージを引き続き使用し、最新のバージョンの修正プログラムが配置されていることを確認するには、次のようにします。

* 開発用コンピューターやビルド サーバー上: 最新の [.NET Core SDK](https://www.microsoft.com/net/download) をインストールします。
* 展開サーバー上: 最新の [.NET Core ランタイム](https://www.microsoft.com/net/download)をインストールします。
 ご利用のアプリは、アプリケーションの再起動時にインストールされている最新バージョンにロールフォワードされます。
