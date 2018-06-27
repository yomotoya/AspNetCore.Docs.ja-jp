---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 0149039f69539b7c69d7ba45efcf09d80ffcba79
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275099"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="f1641-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="f1641-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="f1641-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f1641-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f1641-105">ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="f1641-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="f1641-106">Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。</span><span class="sxs-lookup"><span data-stu-id="f1641-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="f1641-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f1641-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="f1641-108">作業開始</span><span class="sxs-lookup"><span data-stu-id="f1641-108">Get started</span></span>

<span data-ttu-id="f1641-109">サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f1641-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="f1641-110">プロジェクト ファイルで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="f1641-110">In the project file:</span></span>

   1. <span data-ttu-id="f1641-111">ランタイム識別子があることを確認するか、それをターゲット フレームワークを含む **\<PropertyGroup>** に追加します。</span><span class="sxs-lookup"><span data-stu-id="f1641-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="f1641-112">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="f1641-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="f1641-113">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="f1641-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="f1641-114">`host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f1641-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="f1641-115">コードが `UseContentRoot` を呼び出した場合、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f1641-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="f1641-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="f1641-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="f1641-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="f1641-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="f1641-118">アプリをフォルダーに発行します。</span><span class="sxs-lookup"><span data-stu-id="f1641-118">Publish the app to a folder.</span></span> <span data-ttu-id="f1641-119">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) またはフォルダーに発行する [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。</span><span class="sxs-lookup"><span data-stu-id="f1641-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="f1641-120">コマンド ラインからサンプル アプリを発行する場合、コンソール ウィンドウでプロジェクト フォルダーから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f1641-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="f1641-121">[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービス (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`) を作成します。</span><span class="sxs-lookup"><span data-stu-id="f1641-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="f1641-122">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f1641-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="f1641-123">**等号 (=) とパスの開始の引用符文字の間にはスペースが必要です。**</span><span class="sxs-lookup"><span data-stu-id="f1641-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="f1641-124">サンプル アプリとそれ以降のコマンドに関し、サービスは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f1641-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="f1641-125">**MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="f1641-125">Named **MyService**.</span></span>
   * <span data-ttu-id="f1641-126">*c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="f1641-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="f1641-127">*AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="f1641-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="f1641-128">管理者権限でコマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f1641-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="f1641-129">**`binPath=` 引数とその値の間には、空白を必ず含めてください。**</span><span class="sxs-lookup"><span data-stu-id="f1641-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="f1641-130">サービスを `sc start <SERVICE_NAME>` コマンドで開始します。</span><span class="sxs-lookup"><span data-stu-id="f1641-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="f1641-131">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="f1641-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="f1641-132">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="f1641-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="f1641-133">`sc query <SERVICE_NAME>` コマンドは、そのサービスの状態を確認し、その状態を判断するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="f1641-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="f1641-134">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="f1641-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="f1641-135">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="f1641-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="f1641-136">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="f1641-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="f1641-137">`sc stop <SERVICE_NAME>` コマンドを使用して、サービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="f1641-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="f1641-138">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="f1641-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="f1641-139">サービスの停止の少し後に、`sc delete <SERVICE_NAME>` コマンドを使用して、サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="f1641-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="f1641-140">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="f1641-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="f1641-141">サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="f1641-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="f1641-142">サービスの外部で実行するための方法の提供</span><span class="sxs-lookup"><span data-stu-id="f1641-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="f1641-143">サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。</span><span class="sxs-lookup"><span data-stu-id="f1641-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="f1641-144">たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="f1641-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f1641-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="f1641-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="f1641-146">ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。</span><span class="sxs-lookup"><span data-stu-id="f1641-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f1641-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="f1641-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="f1641-148">停止および開始イベントの処理</span><span class="sxs-lookup"><span data-stu-id="f1641-148">Handle stopping and starting events</span></span>

<span data-ttu-id="f1641-149">[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。</span><span class="sxs-lookup"><span data-stu-id="f1641-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="f1641-150">[WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1641-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="f1641-151">カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="f1641-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="f1641-152">`Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f1641-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="f1641-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="f1641-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="f1641-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="f1641-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="f1641-155">カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。</span><span class="sxs-lookup"><span data-stu-id="f1641-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="f1641-156">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="f1641-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="f1641-157">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f1641-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="f1641-158">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1641-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="f1641-159">Kestrel のエンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="f1641-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="f1641-160">HTTPS の構成および SNI のサポートを含む Kestrel のエンドポイントの構成の詳細については、[Kestrel のエンドポイントの構成](xref:fundamentals/servers/kestrel#endpoint-configuration)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f1641-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
