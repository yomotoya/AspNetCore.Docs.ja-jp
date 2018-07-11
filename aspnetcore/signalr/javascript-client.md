---
title: ASP.NET Core SignalR JavaScript クライアント
author: rachelappel
description: ASP.NET Core SignalR JavaScript クライアントの概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938045"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="88bc0-103">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="88bc0-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="88bc0-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="88bc0-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="88bc0-105">ASP.NET Core SignalR JavaScript クライアント ライブラリでは、サーバー側ハブのコードを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="88bc0-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="88bc0-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="88bc0-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="88bc0-107">SignalR クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="88bc0-107">Install the SignalR client package</span></span>

<span data-ttu-id="88bc0-108">SignalR JavaScript クライアント ライブラリとして提供される、 [npm](https://www.npmjs.com/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="88bc0-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="88bc0-109">Visual Studio を使用している場合は、実行`npm install`から、**パッケージ マネージャー コンソール**ルート フォルダー内で。</span><span class="sxs-lookup"><span data-stu-id="88bc0-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="88bc0-110">Visual Studio Code でからのコマンドを実行、**統合ターミナル**します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="88bc0-111">Npm のインストール パッケージの内容を*node_modules\\ @aspnet\signalr\dist\browser* フォルダー。</span><span class="sxs-lookup"><span data-stu-id="88bc0-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="88bc0-112">という名前の新しいフォルダーを作成する*signalr*下、 *wwwroot\\lib*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="88bc0-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="88bc0-113">コピー、 *signalr.js*ファイルを*wwwroot\lib\signalr*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="88bc0-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="88bc0-114">SignalR JavaScript クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="88bc0-115">SignalR JavaScript クライアントでの参照、`<script>`要素。</span><span class="sxs-lookup"><span data-stu-id="88bc0-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="88bc0-116">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-116">Connect to a hub</span></span>

<span data-ttu-id="88bc0-117">次のコードでは、作成し、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="88bc0-118">ハブの名前は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="88bc0-119">クロス オリジンの接続</span><span class="sxs-lookup"><span data-stu-id="88bc0-119">Cross-origin connections</span></span>

<span data-ttu-id="88bc0-120">通常、ブラウザーでは、要求されたページと同じドメインから接続を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="88bc0-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="88bc0-121">ただし、状況が別のドメインへの接続が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="88bc0-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="88bc0-122">悪意のあるサイトが別のサイトから機密データを読み取ることを防ぐために[クロス オリジン接続](xref:security/cors)は既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="88bc0-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="88bc0-123">クロス オリジン要求を許可することで有効にする、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="88bc0-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="88bc0-124">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="88bc0-124">Call hub methods from client</span></span>

<span data-ttu-id="88bc0-125">JavaScript クライアントを使用してのハブでのパブリック メソッドを呼び出す`connection.invoke`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="88bc0-126">`invoke`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="88bc0-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="88bc0-127">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="88bc0-127">The name of the hub method.</span></span> <span data-ttu-id="88bc0-128">次の例ではハブ名は`SendMessage`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="88bc0-129">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="88bc0-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="88bc0-130">次の例では引数名は`message`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="88bc0-131">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="88bc0-131">Call client methods from hub</span></span>

<span data-ttu-id="88bc0-132">ハブからメッセージを受信する定義を使用して、メソッド、`connection.on`メソッド。</span><span class="sxs-lookup"><span data-stu-id="88bc0-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="88bc0-133">JavaScript クライアント メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="88bc0-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="88bc0-134">次の例では、メソッド名は`ReceiveMessage`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="88bc0-135">引数は、hub は、メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="88bc0-136">引数の値は、次の例では、`message`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="88bc0-137">上記のコードで`connection.on`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="88bc0-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="88bc0-138">SignalR を呼び出すメソッド名を照合することによってクライアントの方法を指定してで定義されている引数`SendAsync`と`connection.on`します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="88bc0-139">ベスト プラクティスとして呼び出す`connection.start`後`connection.on`のため、すべてのメッセージを受信する前に、ハンドラーが登録されます。</span><span class="sxs-lookup"><span data-stu-id="88bc0-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="88bc0-140">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="88bc0-140">Error handling and logging</span></span>

<span data-ttu-id="88bc0-141">チェーンを`catch`メソッドの末尾に、`connection.start`クライアント側のエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="88bc0-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="88bc0-142">使用`console.error`ブラウザーのコンソールにエラーを出力します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="88bc0-143">接続が行われたときにログに記録するには、logger とイベントの種類を渡すことによって、クライアント側のログ トレースをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="88bc0-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="88bc0-144">指定されたログ レベル以降、メッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="88bc0-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="88bc0-145">使用可能なログ レベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="88bc0-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="88bc0-146">`signalR.LogLevel.Error` : エラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="88bc0-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="88bc0-147">ログ`Error`メッセージのみです。</span><span class="sxs-lookup"><span data-stu-id="88bc0-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="88bc0-148">`signalR.LogLevel.Warning` : 潜在的なエラーに関する警告メッセージ。</span><span class="sxs-lookup"><span data-stu-id="88bc0-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="88bc0-149">ログ`Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="88bc0-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="88bc0-150">`signalR.LogLevel.Information` : エラー ステータス メッセージです。</span><span class="sxs-lookup"><span data-stu-id="88bc0-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="88bc0-151">ログ`Information`、 `Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="88bc0-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="88bc0-152">`signalR.LogLevel.Trace` : メッセージをトレースします。</span><span class="sxs-lookup"><span data-stu-id="88bc0-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="88bc0-153">ハブとクライアント間で転送されるデータを含め、すべてログに記録します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="88bc0-154">使用して、`configureLogging`メソッド`HubConnectionBuilder`ログ レベルを構成します。</span><span class="sxs-lookup"><span data-stu-id="88bc0-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="88bc0-155">メッセージは、ブラウザーのコンソールに記録されます。</span><span class="sxs-lookup"><span data-stu-id="88bc0-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="88bc0-156">関連資料</span><span class="sxs-lookup"><span data-stu-id="88bc0-156">Related resources</span></span>

* [<span data-ttu-id="88bc0-157">ハブ</span><span class="sxs-lookup"><span data-stu-id="88bc0-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="88bc0-158">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="88bc0-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="88bc0-159">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="88bc0-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="88bc0-160">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="88bc0-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
