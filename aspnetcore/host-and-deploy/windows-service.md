---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5eba685bbe55d43bb063a01798bc691a1ba0d6fc
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613087"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスでの ASP.NET Core のホスト

著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。 Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="get-started"></a>作業開始

サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。

1. プロジェクト ファイルで次を実行します。

   1. ランタイム識別子があることを確認するか、それをターゲット フレームワークを含む **\<PropertyGroup>** に追加します。
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) のパッケージ参照を追加します。

1. `Program.Main` で次の変更を行います。

   * `host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。

   * コードが `UseContentRoot` を呼び出した場合、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. アプリをフォルダーに発行します。 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) またはフォルダーに発行する [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。

   コマンド ラインからサンプル アプリを発行する場合、コンソール ウィンドウでプロジェクト フォルダーから次のコマンドを実行します。

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. [sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービス (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`) を作成します。 `binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。 **等号 (=) とパスの開始の引用符文字の間にはスペースが必要です。**

   サンプル アプリとそれ以降のコマンドに関し、サービスは次のとおりです。

   * **MyService** という名前です。
   * *c:\\svc* フォルダーに発行されます。
   * *AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。

   管理者権限でコマンド シェルを開き、次のコマンドを実行します。

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   **`binPath=` 引数とその値の間には、空白を必ず含めてください。**

1. サービスを `sc start <SERVICE_NAME>` コマンドで開始します。

   サンプル アプリ サービスを開始するには、次のコマンドを使用します。

   ```console
   sc start MyService
   ```

   このコマンドでサービスを開始するには数秒かかります。

1. `sc query <SERVICE_NAME>` コマンドは、そのサービスの状態を確認し、その状態を判断するために使用できます。

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。

   ```console
   sc query MyService
   ```

1. サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。

   サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。

1. `sc stop <SERVICE_NAME>` コマンドを使用して、サービスを停止します。

   サンプル アプリ サービスは、次のコマンドで停止できます。

   ```console
   sc stop MyService
   ```

1. サービスの停止の少し後に、`sc delete <SERVICE_NAME>` コマンドを使用して、サービスをアンインストールします。

   サンプル アプリ サービスの状態を確認します。

   ```console
   sc query MyService
   ```

   サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>サービスの外部で実行するための方法の提供

サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。 たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントの処理

[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。

1. [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。 詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。

## <a name="kestrel-endpoint-configuration"></a>Kestrel のエンドポイントの構成

HTTPS の構成および SNI のサポートを含む Kestrel のエンドポイントの構成の詳細については、[Kestrel のエンドポイントの構成](xref:fundamentals/servers/kestrel#endpoint-configuration)に関するページをご覧ください。
