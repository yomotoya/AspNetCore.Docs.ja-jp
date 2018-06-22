---
title: ASP.NET Core SignalR の .NET クライアント
author: rachelappel
description: ASP.NET Core SignalR の .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273296"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="b99c9-103">ASP.NET Core SignalR の .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="b99c9-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="b99c9-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b99c9-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b99c9-105">ASP.NET Core SignalR の .NET クライアントは、Xamarin、WPF、Windows フォーム、コンソール、および .NET Core のアプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="b99c9-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="b99c9-106">同様に、 [JavaScript クライアント](xref:signalr/javascript-client)、.NET クライアントでは、受信、送信し、リアルタイムでハブにメッセージを受信することができます。</span><span class="sxs-lookup"><span data-stu-id="b99c9-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="b99c9-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b99c9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b99c9-108">この記事のサンプル コードは、ASP.NET Core SignalR の .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="b99c9-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="b99c9-109">SignalR の .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="b99c9-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="b99c9-110">`Microsoft.AspNetCore.SignalR.Client`パッケージは、.NET クライアントの SignalR ハブに接続するために必要です。</span><span class="sxs-lookup"><span data-stu-id="b99c9-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="b99c9-111">クライアント ライブラリをインストールする、次のコマンドを実行、 **Package Manager Console**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="b99c9-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b99c9-112">ハブに接続します。</span><span class="sxs-lookup"><span data-stu-id="b99c9-112">Connect to a hub</span></span>

<span data-ttu-id="b99c9-113">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`です。</span><span class="sxs-lookup"><span data-stu-id="b99c9-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="b99c9-114">接続の構築中には、ハブ URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="b99c9-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="b99c9-115">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`です。</span><span class="sxs-lookup"><span data-stu-id="b99c9-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="b99c9-116">接続を開始`StartAsync`です。</span><span class="sxs-lookup"><span data-stu-id="b99c9-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b99c9-117">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="b99c9-117">Call hub methods from client</span></span>

<span data-ttu-id="b99c9-118">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b99c9-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="b99c9-119">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`です。</span><span class="sxs-lookup"><span data-stu-id="b99c9-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="b99c9-120">SignalR は非同期ので、使用`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="b99c9-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b99c9-121">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b99c9-121">Call client methods from hub</span></span>

<span data-ttu-id="b99c9-122">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前にビルドが終了後します。</span><span class="sxs-lookup"><span data-stu-id="b99c9-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="b99c9-123">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行、`SendAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b99c9-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="b99c9-124">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="b99c9-124">Error handling and logging</span></span>

<span data-ttu-id="b99c9-125">Try-catch ステートメントによるエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="b99c9-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="b99c9-126">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b99c9-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="b99c9-127">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b99c9-127">Additional resources</span></span>

* [<span data-ttu-id="b99c9-128">ハブ</span><span class="sxs-lookup"><span data-stu-id="b99c9-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b99c9-129">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="b99c9-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b99c9-130">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="b99c9-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)