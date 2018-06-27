---
title: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク
author: guardrex
description: ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273782"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core では、バックグラウンド タスクを*ホステッド サービス*として実装することができます。 ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。 このトピックでは、3 つのホステッド サービスの例について説明します。

* タイマーで実行されるバックグラウンド タスク。
* スコープ サービスをアクティブ化するホステッド サービス。 スコープ サービスは依存関係の挿入を使用できます。
* 連続して実行される、キューに格納されたバックグラウンド タスク。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

::: moniker range=">= aspnetcore-2.1"

サンプル アプリには、2 つのバージョンが用意されています。

* Web ホスト &ndash; Web ホストは、Web アプリをホストするのに役立ちます。 このトピックで示すコード例は、サンプルの Web ホスト バージョンからのものです。 詳細については、[Web ホスト](xref:fundamentals/host/web-host)のトピックをご覧ください。
* 汎用ホスト &ndash; 汎用ホストは ASP.NET Core 2.1 の新機能です。 詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)のトピックをご覧ください。

::: moniker-end

## <a name="ihostedservice-interface"></a>IHostedService インターフェイス

ホステッド サービスは [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装します。 このインターフェイスは、ホストによって管理されるオブジェクトの 2 つのメソッドを定義します。

* [StartAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - サーバーが起動し、[IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) がトリガーされた後に呼び出されます。 `StartAsync` には、バックグラウンド タスクを開始するロジックが含まれています。

* [StopAsync (CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - ホストが正常なシャットダウンを実行しているときにトリガーされます。 `StopAsync` には、バックグラウンド タスクを終了し、アンマネージド リソースを破棄するロジックが含まれています。 アプリが予期せずシャットダウンした場合 (たとえば、アプリのプロセスが失敗した場合)、`StopAsync` は呼び出されないことがあります。

ホステッド サービスは、アプリの起動時に一度アクティブ化され、アプリのシャットダウン時に正常にシャットダウンされます。 [IDisposable](/dotnet/api/system.idisposable) が実装されている場合、サービス コンテナーが破棄されるときにリソースを破棄することができます。 バックグラウンド タスクの実行中にエラーがスローされた場合、`StopAsync` が呼び出されていなくても `Dispose` を呼び出す必要があります。

## <a name="timed-background-tasks"></a>時間が指定されたバックグラウンド タスク

時間が指定されたバックグラウンド タスクは、[System.Threading.Timer](/dotnet/api/system.threading.timer) クラスを利用します。 このタイマーはタスクの `DoWork` メソッドをトリガーします。 タイマーは `StopAsync` で無効になり、`Dispose` でサービス コンテナーが破棄されたときに破棄されます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

サービスは、`AddHostedService` 拡張メソッドを使用して `Startup.ConfigureServices` に登録されます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

サービスは `Startup.ConfigureServices` に登録されています。

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a>バックグラウンド タスクでスコープ サービスを使用する

`IHostedService` 内でスコープ サービスを使用するには、スコープを作成します。 既定では、ホステッド サービスのスコープは作成されません。

バックグラウンド タスクのスコープ サービスには、バックグラウンド タスクのロジックが含まれています。 次の例では、[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) がサービスに挿入されています。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

ホステッド サービスはスコープを作成してバックグラウンド タスクのスコープ サービスを解決し、`DoWork` メソッドを呼び出します。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

サービスは `Startup.ConfigureServices` に登録されています。 `IHostedService` の実装は、`AddHostedService` 拡張メソッドで登録されます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

サービスは `Startup.ConfigureServices` に登録されています。

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

## <a name="queued-background-tasks"></a>キューに格納されたバックグラウンド タスク

バックグラウンド タスク キューは、.NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core 3.0 用に暫定的に組み込まれる予定](https://github.com/aspnet/Hosting/issues/1280)) に基づいています。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

`QueueHostedService` では、キュー内のバックグラウンド タスク (`workItem`) がデキューされ、実行されます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

サービスは `Startup.ConfigureServices` に登録されています。 `IHostedService` の実装は、`AddHostedService` 拡張メソッドで登録されます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

サービスは `Startup.ConfigureServices` に登録されています。

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

インデックス ページ モデル クラスでは、`IBackgroundTaskQueue` がコンストラクターに挿入され、`Queue` に割り当てられます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

[インデックス] ページで **[タスクの追加]** ボタンを選択すると、`OnPostAddTask` メソッドが実行されます。 `QueueBackgroundWorkItem` が呼び出され、作業項目がエンキューされます。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>その他の技術情報

* [IHostedService と BackgroundService クラスを使ってマイクロサービスのバックグラウンド タスクを実装する](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
