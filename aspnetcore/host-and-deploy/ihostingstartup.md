---
title: "ASP.NET Core で IHostingStartup を使用して外部のアセンブリからアプリの機能を追加します。"
author: guardrex
description: "IHostingStartup 実装を使用して外部のアセンブリから ASP.NET Core アプリに機能を追加する方法を検出します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a>ASP.NET Core で IHostingStartup を使用して外部のアセンブリからアプリの機能を追加します。

作成者: [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)実装では、アプリの外部からの起動時に、アプリに機能を追加できるように`Startup`クラスです。 たとえば、外部ツール ライブラリを使用できます、`IHostingStartup`追加の構成プロバイダーやアプリへのサービスを提供する実装。 `IHostingStartup`*は以降 ASP.NET Core 2.0 で使用できます。*

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="discover-loaded-hosting-startup-assemblies"></a>読み込まれたホスティング スタートアップ アセンブリを検出します。

ライブラリや、アプリによって読み込まスタートアップ アセンブリのホストを探索するには、ログ記録を有効にし、アプリケーションのログを確認します。 アセンブリを読み込むときに発生するエラーが記録されます。 読み込まれたホスティング スタートアップ アセンブリがデバッグ レベルで記録され、すべてのエラーが記録されます。

サンプル アプリの読み取り、 [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey)に、`string`配列し、アプリのインデックス ページに結果を表示します。

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>ホストしているスタートアップ アセンブリの自動読み込みを無効にします。

ホストしているスタートアップ アセンブリの自動読み込みを無効にする 2 つの方法があります。

* 設定、[をホストしているスタートアップを防ぐ](xref:fundamentals/hosting#prevent-hosting-startup)構成設定をホストします。
* 設定、`ASPNETCORE_preventHostingStartup`環境変数。

ときに、ホストの設定または環境変数のいずれかに設定されている`true`または`1`ホスティング、スタートアップ アセンブリは自動的に読み込まれていません。 どちらも設定されている場合、ホストの設定は、動作を制御します。

ホスト設定または環境変数を使用してホストのスタートアップ アセンブリの無効化、グローバルに無効にし、アプリの複数の機能を無効にすることがあります。 現在、ライブラリが独自の構成オプションを提供しない限り、ライブラリによって追加されたホスト スタートアップ アセンブリを選択的に無効にすることはできません。 将来のリリースはホスティング スタートアップ アセンブリを選択的に無効にする機能を提供 (を参照してください[GitHub 発行 aspnet/ホスティング #1243](https://github.com/aspnet/Hosting/pull/1243))。

## <a name="implement-ihostingstartup-features"></a>IHostingStartup 機能を実装します。

### <a name="create-the-assembly"></a>アセンブリを作成します。

`IHostingStartup`機能は、エントリ ポイントがないコンソール アプリに基づいて、アセンブリとして展開します。 アセンブリ参照、 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/)パッケージ。

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)属性の実装としてクラスを識別する`IHostingStartup`を読み込み、構築するときに実行、 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost)です。 名前空間は、次の例では、 `StartupFeature`、クラスと`StartupFeatureHostingStartup`:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

クラスを実装する`IHostingStartup`です。 クラスの[構成](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)メソッドを使用、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)アプリに機能を追加します。

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

作成するときに、`IHostingStartup`プロジェクト、依存関係ファイル (*\*. deps.json*) 設定、`runtime`するアセンブリの場所、 *bin*フォルダー。

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

ファイルの一部のみが表示されます。 例では、アセンブリ名が`StartupFeature`です。

### <a name="update-the-dependencies-file"></a>更新プログラムの依存関係ファイル

実行時の位置、  *\*. deps.json*ファイル。 アクティブな機能を`runtime`要素は、機能のランタイム アセンブリの場所を指定する必要があります。 プレフィックス、`runtime`場所`lib/netcoreapp2.0/`:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

サンプル アプリでの変更、  *\*. deps.json*はファイルの実行によって、 [PowerShell](/powershell/scripting/powershell-scripting)スクリプト。 PowerShell スクリプトはプロジェクト ファイルでビルド ターゲットによって自動的にトリガーされます。

