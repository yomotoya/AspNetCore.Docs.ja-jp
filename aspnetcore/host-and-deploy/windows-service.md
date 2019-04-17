---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068337"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスでの ASP.NET Core のホスト

著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core アプリは、IIS を 使用せずに、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)として Windows にホストできます。 Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

* [PowerShell 6.2 以降](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> Windows 10 October 2018 Update (バージョン 1809/ビルド 10.0.17763) より前の Windows OS の場合、「[ユーザー アカウントを作成する](#create-a-user-account)」セクションで使用する [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットにアクセスするには、[WindowsCompatibility モジュール](https://github.com/PowerShell/WindowsCompatibility)と共に [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) モジュールをインポートする必要があります。
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a>配置の種類

フレームワーク依存型または自己完結型いずれかの Windows サービス展開を作成できます。 展開のシナリオに関する情報および注意事項については、「[.NET Core アプリケーションの展開](/dotnet/core/deploying/)」をご覧ください。

### <a name="framework-dependent-deployment"></a>フレームワークに依存する展開

フレームワーク依存型展開 (FDD) は、ターゲット システムに .NET Core のシステム全体の共有バージョンが存在することに依存します。 FDD シナリオを ASP.NET Core Windows サービス アプリに対して使用すると、"*フレームワーク依存型実行可能ファイル*" と呼ばれる実行可能ファイル (*\*.exe*) が SDK によって生成されます。

### <a name="self-contained-deployment"></a>自己完結型の展開

自己完結型の展開 (SCD) は、ターゲット システムに共有コンポーネントが存在することに依存しません。 ランタイムとアプリの依存関係が、アプリと共にホスティング システムに展開されます。

## <a name="convert-a-project-into-a-windows-service"></a>プロジェクトを Windows サービスに変換する

既存の ASP.NET Core プロジェクトに次の変更を加え、アプリをサービスとして実行します。

### <a name="project-file-updates"></a>プロジェクト ファイルの更新

どの[展開の種類](#deployment-type)を選択したかに基づいて、プロジェクト ファイルを更新します。

#### <a name="framework-dependent-deployment-fdd"></a>フレームワーク依存型展開 (FDD)

Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) を、ターゲット フレームワークを含む `<PropertyGroup>` に追加します。 次の例では、RID が `win7-x64` に設定されています。 `false` に設定した `<SelfContained>` プロパティを追加します。 これらのプロパティによって、Windows 用の実行可能ファイル (*.exe*) を生成するよう SDK に指示します。

*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。 *web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`true` に設定した `<UseAppHost>` プロパティを追加します。 このプロパティによって、サービスに FDD のアクティブ化パス (実行可能ファイル、*.exe*) を指定します。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a>自己完結型の展開 (SCD)

Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、RID をターゲット フレームワークを含む `<PropertyGroup>` に追加します。 `true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加して、*web.config* ファイルの作成を無効にします。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

複数の RID を発行するには、次の処理を実行します。

* セミコロンで区切られたリストの形式で RID を指定します。
* プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。

  詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。

[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。

Windows イベント ログのログ記録を有効にするには、[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) のパッケージ参照を追加します。

詳しくは、「[イベントの開始と停止を扱う](#handle-starting-and-stopping-events)」セクションをご覧ください。

### <a name="programmain-updates"></a>Program.Main の更新

`Program.Main` で次の変更を行います。

* サービスの外部で実行しているときにテストとデバッグを行うには、アプリがサービスとして実行しているかコンソール アプリとして実行しているかを判別するコードを追加します。 デバッガーがアタッチされているか、`--console` コマンドライン引数が存在するかを検査します。

  いずれかの条件が満たされる場合 (アプリがサービスとして実行していない場合)、Web ホストで <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> を呼び出します。

  条件が満たされない場合 (アプリがサービスとして実行している場合):

  * <xref:System.IO.Directory.SetCurrentDirectory*> を呼び出し、アプリの発行場所のパスを使用します。 パスを取得するために <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出さないでください。<xref:System.IO.Directory.GetCurrentDirectory*> が呼び出されると、Windows サービス アプリは *C:\\WINDOWS\\system32* フォルダーを戻すためです。 詳しくは、「[現在のディレクトリとコンテンツのルート](#current-directory-and-content-root)」セクションをご覧ください。
  * <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> を呼び出して、アプリをサービスとして実行します。

  [コマンドライン構成プロバイダー](xref:fundamentals/configuration/index#command-line-configuration-provider)では、コマンドライン引数に名前と値の組が必要であるため、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> が引数を受け取る前に `--console` スイッチは引数から削除されます。

* Windows イベント ログに書き込むには、EventLog プロバイダーを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> に追加します。 *appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを設定します。 デモとテストの目的のために、サンプル アプリの Production 設定ファイルでは、ログ レベルが `Information` に設定されます。 運用環境で、値は通常は `Error` に設定します。 詳細については、「<xref:fundamentals/logging/index#windows-eventlog-provider>」を参照してください。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a>アプリの発行

[dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。 Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。

コマンドライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、Windows コマンド シェルでプロジェクト フォルダーから [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行し、[-c|--configuration](/dotnet/core/tools/dotnet-publish#options) オプションにリリース構成を渡します。 アプリ外部のフォルダーに発行するには、[-o|--output](/dotnet/core/tools/dotnet-publish#options) オプションをパスと一緒に使用します。

### <a name="publish-a-framework-dependent-deployment-fdd"></a>フレームワーク依存型展開 (FDD) を発行する

次の例では、アプリは *c:\\svc* フォルダーに発行されます。

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a>自己完結型展開 (SCD) を発行する

プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。 ランタイムを `dotnet publish` コマンドの [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) オプションに指定します。

次の例では、`win7-x64` ランタイムに関してアプリが *c:\\svc* フォルダーに発行されます。

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a>ユーザー アカウントを作成する

PowerShell 6 の管理コマンド シェルから [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットを使用して、サービス用のユーザー アカウントを作成します。

```powershell
New-LocalUser -Name {NAME}
```

入力を求められたら、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)を指定します。

サンプル アプリでは、`ServiceUser` という名前のユーザー アカウントを作成します。

```powershell
New-LocalUser -Name ServiceUser
```

[New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットに <xref:System.DateTime> という有効期限で `-AccountExpires` パラメーターを指定しない場合、アカウントは期限切れになりません。

詳しくは、「[Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/)」および「[Service User Accounts (サービス ユーザー アカウント)](/windows/desktop/services/service-user-accounts)」をご覧ください。

Active Directory を使う場合、ユーザーを管理するための別の方法は、マネージド サービス アカウントを使うことです。 詳細については、「[Group Managed Service Accounts Overview (グループ マネージド サービス アカウントの概要)](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)」をご覧ください。

## <a name="set-permission-log-on-as-a-service"></a>アクセス許可の設定: サービスとしてログオンする

PowerShell 6 の管理コマンド シェルから [icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` &ndash; アプリのフォルダーへのパス。
* `{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。
* `(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。
* `(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。
* `{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。
  * 書き込み (`W`)
  * 読み取り (`R`)
  * 実行 (`X`)
  * フル アクセス (`F`)
  * 変更 (`M`)
* `/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。

*c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、PowerShell 6 の管理コマンド シェルで次のコマンドを使用します。

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。

## <a name="create-the-service"></a>サービスを作成する

サービスを登録するには、[RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell スクリプトを使います。 PowerShell 6 の管理コマンド シェルから、次のコマンドでスクリプトを実行します。

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

サンプル アプリの例を次に示します。

* サービスは **MyService** という名前です。
* 発行されたサービスは、*c:\\svc* フォルダーに配置されます。 アプリの実行可能ファイルの名前は *SampleApp.exe* です。
* サービスは `ServiceUser` アカウントで実行されます。 次のコマンド例では、ローカル コンピューターの名前は `Desktop-PC` です。 `Desktop-PC` は、自分のシステムのコンピューター名またはシステムに置き換えます。

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a>サービスを管理する

### <a name="start-the-service"></a>サービスを開始する

PowerShell 6 の `Start-Service -Name {NAME}` コマンドでサービスを開始します。

サンプル アプリ サービスを開始するには、次のコマンドを使用します。

```powershell
Start-Service -Name MyService
```

このコマンドでサービスを開始するには数秒かかります。

### <a name="determine-the-service-status"></a>サービスの状態を確認する

サービスの状態を確認するには、PowerShell 6 の `Get-Service -Name {NAME}` コマンドを使います。 この状態は、次のいずれかの値として報告されます。

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a>Web アプリ サービスを参照する

サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000` )。

サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。

### <a name="stop-the-service"></a>サービスを停止して

PowerShell 6 の `Stop-Service -Name {NAME}` コマンドでサービスを停止します。

サンプル アプリ サービスは、次のコマンドで停止できます。

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a>サービスを削除する

サービスを停止した後少ししてから、Powershell 6 の `Remove-Service -Name {NAME}` コマンドを使ってサービスを削除します。

サンプル アプリ サービスの状態を確認します。

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a>イベントの開始と停止を扱う

<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>、および <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> イベントを処理するには、次の追加の変更を行います。

1. `OnStarting`、`OnStarted`、および `OnStopping` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> から派生するクラスを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. `CustomWebHostService` を <xref:System.ServiceProcess.ServiceBase.Run*> に渡す <xref:Microsoft.AspNetCore.Hosting.IWebHost> の拡張メソッドを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main` で、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ではなく、拡張メソッド `RunAsCustomService` を呼び出します。

   ```csharp
   host.RunAsCustomService();
   ```

   `Program.Main` 内の <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の場所を確認するには、「[プロジェクトを Windows サービスに変換する](#convert-a-project-into-a-windows-service)」セクションに示されているコード サンプルを参照してください。

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。 詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。

## <a name="configure-https"></a>HTTPS の構成

セキュリティで保護されたエンドポイントを使用してサービスを構成するには:

1. プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。

1. [Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。

サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。

## <a name="current-directory-and-content-root"></a>現在のディレクトリとコンテンツのルート

Windows サービスに対して <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。 *system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。 次のいずれかの方法を使用して、サービスのアセットと設定ファイルを管理し、アクセスします。

### <a name="set-the-content-root-path-to-the-apps-folder"></a>アプリのフォルダーにコンテンツ ルート パスを設定する

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。 `GetCurrentDirectory` を呼び出して設定ファイルへのパスを作成する代わりに、アプリのコンテンツ ルートへのパスを指定して <xref:System.IO.Directory.SetCurrentDirectory*> を呼び出します。

`Program.Main` で、サービスの実行可能ファイルがあるフォルダーへのパスを判別し、そのパスを使用してアプリのコンテンツ ルートを確立します。

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>ディスク上の適切な場所にサービスのファイルを格納します。

ファイルを含むフォルダーに対して <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用するときは、<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> を使用して絶対パスを指定します。

## <a name="additional-resources"></a>その他の技術情報

* [Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
