---
title: Windows サービスでの ASP.NET Core のホスト
author: rick-anderson
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153530"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="3a6e3-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="3a6e3-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="3a6e3-104">著者: [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3a6e3-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3a6e3-105">IIS を使用せずに Windows で ASP.NET Core アプリケーションをホストするお勧めの方法は、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)でこれを実行することです。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="3a6e3-106">Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="3a6e3-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="3a6e3-108">サンプル アプリを実行する方法については、サンプルの *README.md* ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a6e3-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3a6e3-109">Prerequisites</span></span>

* <span data-ttu-id="3a6e3-110">アプリは、.NET Framework ランタイムで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="3a6e3-111">*.csproj* ファイルで、[TargetFramework](/nuget/schema/target-frameworks) と [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) の適切な値を指定します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="3a6e3-112">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="3a6e3-113">Visual Studio でプロジェクトを作成する場合は、**ASP.NET Core アプリケーション (.NET Framework)** テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="3a6e3-114">アプリが (内部ネットワークからだけではなく)、インターネットからの要求も受け取る場合は、アプリは [Kestrel](xref:fundamentals/servers/kestrel) ではなく、[HTTP.sys](xref:fundamentals/servers/httpsys) Web サーバー (旧称: ASP.NET Core 1.x アプリ向け [WebListener](xref:fundamentals/servers/weblistener)) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="3a6e3-115">エッジ展開用に Kestrel と共にリバース プロキシ サーバーとして IIS を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="3a6e3-116">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="3a6e3-117">作業開始</span><span class="sxs-lookup"><span data-stu-id="3a6e3-117">Get started</span></span>

<span data-ttu-id="3a6e3-118">このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="3a6e3-119">NuGet パッケージ [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="3a6e3-120">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="3a6e3-121">`host.Run` の代わりに `host.RunAsService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="3a6e3-122">コードが `UseContentRoot` を呼び出した場合、`Directory.GetCurrentDirectory()` の代わりに発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a6e3-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a6e3-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="3a6e3-125">アプリをフォルダーに発行します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-125">Publish the app to a folder.</span></span> <span data-ttu-id="3a6e3-126">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) またはフォルダーに発行する [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="3a6e3-127">サービスを作成、開始してテストします。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="3a6e3-128">管理特権を持つコマンド シェルを開いて、[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用して、サービスを作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="3a6e3-129">サービスに MyService という名前を付けて `c:\svc` に発行し、AspNetCoreService をという名前を付けた場合、コマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="3a6e3-130">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![コンソール ウィンドウの作成および開始例](windows-service/_static/create-start.png)

   <span data-ttu-id="3a6e3-132">これらのコマンドが完了したら、コンソール アプリとしての実行時と同じパスを参照します (既定では `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![サービスでの実行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="3a6e3-134">サービスの外部で実行するための方法の提供</span><span class="sxs-lookup"><span data-stu-id="3a6e3-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="3a6e3-135">サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="3a6e3-136">たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a6e3-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a6e3-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="3a6e3-139">停止および開始イベントの処理</span><span class="sxs-lookup"><span data-stu-id="3a6e3-139">Handle stopping and starting events</span></span>

<span data-ttu-id="3a6e3-140">`OnStarting`、`OnStarted`、および `OnStopping` イベントを処理するには、次の追加の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="3a6e3-141">`WebHostService` から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="3a6e3-142">カスタム `WebHostService` を `ServiceBase.Run` に渡す `IWebHost` の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="3a6e3-143">`Program.Main` で、`RunAsService` ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a6e3-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a6e3-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a6e3-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="3a6e3-146">カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、`IWebHost` の `Services` プロパティから取得します。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3a6e3-147">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="3a6e3-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3a6e3-148">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="3a6e3-149">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="3a6e3-150">謝辞</span><span class="sxs-lookup"><span data-stu-id="3a6e3-150">Acknowledgments</span></span>

<span data-ttu-id="3a6e3-151">この記事は、次の公開されているソースを参照して作成されました。</span><span class="sxs-lookup"><span data-stu-id="3a6e3-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="3a6e3-152">Hosting ASP.NET Core as Windows service (Windows サービスとしての ASP.NET Core のホスト)</span><span class="sxs-lookup"><span data-stu-id="3a6e3-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="3a6e3-153">How to host your ASP.NET Core in a Windows Service (Windows サービスで ASP.NET Core をホストする方法)</span><span class="sxs-lookup"><span data-stu-id="3a6e3-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
