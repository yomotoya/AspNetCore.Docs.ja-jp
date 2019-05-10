---
title: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク
author: guardrex
description: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 613227cdead1d0b62a0dead2fca9fab68fd534cc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889007"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="106aa-103">ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="106aa-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="106aa-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="106aa-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="106aa-105">ASP.NET Core では、バックグラウンド タスクを*ホステッド サービス*として実装することができます。</span><span class="sxs-lookup"><span data-stu-id="106aa-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="106aa-106">ホストされるサービスは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="106aa-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="106aa-107">このトピックでは、3 つのホステッド サービスの例について説明します。</span><span class="sxs-lookup"><span data-stu-id="106aa-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="106aa-108">タイマーで実行されるバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="106aa-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="106aa-109">[スコープ サービス](xref:fundamentals/dependency-injection#service-lifetimes)をアクティブ化するホステッド サービス。</span><span class="sxs-lookup"><span data-stu-id="106aa-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="106aa-110">スコープ サービスは依存関係の挿入を使用できます。</span><span class="sxs-lookup"><span data-stu-id="106aa-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="106aa-111">連続して実行される、キューに格納されたバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="106aa-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="106aa-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="106aa-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="106aa-113">サンプル アプリには、2 つのバージョンが用意されています。</span><span class="sxs-lookup"><span data-stu-id="106aa-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="106aa-114">Web ホスト &ndash; Web ホストは、Web アプリをホストするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="106aa-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="106aa-115">このトピックで示すコード例は、サンプルの Web ホスト バージョンからのものです。</span><span class="sxs-lookup"><span data-stu-id="106aa-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="106aa-116">詳細については、[Web ホスト](xref:fundamentals/host/web-host)のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="106aa-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="106aa-117">汎用ホスト &ndash; 汎用ホストは ASP.NET Core 2.1 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="106aa-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="106aa-118">詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="106aa-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="106aa-119">Package</span><span class="sxs-lookup"><span data-stu-id="106aa-119">Package</span></span>

<span data-ttu-id="106aa-120">[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)を参照するか、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) パッケージへのパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="106aa-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="106aa-121">IHostedService インターフェイス</span><span class="sxs-lookup"><span data-stu-id="106aa-121">IHostedService interface</span></span>

<span data-ttu-id="106aa-122">ホステッド サービスでは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="106aa-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="106aa-123">このインターフェイスは、ホストによって管理されるオブジェクトの 2 つのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="106aa-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="106aa-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` には、バックグラウンド タスクを開始するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="106aa-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="106aa-125">[Web ホスト](xref:fundamentals/host/web-host) を使用する場合は、サーバーが起動し、[IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) がトリガーされた後で、`StartAsync` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="106aa-126">[汎用ホスト](xref:fundamentals/host/generic-host) を使用する場合は、`ApplicationStarted` がトリガーされる前に `StartAsync` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="106aa-127">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; ホストが正常なシャットダウンを実行しているときにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="106aa-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="106aa-128">`StopAsync` には、バックグラウンド タスクを終了するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="106aa-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="106aa-129">アンマネージ リソースを破棄するには、<xref:System.IDisposable> と[ファイナライザー (デストラクター)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) を実装します。</span><span class="sxs-lookup"><span data-stu-id="106aa-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="106aa-130">キャンセル トークンには、シャットダウン プロセスが正常に行われないことを示す、既定の 5 秒間のタイムアウトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="106aa-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="106aa-131">キャンセルがトークンに要求された場合:</span><span class="sxs-lookup"><span data-stu-id="106aa-131">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="106aa-132">アプリで実行されている残りのバックグラウンド操作が中止します。</span><span class="sxs-lookup"><span data-stu-id="106aa-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="106aa-133">`StopAsync` で呼び出されたすべてのメソッドが速やかに戻ります。</span><span class="sxs-lookup"><span data-stu-id="106aa-133">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="106aa-134">ただし、キャンセルが要求された後もタスクは破棄されません&mdash;呼び出し元がすべてのタスクの完了を待機します。</span><span class="sxs-lookup"><span data-stu-id="106aa-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="106aa-135">アプリが予期せずシャットダウンした場合 (たとえば、アプリのプロセスが失敗した場合)、`StopAsync` は呼び出されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="106aa-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="106aa-136">そのため、`StopAsync` で呼び出されたメソッドや行われた操作が実行されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="106aa-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="106aa-137">既定の 5 秒のシャットダウン タイムアウトを延長するには、次を設定します。</span><span class="sxs-lookup"><span data-stu-id="106aa-137">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="106aa-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (汎用ホストを使用するとき)</span><span class="sxs-lookup"><span data-stu-id="106aa-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="106aa-139">詳細については、「<xref:fundamentals/host/generic-host#shutdown-timeout>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="106aa-139">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="106aa-140">シャットダウン タイムアウトのホスト構成設定 (Web ホストを使用するとき)</span><span class="sxs-lookup"><span data-stu-id="106aa-140">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="106aa-141">詳細については、「<xref:fundamentals/host/web-host#shutdown-timeout>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="106aa-141">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="106aa-142">ホステッド サービスは、アプリの起動時に一度アクティブ化され、アプリのシャットダウン時に正常にシャットダウンされます。</span><span class="sxs-lookup"><span data-stu-id="106aa-142">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="106aa-143">バックグラウンド タスクの実行中にエラーがスローされた場合、`StopAsync` が呼び出されていなくても `Dispose` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="106aa-143">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="106aa-144">時間が指定されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="106aa-144">Timed background tasks</span></span>

