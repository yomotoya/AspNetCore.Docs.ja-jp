---
title: ASP.NET Core SignalR .NET クライアント
author: bradygaster
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 640d75157e42ffa6d78235c5be03e4846e8dcde9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982947"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="a65be-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="a65be-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="a65be-104">ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。</span><span class="sxs-lookup"><span data-stu-id="a65be-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="a65be-105">Xamarin には、Visual Studio バージョンについての特別な要件があります。</span><span class="sxs-lookup"><span data-stu-id="a65be-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="a65be-106">詳細については、次を参照してください。 [Xamarin で SignalR クライアント 2.1.1](https://github.com/aspnet/Announcements/issues/305)します。</span><span class="sxs-lookup"><span data-stu-id="a65be-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="a65be-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a65be-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a65be-108">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="a65be-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="a65be-109">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="a65be-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="a65be-110">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="a65be-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="a65be-111">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="a65be-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="a65be-112">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="a65be-112">Connect to a hub</span></span>

<span data-ttu-id="a65be-113">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="a65be-114">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="a65be-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="a65be-115">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="a65be-116">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="a65be-117">接続が失われたハンドル</span><span class="sxs-lookup"><span data-stu-id="a65be-117">Handle lost connection</span></span>

<span data-ttu-id="a65be-118">使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。</span><span class="sxs-lookup"><span data-stu-id="a65be-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="a65be-119">たとえば、再接続を自動化することがあります。</span><span class="sxs-lookup"><span data-stu-id="a65be-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="a65be-120">`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="a65be-121">デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="a65be-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="a65be-122">非同期サポートの主な理由とは、接続を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="a65be-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="a65be-123">非同期アクションは、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="a65be-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="a65be-124">`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。</span><span class="sxs-lookup"><span data-stu-id="a65be-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="a65be-125">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="a65be-125">Call hub methods from client</span></span>

<span data-ttu-id="a65be-126">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a65be-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="a65be-127">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="a65be-128">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="a65be-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="a65be-129">`InvokeAsync`メソッドを返します。 を`Task`サーバー メソッドが戻るときに完了します。</span><span class="sxs-lookup"><span data-stu-id="a65be-129">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="a65be-130">戻り値は、存在する場合の結果として提供されます、`Task`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-130">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="a65be-131">サーバー上のメソッドによってスローされた例外を生成するエラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-131">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="a65be-132">使用`await`サーバー メソッドが完了するまで待機するための構文と`try...catch`構文エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="a65be-132">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="a65be-133">`SendAsync`メソッドを返します。 を`Task`メッセージがサーバーに送信されたときにこれが完了するとします。</span><span class="sxs-lookup"><span data-stu-id="a65be-133">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="a65be-134">この戻り値が指定されていない`Task`サーバー メソッドが完了するまで待機しません。</span><span class="sxs-lookup"><span data-stu-id="a65be-134">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="a65be-135">メッセージの送信中に、クライアントでスローされた例外を生成、エラーが発生した`Task`します。</span><span class="sxs-lookup"><span data-stu-id="a65be-135">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="a65be-136">使用`await`と`try...catch`処理するために構文エラーを送信します。</span><span class="sxs-lookup"><span data-stu-id="a65be-136">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="a65be-137">Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="a65be-137">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="a65be-138">詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。</span><span class="sxs-lookup"><span data-stu-id="a65be-138">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="a65be-139">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a65be-139">Call client methods from hub</span></span>

<span data-ttu-id="a65be-140">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="a65be-140">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="a65be-141">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a65be-141">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="a65be-142">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="a65be-142">Error handling and logging</span></span>

<span data-ttu-id="a65be-143">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="a65be-143">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="a65be-144">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a65be-144">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="a65be-145">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a65be-145">Additional resources</span></span>

* [<span data-ttu-id="a65be-146">ハブ</span><span class="sxs-lookup"><span data-stu-id="a65be-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a65be-147">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="a65be-147">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a65be-148">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="a65be-148">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="a65be-149">Azure SignalR サービスのサーバーレス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="a65be-149">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
