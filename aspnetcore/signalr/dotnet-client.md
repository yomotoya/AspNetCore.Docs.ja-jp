---
title: ASP.NET Core SignalR .NET クライアント
author: rachelappel
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 35dc1d3abf0d35e17d1835ec462f8cc4feb728eb
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938255"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c9d96-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="c9d96-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c9d96-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c9d96-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c9d96-105">ASP.NET Core SignalR .NET クライアントは、Xamarin、WPF、Windows フォーム、コンソール、および .NET Core アプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="c9d96-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="c9d96-106">ように、 [JavaScript クライアント](xref:signalr/javascript-client)、.NET クライアントでは、受信、送信し、リアルタイムでハブにメッセージを受信することができます。</span><span class="sxs-lookup"><span data-stu-id="c9d96-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="c9d96-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c9d96-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c9d96-108">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="c9d96-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c9d96-109">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c9d96-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c9d96-110">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="c9d96-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c9d96-111">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="c9d96-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c9d96-112">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-112">Connect to a hub</span></span>

<span data-ttu-id="c9d96-113">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c9d96-114">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="c9d96-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c9d96-115">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c9d96-116">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c9d96-117">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="c9d96-117">Call hub methods from client</span></span>

<span data-ttu-id="c9d96-118">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c9d96-119">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c9d96-120">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="c9d96-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c9d96-121">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="c9d96-121">Call client methods from hub</span></span>

<span data-ttu-id="c9d96-122">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="c9d96-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="c9d96-123">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c9d96-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c9d96-124">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="c9d96-124">Error handling and logging</span></span>

<span data-ttu-id="c9d96-125">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="c9d96-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c9d96-126">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c9d96-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="c9d96-127">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c9d96-127">Additional resources</span></span>

* [<span data-ttu-id="c9d96-128">ハブ</span><span class="sxs-lookup"><span data-stu-id="c9d96-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c9d96-129">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="c9d96-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c9d96-130">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="c9d96-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
