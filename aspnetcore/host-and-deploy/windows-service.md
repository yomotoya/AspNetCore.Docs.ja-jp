---
title: Windows サービスで ASP.NET Core をホストします。
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
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="81735-103">Windows サービスで ASP.NET Core をホストします。</span><span class="sxs-lookup"><span data-stu-id="81735-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="81735-104">著者: [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="81735-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="81735-105">含まないで実行するには IIS を使用して、Windows 上の ASP.NET Core アプリケーションをホストすることをお勧め、 [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。</span><span class="sxs-lookup"><span data-stu-id="81735-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="81735-106">Windows サービスとしてホストされた場合、アプリが自動的に実行できます後の開始を再起動し、人間の介入を必要とせずにクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="81735-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="81735-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="81735-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="81735-108">サンプル アプリを実行する方法の手順は、サンプルを参照してください*README.md*ファイル。</span><span class="sxs-lookup"><span data-stu-id="81735-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81735-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="81735-109">Prerequisites</span></span>

* <span data-ttu-id="81735-110">アプリは、.NET Framework ランタイムで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81735-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="81735-111">*.Csproj*ファイルでの適切な値を指定[TargetFramework](/nuget/schema/target-frameworks)と[RuntimeIdentifier](/dotnet/articles/core/rid-catalog)です。</span><span class="sxs-lookup"><span data-stu-id="81735-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="81735-112">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="81735-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="81735-113">Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)** テンプレート。</span><span class="sxs-lookup"><span data-stu-id="81735-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="81735-114">アプリは、(内部ネットワーク) からだけでなく、インターネットから要求を受け取る場合は使用する必要があります、 [HTTP.sys](xref:fundamentals/servers/httpsys) web サーバー (以前の[WebListener](xref:fundamentals/servers/weblistener) 1.x アプリの ASP.NET Core)ではなく[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="81735-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="81735-115">IIS は推奨リバース プロキシ サーバーとして使用する Kestrel でエッジに導入されます。</span><span class="sxs-lookup"><span data-stu-id="81735-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="81735-116">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="81735-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="81735-117">作業開始</span><span class="sxs-lookup"><span data-stu-id="81735-117">Get started</span></span>

<span data-ttu-id="81735-118">このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="81735-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="81735-119">NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。</span><span class="sxs-lookup"><span data-stu-id="81735-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="81735-120">次の変更を加え`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="81735-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="81735-121">呼び出す`host.RunAsService`の代わりに`host.Run`です。</span><span class="sxs-lookup"><span data-stu-id="81735-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="81735-122">コードを呼び出す場合`UseContentRoot`、パスの代わりに、発行場所を使用する`Directory.GetCurrentDirectory()`です。</span><span class="sxs-lookup"><span data-stu-id="81735-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81735-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81735-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81735-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81735-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="81735-125">フォルダーに、アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="81735-125">Publish the app to a folder.</span></span> <span data-ttu-id="81735-126">使用して[dotnet 発行](/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles)フォルダーに発行します。</span><span class="sxs-lookup"><span data-stu-id="81735-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="81735-127">テストを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="81735-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="81735-128">使用する管理者の特権でコマンド シェルを開いて、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="81735-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="81735-129">MyService サービスの名前が場合に、発行`c:\svc`AspNetCoreService をという名前のコマンドとします。</span><span class="sxs-lookup"><span data-stu-id="81735-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="81735-130">`binPath`値は、アプリの実行可能ファイル、実行可能ファイル名を含むへのパス。</span><span class="sxs-lookup"><span data-stu-id="81735-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

   <span data-ttu-id="81735-132">これらのコマンドが完了、[参照] と同じパスに、コンソール アプリケーションとして実行する場合 (既定では、 `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="81735-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![サービスで実行されています。](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="81735-134">サービスの外部で実行するための手段します。</span><span class="sxs-lookup"><span data-stu-id="81735-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="81735-135">テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行中にデバッグしやすく`RunAsService`特定の条件下でのみです。</span><span class="sxs-lookup"><span data-stu-id="81735-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="81735-136">コンソール アプリとしてアプリを実行できますなど、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。</span><span class="sxs-lookup"><span data-stu-id="81735-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81735-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81735-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81735-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81735-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="81735-139">停止および開始イベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="81735-139">Handle stopping and starting events</span></span>

<span data-ttu-id="81735-140">処理するために`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加します。</span><span class="sxs-lookup"><span data-stu-id="81735-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="81735-141">派生するクラスを作成する`WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="81735-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="81735-142">拡張メソッドを作成する`IWebHost`カスタムをパスする`WebHostService`に`ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="81735-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="81735-143">`Program.Main`、新しい拡張メソッドを呼び出し、`RunAsCustomService`の代わりに`RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="81735-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81735-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81735-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81735-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81735-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="81735-146">場合、カスタム`WebHostService`コード (ロガー) などの依存関係の挿入からサービスを必要と、これからは取得、`Services`プロパティ`IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="81735-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="81735-147">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="81735-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="81735-148">インターネットや企業ネットワークからの要求との対話しプロキシの背後にある、またはロード バランサーをサービスには、追加の構成を必要があります。</span><span class="sxs-lookup"><span data-stu-id="81735-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="81735-149">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="81735-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="81735-150">謝辞</span><span class="sxs-lookup"><span data-stu-id="81735-150">Acknowledgments</span></span>

<span data-ttu-id="81735-151">この記事の内容が公開されているリソースを利用して書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="81735-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="81735-152">Windows サービスとしての ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="81735-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="81735-153">Windows サービスで、ASP.NET Core をホストする方法</span><span class="sxs-lookup"><span data-stu-id="81735-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
