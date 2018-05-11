---
title: ASP.NET Core SignalR JavaScript クライアント
author: rachelappel
description: ASP.NET Core SignalR JavaScript クライアントの概要です。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 1701d9ac5222bf64f9690c1cecdf54ef95fe4a49
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="9f472-103">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="9f472-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="9f472-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9f472-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9f472-105">ASP.NET Core SignalR JavaScript クライアント ライブラリをサーバー側ハブのコードを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="9f472-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="9f472-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="9f472-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="9f472-107">SignalR クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9f472-107">Install the SignalR client package</span></span>

<span data-ttu-id="9f472-108">SignalR JavaScript クライアント ライブラリとして提供される、 [npm](https://www.npmjs.com/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="9f472-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="9f472-109">Visual Studio を使用している場合は、実行`npm install`から、 **Package Manager Console**ルート フォルダーの中にします。</span><span class="sxs-lookup"><span data-stu-id="9f472-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="9f472-110">Visual Studio のコードからコマンドを実行、**統合ターミナル**です。</span><span class="sxs-lookup"><span data-stu-id="9f472-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="9f472-111">Npm インストールで、パッケージのコンテンツ、 *node_modules\\ @aspnet\signalr\dist\browser* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f472-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="9f472-112">という名前の新しいフォルダーを作成する*signalr*下にある、 *wwwroot\\lib*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f472-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="9f472-113">コピー、 *signalr.js*ファイルの名前を*wwwroot\lib\signalr*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f472-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="9f472-114">SignalR JavaScript クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="9f472-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="9f472-115">SignalR JavaScript クライアントでの参照、`<script>`要素。</span><span class="sxs-lookup"><span data-stu-id="9f472-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="9f472-116">ハブに接続します。</span><span class="sxs-lookup"><span data-stu-id="9f472-116">Connect to a hub</span></span>

<span data-ttu-id="9f472-117">次のコードでは、作成し、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="9f472-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="9f472-118">ハブの名前は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="9f472-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="9f472-119">クロス オリジンの接続</span><span class="sxs-lookup"><span data-stu-id="9f472-119">Cross-origin connections</span></span>

<span data-ttu-id="9f472-120">通常、ブラウザーは要求されたページと同じドメインから接続を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="9f472-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="9f472-121">ただし、別のドメインへの接続が必要な場合の機会があります。</span><span class="sxs-lookup"><span data-stu-id="9f472-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="9f472-122">悪意のあるサイトが別のサイトから機密データを読み取ることを防ぐため[クロス オリジン接続](xref:security/cors)既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="9f472-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="9f472-123">クロス オリジン要求を許可するには、有効にする必要で、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="9f472-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="9f472-124">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="9f472-124">Call hub methods from client</span></span>

<span data-ttu-id="9f472-125">JavaScript クライアント パブリック メソッドの呼び出しによってハブを使用して`connection.invoke`です。</span><span class="sxs-lookup"><span data-stu-id="9f472-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="9f472-126">`invoke`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="9f472-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="9f472-127">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="9f472-127">The name of the hub method.</span></span> <span data-ttu-id="9f472-128">次の例ではハブ名は`SendMessage`します。</span><span class="sxs-lookup"><span data-stu-id="9f472-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="9f472-129">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="9f472-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="9f472-130">次の例では引数名は`message`します。</span><span class="sxs-lookup"><span data-stu-id="9f472-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="9f472-131">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="9f472-131">Call client methods from hub</span></span>

<span data-ttu-id="9f472-132">メッセージを受信するハブから、定義を使用して、メソッド、`connection.on`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9f472-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="9f472-133">JavaScript クライアント メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="9f472-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="9f472-134">次の例ではメソッド名は`ReceiveMessage`します。</span><span class="sxs-lookup"><span data-stu-id="9f472-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="9f472-135">ハブがメソッドに渡す引数。</span><span class="sxs-lookup"><span data-stu-id="9f472-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="9f472-136">引数の値は、次の例では、`message`です。</span><span class="sxs-lookup"><span data-stu-id="9f472-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="9f472-137">上記のコードで`connection.on`を使用してサーバー側コードから呼び出すときに実行、`SendAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9f472-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="9f472-138">SignalR がどのクライアント メソッドをメソッドの名前を照合することによって呼び出すを決定しで定義されている引数`SendAsync`と`connection.on`です。</span><span class="sxs-lookup"><span data-stu-id="9f472-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="9f472-139">ベスト プラクティスとして呼び出す`connection.start`後`connection.on`のため、すべてのメッセージを受信する前に、ハンドラーが登録されています。</span><span class="sxs-lookup"><span data-stu-id="9f472-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="9f472-140">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="9f472-140">Error handling and logging</span></span>

<span data-ttu-id="9f472-141">チェーン、`catch`メソッドの末尾に、`connection.start`クライアント側のエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="9f472-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="9f472-142">使用して`console.error`ブラウザーのコンソールにエラーを出力します。</span><span class="sxs-lookup"><span data-stu-id="9f472-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="9f472-143">接続が行われたときにログに記録する logger とイベントの種類を渡すことによって、クライアント側のログのトレースを設定します。</span><span class="sxs-lookup"><span data-stu-id="9f472-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="9f472-144">メッセージは、指定されたログのレベル以降に記録されます。</span><span class="sxs-lookup"><span data-stu-id="9f472-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="9f472-145">使用可能なログ レベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9f472-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="9f472-146">`signalR.LogLevel.Error` : エラー メッセージです。</span><span class="sxs-lookup"><span data-stu-id="9f472-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="9f472-147">ログ`Error`メッセージのみです。</span><span class="sxs-lookup"><span data-stu-id="9f472-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="9f472-148">`signalR.LogLevel.Warning` : 可能性のあるエラーについての警告メッセージ。</span><span class="sxs-lookup"><span data-stu-id="9f472-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="9f472-149">ログ`Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="9f472-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9f472-150">`signalR.LogLevel.Information` : エラー ステータス メッセージ。</span><span class="sxs-lookup"><span data-stu-id="9f472-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="9f472-151">ログ`Information`、 `Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="9f472-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9f472-152">`signalR.LogLevel.Trace` : メッセージをトレースします。</span><span class="sxs-lookup"><span data-stu-id="9f472-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="9f472-153">ハブとクライアント間で転送されるデータを含むすべてのログに記録します。</span><span class="sxs-lookup"><span data-stu-id="9f472-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="9f472-154">使用して、`configureLogging`メソッド`HubConnectionBuilder`ログ レベルを構成します。</span><span class="sxs-lookup"><span data-stu-id="9f472-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="9f472-155">メッセージは、ブラウザーのコンソールに記録されます。</span><span class="sxs-lookup"><span data-stu-id="9f472-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="9f472-156">関連資料</span><span class="sxs-lookup"><span data-stu-id="9f472-156">Related resources</span></span>

* [<span data-ttu-id="9f472-157">ASP.NET Core SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="9f472-157">ASP.NET Core SignalR Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9f472-158">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="9f472-158">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)