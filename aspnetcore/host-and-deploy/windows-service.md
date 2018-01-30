---
title: "Windows サービスのホスト"
author: tdykstra
description: "Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="2366a-103">Windows サービスでの ASP.NET Core アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="2366a-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="2366a-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2366a-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2366a-105">含まないで実行するには IIS を使用して、Windows 上の ASP.NET Core アプリケーションをホストすることをお勧め、 [Windows サービス](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。</span><span class="sxs-lookup"><span data-stu-id="2366a-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="2366a-106">このようにして自動的に開始できますが再起動し、クラッシュの後にログインするユーザーが待機することがなく。</span><span class="sxs-lookup"><span data-stu-id="2366a-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="2366a-107">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2366a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="2366a-108">参照してください、[次の手順](#next-steps)それを実行する方法についてのセクションでします。</span><span class="sxs-lookup"><span data-stu-id="2366a-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2366a-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2366a-109">Prerequisites</span></span>

* <span data-ttu-id="2366a-110">アプリは、.NET Framework ランタイムで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2366a-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="2366a-111">*.Csproj*ファイルでの適切な値を指定[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)と[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)です。</span><span class="sxs-lookup"><span data-stu-id="2366a-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="2366a-112">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="2366a-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="2366a-113">Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="2366a-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="2366a-114">アプリは、(内部ネットワーク) からだけでなく、インターネットから要求を受け取る場合は使用する必要があります、 [WebListener](xref:fundamentals/servers/weblistener) web サーバーではなくより[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="2366a-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="2366a-115">Kestrel は、IIS とエッジ展開するために使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2366a-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="2366a-116">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2366a-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="2366a-117">作業の開始</span><span class="sxs-lookup"><span data-stu-id="2366a-117">Getting started</span></span>

<span data-ttu-id="2366a-118">このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="2366a-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="2366a-119">NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。</span><span class="sxs-lookup"><span data-stu-id="2366a-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="2366a-120">次の変更を加え`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="2366a-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="2366a-121">呼び出す`host.RunAsService`の代わりに`host.Run`です。</span><span class="sxs-lookup"><span data-stu-id="2366a-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="2366a-122">コードを呼び出す場合`UseContentRoot`パスの代わりに、発行場所を使用します。`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="2366a-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="2366a-123">フォルダーにアプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="2366a-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="2366a-124">使用して[dotnet 発行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles)フォルダーに発行します。</span><span class="sxs-lookup"><span data-stu-id="2366a-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="2366a-125">テストを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="2366a-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="2366a-126">使用する管理者コマンド プロンプト ウィンドウを開き、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="2366a-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="2366a-127">MyService サービスの名前が場合に、アプリの発行`c:\svc`AspNetCoreService の名前は、アプリ自体は、コマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2366a-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="2366a-128">`binPath`値は、アプリの実行可能ファイル、実行可能ファイル名そのものを含むへのパス。</span><span class="sxs-lookup"><span data-stu-id="2366a-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

  <span data-ttu-id="2366a-130">これらのコマンドが完了、[参照] と同じパスに、コンソール アプリケーションとして実行する場合 (既定では、 `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="2366a-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![サービスで実行されています。](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="2366a-132">サービスの外部で実行するための手段します。</span><span class="sxs-lookup"><span data-stu-id="2366a-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="2366a-133">テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行中にデバッグしやすく`host.RunAsService`特定の条件下でのみです。</span><span class="sxs-lookup"><span data-stu-id="2366a-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="2366a-134">コンソール アプリとしてアプリを実行できますなど、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。</span><span class="sxs-lookup"><span data-stu-id="2366a-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="2366a-135">停止および開始イベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="2366a-135">Handle stopping and starting events</span></span>

<span data-ttu-id="2366a-136">処理するために`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加します。</span><span class="sxs-lookup"><span data-stu-id="2366a-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="2366a-137">`WebHostService` から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2366a-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="2366a-138">拡張メソッドを作成する`IWebHost`カスタムをパスする`WebHostService`に`ServiceBase.Run`です。</span><span class="sxs-lookup"><span data-stu-id="2366a-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="2366a-139">`Program.Main`変更メソッドを呼び出して、新しい拡張機能の代わりに`host.RunAsService`です。</span><span class="sxs-lookup"><span data-stu-id="2366a-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="2366a-140">場合、カスタム`WebHostService`(ロガー) などの依存関係の挿入からサービスを取得から取得する必要があるコード、`Services`プロパティ`IWebHost`です。</span><span class="sxs-lookup"><span data-stu-id="2366a-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="2366a-141">次の手順</span><span class="sxs-lookup"><span data-stu-id="2366a-141">Next steps</span></span>

<span data-ttu-id="2366a-142">[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)に付属しているこの記事は、前のコード例に示すように変更されている単純な MVC web アプリです。</span><span class="sxs-lookup"><span data-stu-id="2366a-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="2366a-143">実行するには、サービスで、次の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="2366a-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="2366a-144">公開*c:\svc*です。</span><span class="sxs-lookup"><span data-stu-id="2366a-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="2366a-145">管理者のウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="2366a-145">Open an administrator window.</span></span>

* <span data-ttu-id="2366a-146">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2366a-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="2366a-147">ブラウザーで実行されていることを確認する http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="2366a-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="2366a-148">エラー メッセージにアクセスできるようにする簡単な方法がなどのログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](xref:fundamentals/logging/index#eventlog)です。</span><span class="sxs-lookup"><span data-stu-id="2366a-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="2366a-149">受信確認</span><span class="sxs-lookup"><span data-stu-id="2366a-149">Acknowledgments</span></span>

<span data-ttu-id="2366a-150">この記事は、既に発行されたソースを利用して記述されています。</span><span class="sxs-lookup"><span data-stu-id="2366a-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="2366a-151">最も古いとそれらの最も役に立つ以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2366a-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="2366a-152">Windows サービスとしての ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="2366a-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="2366a-153">Windows サービスで、ASP.NET Core をホストする方法</span><span class="sxs-lookup"><span data-stu-id="2366a-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
