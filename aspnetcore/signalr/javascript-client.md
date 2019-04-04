---
title: ASP.NET Core SignalR JavaScript クライアント
author: bradygaster
description: ASP.NET Core SignalR JavaScript クライアントの概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/javascript-client
ms.openlocfilehash: a0980dca2eb8d483a9d9f1c5667fb74ee06364f0
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978343"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="13b9b-103">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="13b9b-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="13b9b-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="13b9b-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="13b9b-105">ASP.NET Core SignalR JavaScript クライアント ライブラリでは、サーバー側ハブのコードを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="13b9b-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="13b9b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="13b9b-107">SignalR クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="13b9b-107">Install the SignalR client package</span></span>

<span data-ttu-id="13b9b-108">SignalR JavaScript クライアント ライブラリとして提供される、 [npm](https://www.npmjs.com/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="13b9b-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="13b9b-109">Visual Studio を使用している場合は、実行`npm install`から、**パッケージ マネージャー コンソール**ルート フォルダー内で。</span><span class="sxs-lookup"><span data-stu-id="13b9b-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="13b9b-110">Visual Studio Code でからのコマンドを実行、**統合ターミナル**します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="13b9b-111">npm のインストール パッケージの内容を *node_modules\\@aspnet\signalr\dist\browser* フォルダー。</span><span class="sxs-lookup"><span data-stu-id="13b9b-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="13b9b-112">という名前の新しいフォルダーを作成する*signalr*下、 *wwwroot\\lib*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="13b9b-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="13b9b-113">コピー、 *signalr.js*ファイルを*wwwroot\lib\signalr*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="13b9b-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="13b9b-114">SignalR JavaScript クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="13b9b-115">SignalR JavaScript クライアントでの参照、`<script>`要素。</span><span class="sxs-lookup"><span data-stu-id="13b9b-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="13b9b-116">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-116">Connect to a hub</span></span>

<span data-ttu-id="13b9b-117">次のコードでは、作成し、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="13b9b-118">ハブの名前は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="13b9b-119">クロス オリジンの接続</span><span class="sxs-lookup"><span data-stu-id="13b9b-119">Cross-origin connections</span></span>

<span data-ttu-id="13b9b-120">通常、ブラウザーでは、要求されたページと同じドメインから接続を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="13b9b-121">ただし、状況が別のドメインへの接続が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="13b9b-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="13b9b-122">悪意のあるサイトが別のサイトから機密データを読み取ることを防ぐために[クロス オリジン接続](xref:security/cors)は既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="13b9b-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="13b9b-123">クロス オリジン要求を許可することで有効にする、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="13b9b-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="13b9b-124">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="13b9b-124">Call hub methods from client</span></span>

<span data-ttu-id="13b9b-125">JavaScript クライアントは、ハブ経由でのパブリック メソッドを呼び出して、[呼び出す](/javascript/api/%40aspnet/signalr/hubconnection#invoke)のメソッド、 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="13b9b-126">`invoke`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="13b9b-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="13b9b-127">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="13b9b-127">The name of the hub method.</span></span> <span data-ttu-id="13b9b-128">次の例では、ハブのメソッド名は`SendMessage`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="13b9b-129">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="13b9b-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="13b9b-130">次の例では引数名は`message`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="13b9b-131">コード例では、現在のバージョンの Internet Explorer を除くすべての主要ブラウザーでサポートされている矢印関数の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="13b9b-132">Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="13b9b-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="13b9b-133">詳細については、、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="13b9b-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="13b9b-134">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="13b9b-134">Call client methods from hub</span></span>

<span data-ttu-id="13b9b-135">ハブからメッセージを受信する定義を使用して、メソッド、[で](/javascript/api/%40aspnet/signalr/hubconnection#on)のメソッド、`HubConnection`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-135">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="13b9b-136">JavaScript クライアント メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="13b9b-136">The name of the JavaScript client method.</span></span> <span data-ttu-id="13b9b-137">次の例では、メソッド名は`ReceiveMessage`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-137">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="13b9b-138">引数は、hub は、メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-138">Arguments the hub passes to the method.</span></span> <span data-ttu-id="13b9b-139">引数の値は、次の例では、`message`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-139">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="13b9b-140">上記のコードで`connection.on`を使用してサーバー側コードから呼び出すときに実行される、 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)メソッド。</span><span class="sxs-lookup"><span data-stu-id="13b9b-140">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="13b9b-141">SignalR を呼び出すメソッド名を照合することによってクライアントの方法を指定してで定義されている引数`SendAsync`と`connection.on`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-141">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="13b9b-142">ベスト プラクティスを呼び出して、[開始](/javascript/api/%40aspnet/signalr/hubconnection#start)メソッドを`HubConnection`後`on`します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-142">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="13b9b-143">これにより、すべてのメッセージを受信する前に、ハンドラーが登録されます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-143">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="13b9b-144">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="13b9b-144">Error handling and logging</span></span>

<span data-ttu-id="13b9b-145">チェーンを`catch`メソッドの末尾に、`start`クライアント側のエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="13b9b-145">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="13b9b-146">使用`console.error`ブラウザーのコンソールにエラーを出力します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-146">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="13b9b-147">接続が行われたときにログに記録するには、logger とイベントの種類を渡すことによって、クライアント側のログ トレースをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="13b9b-147">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="13b9b-148">指定されたログ レベル以降、メッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-148">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="13b9b-149">使用可能なログ レベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="13b9b-149">Available log levels are as follows:</span></span>

* <span data-ttu-id="13b9b-150">`signalR.LogLevel.Error` &ndash; エラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="13b9b-150">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="13b9b-151">ログ`Error`メッセージのみです。</span><span class="sxs-lookup"><span data-stu-id="13b9b-151">Logs `Error` messages only.</span></span>
* <span data-ttu-id="13b9b-152">`signalR.LogLevel.Warning` &ndash; 可能性のあるエラーについての警告メッセージ。</span><span class="sxs-lookup"><span data-stu-id="13b9b-152">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="13b9b-153">ログ`Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="13b9b-153">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="13b9b-154">`signalR.LogLevel.Information` &ndash; ステータス メッセージがエラーなし。</span><span class="sxs-lookup"><span data-stu-id="13b9b-154">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="13b9b-155">ログ`Information`、 `Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="13b9b-155">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="13b9b-156">`signalR.LogLevel.Trace` &ndash; メッセージをトレースします。</span><span class="sxs-lookup"><span data-stu-id="13b9b-156">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="13b9b-157">ハブとクライアント間で転送されるデータを含め、すべてログに記録します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-157">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="13b9b-158">使用して、 [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)メソッド[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)ログ レベルを構成します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-158">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="13b9b-159">メッセージは、ブラウザーのコンソールに記録されます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-159">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="13b9b-160">クライアントを再接続します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-160">Reconnect clients</span></span>

<span data-ttu-id="13b9b-161">SignalR JavaScript クライアントは自動的に再接続しません。</span><span class="sxs-lookup"><span data-stu-id="13b9b-161">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="13b9b-162">クライアントを手動で再接続するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="13b9b-162">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="13b9b-163">次のコードでは、再接続の一般的なアプローチを示します。</span><span class="sxs-lookup"><span data-stu-id="13b9b-163">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="13b9b-164">関数 (ここで、`start`関数)、接続を開始するが作成されます。</span><span class="sxs-lookup"><span data-stu-id="13b9b-164">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="13b9b-165">呼び出す、`start`関数で、接続の`onclose`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="13b9b-165">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="13b9b-166">実際の実装は、指数バックオフを使用して、または断念する前に指定された回数を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="13b9b-166">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="13b9b-167">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="13b9b-167">Additional resources</span></span>

* [<span data-ttu-id="13b9b-168">JavaScript API リファレンス</span><span class="sxs-lookup"><span data-stu-id="13b9b-168">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="13b9b-169">JavaScript のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="13b9b-169">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="13b9b-170">WebPack と TypeScript のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="13b9b-170">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="13b9b-171">ハブ</span><span class="sxs-lookup"><span data-stu-id="13b9b-171">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="13b9b-172">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="13b9b-172">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="13b9b-173">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="13b9b-173">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="13b9b-174">クロス オリジン要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="13b9b-174">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="13b9b-175">Azure SignalR サービスのサーバーレス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="13b9b-175">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
