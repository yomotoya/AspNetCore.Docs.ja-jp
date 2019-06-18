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
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET クライアント

ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET クライアント パッケージをインストールします。

`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。 クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>ハブへの接続します。

接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。 接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。 挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。 接続を開始`StartAsync`します。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>接続が失われたハンドル

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>自動的に再接続します。

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>を使用して自動的に再接続するように構成できる、`WithAutomaticReconnect`メソッドを<xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>します。 既定では、自動的に再接続しません。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

任意のパラメーターを指定せず`WithAutomaticReconnect()`後、4 つの失敗した試行を停止する各再接続の試行を試す前に、それぞれ 0、2、10、および 30 秒間待機するクライアントを構成します。

任意の再接続試行を開始する前に、`HubConnection`への移行には、`HubConnectionState.Reconnecting`状態にあり、起動、`Reconnecting`イベント。  これは、UI 要素を無効にして、接続が失われたことをユーザーに警告する機会を提供します。 非対話型のアプリには、キューまたはメッセージのドロップを開始できます。

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

内の最初の 4 つの試行で、クライアントが再接続に成功した場合、`HubConnection`から戻るへの移行、`Connected`状態にあり、起動、`Reconnected`イベント。 これは、接続が再確立された、すべてのキューに置かれたメッセージをデキューするユーザーに通知する機会を提供します。

接続は、サーバーから完全に新規に見えるため、新しい`ConnectionId`に提供される、`Reconnected`イベント ハンドラー。

> [!WARNING]
> `Reconnected`イベント ハンドラーの`connectionId`パラメーターが null にする場合、`HubConnection`するように構成された[ネゴシエーションをスキップ](xref:signalr/configuration#configure-client-options)します。

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` 構成はありません、`HubConnection`起動障害を手動で処理する必要があるために、初回起動時の障害を再試行します。

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

内の最初の 4 つの試行で、クライアントが再接続が正常に場合、`HubConnection`への移行には、`Disconnected`状態にあり、起動、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>イベント。 これは、接続を手動で再起動するか、接続が完全に失われたことをユーザーに通知しようとする機会を提供します。

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

切断する前に再接続試行のカスタムの数を構成または再接続のタイミングを変更するには`WithAutomaticReconnect`再接続が試みられるたびに開始する前に待機するミリ秒の遅延を表す数値の配列を受け入れます。

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

上記の例では、`HubConnection`接続が失われた後にすぐに再接続しようとすると開始します。 またこれは、既定の構成に当てはまります。

既定の構成でこれと同じように、2 秒待機しているのではなく、最初の再接続試行が失敗すると、2 回目の再接続試行がすぐに開始もは。

2 回目の再接続の試行が失敗した場合、これは、既定の構成と同様にもう一度、10 秒後に、3 番目の再接続の試行が開始されます。

カスタム動作し、もう一度ブロックから分化既定の動作を 3 番目の再接続試行の失敗後に停止します。 既定の構成ではする 1 つより再接続別の 30 秒以内に試行します。

タイミングと自動の数をさらに多くのコントロールの接続試行、なら`WithAutomaticReconnect`実装するオブジェクトを受け入れる、`IRetryPolicy`インターフェイスという単一のメソッドを`NextRetryDelay`します。

`NextRetryDelay` 型の 1 つの引数`RetryContext`します。 `RetryContext`は 3 つのプロパティがあります: `PreviousRetryCount`、`ElapsedTime`と`RetryReason`である、 `long`、`TimeSpan`と`Exception`それぞれします。 最初の再接続試行する前に両方`PreviousRetryCount`と`ElapsedTime`ゼロ、および`RetryReason`接続が失われる原因となった例外になります。 各障害が発生した再試行後`PreviousRetryCount`、1 つずつ増えます`ElapsedTime`が再接続をこれまでに費やされた時間数を反映するように更新されます、`RetryReason`最後に再接続試行失敗の原因となった例外になります。

`NextRetryDelay` いずれかを表す TimeSpan 時間待ってから、[次へ] の再接続の試行を返す必要がありますまたは`null`場合、`HubConnection`再接続を停止する必要があります。

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

再接続で示すように手動で、クライアント コードを記述する代わりに、[手動で再接続](#manually-reconnect)します。

::: moniker-end

### <a name="manually-reconnect"></a>手動での再接続します。

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 3\.0 では、前に SignalR 用の .NET クライアントは自動的に再接続しません。 クライアントを手動で再接続するコードを記述する必要があります。

::: moniker-end

使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。 たとえば、再接続を自動化することがあります。

`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。 デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同期サポートの主な理由とは、接続を再起動するためです。 非同期アクションは、接続を開始します。

`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

`InvokeAsync` ハブのメソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。 SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync`メソッドを返します。 を`Task`サーバー メソッドが戻るときに完了します。 戻り値は、存在する場合の結果として提供されます、`Task`します。 サーバー上のメソッドによってスローされた例外を生成するエラーが発生した`Task`します。 使用`await`サーバー メソッドが完了するまで待機するための構文と`try...catch`構文エラーを処理します。

`SendAsync`メソッドを返します。 を`Task`メッセージがサーバーに送信されたときにこれが完了するとします。 この戻り値が指定されていない`Task`サーバー メソッドが完了するまで待機しません。 メッセージの送信中に、クライアントでスローされた例外を生成、エラーが発生した`Task`します。 使用`await`と`try...catch`処理するために構文エラーを送信します。

> [!NOTE]
> Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。 詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>エラー処理とログ記録

Try-catch ステートメントでエラーを処理します。 検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR サービスのサーバーレス ドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)
