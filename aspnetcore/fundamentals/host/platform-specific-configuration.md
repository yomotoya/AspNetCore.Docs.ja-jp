---
title: IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する
author: guardrex
description: IHostingStartup 実装を使用して、外部アセンブリから ASP.NET Core アプリを拡張する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207564"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する

作成者: [Luke Latham](https://github.com/guardrex)

外部アセンブリからの起動時には、[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (ホスティング スタートアップ) 実装によって拡張機能がアプリに追加されます。 たとえば、外部ライブラリではホスティング スタートアップ実装を使用して、追加の構成プロバイダーまたはサービスをアプリに提供できます。 `IHostingStartup` *は、ASP.NET Core 2.0 以降で使用できます。*

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="hostingstartup-attribute"></a>HostingStartup 属性

[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性は、実行時にアクティブ化されるホスティング スタートアップ アセンブリが存在することを示しています。

入力アセンブリまたは `Startup` クラスを含むアセンブリは、`HostingStartup` 属性について自動的にスキャンされます。 `HostingStartup` 属性を検索するアセンブリの一覧は、[WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 内の構成からランタイム時に読み込まれます。 検出から除外されるアセンブリの一覧は、[WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) から読み込まれます。 詳細については、Web ホストに関するページの「[ホスティング スタートアップ アセンブリ](xref:fundamentals/host/web-host#hosting-startup-assemblies)」および[除外するホスティング スタートアップ アセンブリ](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)に関するセクションを参照してください。

次に例では、ホスティング スタートアップ アセンブリの名前空間は `StartupEnhancement` となります。 ホスティング スタートアップ コードを含むクラスは `StartupEnhancementHostingStartup` です。

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` 属性は、通常、ホスティング スタートアップ アセンブリの `IHostingStartup` 実装クラス ファイル内に置かれます。

## <a name="discover-loaded-hosting-startup-assemblies"></a>読み込まれたホスティング スタートアップ アセンブリを見つける

読み込まれたホスティング スタートアップ アセンブリを検出するには、ログ記録を有効にしてアプリのログを確認します。 アセンブリの読み込み時に発生したエラーがログ記録されます。 読み込まれたホスティング スタートアップ アセンブリは、デバッグ レベルでログ記録され、すべてのエラーが記録されます。

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>ホスティング スタートアップ アセンブリの自動読み込みを無効にする

::: moniker range=">= aspnetcore-2.1"

ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、次の方法のいずれかを使用します。

* すべてのホスティング スタートアップ アセンブリの読み込みを回避するには、次のいずれかを `true` または `1` に設定します。
  * [[ホスティング スタートアップを回避する]](xref:fundamentals/host/web-host#prevent-hosting-startup) ホスト構成設定。
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数。
* 特定のホスティング スタートアップ アセンブリの読み込みを回避するには、次に示すいずれかを、起動時に除外するホスティング スタートアップ アセンブリを示すセミコロンで区切られた文字列に設定します。
  * [[Hosting Startup Exclude Assemblies]\(除外するホスティング スタートアップ アセンブリ\)](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ホスト構成設定。
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境変数。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、次に示すいずれかを `true` または `1` に設定します。

* [[ホスティング スタートアップを回避する]](xref:fundamentals/host/web-host#prevent-hosting-startup) ホスト構成設定。
* `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数。

::: moniker-end

ホスト構成設定と環境変数が両方とも設定されている場合は、ホスト設定によって動作が制御されます。

ホスト設定または環境変数を使用してホスティング スタートアップ アセンブリを無効にすると、アセンブリがグローバルに無効になり、場合によってはアプリのいくつかの属性も無効になります。

## <a name="project"></a>プロジェクト

次のいずれかの種類のプロジェクトを使用して、ホスティング スタートアップを作成します。

* [クラス ライブラリ](#class-library)
* [エントリ ポイントのないコンソール アプリ](#console-app-without-an-entry-point)

### <a name="class-library"></a>クラス ライブラリ

クラス ライブラリでは、ホスティング スタートアップの拡張機能を提供できます。 このライブラリには `HostingStartup` 属性が含まれています。

[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)には、Razor Pages アプリ、*HostingStartupApp*、クラス ライブラリ、および *HostingStartupLibrary* が含まれています。 クラス ライブラリには次のものが含まれています。

* `IHostingStartup` を実装するホスティング スタートアップ クラス `ServiceKeyInjection`。 `ServiceKeyInjection` では、メモリ内の構成プロバイダー ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)) を使用して、サービスの文字列のペアがアプリの構成に追加されます。
* ホスティング スタートアップの名前空間とクラスを識別する `HostingStartup` 属性。

`ServiceKeyInjection` クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能が追加されます。 ホスティング スタートアップ アセンブリ内の `IHostingStartup.Configure` は、ユーザー コード内の `Startup.Configure` よりも前にランタイムによって呼び出されます。このため、ホスティング スタートアップ アセンブリによって提供されるすべての構成をユーザー コードで上書きすることができます。

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

アプリのインデックス ページでは、クラス ライブラリのホスティング スタートアップ アセンブリによって設定された 2 つのキーの構成値が読み取られて表示されます。

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) には、別のホスティング スタートアップ *HostingStartupPackage* を提供する NuGet パッケージ プロジェクトも含まれています。 パッケージには、前に説明したクラス ライブラリと同じ特性があります。 パッケージには次のものが含まれます。

* `IHostingStartup` を実装するホスティング スタートアップ クラス `ServiceKeyInjection`。 `ServiceKeyInjection` では、アプリの構成にサービス文字列のペアが追加されます。
* `HostingStartup`属性。

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

アプリのインデックス ページでは、パッケージのホスティング スタートアップ アセンブリによって設定された 2 つのキーの構成値が読み取られて表示されます。

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>エントリ ポイントのないコンソール アプリ

*このアプローチは、.NET Core アプリでのみ利用でき、.NET Framework では利用できません。*

エントリ ポイントのないコンソール アプリ内では、アクティブ化に関するコンパイル時参照を必要としない動的ホスティング スタートアップ拡張機能を指定できます。 このアプリには `HostingStartup` 属性が含まれています。 動的ホスティング スタートアップを作成するには:

1. 実装ライブラリは、`IHostingStartup` 実装を含むクラスから作成されます。 実装ライブラリは、通常のパッケージとして扱われます。
1. エントリ ポイントのないコンソール アプリでは、実装ライブラリ パッケージが参照されます。 コンソール アプリを使用する理由:
   * 依存関係ファイルは実行可能なアプリ資産です。そのため、依存関係ファイルをライブラリで提供することはできません。
   * [ランタイム パッケージ ストア](/dotnet/core/deploying/runtime-store) にライブラリを直接追加することはできません。それには、共有ランタイムを対象とする実行可能なプロジェクトが必要です。
1. ホスティング スタートアップの依存関係を取得するには、コンソール アプリを発行します。 コンソール アプリを発行すると、その結果として、依存関係ファイルから未使用の依存関係が切り捨てられます。
1. アプリとその依存関係ファイルは、ランタイム パッケージ ストアに格納されます。 ホスティング スタートアップ アセンブリとその依存関係ファイルは、環境変数のペアで参照され、検出できるようになっています。

コンソール アセンブリでは、[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) パッケージが参照されます。

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性では、[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) のビルド時に、読み込みと実行のために `IHostingStartup` の実装としてクラスを識別します。 次の例では、名前空間は `StartupEnhancement`、クラスは `StartupEnhancementHostingStartup` です。

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

クラスは `IHostingStartup` を実装しています。 クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能を追加します。 ホスティング スタートアップ アセンブリ内の `IHostingStartup.Configure` は、ユーザー コード内の `Startup.Configure` よりも前にランタイムによって呼び出されます。このため、ホスティング スタートアップ アセンブリによって提供されるすべての構成をユーザー コードで上書きすることができます。

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

`IHostingStartup` プロジェクトのビルド時に、依存関係ファイル (*\*.deps.json*) によってアセンブリの `runtime` の場所が *bin* フォルダーに設定されます。

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

ファイルの一部のみが示されています。 例のアセンブリ名は `StartupEnhancement` です。

## <a name="specify-the-hosting-startup-assembly"></a>ホスティング スタートアップ アセンブリを指定する

クラス ライブラリまたはコンソール アプリのいずれかで提供されるホスティング スタートアップの場合は、`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数内にホスティング スタートアップ アセンブリの名前を指定します。 環境変数はアセンブリのセミコロン区切りのリストです。

ホスティング スタートアップ アセンブリのみが、`HostingStartup` 属性に対してスキャンされます。 サンプル アプリ *HostingStartupApp* では、前述したホスティング スタートアップを検出するために、環境変数が次の値に設定されています。

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

ホスティング スタートアップ アセンブリはまた、[[ホスティング スタートアップ アセンブリ]](xref:fundamentals/host/web-host#hosting-startup-assemblies) ホスト構成設定を使用して設定することもできます。

複数のホスティング スタートアップ アセンブリがある場合、それらの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドは、アセンブリが一覧表示されている順序で実行されます。

## <a name="activation"></a>アクティベーション

ホスティング スタートアップのアクティブ化のオプションは次のとおりです。

* [ランタイム ストア](#runtime-store) &ndash; アクティブ化では、アクティブ化に関するコンパイル時参照を必要としません。 サンプル アプリでは、ホスティング スタートアップ アセンブリおよび依存関係のファイルが *deployment* フォルダーに配置されています。これにより、複数のコンピューターから成る環境でのホスティング スタートアップの配置が容易になります。 *deployment* フォルダーには、ホスティングスタートアップが有効になるように配置システム上で環境変数を作成および変更する PowerShell スクリプトも含まれています。
* アクティブ化で必須のコンパイル時参照
  * [NuGet パッケージ](#nuget-package)
  * [プロジェクトの bin フォルダー](#project-bin-folder)

### <a name="runtime-store"></a>ランタイム ストア

ホスティング スタートアップ実装は、[ランタイム ストア](/dotnet/core/deploying/runtime-store)に置かれます。 アセンブリへのコンパイル時参照は、機能強化されたアプリで必須ではありません。

ホスティング スタートアップがビルドされると、ホスティング スタートアップのプロジェクト ファイルが [dotnet store](/dotnet/core/tools/dotnet-store) コマンド用のマニフェスト ファイルとして機能します。

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

共有フレームワークに属していないホスティング スタートアップ アセンブリとその他の依存関係は、このコマンドによって次の場所にある、ユーザー プロファイルのランタイム ストアに置かれます。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

グローバルに使用できるようにアセンブリと依存関係を配置したい場合は、次のパスを使用して `dotnet store` コマンドに `-o|--output` オプションを追加します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**ホスティング スタートアップの依存関係ファイルを変更して配置する**

ランタイムの場所は、*\*.deps.json* ファイルで指定されます。 拡張機能をアクティブ化するには、`runtime` 要素で拡張機能のランタイム アセンブリの場所を指定する必要があります。 `runtime` の場所には `lib/<TARGET_FRAMEWORK_MONIKER>/` のプレフィックスを付けます。

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

サンプル コード (*StartupDiagnostics* プロジェクト) では、*\*.deps.json* ファイルの変更は [PowerShell](/powershell/scripting/powershell-scripting) スクリプトによって実行されます。 PowerShell スクリプトは、プロジェクト ファイル内のビルド ターゲットによって自動的にトリガーされます。

実装の *\*.deps.json* ファイルは、アセンブリの場所に配置されている必要があります。

ユーザーごとに使用する場合は、ユーザー プロファイルの `.dotnet` 設定の *additonalDeps* フォルダーにファイルを配置します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

グローバルに使用する場合は、.NET Core インストールの *additonalDeps* フォルダーにファイルを配置します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

共有フレームワークのバージョンには、ターゲット アプリが使用する共有ランタイムのバージョンが反映されます。 共有ランタイムは、*\*.runtimeconfig.json* ファイルに示されます。 サンプル アプリ (*HostingStartupApp*) では、共有ランタイムは、*HostingStartupApp.runtimeconfig.json* ファイル内に指定されます。

**ホスティング スタートアップの依存関係ファイルを一覧表示する**

実装の *\*.deps.json* ファイルの場所は、`DOTNET_ADDITIONAL_DEPS` 環境変数内に一覧表示されます。

そのファイルがユーザー プロファイルの *.dotnet* フォルダー内に置かれている場合は、環境変数の値を次のように設定します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

ファイルがグローバルに使用するために .NET Core インストールに配置される場合は、ファイルに完全なパスを指定します。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

依存関係ファイル (*HostingStartupApp.runtimeconfig.json*) を検索するサンプル アプリ (*HostingStartupApp*) の場合、依存関係ファイルはユーザーのプロファイルに置かれます。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

さまざまなオペレーティング システムの環境変数を設定する方法の例については、[複数の環境の使用](xref:fundamentals/environments)に関するページを参照してください。

**配置**

サンプル アプリでは、複数のコンピューターから成る環境へのホスティング スタートアップの配置を容易にするために、発行された出力内に、次を含む *deployment* フォルダーが作成されます。

* ホスティング スタートアップ アセンブリ。
* ホスティング スタートアップの依存関係ファイル。
* ホスティング スタートアップのアクティブ化をサポートできるように `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` と `DOTNET_ADDITIONAL_DEPS` を作成および変更する PowerShell スクリプト。 このスクリプトは、配置システム上の PowerShell 管理用コマンド プロンプトから実行します。

### <a name="nuget-package"></a>NuGet パッケージ

NuGet パッケージでは、ホスティング スタートアップの拡張機能を提供できます。 このパッケージには `HostingStartup` 属性があります。 このパッケージで提供される種類のホスティング スタートアップは、次のアプローチのいずれかを使用してアプリで利用できるようになります。

* 機能強化されたアプリのプロジェクト ファイルでは、アプリのプロジェクト ファイル内のホスティング スタートアップに対するパッケージ参照 (コンパイル時参照) が作成されます。 コンパイル時参照を適用すると、ホスティング スタートアップ アセンブリとその依存関係のすべてが、アプリの依存関係ファイル (*\*.deps.json*) に組み込まれます。 このアプローチは、[nuget.org](https://www.nuget.org/) に発行されるホスティング スタートアップ アセンブリ パッケージに適用されます。
* ホスティング スタートアップの依存関係ファイルは、「[ランタイム ストア](#runtime-store)」のセクションで説明にしたように (コンパイル参照がない場合)、機能強化されたアプリで利用できるようになります。

NuGet パッケージとランタイム ストアの詳細については、次のトピックを参照してください。

* [クロスプラットフォーム ツールを使用して NuGet パッケージを作成する方法](/dotnet/core/deploying/creating-nuget-packages)
* [パッケージを公開する](/nuget/create-packages/publish-a-package)
* [ランタイム パッケージ ストア](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>プロジェクトの bin フォルダー

ホスティング スタートアップの拡張機能は、機能強化されたアプリ内の *bin* 配置アセンブリによって提供できます。 このアセンブリで提供される種類のホスティング スタートアップは、次のアプローチのいずれかを使用してアプリで利用できるようになります。

* 機能強化されたアプリのプロジェクト ファイルでは、ホスティング スタートアップへのアセンブリ参照 (コンパイル時参照) が作成されます。 コンパイル時参照を適用すると、ホスティング スタートアップ アセンブリとその依存関係のすべてが、アプリの依存関係ファイル (*\*.deps.json*) に組み込まれます。 このアプローチが適用されるのは、コンパイルされたホスティング スタートアップ ライブラリのアセンブリ (DLL ファイル) を、使用するプロジェクトに移動するか、または使用するプロジェクトからアクセス可能な場所に移動することが配置シナリオで必要であり、かつホスティング スタートアップのアセンブリへの参照が作成される場合です。
* ホスティング スタートアップの依存関係ファイルは、「[ランタイム ストア](#runtime-store)」のセクションで説明にしたように (コンパイル参照がない場合)、機能強化されたアプリで利用できるようになります。

## <a name="sample-code"></a>サンプル コード

[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([ダウンロードする方法](xref:index#how-to-download-a-sample)) では、ホスティング スタートアップ実装のシナリオを示します。

* 2 つのホスティング スタートアップ アセンブリ (クラス ライブラリ) ではそれぞれ、メモリ内の構成のキーと値のペアの組が設定されます。
  * NuGet パッケージ (*HostingStartupPackage*)
  * クラス ライブラリ (*HostingStartupLibrary*)
* ホスティング スタートアップは、ランタイム ストア配置アセンブリ (*StartupDiagnostics*) からアクティブ化されます。 このアセンブリによってスタートアップ時に 2 つのミドルウェアがアプリに追加され、これにより診断情報が提供されます。
  * 登録サービス
  * アドレス (スキーム、ホスト、パス ベース、パス、クエリ文字列)
  * 接続 (リモート IP、リモート ポート、ローカル IP、ローカル ポート、クライアント証明書)
  * 要求ヘッダー
  * 環境変数

サンプルを実行するには

**NuGet パッケージからのアクティブ化**

1. [dotnet pack](/dotnet/core/tools/dotnet-pack) コマンドを使用して *HostingStartupPackage* パッケージをコンパイルします。
1. *HostingStartupPackage* のパッケージのアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。
1. アプリをコンパイルして実行します。 機能拡張されたアプリにはパッケージ参照 (コンパイル時参照) が存在します。 アプリのプロジェクト ファイル内の `<PropertyGroup>` では、パッケージ プロジェクトの出力 (*../HostingStartupPackage/Bin/debug*) がパッケージ ソースとして指定されます。 これにより、[nuget.org](https://www.nuget.org/) にパッケージをアップロードすることなく、アプリによりパッケージが使用されるようになります。詳細については、HostingStartupApp のプロジェクト ファイル内の注を参照してください。

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. インデックス ページで表示されるサービス構成キー値が、パッケージの `ServiceKeyInjection.Configure` メソッドによって設定された値と一致することを確認します。

*HostingStartupPackage* プロジェクトに変更を加えてそれをコンパイルする場合は、ローカル キャッシュから、古いパッケージではなく更新されたパッケージが、*HostingStartupApp* によって確実に受信されるように、ローカルの NuGet パッケージ キャッシュをクリアします。 ローカルの NuGet キャッシュをクリアするには、次の [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) コマンドを実行します。

```console
dotnet nuget locals all --clear
```

**クラス ライブラリからのアクティブ化**

1. [dotnet build](/dotnet/core/tools/dotnet-build) コマンドを使用して *HostingStartupLibrary* クラス ライブラリをコンパイルします。
1. *HostingStartupLibrary* のクラス ライブラリのアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。
1. クラス ライブラリのアセンブリをアプリに *bin* 配置するには、クラス ライブラリのコンパイルされた出力からアプリの *bin/Debug* フォルダーに *HostingStartupLibrary.dll* ファイルをコピーします。
1. アプリをコンパイルして実行します。 アプリのプロジェクト ファイル内の `<ItemGroup>` は、クラス ライブラリのアセンブリ (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) を参照します (コンパイル時参照)。 詳細については、HostingStartupApp のプロジェクト ファイル内の注を参照してください。

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. インデックス ページで表示されるサービス構成キー値が、クラス ライブラリの `ServiceKeyInjection.Configure` メソッドによって設定された値と一致することを確認します。

**ランタイム ストア配置アセンブリからのアクティブ化**

1. *StartupDiagnostics* プロジェクトでは、[PowerShell](/powershell/scripting/powershell-scripting) を使用して、その *StartupDiagnostics.deps.json* ファイルを変更します。 既定では、PowerShell は Windows 7 SP1 と Windows Server 2008 R2 SP1 以降の Windows 上にインストールされます。 他のプラットフォームで PowerShell を取得するには、「[Windows PowerShell のインストール](/powershell/scripting/setup/installing-powershell#powershell-core)」を参照してください。
1. *StartupDiagnostics* プロジェクトをビルドします。 プロジェクトをビルドすると、プロジェクト ファイル内のビルド ターゲットによって自動的に次のことが行われます。
   * PowerShell スクリプトをトリガーして、*StartupDiagnostics.deps.json* ファイルを変更する。
   * *StartupDiagnostics.deps.json* ファイルをユーザー プロファイルの *additionalDeps* フォルダーに移動する。
1. ホスティング スタートアップのディレクトリ内のコマンド プロンプトで `dotnet store` を実行して、アセンブリとその依存関係をユーザー プロファイルのランタイム ストアに格納します。

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Windows の場合、そのコマンドでは `win7-x64` [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) が使用されます。 別のランタイムに対してホスティング スタートアップを指定する場合は、適切な RID に置き換えます。
1. 環境変数を設定します。
   * *StartupDiagnostics* のアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。
   * Windows の場合は、`DOTNET_ADDITIONAL_DEPS` 環境変数を `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\` に設定します。 macOS または Linux の場合は、`DOTNET_ADDITIONAL_DEPS` 環境変数を `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/` に設定します。ここで、`<USER>` は、ホスティング スタートアップを含むユーザー プロファイルです。
1. サンプル アプリを実行します。
1. `/services` エンドポイントを要求して、アプリの登録サービスを参照します。 `/diag` エンドポイントを要求して、診断情報を参照します。
