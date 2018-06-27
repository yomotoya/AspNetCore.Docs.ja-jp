---
title: IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する
author: guardrex
description: IHostingStartup 実装を使用して、外部アセンブリから ASP.NET Core アプリを拡張する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 47d3a64ce0cc543162a066eeeaa0aaaf7dc96a5f
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2018
ms.locfileid: "35415009"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する

作成者: [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) の実装により、アプリの `Startup` クラスの外部にある外部アセンブリからの起動時に拡張機能をアプリに追加できるようになります。 たとえば、外部ツール ライブラリでは `IHostingStartup` 実装を使用して、アプリに追加の構成プロバイダーやサービスを提供できます。 `IHostingStartup`  *は、ASP.NET Core 2.0 以降でのみ使用できます。*

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="discover-loaded-hosting-startup-assemblies"></a>読み込まれたホスティング スタートアップ アセンブリを見つける

アプリまたはライブラリによって読み込まれたホスティング スタートアップ アセンブリを見つけるには、ログ記録を有効にして、アプリケーション ログを確認します。 アセンブリの読み込み時に発生したエラーがログ記録されます。 読み込まれたホスティング スタートアップ アセンブリは、デバッグ レベルでログ記録され、すべてのエラーが記録されます。

サンプル アプリでは、[HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) を `string` 配列に読み込み、アプリのインデックス ページに結果を表示します。

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>ホスティング スタートアップ アセンブリの自動読み込みを無効にする

ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、2 つの方法があります。

