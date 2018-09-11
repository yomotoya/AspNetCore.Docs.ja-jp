---
title: ASP.NET Core SignalR .NET クライアント
author: tdykstra
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373320"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="f1a4b-103">ASP.NET Core SignalR .NET クライアント</span><span class="sxs-lookup"><span data-stu-id="f1a4b-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="f1a4b-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f1a4b-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f1a4b-105">ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="f1a4b-106">Xamarin には、Visual Studio バージョンについての特別な要件があります。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="f1a4b-107">詳細については、次を参照してください。 [Xamarin で SignalR クライアント 2.1.1](https://github.com/aspnet/Announcements/issues/305)します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="f1a4b-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f1a4b-109">この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="f1a4b-110">SignalR .NET クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="f1a4b-111">`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="f1a4b-112">クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="f1a4b-113">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-113">Connect to a hub</span></span>

<span data-ttu-id="f1a4b-114">接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="f1a4b-115">接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="f1a4b-116">挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="f1a4b-117">接続を開始`StartAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="f1a4b-118">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="f1a4b-118">Call hub methods from client</span></span>

<span data-ttu-id="f1a4b-119">`InvokeAsync` ハブのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="f1a4b-120">ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="f1a4b-121">SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="f1a4b-122">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="f1a4b-122">Call client methods from hub</span></span>

<span data-ttu-id="f1a4b-123">ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="f1a4b-124">上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="f1a4b-125">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="f1a4b-125">Error handling and logging</span></span>

<span data-ttu-id="f1a4b-126">Try-catch ステートメントでエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="f1a4b-127">検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f1a4b-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="f1a4b-128">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f1a4b-128">Additional resources</span></span>

* [<span data-ttu-id="f1a4b-129">ハブ</span><span class="sxs-lookup"><span data-stu-id="f1a4b-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f1a4b-130">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="f1a4b-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="f1a4b-131">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="f1a4b-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
