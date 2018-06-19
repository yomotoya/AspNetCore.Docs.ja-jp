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
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374692"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="e3fe0-103">ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="e3fe0-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="e3fe0-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3fe0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3fe0-105">ASP.NET Core では、バックグラウンド タスクを*ホステッド サービス*として実装することができます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="e3fe0-106">ホステッド サービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="e3fe0-107">このトピックでは、3 つのホステッド サービスの例について説明します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="e3fe0-108">タイマーで実行されるバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="e3fe0-109">スコープ サービスをアクティブ化するホステッド サービス。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="e3fe0-110">スコープ サービスは依存関係の挿入を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="e3fe0-111">連続して実行される、キューに格納されたバックグラウンド タスク。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="e3fe0-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="e3fe0-113">IHostedService インターフェイス</span><span class="sxs-lookup"><span data-stu-id="e3fe0-113">IHostedService interface</span></span>

<span data-ttu-id="e3fe0-114">ホステッド サービスは [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="e3fe0-115">このインターフェイスは、ホストによって管理されるオブジェクトの 2 つのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="e3fe0-116">[StartAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - サーバーが起動し、[IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) がトリガーされた後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="e3fe0-117">`StartAsync` には、バックグラウンド タスクを開始するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="e3fe0-118">[StopAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - ホストが正常なシャットダウンを実行しているときにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="e3fe0-119">`StopAsync` には、バックグラウンド タスクを終了し、アンマネージド リソースを破棄するロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="e3fe0-120">アプリが予期せずシャットダウンした場合 (たとえば、アプリのプロセスが失敗した場合)、`StopAsync` は呼び出されないことがあります。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="e3fe0-121">ホステッド サービスは、アプリの起動時に一度アクティブ化され、アプリのシャットダウン時に正常にシャットダウンするシングルトンです。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="e3fe0-122">[IDisposable](/dotnet/api/system.idisposable) が実装されている場合、サービス コンテナーが破棄されるときにリソースを破棄することができます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="e3fe0-123">バックグラウンド タスクの実行中にエラーがスローされた場合、`StopAsync` が呼び出されていなくても `Dispose` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="e3fe0-124">時間が指定されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="e3fe0-124">Timed background tasks</span></span>

<span data-ttu-id="e3fe0-125">時間が指定されたバックグラウンド タスクは、[System.Threading.Timer](/dotnet/api/system.threading.timer) クラスを利用します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="e3fe0-126">このタイマーはタスクの `DoWork` メソッドをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="e3fe0-127">タイマーは `StopAsync` で無効になり、`Dispose` でサービス コンテナーが破棄されたときに破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="e3fe0-128">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="e3fe0-129">バックグラウンド タスクでスコープ サービスを使用する</span><span class="sxs-lookup"><span data-stu-id="e3fe0-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="e3fe0-130">`IHostedService` 内でスコープ サービスを使用するには、スコープを作成します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="e3fe0-131">既定では、ホステッド サービスのスコープは作成されません。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="e3fe0-132">バックグラウンド タスクのスコープ サービスには、バックグラウンド タスクのロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="e3fe0-133">次の例では、[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) がサービスに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="e3fe0-134">ホステッド サービスはスコープを作成してバックグラウンド タスクのスコープ サービスを解決し、`DoWork` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="e3fe0-135">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="e3fe0-136">キューに格納されたバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="e3fe0-136">Queued background tasks</span></span>

<span data-ttu-id="e3fe0-137">バックグラウンド タスク キューは、.NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core2.2 用に暫定的に組み込まれる予定](https://github.com/aspnet/Hosting/issues/1280)) に基づいています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="e3fe0-138">`QueueHostedService` では、キュー内のバックグラウンド タスク (`workItem`) がデキューされ、実行されます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="e3fe0-139">サービスは `Startup.ConfigureServices` に登録されています。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="e3fe0-140">インデックス ページ モデル クラスでは、`IBackgroundTaskQueue` がコンストラクターに挿入され、`Queue` に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="e3fe0-141">[インデックス] ページで **[タスクの追加]** ボタンを選択すると、`OnPostAddTask` メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="e3fe0-142">`QueueBackgroundWorkItem` が呼び出され、作業項目がエンキューされます。</span><span class="sxs-lookup"><span data-stu-id="e3fe0-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="e3fe0-143">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e3fe0-143">Additional resources</span></span>

* [<span data-ttu-id="e3fe0-144">IHostedService と BackgroundService クラスを使ってマイクロサービスのバックグラウンド タスクを実装する</span><span class="sxs-lookup"><span data-stu-id="e3fe0-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="e3fe0-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="e3fe0-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