* 「[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup)」(ホスティング スタートアップの禁止) のホスト構成設定を設定する
* `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数を設定する

ホストの設定または環境変数が `true` または `1` に設定されている場合、ホスティング スタートアップ アセンブリは自動的に読み込まれません。 どちらも設定されている場合は、ホスト設定で動作が制御されます。

ホスト設定または環境変数を使用してホスティング スタートアップ アセンブリを無効にすると、グローバルに無効化され、アプリのいくつかの属性も無効にされる場合があります。 ライブラリで独自の構成オプションが提供されない限り、現在、追加されたホスティング スタートアップ アセンブリを部分的に無効にすることはできません。 将来のリリースで、ホスティング スタートアップ アセンブリを部分的に無効にする機能が提供されます ([GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243) を参照)。

## <a name="implement-ihostingstartup"></a>IHostingStartup の実装

### <a name="create-the-assembly"></a>アセンブリの作成

`IHostingStartup` 拡張機能は、エントリ ポイントのない、コンソール アプリに基づいてアセンブリとして配置されます。 アセンブリでは、[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) パッケージが参照されます。

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性では、[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) のビルド時に、読み込みと実行のために `IHostingStartup` の実装としてクラスを識別します。 次の例では、名前空間は `StartupEnhancement`、クラスは `StartupEnhancementHostingStartup` です。

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

クラスは `IHostingStartup` を実装しています。 クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能を追加します。 ホスティング スタートアップ アセンブリでの `IHostingStartup.Configure` は、ユーザー コードの `Startup.Configure` よりも前にランタイムによって呼び出されます。これにより、ユーザー コードがホスティング スタートアップ アセンブリによって提供されるすべての構成を上書きすることができます。

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

`IHostingStartup` プロジェクトのビルド時に、依存関係ファイル (*\*.deps.json*) によってアセンブリの `runtime` の場所が *bin* フォルダーに設定されます。

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

ファイルの一部のみが示されています。 例のアセンブリ名は `StartupEnhancement` です。

### <a name="update-the-dependencies-file"></a>依存関係ファイルの更新

ランタイムの場所は、*\*.deps.json* ファイルで指定されます。 拡張機能をアクティブ化するには、`runtime` 要素で拡張機能のランタイム アセンブリの場所を指定する必要があります。 `runtime` の場所には `lib/<TARGET_FRAMEWORK_MONIKER>/` のプレフィックスを付けます。

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

サンプル アプリでは、*\*.deps.json* ファイルの変更は [PowerShell](/powershell/scripting/powershell-scripting) スクリプトによって実行されます。 PowerShell スクリプトは、プロジェクト ファイル内のビルド ターゲットによって自動的にトリガーされます。

### <a name="enhancement-activation"></a>拡張機能のアクティブ化

**アセンブリ ファイルの配置**

`IHostingStartup` 実装のアセンブリ ファイルは、アプリの *bin* に配置されるか、[ランタイム ストア](/dotnet/core/deploying/runtime-store)に配置される必要があります。

ユーザーごとに使用する場合は、ユーザー プロファイルのプロファイルのランタイム ストアにアセンブリを配置します。

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

グローバルに使用する場合は、.NET Core インストールのランタイム ストアにアセンブリを配置します。

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

アセンブリをランタイム ストアに配置するときに、シンボル ファイルも配置されますが、拡張機能を機能させるためには必要ありません。

**依存関係ファイルの配置**

実装の *\*.deps.json* ファイルは、アセンブリの場所に配置されている必要があります。

ユーザーごとに使用する場合は、ユーザー プロファイルの `.dotnet` 設定の `additonalDeps` フォルダーにファイルを配置します。

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

グローバルに使用する場合は、.NET Core インストールの `additonalDeps` フォルダーにファイルを配置します。

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

共有フレームワークのバージョンには、ターゲット アプリが使用する共有ランタイムのバージョンが反映されます。 共有ランタイムは、*\*.runtimeconfig.json* ファイルに示されます。 サンプル アプリでは、共有ランタイムは、*HostingStartupSample.runtimeconfig.json* ファイルで指定されます。

**環境変数の設定**

拡張機能を使用するアプリのコンテキストで次の環境変数を設定します。

ASPNETCORE_HOSTINGSTARTUPASSEMBLIES

ホスティング スタートアップ アセンブリのみが、`HostingStartupAttribute` に対してスキャンされます。 実装のアセンブリ名は、この環境変数で提供されます。 サンプル アプリでは、この値を `StartupDiagnostics` に設定します。

また、この値は「[Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies)」(ホスティング スタートアップ アセンブリ) のホスト構成設定を使用して設定することもできます。

複数のホスティング スタートアップ アセンブリがある場合、それらの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドは、アセンブリが一覧表示されている順序で実行されます。

DOTNET_ADDITIONAL_DEPS

実装の *\*.deps.json* ファイルの場所。

ファイルがユーザーごとに使用するためにユーザー プロファイルの *.dotnet* フォルダーに配置される場合は、次を使用します。

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

ファイルがグローバルに使用するために .NET Core インストールに配置される場合は、ファイルに完全なパスを指定します。

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

サンプル アプリではこの値を次のように設定します。

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

さまざまなオペレーティング システムの環境変数を設定する方法の例については、[複数の環境の使用](xref:fundamentals/environments)に関するページを参照してください。

## <a name="sample-app"></a>サンプル アプリ

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) では、`IHostingStartup` を使用して診断ツールを作成します。 ツールはスタートアップ時に 2 つのミドルウェアをアプリに追加し、これにより診断情報が提供されます。

* 登録サービス
* アドレス: スキーム、ホスト、パス ベース、パス、クエリ文字列
* 接続: リモート IP、リオート ポート、ローカル IP、ローカル ポート、クライアント証明書
* 要求ヘッダー
* 環境変数

サンプルを実行するには

1. スタートアップ診断プロジェクトでは、[PowerShell](/powershell/scripting/powershell-scripting) を使用して、その *StartupDiagnostics.deps.json* ファイルを変更します。 既定では、PowerShell は Windows 7 SP1 と Windows Server 2008 R2 SP1 以降の Windows OS 上にインストールされます。 他のプラットフォームで PowerShell を取得するには、「[Windows PowerShell のインストール](/powershell/scripting/setup/installing-windows-powershell)」を参照してください。
2. スタートアップ診断プロジェクトをビルドします。 プロジェクト ファイル内のビルド ターゲット:
   * アセンブリ ファイルとシンボル ファイルをユーザー プロファイルのランタイム ストアに移動する。
   * PowerShell スクリプトをトリガーして、*StartupDiagnostics.deps.json* ファイルを変更する。
   * *StartupDiagnostics.deps.json* ファイルをユーザー プロファイルの `additionalDeps` フォルダーに移動する。
3. 環境変数を設定します。
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. サンプル アプリを実行します。
5. `/services` エンドポイントを要求して、アプリの登録サービスを参照します。 `/diag` エンドポイントを要求して、診断情報を参照します。
