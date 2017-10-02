---
title: "Windows サービスのホスト"
author: tdykstra
description: "Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。"
keywords: "ASP.NET Core、Windows サービスをホストしています。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: ca3b98f0b0405fcd5751cb7d9bc7a40257739084
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="5fd07-104">Windows サービスでの ASP.NET Core アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="5fd07-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="5fd07-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5fd07-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5fd07-106">IIS を使用しない場合は、Windows 上の ASP.NET Core アプリケーションをホストするための推奨方法がで実行するには、 [Windows サービス](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="5fd07-107">このようにして自動的に開始できますが再起動し、クラッシュの後にログインするユーザーが待機することがなく。</span><span class="sxs-lookup"><span data-stu-id="5fd07-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="5fd07-108">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="5fd07-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5fd07-109">参照してください、[次の手順](#next-steps)それを実行する方法についてのセクションでします。</span><span class="sxs-lookup"><span data-stu-id="5fd07-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fd07-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5fd07-110">Prerequisites</span></span>

* <span data-ttu-id="5fd07-111">.NET framework ランタイムでは、アプリが実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5fd07-111">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="5fd07-112">*.Csproj*ファイルでの適切な値を指定[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)と[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="5fd07-113">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="5fd07-114">Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="5fd07-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="5fd07-115">アプリは、要求を取得する (内部ネットワーク) からだけでなく、インターネットからは場合は、使用する必要があります、 [WebListener](xref:fundamentals/servers/weblistener) web サーバーではなくより[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="5fd07-116">Kestrel は、IIS とエッジ展開するために使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5fd07-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="5fd07-117">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5fd07-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="5fd07-118">作業の開始</span><span class="sxs-lookup"><span data-stu-id="5fd07-118">Getting started</span></span>

<span data-ttu-id="5fd07-119">このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="5fd07-120">NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="5fd07-121">次の変更を加え`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5fd07-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="5fd07-122">呼び出す`host.RunAsService`の代わりに`host.Run`です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="5fd07-123">コードを呼び出す場合`UseContentRoot`パスの代わりに、発行場所を使用します。`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="5fd07-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="5fd07-124">フォルダーにアプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="5fd07-125">使用して[dotnet 発行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:publishing/web-publishing-vs)フォルダーに発行します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="5fd07-126">テストを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="5fd07-127">使用する管理者コマンド プロンプト ウィンドウを開き、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="5fd07-128">アプリを公開する場合は MyService サービスの名前を入力`c:\svc`AspNetCoreService の名前は、アプリ自体は、コマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5fd07-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="5fd07-129">`binPath`値は、アプリの実行可能ファイル、実行可能ファイル名そのものを含むへのパス。</span><span class="sxs-lookup"><span data-stu-id="5fd07-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

  <span data-ttu-id="5fd07-131">これらのコマンドが完了は、コンソール アプリケーションとして実行したときと同じパスを参照することができます (既定では、 `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="5fd07-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![サービスで実行されています。](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="5fd07-133">サービスの外部で実行するための手段します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="5fd07-134">テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行している場合にデバッグしやすく`host.RunAsService`特定の条件下でのみです。</span><span class="sxs-lookup"><span data-stu-id="5fd07-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="5fd07-135">たとえば、でしたを実行するコンソール アプリケーションとして表示される場合、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。</span><span class="sxs-lookup"><span data-stu-id="5fd07-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="5fd07-136">停止および開始イベントを処理します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-136">Handle stopping and starting events</span></span>

<span data-ttu-id="5fd07-137">処理する場合`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加。</span><span class="sxs-lookup"><span data-stu-id="5fd07-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="5fd07-138">`WebHostService` から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="5fd07-139">拡張メソッドを作成する`IWebHost`独自で渡された`WebHostService`に`ServiceBase.Run`です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="5fd07-140">`Program.Main`変更メソッドを呼び出して、新しい拡張機能の代わりに`host.RunAsService`です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="5fd07-141">場合、カスタム`WebHostService`(ロガー) などの依存関係の挿入からサービスを取得する必要があるコードから取得できます、`Services`プロパティ`IWebHost`です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="5fd07-142">次のステップ</span><span class="sxs-lookup"><span data-stu-id="5fd07-142">Next steps</span></span>

<span data-ttu-id="5fd07-143">[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)に付属しているこの記事は、前のコード例に示すように変更されている単純な MVC web アプリです。</span><span class="sxs-lookup"><span data-stu-id="5fd07-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="5fd07-144">実行するには、サービスで、次の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="5fd07-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="5fd07-145">公開*c:\svc*です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="5fd07-146">管理者のウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="5fd07-146">Open an administrator window.</span></span>

* <span data-ttu-id="5fd07-147">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="5fd07-148">ブラウザーで実行されていることを確認する http://localhost:5000 に移動します。</span><span class="sxs-lookup"><span data-stu-id="5fd07-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="5fd07-149">エラー メッセージにアクセスできるようにする簡単な方法がなどのログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](xref:fundamentals/logging#eventlog)です。</span><span class="sxs-lookup"><span data-stu-id="5fd07-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5fd07-150">受信確認</span><span class="sxs-lookup"><span data-stu-id="5fd07-150">Acknowledgments</span></span>

<span data-ttu-id="5fd07-151">この記事は、既に発行されたソースを利用して記述されています。</span><span class="sxs-lookup"><span data-stu-id="5fd07-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="5fd07-152">最も古いとそれらの最も役に立つ以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5fd07-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="5fd07-153">Windows サービスとしての ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="5fd07-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="5fd07-154">Windows サービスで、ASP.NET Core をホストする方法</span><span class="sxs-lookup"><span data-stu-id="5fd07-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