<span data-ttu-id="106aa-145">時間が指定されたバックグラウンド タスクは、[System.Threading.Timer](xref:System.Threading.Timer) クラスを利用します。</span><span class="sxs-lookup"><span data-stu-id="106aa-145">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="106aa-146">このタイマーはタスクの `DoWork` メソッドをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="106aa-146">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="106aa-147">タイマーは `StopAsync` で無効になり、`Dispose` でサービス コンテナーが破棄されたときに破棄されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-147">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="106aa-148">サービスは、`AddHostedService` 拡張メソッドを使用して `Startup.ConfigureServices` に登録されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-148">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="106aa-149">バックグラウンド タスクでスコープ サービスを使用する</span><span class="sxs-lookup"><span data-stu-id="106aa-149">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="106aa-150">`IHostedService` 内で[スコープ サービス](xref:fundamentals/dependency-injection#service-lifetimes)を使用するには、スコープを作成します。</span><span class="sxs-lookup"><span data-stu-id="106aa-150">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="106aa-151">既定では、ホステッド サービスのスコープは作成されません。</span><span class="sxs-lookup"><span data-stu-id="106aa-151">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="106aa-152">バックグラウンド タスクのスコープ サービスには、バックグラウンド タスクのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="106aa-152">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="106aa-153">次の例では、<xref:Microsoft.Extensions.Logging.ILogger> がサービスに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="106aa-153">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="106aa-154">ホステッド サービスはスコープを作成してバックグラウンド タスクのスコープ サービスを解決し、`DoWork` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="106aa-154">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="106aa-155">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="106aa-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="106aa-156">`IHostedService` の実装は、`AddHostedService` 拡張メソッドで登録されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="106aa-157">キューに格納されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="106aa-157">Queued background tasks</span></span>

<span data-ttu-id="106aa-158">バックグラウンド タスク キューは、.NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([ASP.NET Core 3.0 用に暫定的に組み込まれる予定](https://github.com/aspnet/Hosting/issues/1280)) に基づいています。</span><span class="sxs-lookup"><span data-stu-id="106aa-158">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="106aa-159">`QueueHostedService` では、キュー内のバックグラウンド タスクは、<xref:Microsoft.Extensions.Hosting.BackgroundService> としてデキューされ、実行されます。これは、長時間実行する `IHostedService` を構成するための基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="106aa-159">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="106aa-160">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="106aa-160">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="106aa-161">`IHostedService` の実装は、`AddHostedService` 拡張メソッドで登録されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-161">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="106aa-162">インデックス ページ モデル クラスで:</span><span class="sxs-lookup"><span data-stu-id="106aa-162">In the Index page model class:</span></span>

* <span data-ttu-id="106aa-163">`IBackgroundTaskQueue` がコンストラクターに挿入され、`Queue` に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="106aa-163">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="106aa-164"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> が挿入され、`_serviceScopeFactory` に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="106aa-164">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="106aa-165">ファクトリは、スコープ内でサービス作成するための <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> のインスタンス作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-165">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="106aa-166">スコープは、アプリの `AppDbContext` ([スコープ サービス](xref:fundamentals/dependency-injection#service-lifetimes)) を使用し、データベース レコードを `IBackgroundTaskQueue` (シングルトン サービス) に書き込むために作成されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-166">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="106aa-167">[インデックス] ページで **[タスクの追加]** ボタンを選択すると、`OnPostAddTask` メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="106aa-167">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="106aa-168">`QueueBackgroundWorkItem` が呼び出され、作業項目がエンキューされます。</span><span class="sxs-lookup"><span data-stu-id="106aa-168">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="106aa-169">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="106aa-169">Additional resources</span></span>

* [<span data-ttu-id="106aa-170">IHostedService と BackgroundService クラスを使ってマイクロサービスのバックグラウンド タスクを実装する</span><span class="sxs-lookup"><span data-stu-id="106aa-170">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