### <a name="feature-activation"></a>機能のアクティブ化

**アセンブリ ファイルを配置します。**

`IHostingStartup`実装のアセンブリ ファイルである必要があります*bin*-アプリの展開またはに配置されて、[ランタイム ストア](/dotnet/core/deploying/runtime-store):

ユーザーごとの使用時にユーザー プロファイルのランタイム ストアにアセンブリを配置します。

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

グローバルに使用できる .NET Core のインストールの実行時のストアにアセンブリを配置します。

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

アセンブリをランタイム ストアを展開する場合、シンボル ファイルはも展開することがありますが、機能を使用するために必要ではありません。

**依存関係ファイルを配置します。**

実装の *\*. deps.json*ファイルがアクセスできる場所である必要があります。

ユーザーごとの使用するためにファイルを置きます、`additonalDeps`ユーザー プロファイルのフォルダー`.dotnet`設定。 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

グローバルを使用して、配置内のファイル、 `additonalDeps` .NET Core のインストール フォルダー。

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

バージョンをメモ`2.0.0`対象とするアプリケーションで使用される共有のランタイムのバージョンを反映します。 共有のランタイムが示すように、  *\*. runtimeconfig.json*ファイル。 指定された共有のランタイムのサンプル アプリで、 *HostingStartupSample.runtimeconfig.json*ファイル。

**環境変数な設定**

機能を使用するアプリのコンテキストでは、次の環境変数を設定します。

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

ホスティングのスタートアップ アセンブリだけがスキャン、`HostingStartupAttribute`です。 実装のアセンブリ名は、この環境変数で提供されます。 サンプル アプリでは、この値を設定`StartupDiagnostics`です。

使用して、値を設定することも、[をホストしているスタートアップ アセンブリ](xref:fundamentals/hosting#hosting-startup-assemblies)構成設定をホストします。

DOTNET\_追加\_DEPS

実装の場所 *\*. deps.json*ファイル。

ユーザー プロファイルのかどうか、ファイルを配置*.dotnet*ユーザーごとの使用フォルダー。

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

ファイルがグローバルに使用できる .NET Core のインストールに配置されている場合は、ファイルへの完全パスを提供します。

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

サンプル アプリにこの値を設定します。

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

さまざまなオペレーティング システムの環境変数を設定する方法の例については、次を参照してください。[複数の環境で作業](xref:fundamentals/environments)です。

## <a name="sample-app"></a>サンプル アプリ

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) を使用して`IHostingStartup`診断ツールを作成します。 ツールでは、2 つの middlewares を診断情報を提供するための起動時に、アプリに追加します。

* 登録されているサービス
* アドレス: スキーム、ホスト、パスのベース パス、クエリ文字列
* 接続ですリモート IP、ローカル IP は、リモート ポートは、ローカル ポート、クライアント証明書。
* 要求ヘッダー
* 環境変数

このサンプルを実行します。

1. 診断スタートアップ プロジェクトを使用して[PowerShell](/powershell/scripting/powershell-scripting)を変更するその*StartupDiagnostics.deps.json*ファイル。 Windows 7 SP1 および Windows Server 2008 R2 SP1 以降の Windows OS 上の既定では、PowerShell をインストールします。 他のプラットフォームで PowerShell を入手する[Windows PowerShell をインストールする](/powershell/scripting/setup/installing-windows-powershell)です。
2. 診断スタートアップ プロジェクトをビルドします。 プロジェクト ファイルでビルド ターゲット:
   * アセンブリに移動し、シンボル ファイルをユーザー プロファイルのランタイム ストア。
   * トリガーを変更する PowerShell スクリプト、 *StartupDiagnostics.deps.json*ファイル。
   * 移動、 *StartupDiagnostics.deps.json*をユーザー プロファイルのファイル`additionalDeps`フォルダーです。
3. 環境変数を設定します。
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. サンプル アプリを実行します。
5. 要求、`/services`して、アプリのエンドポイントがサービスを登録します。 要求、`/diag`診断情報を表示するエンドポイント。
