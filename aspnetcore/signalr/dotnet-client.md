---
title: ASP.NET Core SignalR .NET クライアント
author: tdykstra
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655253"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="ac52a-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="ac52a-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="ac52a-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ac52a-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="ac52a-105">ASP.NET Core SignalR .NET クライアントは、Xamarin、WPF、Windows フォーム、コンソール、および .NET Core アプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ac52a-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="ac52a-106">ように、 [JavaScript クライアント](xref:signalr/javascript-client)、.NET クライアントでは、受信、送信し、リアルタイムでハブにメッセージを受信することができます。</span><span class="sxs-lookup"><span data-stu-id="ac52a-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="ac52a-107">Xamarin には、Visual Studio バージョンについての特別な要件があります。</span><span class="sxs-lookup"><span data-stu-id="ac52a-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="ac52a-108">詳細については、次を参照してください。 [Xamarin で SignalR クライアント 2.1.1](https://github.com/aspnet/Announcements/issues/305)します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="ac52a-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ac52a-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ac52a-110">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="ac52a-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="ac52a-111">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ac52a-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="ac52a-112">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="ac52a-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ac52a-113">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="ac52a-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="ac52a-114">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-114">Connect to a hub</span></span>

<span data-ttu-id="ac52a-115">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="ac52a-116">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="ac52a-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="ac52a-117">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="ac52a-118">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ac52a-119">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="ac52a-119">Call hub methods from client</span></span>

<span data-ttu-id="ac52a-120">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="ac52a-121">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="ac52a-122">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="ac52a-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ac52a-123">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="ac52a-123">Call client methods from hub</span></span>

<span data-ttu-id="ac52a-124">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="ac52a-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="ac52a-125">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ac52a-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="ac52a-126">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="ac52a-126">Error handling and logging</span></span>

<span data-ttu-id="ac52a-127">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="ac52a-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="ac52a-128">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ac52a-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="ac52a-129">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ac52a-129">Additional resources</span></span>

* [<span data-ttu-id="ac52a-130">ハブ</span><span class="sxs-lookup"><span data-stu-id="ac52a-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ac52a-131">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="ac52a-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ac52a-132">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="ac52a-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
