---
title: ASP.NET Core SignalR .NET クライアント
author: bradygaster
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 25b618f7a424b217c0fb55417754ea358280b95a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836307"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="e9903-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="e9903-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="e9903-104">ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。</span><span class="sxs-lookup"><span data-stu-id="e9903-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="e9903-105">Xamarin には、Visual Studio バージョンについての特別な要件があります。</span><span class="sxs-lookup"><span data-stu-id="e9903-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="e9903-106">詳細については、次を参照してください。 [Xamarin で SignalR クライアント 2.1.1](https://github.com/aspnet/Announcements/issues/305)します。</span><span class="sxs-lookup"><span data-stu-id="e9903-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="e9903-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e9903-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e9903-108">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="e9903-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="e9903-109">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9903-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="e9903-110">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="e9903-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="e9903-111">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="e9903-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e9903-112">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="e9903-112">Connect to a hub</span></span>

<span data-ttu-id="e9903-113">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="e9903-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="e9903-114">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="e9903-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="e9903-115">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="e9903-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="e9903-116">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="e9903-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="e9903-117">接続が失われたハンドル</span><span class="sxs-lookup"><span data-stu-id="e9903-117">Handle lost connection</span></span>

<span data-ttu-id="e9903-118">使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。</span><span class="sxs-lookup"><span data-stu-id="e9903-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="e9903-119">たとえば、再接続を自動化することがあります。</span><span class="sxs-lookup"><span data-stu-id="e9903-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="e9903-120">`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。</span><span class="sxs-lookup"><span data-stu-id="e9903-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="e9903-121">デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="e9903-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="e9903-122">非同期サポートの主な理由とは、接続を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="e9903-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="e9903-123">非同期アクションは、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="e9903-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="e9903-124">`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。</span><span class="sxs-lookup"><span data-stu-id="e9903-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e9903-125">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="e9903-125">Call hub methods from client</span></span>

<span data-ttu-id="e9903-126">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e9903-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="e9903-127">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="e9903-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="e9903-128">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="e9903-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e9903-129">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e9903-129">Call client methods from hub</span></span>

<span data-ttu-id="e9903-130">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="e9903-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="e9903-131">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e9903-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="e9903-132">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="e9903-132">Error handling and logging</span></span>

<span data-ttu-id="e9903-133">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="e9903-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="e9903-134">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e9903-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="e9903-135">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e9903-135">Additional resources</span></span>

* [<span data-ttu-id="e9903-136">ハブ</span><span class="sxs-lookup"><span data-stu-id="e9903-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e9903-137">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="e9903-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e9903-138">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="e9903-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
