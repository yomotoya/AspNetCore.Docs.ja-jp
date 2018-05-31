---
title: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク
author: guardrex
description: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc39d125b639719599eca68d627fda014fb107e0
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555301"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="78a69-103">ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="78a69-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="78a69-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="78a69-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="78a69-105">ASP.NET Core では、バックグラウンド タスクを*ホステッド サービス*として実装することができます。</span><span class="sxs-lookup"><span data-stu-id="78a69-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="78a69-106">ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="78a69-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="78a69-107">このトピックでは、3 つのホステッド サービスの例について説明します。</span><span class="sxs-lookup"><span data-stu-id="78a69-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="78a69-108">タイマーで実行されるバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="78a69-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="78a69-109">スコープ サービスをアクティブ化するホステッド サービス。</span><span class="sxs-lookup"><span data-stu-id="78a69-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="78a69-110">スコープ サービスは依存関係の挿入を使用できます。</span><span class="sxs-lookup"><span data-stu-id="78a69-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="78a69-111">連続して実行される、キューに格納されたバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="78a69-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="78a69-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="78a69-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="78a69-113">サンプル アプリには、2 つのバージョンが用意されています。</span><span class="sxs-lookup"><span data-stu-id="78a69-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="78a69-114">Web ホスト &ndash; Web ホストは、Web アプリをホストするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="78a69-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="78a69-115">このトピックで示すコード例は、サンプルの Web ホスト バージョンからのものです。</span><span class="sxs-lookup"><span data-stu-id="78a69-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="78a69-116">詳細については、[Web ホスト](xref:fundamentals/host/web-host)のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="78a69-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="78a69-117">汎用ホスト &ndash; 汎用ホストは ASP.NET Core 2.1 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="78a69-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="78a69-118">詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="78a69-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="78a69-119">IHostedService インターフェイス</span><span class="sxs-lookup"><span data-stu-id="78a69-119">IHostedService interface</span></span>

<span data-ttu-id="78a69-120">ホステッド サービスは [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="78a69-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="78a69-121">このインターフェイスは、ホストによって管理されるオブジェクトの 2 つのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="78a69-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="78a69-122">[StartAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - サーバーが起動し、[IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) がトリガーされた後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="78a69-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="78a69-123">`StartAsync` には、バックグラウンド タスクを開始するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78a69-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="78a69-124">[StopAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - ホストが正常なシャットダウンを実行しているときにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="78a69-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="78a69-125">`StopAsync` には、バックグラウンド タスクを終了し、アンマネージド リソースを破棄するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78a69-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="78a69-126">アプリが予期せずシャットダウンした場合 (たとえば、アプリのプロセスが失敗した場合)、`StopAsync` は呼び出されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="78a69-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="78a69-127">ホステッド サービスは、アプリの起動時に一度アクティブ化され、アプリのシャットダウン時に正常にシャットダウンするシングルトンです。</span><span class="sxs-lookup"><span data-stu-id="78a69-127">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="78a69-128">[IDisposable](/dotnet/api/system.idisposable) が実装されている場合、サービス コンテナーが破棄されるときにリソースを破棄することができます。</span><span class="sxs-lookup"><span data-stu-id="78a69-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="78a69-129">バックグラウンド タスクの実行中にエラーがスローされた場合、`StopAsync` が呼び出されていなくても `Dispose` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="78a69-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="78a69-130">時間が指定されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="78a69-130">Timed background tasks</span></span>

<span data-ttu-id="78a69-131">時間が指定されたバックグラウンド タスクは、[System.Threading.Timer](/dotnet/api/system.threading.timer) クラスを利用します。</span><span class="sxs-lookup"><span data-stu-id="78a69-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="78a69-132">このタイマーはタスクの `DoWork` メソッドをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="78a69-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="78a69-133">タイマーは `StopAsync` で無効になり、`Dispose` でサービス コンテナーが破棄されたときに破棄されます。</span><span class="sxs-lookup"><span data-stu-id="78a69-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="78a69-134">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="78a69-134">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="78a69-135">バックグラウンド タスクでスコープ サービスを使用する</span><span class="sxs-lookup"><span data-stu-id="78a69-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="78a69-136">`IHostedService` 内でスコープ サービスを使用するには、スコープを作成します。</span><span class="sxs-lookup"><span data-stu-id="78a69-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="78a69-137">既定では、ホステッド サービスのスコープは作成されません。</span><span class="sxs-lookup"><span data-stu-id="78a69-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="78a69-138">バックグラウンド タスクのスコープ サービスには、バックグラウンド タスクのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78a69-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="78a69-139">次の例では、[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) がサービスに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="78a69-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="78a69-140">ホステッド サービスはスコープを作成してバックグラウンド タスクのスコープ サービスを解決し、`DoWork` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="78a69-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="78a69-141">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="78a69-141">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="78a69-142">キューに格納されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="78a69-142">Queued background tasks</span></span>

<span data-ttu-id="78a69-143">バックグラウンド タスク キューは、.NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core2.2 用に暫定的に組み込まれる予定](https://github.com/aspnet/Hosting/issues/1280)) に基づいています。</span><span class="sxs-lookup"><span data-stu-id="78a69-143">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="78a69-144">`QueueHostedService` では、キュー内のバックグラウンド タスク (`workItem`) がデキューされ、実行されます。</span><span class="sxs-lookup"><span data-stu-id="78a69-144">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="78a69-145">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="78a69-145">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="78a69-146">インデックス ページ モデル クラスでは、`IBackgroundTaskQueue` がコンストラクターに挿入され、`Queue` に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="78a69-146">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="78a69-147">[インデックス] ページで **[タスクの追加]** ボタンを選択すると、`OnPostAddTask` メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="78a69-147">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="78a69-148">`QueueBackgroundWorkItem` が呼び出され、作業項目がエンキューされます。</span><span class="sxs-lookup"><span data-stu-id="78a69-148">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="78a69-149">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="78a69-149">Additional resources</span></span>

* [<span data-ttu-id="78a69-150">IHostedService と BackgroundService クラスを使ってマイクロサービスのバックグラウンド タスクを実装する</span><span class="sxs-lookup"><span data-stu-id="78a69-150">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="78a69-151">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="78a69-151">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
