---
title: ASP.NET Core SignalR JavaScript クライアント
author: tdykstra
description: ASP.NET Core SignalR JavaScript クライアントの概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 02844c35d1933d36576c25ff335a572fb65eff5c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50208019"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="c2849-103">ASP.NET Core SignalR JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="c2849-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="c2849-104">作成者: [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c2849-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c2849-105">ASP.NET Core SignalR JavaScript クライアント ライブラリでは、サーバー側ハブのコードを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="c2849-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="c2849-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c2849-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="c2849-107">SignalR クライアント パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c2849-107">Install the SignalR client package</span></span>

<span data-ttu-id="c2849-108">SignalR JavaScript クライアント ライブラリとして提供される、 [npm](https://www.npmjs.com/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="c2849-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="c2849-109">Visual Studio を使用している場合は、実行`npm install`から、**パッケージ マネージャー コンソール**ルート フォルダー内で。</span><span class="sxs-lookup"><span data-stu-id="c2849-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="c2849-110">Visual Studio Code でからのコマンドを実行、**統合ターミナル**します。</span><span class="sxs-lookup"><span data-stu-id="c2849-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="c2849-111">npm のインストール パッケージの内容を *node_modules\\@aspnet\signalr\dist\browser* フォルダー。</span><span class="sxs-lookup"><span data-stu-id="c2849-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="c2849-112">という名前の新しいフォルダーを作成する*signalr*下、 *wwwroot\\lib*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="c2849-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="c2849-113">コピー、 *signalr.js*ファイルを*wwwroot\lib\signalr*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="c2849-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="c2849-114">SignalR JavaScript クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2849-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="c2849-115">SignalR JavaScript クライアントでの参照、`<script>`要素。</span><span class="sxs-lookup"><span data-stu-id="c2849-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c2849-116">ハブへの接続します。</span><span class="sxs-lookup"><span data-stu-id="c2849-116">Connect to a hub</span></span>

<span data-ttu-id="c2849-117">次のコードでは、作成し、接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="c2849-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="c2849-118">ハブの名前は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="c2849-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="c2849-119">クロス オリジンの接続</span><span class="sxs-lookup"><span data-stu-id="c2849-119">Cross-origin connections</span></span>

<span data-ttu-id="c2849-120">通常、ブラウザーでは、要求されたページと同じドメインから接続を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="c2849-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="c2849-121">ただし、状況が別のドメインへの接続が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="c2849-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="c2849-122">悪意のあるサイトが別のサイトから機密データを読み取ることを防ぐために[クロス オリジン接続](xref:security/cors)は既定で無効になります。</span><span class="sxs-lookup"><span data-stu-id="c2849-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="c2849-123">クロス オリジン要求を許可することで有効にする、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="c2849-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c2849-124">クライアントからのハブ メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="c2849-124">Call hub methods from client</span></span>

<span data-ttu-id="c2849-125">JavaScript クライアントは、ハブ経由でのパブリック メソッドを呼び出して、[呼び出す](/javascript/api/%40aspnet/signalr/hubconnection#invoke)のメソッド、 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)します。</span><span class="sxs-lookup"><span data-stu-id="c2849-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="c2849-126">`invoke`メソッドは 2 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="c2849-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="c2849-127">ハブ メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="c2849-127">The name of the hub method.</span></span> <span data-ttu-id="c2849-128">次の例では、ハブのメソッド名は`SendMessage`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="c2849-129">ハブ メソッドで定義されている引数。</span><span class="sxs-lookup"><span data-stu-id="c2849-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="c2849-130">次の例では引数名は`message`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c2849-131">ハブからのクライアント メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="c2849-131">Call client methods from hub</span></span>

<span data-ttu-id="c2849-132">ハブからメッセージを受信する定義を使用して、メソッド、[で](/javascript/api/%40aspnet/signalr/hubconnection#on)のメソッド、`HubConnection`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="c2849-133">JavaScript クライアント メソッドの名前。</span><span class="sxs-lookup"><span data-stu-id="c2849-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="c2849-134">次の例では、メソッド名は`ReceiveMessage`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="c2849-135">引数は、hub は、メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="c2849-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="c2849-136">引数の値は、次の例では、`message`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="c2849-137">上記のコードで`connection.on`を使用してサーバー側コードから呼び出すときに実行される、 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)メソッド。</span><span class="sxs-lookup"><span data-stu-id="c2849-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="c2849-138">SignalR を呼び出すメソッド名を照合することによってクライアントの方法を指定してで定義されている引数`SendAsync`と`connection.on`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="c2849-139">ベスト プラクティスを呼び出して、[開始](/javascript/api/%40aspnet/signalr/hubconnection#start)メソッドを`HubConnection`後`on`します。</span><span class="sxs-lookup"><span data-stu-id="c2849-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="c2849-140">これにより、すべてのメッセージを受信する前に、ハンドラーが登録されます。</span><span class="sxs-lookup"><span data-stu-id="c2849-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="c2849-141">エラー処理とログ記録</span><span class="sxs-lookup"><span data-stu-id="c2849-141">Error handling and logging</span></span>

<span data-ttu-id="c2849-142">チェーンを`catch`メソッドの末尾に、`start`クライアント側のエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="c2849-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="c2849-143">使用`console.error`ブラウザーのコンソールにエラーを出力します。</span><span class="sxs-lookup"><span data-stu-id="c2849-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="c2849-144">接続が行われたときにログに記録するには、logger とイベントの種類を渡すことによって、クライアント側のログ トレースをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="c2849-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="c2849-145">指定されたログ レベル以降、メッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="c2849-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="c2849-146">使用可能なログ レベルは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c2849-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="c2849-147">`signalR.LogLevel.Error` &ndash; エラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="c2849-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="c2849-148">ログ`Error`メッセージのみです。</span><span class="sxs-lookup"><span data-stu-id="c2849-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="c2849-149">`signalR.LogLevel.Warning` &ndash; 可能性のあるエラーについての警告メッセージ。</span><span class="sxs-lookup"><span data-stu-id="c2849-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="c2849-150">ログ`Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="c2849-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c2849-151">`signalR.LogLevel.Information` &ndash; ステータス メッセージがエラーなし。</span><span class="sxs-lookup"><span data-stu-id="c2849-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="c2849-152">ログ`Information`、 `Warning`、および`Error`メッセージ。</span><span class="sxs-lookup"><span data-stu-id="c2849-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c2849-153">`signalR.LogLevel.Trace` &ndash; メッセージをトレースします。</span><span class="sxs-lookup"><span data-stu-id="c2849-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="c2849-154">ハブとクライアント間で転送されるデータを含め、すべてログに記録します。</span><span class="sxs-lookup"><span data-stu-id="c2849-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="c2849-155">使用して、 [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)メソッド[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)ログ レベルを構成します。</span><span class="sxs-lookup"><span data-stu-id="c2849-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="c2849-156">メッセージは、ブラウザーのコンソールに記録されます。</span><span class="sxs-lookup"><span data-stu-id="c2849-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a><span data-ttu-id="c2849-157">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c2849-157">Additional resources</span></span>

* [<span data-ttu-id="c2849-158">JavaScript API リファレンス</span><span class="sxs-lookup"><span data-stu-id="c2849-158">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="c2849-159">ハブ</span><span class="sxs-lookup"><span data-stu-id="c2849-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c2849-160">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="c2849-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c2849-161">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="c2849-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c2849-162">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c2849-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
