---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4fd0cc881eff3b1bbdfdf51e223d0fd42051c31d
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320740"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d23a1-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="d23a1-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d23a1-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d23a1-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d23a1-105">ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="d23a1-106">Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="d23a1-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d23a1-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="d23a1-108">作業開始</span><span class="sxs-lookup"><span data-stu-id="d23a1-108">Get started</span></span>

<span data-ttu-id="d23a1-109">サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d23a1-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="d23a1-110">プロジェクト ファイルで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-110">In the project file:</span></span>

   1. <span data-ttu-id="d23a1-111">ランタイム識別子があることを確認するか、それをターゲット フレームワークを含む **\<PropertyGroup>** に追加します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="d23a1-112">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="d23a1-113">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="d23a1-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="d23a1-114">`host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="d23a1-115">[UseContentRoot](xref:fundamentals/host/web-host#content-root) を呼び出し、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="d23a1-116">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="d23a1-116">Publish the app.</span></span> <span data-ttu-id="d23a1-117">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) または [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="d23a1-118">コマンド ラインからサンプル アプリを発行する場合、コンソール ウィンドウでプロジェクト フォルダーから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="d23a1-119">[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="d23a1-120">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="d23a1-121">**等号 (=) とパスの開始の引用符文字の間にはスペースが必要です。**</span><span class="sxs-lookup"><span data-stu-id="d23a1-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="d23a1-122">プロジェクト フォルダーに発行されるサービスの場合は、*publish* フォルダーへのパスを使用してサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="d23a1-123">次に例では、サービスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d23a1-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="d23a1-124">**MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="d23a1-124">Named **MyService**.</span></span>
   * <span data-ttu-id="d23a1-125">*c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="d23a1-126">*AspNetCoreService.exe* という名前のアプリの実行可能ファイルで表されます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="d23a1-127">管理者権限でコマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\<TARGET_FRAMEWORK>\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="d23a1-128">`binPath=` 引数とその値の間には、空白を必ず含めてください。</span><span class="sxs-lookup"><span data-stu-id="d23a1-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="d23a1-129">別のフォルダーからサービスを発行および開始するには</span><span class="sxs-lookup"><span data-stu-id="d23a1-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="d23a1-130">`dotnet publish` コマンドで [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="d23a1-131">出力フォルダーのパスを使用し、`sc.exe` コマンドでサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="d23a1-132">`binPath` に指定したパスに、サービスの実行可能ファイルの名前を含めます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="d23a1-133">サービスを `sc start <SERVICE_NAME>` コマンドで開始します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d23a1-134">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="d23a1-135">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="d23a1-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="d23a1-136">`sc query <SERVICE_NAME>` コマンドは、そのサービスの状態を確認し、その状態を判断するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="d23a1-137">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="d23a1-138">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="d23a1-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="d23a1-139">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="d23a1-140">`sc stop <SERVICE_NAME>` コマンドを使用して、サービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d23a1-141">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="d23a1-142">サービスの停止の少し後に、`sc delete <SERVICE_NAME>` コマンドを使用して、サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="d23a1-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="d23a1-143">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="d23a1-144">サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="d23a1-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="d23a1-145">サービスの外部で実行するための方法の提供</span><span class="sxs-lookup"><span data-stu-id="d23a1-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="d23a1-146">サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。</span><span class="sxs-lookup"><span data-stu-id="d23a1-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="d23a1-147">たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="d23a1-148">ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。</span><span class="sxs-lookup"><span data-stu-id="d23a1-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="d23a1-149">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="d23a1-149">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="d23a1-150">停止および開始イベントの処理</span><span class="sxs-lookup"><span data-stu-id="d23a1-150">Handle stopping and starting events</span></span>

<span data-ttu-id="d23a1-151">[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。</span><span class="sxs-lookup"><span data-stu-id="d23a1-151">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="d23a1-152">[WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-152">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="d23a1-153">カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-153">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d23a1-154">`Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-154">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="d23a1-155">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="d23a1-155">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="d23a1-156">カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。</span><span class="sxs-lookup"><span data-stu-id="d23a1-156">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d23a1-157">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="d23a1-157">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d23a1-158">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="d23a1-158">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d23a1-159">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d23a1-159">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d23a1-160">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d23a1-160">Additional resources</span></span>

* <span data-ttu-id="d23a1-161">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="d23a1-161">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
