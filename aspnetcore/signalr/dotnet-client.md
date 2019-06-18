---
title: ASP.NET Core SignalR .NET クライアント
author: bradygaster
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153122"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c4731-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="c4731-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c4731-104">ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。</span><span class="sxs-lookup"><span data-stu-id="c4731-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="c4731-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c4731-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c4731-106">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="c4731-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c4731-107">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c4731-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c4731-108">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="c4731-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c4731-109">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="c4731-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c4731-110">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="c4731-110">Connect to a hub</span></span>

<span data-ttu-id="c4731-111">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c4731-112">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="c4731-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c4731-113">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c4731-114">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="c4731-115">接続が失われたハンドル</span><span class="sxs-lookup"><span data-stu-id="c4731-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="c4731-116">自動的に再接続します。</span><span class="sxs-lookup"><span data-stu-id="c4731-116">Automatically reconnect</span></span>

<span data-ttu-id="c4731-117"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>を使用して自動的に再接続するように構成できる、`WithAutomaticReconnect`メソッドを<xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>します。</span><span class="sxs-lookup"><span data-stu-id="c4731-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="c4731-118">既定では、自動的に再接続しません。</span><span class="sxs-lookup"><span data-stu-id="c4731-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="c4731-119">任意のパラメーターを指定せず`WithAutomaticReconnect()`後、4 つの失敗した試行を停止する各再接続の試行を試す前に、それぞれ 0、2、10、および 30 秒間待機するクライアントを構成します。</span><span class="sxs-lookup"><span data-stu-id="c4731-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="c4731-120">任意の再接続試行を開始する前に、`HubConnection`への移行には、`HubConnectionState.Reconnecting`状態にあり、起動、`Reconnecting`イベント。</span><span class="sxs-lookup"><span data-stu-id="c4731-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="c4731-121">これは、UI 要素を無効にして、接続が失われたことをユーザーに警告する機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="c4731-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="c4731-122">非対話型のアプリには、キューまたはメッセージのドロップを開始できます。</span><span class="sxs-lookup"><span data-stu-id="c4731-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c4731-123">内の最初の 4 つの試行で、クライアントが再接続に成功した場合、`HubConnection`から戻るへの移行、`Connected`状態にあり、起動、`Reconnected`イベント。</span><span class="sxs-lookup"><span data-stu-id="c4731-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="c4731-124">これは、接続が再確立された、すべてのキューに置かれたメッセージをデキューするユーザーに通知する機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="c4731-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="c4731-125">接続は、サーバーから完全に新規に見えるため、新しい`ConnectionId`に提供される、`Reconnected`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="c4731-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="c4731-126">`Reconnected`イベント ハンドラーの`connectionId`パラメーターが null にする場合、`HubConnection`するように構成された[ネゴシエーションをスキップ](xref:signalr/configuration#configure-client-options)します。</span><span class="sxs-lookup"><span data-stu-id="c4731-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c4731-127">`WithAutomaticReconnect()` 構成はありません、`HubConnection`起動障害を手動で処理する必要があるために、初回起動時の障害を再試行します。</span><span class="sxs-lookup"><span data-stu-id="c4731-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

<span data-ttu-id="c4731-128">内の最初の 4 つの試行で、クライアントが再接続が正常に場合、`HubConnection`への移行には、`Disconnected`状態にあり、起動、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>イベント。</span><span class="sxs-lookup"><span data-stu-id="c4731-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="c4731-129">これは、接続を手動で再起動するか、接続が完全に失われたことをユーザーに通知しようとする機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="c4731-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c4731-130">切断する前に再接続試行のカスタムの数を構成または再接続のタイミングを変更するには`WithAutomaticReconnect`再接続が試みられるたびに開始する前に待機するミリ秒の遅延を表す数値の配列を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="c4731-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="c4731-131">上記の例では、`HubConnection`接続が失われた後にすぐに再接続しようとすると開始します。</span><span class="sxs-lookup"><span data-stu-id="c4731-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="c4731-132">またこれは、既定の構成に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="c4731-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="c4731-133">既定の構成でこれと同じように、2 秒待機しているのではなく、最初の再接続試行が失敗すると、2 回目の再接続試行がすぐに開始もは。</span><span class="sxs-lookup"><span data-stu-id="c4731-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c4731-134">2 回目の再接続の試行が失敗した場合、これは、既定の構成と同様にもう一度、10 秒後に、3 番目の再接続の試行が開始されます。</span><span class="sxs-lookup"><span data-stu-id="c4731-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="c4731-135">カスタム動作し、もう一度ブロックから分化既定の動作を 3 番目の再接続試行の失敗後に停止します。</span><span class="sxs-lookup"><span data-stu-id="c4731-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="c4731-136">既定の構成ではする 1 つより再接続別の 30 秒以内に試行します。</span><span class="sxs-lookup"><span data-stu-id="c4731-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="c4731-137">タイミングと自動の数をさらに多くのコントロールの接続試行、なら`WithAutomaticReconnect`実装するオブジェクトを受け入れる、`IRetryPolicy`インターフェイスという単一のメソッドを`NextRetryDelay`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="c4731-138">`NextRetryDelay` 型の 1 つの引数`RetryContext`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="c4731-139">`RetryContext`は 3 つのプロパティがあります: `PreviousRetryCount`、`ElapsedTime`と`RetryReason`である、 `long`、`TimeSpan`と`Exception`それぞれします。</span><span class="sxs-lookup"><span data-stu-id="c4731-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="c4731-140">最初の再接続試行する前に両方`PreviousRetryCount`と`ElapsedTime`ゼロ、および`RetryReason`接続が失われる原因となった例外になります。</span><span class="sxs-lookup"><span data-stu-id="c4731-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="c4731-141">各障害が発生した再試行後`PreviousRetryCount`、1 つずつ増えます`ElapsedTime`が再接続をこれまでに費やされた時間数を反映するように更新されます、`RetryReason`最後に再接続試行失敗の原因となった例外になります。</span><span class="sxs-lookup"><span data-stu-id="c4731-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="c4731-142">`NextRetryDelay` いずれかを表す TimeSpan 時間待ってから、[次へ] の再接続の試行を返す必要がありますまたは`null`場合、`HubConnection`再接続を停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4731-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

<span data-ttu-id="c4731-143">再接続で示すように手動で、クライアント コードを記述する代わりに、[手動で再接続](#manually-reconnect)します。</span><span class="sxs-lookup"><span data-stu-id="c4731-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="c4731-144">手動での再接続します。</span><span class="sxs-lookup"><span data-stu-id="c4731-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="c4731-145">3\.0 では、前に SignalR 用の .NET クライアントは自動的に再接続しません。</span><span class="sxs-lookup"><span data-stu-id="c4731-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="c4731-146">クライアントを手動で再接続するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4731-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="c4731-147">使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。</span><span class="sxs-lookup"><span data-stu-id="c4731-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="c4731-148">たとえば、再接続を自動化することがあります。</span><span class="sxs-lookup"><span data-stu-id="c4731-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="c4731-149">`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="c4731-150">デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="c4731-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="c4731-151">非同期サポートの主な理由とは、接続を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="c4731-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="c4731-152">非同期アクションは、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="c4731-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="c4731-153">`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。</span><span class="sxs-lookup"><span data-stu-id="c4731-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c4731-154">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="c4731-154">Call hub methods from client</span></span>

<span data-ttu-id="c4731-155">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c4731-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c4731-156">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c4731-157">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="c4731-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="c4731-158">`InvokeAsync`メソッドを返します。 を`Task`サーバー メソッドが戻るときに完了します。</span><span class="sxs-lookup"><span data-stu-id="c4731-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="c4731-159">戻り値は、存在する場合の結果として提供されます、`Task`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="c4731-160">サーバー上のメソッドによってスローされた例外を生成するエラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="c4731-161">使用`await`サーバー メソッドが完了するまで待機するための構文と`try...catch`構文エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="c4731-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="c4731-162">`SendAsync`メソッドを返します。 を`Task`メッセージがサーバーに送信されたときにこれが完了するとします。</span><span class="sxs-lookup"><span data-stu-id="c4731-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="c4731-163">この戻り値が指定されていない`Task`サーバー メソッドが完了するまで待機しません。</span><span class="sxs-lookup"><span data-stu-id="c4731-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="c4731-164">メッセージの送信中に、クライアントでスローされた例外を生成、エラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="c4731-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="c4731-165">使用`await`と`try...catch`処理するために構文エラーを送信します。</span><span class="sxs-lookup"><span data-stu-id="c4731-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="c4731-166">Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="c4731-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="c4731-167">詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。</span><span class="sxs-lookup"><span data-stu-id="c4731-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c4731-168">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="c4731-168">Call client methods from hub</span></span>

<span data-ttu-id="c4731-169">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="c4731-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="c4731-170">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c4731-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c4731-171">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="c4731-171">Error handling and logging</span></span>

<span data-ttu-id="c4731-172">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="c4731-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c4731-173">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c4731-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="c4731-174">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c4731-174">Additional resources</span></span>

* [<span data-ttu-id="c4731-175">ハブ</span><span class="sxs-lookup"><span data-stu-id="c4731-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c4731-176">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="c4731-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c4731-177">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="c4731-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c4731-178">Azure SignalR サービスのサーバーレス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="c4731-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
