---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: eb88b0bb2e9ce4cfd3a7db2081ad7d62d5dcb08e
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211040"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b10d0-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="b10d0-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b10d0-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b10d0-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b10d0-105">ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="b10d0-106">Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="b10d0-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b10d0-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="b10d0-108">プロジェクトを Windows サービスに変換する</span><span class="sxs-lookup"><span data-stu-id="b10d0-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="b10d0-109">サービスでとして実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b10d0-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="b10d0-110">プロジェクト ファイルで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-110">In the project file:</span></span>

   * <span data-ttu-id="b10d0-111">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、それをターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

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

      <span data-ttu-id="b10d0-112">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="b10d0-113">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="b10d0-114">プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="b10d0-115">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b10d0-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="b10d0-116">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="b10d0-117">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="b10d0-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="b10d0-118">`host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="b10d0-119">[UseContentRoot](xref:fundamentals/host/web-host#content-root) を呼び出し、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="b10d0-120">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="b10d0-120">Publish the app.</span></span> <span data-ttu-id="b10d0-121">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) または [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="b10d0-122">Visual Studio を使用する場合は、**FolderProfile** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="b10d0-123">コマンド ライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="b10d0-124">プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b10d0-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="b10d0-125">次の例では、アプリが `win7-x64` ランタイムのリリース構成で発行されます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="b10d0-126">[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="b10d0-127">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="b10d0-128">**等号 (=) とパスの開始の引用符文字の間にはスペースが必要です。**</span><span class="sxs-lookup"><span data-stu-id="b10d0-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="b10d0-129">プロジェクト フォルダーに発行されるサービスの場合は、*publish* フォルダーへのパスを使用してサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="b10d0-130">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-130">In the following example:</span></span>

   * <span data-ttu-id="b10d0-131">プロジェクトは *c:\\my_services\\AspNetCoreService* フォルダーに存在します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="b10d0-132">プロジェクトは `Release` 構成で発行されます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="b10d0-133">ターゲット フレームワーク モニカー (TFM) は `netcoreapp2.1` です。</span><span class="sxs-lookup"><span data-stu-id="b10d0-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="b10d0-134">ランタイム識別子 (RID) は `win7-x64` です。</span><span class="sxs-lookup"><span data-stu-id="b10d0-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="b10d0-135">*AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="b10d0-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="b10d0-136">サービスは **MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="b10d0-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="b10d0-137">例:</span><span class="sxs-lookup"><span data-stu-id="b10d0-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="b10d0-138">`binPath=` 引数とその値の間には、空白を必ず含めてください。</span><span class="sxs-lookup"><span data-stu-id="b10d0-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="b10d0-139">別のフォルダーからサービスを発行および開始するには</span><span class="sxs-lookup"><span data-stu-id="b10d0-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="b10d0-140">`dotnet publish` コマンドで [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="b10d0-141">Visual Studio を使用するには、**[発行]** ボタンを選択する前に、**[FolderProfile]** 発行プロパティ ページの **[ターゲットの場所]** を構成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="b10d0-142">出力フォルダーのパスを使用し、`sc.exe` コマンドでサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="b10d0-143">`binPath` に指定したパスに、サービスの実行可能ファイルの名前を含めます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="b10d0-144">サービスを `sc start <SERVICE_NAME>` コマンドで開始します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b10d0-145">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="b10d0-146">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="b10d0-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="b10d0-147">サービスの状態を確認するには、`sc query <SERVICE_NAME>` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="b10d0-148">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="b10d0-149">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="b10d0-150">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="b10d0-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="b10d0-151">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="b10d0-152">`sc stop <SERVICE_NAME>` コマンドを使用して、サービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b10d0-153">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="b10d0-154">サービスの停止の少し後に、`sc delete <SERVICE_NAME>` コマンドを使用して、サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="b10d0-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="b10d0-155">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="b10d0-156">サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="b10d0-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="b10d0-157">サービスの外部でアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="b10d0-157">Run the app outside of a service</span></span>

<span data-ttu-id="b10d0-158">サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。</span><span class="sxs-lookup"><span data-stu-id="b10d0-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="b10d0-159">たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="b10d0-160">ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。</span><span class="sxs-lookup"><span data-stu-id="b10d0-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="b10d0-161">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="b10d0-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="b10d0-162">停止および開始イベントの処理</span><span class="sxs-lookup"><span data-stu-id="b10d0-162">Handle stopping and starting events</span></span>

<span data-ttu-id="b10d0-163">[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。</span><span class="sxs-lookup"><span data-stu-id="b10d0-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="b10d0-164">[WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="b10d0-165">カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b10d0-166">`Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="b10d0-167">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="b10d0-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="b10d0-168">カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b10d0-169">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="b10d0-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b10d0-170">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="b10d0-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b10d0-171">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b10d0-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="b10d0-172">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="b10d0-172">Configure HTTPS</span></span>

<span data-ttu-id="b10d0-173">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b10d0-174">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="b10d0-174">Current directory and content root</span></span>

<span data-ttu-id="b10d0-175">Windows サービスに対して `Directory.GetCurrentDirectory()` を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b10d0-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="b10d0-176">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="b10d0-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b10d0-177">[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使用する場合、次のいずれかの方法を使用して、サービスの資産と設定ファイルを [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) に格納し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="b10d0-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="b10d0-178">コンテンツのルート パスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-178">Use the content root path.</span></span> <span data-ttu-id="b10d0-179">`IHostingEnvironment.ContentRootPath` は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="b10d0-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="b10d0-180">`Directory.GetCurrentDirectory()` を使用して設定ファイルのパスを作成する代わりに、コンテンツのルートパスを使用して、アプリのコンテンツ ルートにファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="b10d0-181">ディスク上の適切な場所にファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="b10d0-182">ファイルを含むフォルダーを示す絶対パスを `SetBasePath` で指定します。</span><span class="sxs-lookup"><span data-stu-id="b10d0-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b10d0-183">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b10d0-183">Additional resources</span></span>

* <span data-ttu-id="b10d0-184">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="b10d0-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
