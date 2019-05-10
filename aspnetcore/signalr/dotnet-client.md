---
title: ASP.NET Core SignalR .NET クライアント
author: bradygaster
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087719"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="98bef-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="98bef-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="98bef-104">ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。</span><span class="sxs-lookup"><span data-stu-id="98bef-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="98bef-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="98bef-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="98bef-106">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="98bef-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="98bef-107">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="98bef-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="98bef-108">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="98bef-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="98bef-109">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="98bef-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="98bef-110">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="98bef-110">Connect to a hub</span></span>

<span data-ttu-id="98bef-111">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="98bef-112">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="98bef-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="98bef-113">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="98bef-114">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="98bef-115">接続が失われたハンドル</span><span class="sxs-lookup"><span data-stu-id="98bef-115">Handle lost connection</span></span>

<span data-ttu-id="98bef-116">使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。</span><span class="sxs-lookup"><span data-stu-id="98bef-116">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="98bef-117">たとえば、再接続を自動化することがあります。</span><span class="sxs-lookup"><span data-stu-id="98bef-117">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="98bef-118">`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-118">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="98bef-119">デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="98bef-119">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="98bef-120">非同期サポートの主な理由とは、接続を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="98bef-120">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="98bef-121">非同期アクションは、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="98bef-121">Starting a connection is an async action.</span></span>

<span data-ttu-id="98bef-122">`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。</span><span class="sxs-lookup"><span data-stu-id="98bef-122">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="98bef-123">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="98bef-123">Call hub methods from client</span></span>

<span data-ttu-id="98bef-124">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="98bef-124">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="98bef-125">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-125">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="98bef-126">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="98bef-126">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="98bef-127">`InvokeAsync`メソッドを返します。 を`Task`サーバー メソッドが戻るときに完了します。</span><span class="sxs-lookup"><span data-stu-id="98bef-127">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="98bef-128">戻り値は、存在する場合の結果として提供されます、`Task`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-128">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="98bef-129">サーバー上のメソッドによってスローされた例外を生成するエラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-129">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="98bef-130">使用`await`サーバー メソッドが完了するまで待機するための構文と`try...catch`構文エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="98bef-130">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="98bef-131">`SendAsync`メソッドを返します。 を`Task`メッセージがサーバーに送信されたときにこれが完了するとします。</span><span class="sxs-lookup"><span data-stu-id="98bef-131">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="98bef-132">この戻り値が指定されていない`Task`サーバー メソッドが完了するまで待機しません。</span><span class="sxs-lookup"><span data-stu-id="98bef-132">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="98bef-133">メッセージの送信中に、クライアントでスローされた例外を生成、エラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="98bef-133">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="98bef-134">使用`await`と`try...catch`処理するために構文エラーを送信します。</span><span class="sxs-lookup"><span data-stu-id="98bef-134">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="98bef-135">Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="98bef-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="98bef-136">詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。</span><span class="sxs-lookup"><span data-stu-id="98bef-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="98bef-137">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="98bef-137">Call client methods from hub</span></span>

<span data-ttu-id="98bef-138">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="98bef-138">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="98bef-139">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="98bef-139">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="98bef-140">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="98bef-140">Error handling and logging</span></span>

<span data-ttu-id="98bef-141">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="98bef-141">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="98bef-142">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="98bef-142">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="98bef-143">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="98bef-143">Additional resources</span></span>

* [<span data-ttu-id="98bef-144">ハブ</span><span class="sxs-lookup"><span data-stu-id="98bef-144">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="98bef-145">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="98bef-145">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="98bef-146">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="98bef-146">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="98bef-147">Azure SignalR サービスのサーバーレス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="98bef-147">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
