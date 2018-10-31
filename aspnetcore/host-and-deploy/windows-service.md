---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206674"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスでの ASP.NET Core のホスト

著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。 Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="convert-a-project-into-a-windows-service"></a>プロジェクトを Windows サービスに変換する

サービスでとして実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。

1. プロジェクト ファイルで次を実行します。

   * Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、それをターゲット フレームワークを含む `<PropertyGroup>` に追加します。

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      複数の RID を発行するには、次の処理を実行します。

      * セミコロンで区切られたリストの形式で RID を指定します。
      * プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。

      詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。

   * [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。

1. `Program.Main` で次の変更を行います。

   * `host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。

   * [UseContentRoot](xref:fundamentals/host/web-host#content-root) を呼び出し、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. アプリの発行 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) または [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。 Visual Studio を使用する場合は、**FolderProfile** を選択します。

   コマンド ライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行します。 プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。 次の例では、アプリが `win7-x64` ランタイムのリリース構成で発行されます。

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. [sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。 `binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。 **等号 (=) とパスの開始の引用符文字の間にはスペースが必要です。**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   プロジェクト フォルダーに発行されるサービスの場合は、*publish* フォルダーへのパスを使用してサービスを作成します。 次に例を示します。

   * プロジェクトは *c:\\my_services\\AspNetCoreService* フォルダーに存在します。
   * プロジェクトは `Release` 構成で発行されます。
   * ターゲット フレームワーク モニカー (TFM) は `netcoreapp2.1` です。
   * ランタイム識別子 (RID) は `win7-x64` です。
   * *AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。
   * サービスは **MyService** という名前です。

   例:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > `binPath=` 引数とその値の間には、空白を必ず含めてください。

   別のフォルダーからサービスを発行および開始するには

      * `dotnet publish` コマンドで [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) オプションを使用します。 Visual Studio を使用するには、**[発行]** ボタンを選択する前に、**[FolderProfile]** 発行プロパティ ページの **[ターゲットの場所]** を構成します。
      * 出力フォルダーのパスを使用し、`sc.exe` コマンドでサービスを作成します。 `binPath` に指定したパスに、サービスの実行可能ファイルの名前を含めます。

1. サービスを `sc start <SERVICE_NAME>` コマンドで開始します。

   サンプル アプリ サービスを開始するには、次のコマンドを使用します。

   ```console
   sc start MyService
   ```

   このコマンドでサービスを開始するには数秒かかります。

1. サービスの状態を確認するには、`sc query <SERVICE_NAME>` コマンドを使用します。 この状態は、次のいずれかの値として報告されます。

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

## <a name="run-the-app-outside-of-a-service"></a>サービスの外部でアプリを実行する

サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。 たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。

> [!NOTE]
> [統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントの処理

[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。

1. [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > [統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。 詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。

## <a name="configure-https"></a>HTTPS の構成

[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定します。

## <a name="current-directory-and-content-root"></a>現在のディレクトリとコンテンツのルート

Windows サービスに対して `Directory.GetCurrentDirectory()` を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。 *system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使用する場合、次のいずれかの方法を使用して、サービスの資産と設定ファイルを [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) に格納し、アクセスします。

* コンテンツのルート パスを使用します。 `IHostingEnvironment.ContentRootPath` は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。 `Directory.GetCurrentDirectory()` を使用して設定ファイルのパスを作成する代わりに、コンテンツのルートパスを使用して、アプリのコンテンツ ルートにファイルを格納します。
* ディスク上の適切な場所にファイルを格納します。 ファイルを含むフォルダーを示す絶対パスを `SetBasePath` で指定します。

## <a name="additional-resources"></a>その他の技術情報

* [Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)
* <xref:fundamentals/host/web-host>
