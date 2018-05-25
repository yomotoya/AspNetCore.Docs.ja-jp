---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: SignalR のトラブルシューティング |Microsoft ドキュメント
author: pfletcher
description: この記事では、SignalR アプリケーションを開発の一般的な問題について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 2394ee81f4592417a034e47db6eefd3e4b91a9af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-troubleshooting"></a><span data-ttu-id="8bc88-103">SignalR のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="8bc88-103">SignalR Troubleshooting</span></span>
====================
<span data-ttu-id="8bc88-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8bc88-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="8bc88-105">このドキュメントでは、SignalR の一般的な問題のトラブルシューティングについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-105">This document describes common troubleshooting issues with SignalR.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8bc88-106">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="8bc88-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="8bc88-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8bc88-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8bc88-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8bc88-108">.NET 4.5</span></span>
> - <span data-ttu-id="8bc88-109">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="8bc88-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8bc88-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="8bc88-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="8bc88-111">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8bc88-112">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="8bc88-112">Questions and comments</span></span>
> 
> <span data-ttu-id="8bc88-113">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="8bc88-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8bc88-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="8bc88-115">このドキュメントには、次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8bc88-115">This document contains the following sections.</span></span>

- [<span data-ttu-id="8bc88-116">サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="8bc88-116">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="8bc88-117">Ping/pong を停止しているクライアントを検出する IIS websocket を構成します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-117">Configuring IIS websockets to ping/pong to detect a dead client</span></span>](#pong)
- [<span data-ttu-id="8bc88-118">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-118">Other connection issues</span></span>](#other)
- [<span data-ttu-id="8bc88-119">コンパイル、およびサーバー側エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-119">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="8bc88-120">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-120">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="8bc88-121">インターネット インフォメーション サービスの問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-121">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="8bc88-122">Microsoft Azure を発行します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-122">Microsoft Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="8bc88-123">サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="8bc88-123">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="8bc88-124">このセクションでは、メソッドの呼び出しがわかりやすいエラー メッセージを表示せずに失敗するには、クライアントとサーバー間の考えられる原因について説明します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-124">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="8bc88-125">SignalR のアプリケーションで、サーバー情報がありません。 クライアントを実装する方法の詳細サーバーでは、クライアント メソッドを呼び出すとメソッドの名前とパラメーターのデータは、クライアントに送信されます、サーバーが指定した形式で存在する場合にのみ、メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-125">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="8bc88-126">一致するメソッドが検出されないクライアントでは、何も起こりません、サーバー上のエラー メッセージは発生しません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-126">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="8bc88-127">が呼び出されたクライアント メソッドをさらに調査するには、どのような呼び出しを表示するハブの start メソッドが、サーバーから送信されて呼び出しの前にログ記録をオンにすることができません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-127">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="8bc88-128">JavaScript アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (JavaScript クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-128">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="8bc88-129">.NET クライアント アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (.NET クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-net-client.md#logging)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-129">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="8bc88-130">スペル ミスがあるメソッド、不適切なメソッドのシグネチャまたは不適切なハブ名</span><span class="sxs-lookup"><span data-stu-id="8bc88-130">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="8bc88-131">名前または呼び出されたメソッドのシグネチャが一致しない場合、クライアントに適切なメソッド、呼び出しは失敗します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-131">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="8bc88-132">サーバーによって呼び出されるメソッドの名前が、クライアントのメソッドの名前と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-132">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="8bc88-133">SignalR による camel 形式のメソッドを使用するハブ プロキシの作成も、JavaScript では、そのため、メソッドと呼ばれます`SendMessage`サーバーでが呼び出されます`sendMessage`クライアント プロキシでします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-133">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="8bc88-134">使用する場合、`HubName`サーバー側コードで属性を使用する名前が、クライアントで、ハブの作成に使用する名前と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-134">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="8bc88-135">使用しない場合、`HubName`属性、JavaScript クライアントのハブの名前が camel 形式の ChatHub ではなく chatHub などであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-135">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="8bc88-136">クライアント上の重複したメソッド名</span><span class="sxs-lookup"><span data-stu-id="8bc88-136">Duplicate method name on client</span></span>

<span data-ttu-id="8bc88-137">大文字小文字によってのみとは異なるクライアントのメソッドの重複がないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-137">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="8bc88-138">場合は、クライアント アプリケーションがある呼び出されるメソッド`sendMessage`、いないという名前のメソッドもを確認して`SendMessage`もします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-138">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="8bc88-139">クライアントで JSON パーサーが不足しています。</span><span class="sxs-lookup"><span data-stu-id="8bc88-139">Missing JSON parser on the client</span></span>

<span data-ttu-id="8bc88-140">SignalR には、JSON パーサーは、サーバーとクライアント間の呼び出しをシリアル化に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-140">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="8bc88-141">クライアントには、Internet Explorer 7) などの組み込み JSON パーサーが割り当てられていないは、アプリケーションで 1 つ含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-141">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="8bc88-142">JSON パーサーをダウンロードする[ここ](http://nuget.org/packages/json2)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-142">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="8bc88-143">ハブと PersistentConnection 構文を混在させる</span><span class="sxs-lookup"><span data-stu-id="8bc88-143">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="8bc88-144">SignalR が 2 つの通信モデルを使用します。 ハブおよび PersistentConnections です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-144">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="8bc88-145">これら 2 つの通信のモデルを呼び出すための構文は、クライアント コードで異なります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-145">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="8bc88-146">サーバー コードに、ハブを追加した場合は、適切なハブの構文を使用してすべてのクライアント コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-146">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="8bc88-147">**JavaScript クライアント コード、JavaScript クライアントで、PersistentConnection を作成します。**</span><span class="sxs-lookup"><span data-stu-id="8bc88-147">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="8bc88-148">**JavaScript クライアント コードで、Javascript クライアント ハブ プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="8bc88-148">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="8bc88-149">**C# サーバー コード、PersistentConnection にルートをマップします。**</span><span class="sxs-lookup"><span data-stu-id="8bc88-149">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="8bc88-150">**複数のアプリケーションがある場合、ハブ、または複数のハブにルートをマップ c# サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="8bc88-150">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="8bc88-151">サブスクリプションが追加される前に開始した接続</span><span class="sxs-lookup"><span data-stu-id="8bc88-151">Connection started before subscriptions are added</span></span>

<span data-ttu-id="8bc88-152">プロキシ サーバーから呼び出すことができるメソッドが追加される前に、ハブの接続が開始されている場合、メッセージは受信されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-152">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="8bc88-153">次の JavaScript コードは、ハブを正しく開始できません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-153">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="8bc88-154">**ハブのメッセージを受信することはできません。 JavaScript クライアント コードが正しくないです。**</span><span class="sxs-lookup"><span data-stu-id="8bc88-154">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="8bc88-155">代わりに、開始を呼び出す前に、メソッドのサブスクリプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-155">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="8bc88-156">**正しくハブにサブスクリプションを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="8bc88-156">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="8bc88-157">ハブ プロキシのメソッド名がありません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-157">Missing method name on the hub proxy</span></span>

<span data-ttu-id="8bc88-158">クライアントで、サーバーで定義されているメソッドにサブスクライブを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-158">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="8bc88-159">メソッドを定義して、サーバー、場合でも、クライアント プロキシを引き続き追加できます必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-159">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="8bc88-160">次の方法でクライアントのプロキシ メソッドを追加することができます (注メソッドに追加される、`client`ハブ、いないハブ直接のメンバー)。</span><span class="sxs-lookup"><span data-stu-id="8bc88-160">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="8bc88-161">**ハブ プロキシのメソッドを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="8bc88-161">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="8bc88-162">ハブまたはハブのメソッドがパブリックとして宣言されていません</span><span class="sxs-lookup"><span data-stu-id="8bc88-162">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="8bc88-163">クライアントに表示する、ハブの実装とメソッドとして宣言されなければなりません`public`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-163">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="8bc88-164">別のアプリケーションからハブにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-164">Accessing hub from a different application</span></span>

<span data-ttu-id="8bc88-165">SignalR ハブは、SignalR クライアントを実装するアプリケーションを介してのみアクセスできることができます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-165">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="8bc88-166">SignalR は、その他の通信ライブラリ (SOAP または WCF web サービスなどです。) と相互運用することはできません。ターゲット プラットフォームの利用可能な SignalR クライアントがない場合は、サーバーのエンドポイントを直接アクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-166">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="8bc88-167">手動でデータをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-167">Manually serializing data</span></span>

<span data-ttu-id="8bc88-168">SignalR は自動的に JSON シリアル化に使用、メソッド パラメーター-がのしなくても自分で行います。</span><span class="sxs-lookup"><span data-stu-id="8bc88-168">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="8bc88-169">リモートのハブ メソッドをクライアントの OnDisconnected 関数には実行されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-169">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="8bc88-170">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-170">This behavior is by design.</span></span> <span data-ttu-id="8bc88-171">ときに`OnDisconnected`が呼び出されると、ハブが既に入力、`Disconnected`が許可されていないさらにハブ メソッドを呼び出せる状態です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-171">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="8bc88-172">**C# サーバー コードを正しく OnDisconnected イベント内のコードを実行**</span><span class="sxs-lookup"><span data-stu-id="8bc88-172">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a><span data-ttu-id="8bc88-173">一貫したタイミングで発生しない OnDisconnect</span><span class="sxs-lookup"><span data-stu-id="8bc88-173">OnDisconnect not firing at consistent times</span></span>

<span data-ttu-id="8bc88-174">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-174">This behavior is by design.</span></span> <span data-ttu-id="8bc88-175">ユーザーがアクティブな SignalR 接続に関するページから移動しようとしたとき、SignalR クライアントは、クライアント接続が停止されることをサーバーに通知するベストエフォートの試行を加えます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-175">When a user attempts to navigate away from a page with an active SignalR connection, the SignalR client will then make a best-effort attempt to notify the server that the client connection will be stopped.</span></span> <span data-ttu-id="8bc88-176">SignalR クライアントの最善の努力はサーバーに到達する試行が失敗した、サーバーが後に、構成可能な接続の破棄は`DisconnectTimeout`れた時点で、後で、`OnDisconnected`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-176">If the SignalR client's best-effort attempt fails to reach the server, the server will dispose of the connection after a configurable `DisconnectTimeout` later, at which time the `OnDisconnected` event will fire.</span></span> <span data-ttu-id="8bc88-177">SignalR クライアント ベスト エフォートの試行が成功すると、`OnDisconnected`イベントがすぐに起動されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-177">If the SignalR client's best-effort attempt is successful, the `OnDisconnected` event will fire immediately.</span></span>

<span data-ttu-id="8bc88-178">設定の詳細について、`DisconnectTimeout`設定、表示[接続の有効期間イベントの処理: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-178">For information on setting the `DisconnectTimeout` setting, see [Handling connection lifetime events: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).</span></span>

### <a name="connection-limit-reached"></a><span data-ttu-id="8bc88-179">接続の制限に達しました</span><span class="sxs-lookup"><span data-stu-id="8bc88-179">Connection limit reached</span></span>

<span data-ttu-id="8bc88-180">を Windows 7 などのクライアント オペレーティング システムで、フル バージョンの IIS を使用する場合は、10 接続制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-180">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="8bc88-181">クライアントの OS を使用して、IIS Express 代わりに使用してこの制限を回避します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-181">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="8bc88-182">ドメイン間の接続が正しく設定されていません</span><span class="sxs-lookup"><span data-stu-id="8bc88-182">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="8bc88-183">接続がドメインを越えた場合 (対象の SignalR URL がホストするページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-183">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="8bc88-184">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-184">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="8bc88-185">.NET クライアントで作業していない NTLM (Active Directory) を使用して接続</span><span class="sxs-lookup"><span data-stu-id="8bc88-185">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="8bc88-186">ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続には、接続が正しく構成されていない場合があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-186">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="8bc88-187">SignalR を使用して、ドメイン環境では、次のように、必要な接続プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-187">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="8bc88-188">**C# クライアントを実装するコード接続の資格情報**</span><span class="sxs-lookup"><span data-stu-id="8bc88-188">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a><span data-ttu-id="8bc88-189">Ping/pong を停止しているクライアントを検出する IIS websocket を構成します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-189">Configuring IIS websockets to ping/pong to detect a dead client</span></span>

<span data-ttu-id="8bc88-190">SignalR のサーバーには、クライアントが切れているか、その依存している接続の障害を基になる websocket からの通知は、OnClose コールバックが不明です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-190">SignalR servers don't know if the client is dead or not and they rely on notification from the underlying websocket for connection failures, that is, the OnClose callback.</span></span> <span data-ttu-id="8bc88-191">この問題の 1 つのソリューションでは、IIS websocket ping/pong を自動的に行うを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-191">One solution to this problem is to configure IIS websockets to do the ping/pong for you.</span></span> <span data-ttu-id="8bc88-192">これにより、予期せずが壊れた場合、接続が閉じられます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-192">This ensures that your connection will close if it breaks unexpectedly.</span></span> <span data-ttu-id="8bc88-193">詳細については、次を参照してください。[この stackoverflow の投稿](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-193">For more information see [this stackoverflow post](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).</span></span>

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="8bc88-194">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-194">Other connection issues</span></span>

<span data-ttu-id="8bc88-195">このセクションでは、原因と解決策の特定の現象または接続中に発生するエラー メッセージについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-195">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="8bc88-196">「開始にはデータを送信する前に呼び出す必要があります」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-196">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="8bc88-197">接続を開始する前に、コードが SignalR オブジェクトを参照している場合、このエラーは発生一般的です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-197">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="8bc88-198">ワイヤアップ ハンドラーなどのメソッドの呼び出しに、サーバーで定義されている必要があります追加することが、接続が完了した後にします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-198">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="8bc88-199">なおへの呼び出し`Start`はその前に呼び出しを実行することが後にコードが完了するために非同期。</span><span class="sxs-lookup"><span data-stu-id="8bc88-199">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="8bc88-200">接続が完全に開始した後にハンドラーを追加する最善の方法は、start メソッドにパラメーターとして渡されるコールバック関数には。</span><span class="sxs-lookup"><span data-stu-id="8bc88-200">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="8bc88-201">**正しく SignalR オブジェクトを参照するイベント ハンドラーを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="8bc88-201">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="8bc88-202">このエラーは、SignalR オブジェクトが参照されているときに、接続が停止した場合にも表示されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-202">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="8bc88-203">「301 が完全に移動しました」または「302 は一時的に移動されました」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-203">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="8bc88-204">このエラーは、プロジェクトには、SignalR で、自動的に作成されたプロキシには影響をという名前のフォルダーが含まれている場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-204">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="8bc88-205">このエラーを避けるためには、使用しないという名前のフォルダー`SignalR`アプリケーション、または有効にするプロキシの自動生成をオフにします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-205">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="8bc88-206">参照してください[、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-206">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="8bc88-207">.NET または Silverlight クライアントで「403 アクセス不可」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-207">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="8bc88-208">このエラーは、ドメイン間の通信が正しく有効化されませんクロス ドメイン環境で発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-208">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="8bc88-209">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-209">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="8bc88-210">Silverlight クライアントで、ドメイン間の接続を確立する、次を参照してください。 [Silverlight クライアントからドメイン間の接続](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-210">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="8bc88-211">"404 Not Found" error</span><span class="sxs-lookup"><span data-stu-id="8bc88-211">"404 Not Found" error</span></span>

<span data-ttu-id="8bc88-212">この問題のいくつかの原因があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-212">There are several causes for this issue.</span></span> <span data-ttu-id="8bc88-213">次のすべてを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-213">Verify all of the following:</span></span>

- <span data-ttu-id="8bc88-214">**ハブ プロキシ アドレスの参照が正しくフォーマットされていません:** このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合も表示されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-214">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="8bc88-215">ハブのアドレスへの参照が正しく行われたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-215">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="8bc88-216">参照してください[を動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-216">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="8bc88-217">**ハブ ルートを追加する前に、アプリケーションへのルートの追加:** 場合、アプリケーションでは、他のルートを使用することを確認追加された最初のルートへの呼び出し`MapSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-217">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapSignalR`.</span></span>
- <span data-ttu-id="8bc88-218">**IIS 7 または更新しないで 7.5 拡張子のない Url を使用する:** を使用して IIS 7 または 7.5 更新が要求される拡張子のない Url のサーバーで、ハブの定義へのアクセスを提供できるように`/signalr/hubs`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-218">**Using IIS 7 or 7.5 without the update for extensionless URLs:** Using IIS 7 or 7.5 requires an update for extensionless URLs so that the server can provide access to the hub definitions at `/signalr/hubs`.</span></span> <span data-ttu-id="8bc88-219">更新プログラムが見つかります[ここ](https://support.microsoft.com/kb/980368)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-219">The update can be found [here](https://support.microsoft.com/kb/980368).</span></span>
- <span data-ttu-id="8bc88-220">**IIS が最新でないキャッシュまたは破損している:** キャッシュの内容が最新でないでないことを確認するには、キャッシュをクリアする PowerShell ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-220">**IIS cache out of date or corrupt:** To verify that the cache contents are not out of date, enter the following command in a PowerShell window to clear the cache:</span></span>

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a><span data-ttu-id="8bc88-221">「500 内部サーバー エラー」</span><span class="sxs-lookup"><span data-stu-id="8bc88-221">"500 Internal Server Error"</span></span>

<span data-ttu-id="8bc88-222">これは、さまざまな原因の可能性がある非常に一般的なエラーです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-222">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="8bc88-223">エラーの詳細は、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグから確認できます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-223">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="8bc88-224">サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-224">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="8bc88-225">詳細については、次を参照してください。[ハブ クラスのエラーを処理する方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-225">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

<span data-ttu-id="8bc88-226">ファイアウォールまたはプロキシが構成されていない場合、適切要求ヘッダーを書き直すことが原因と、このエラーは発生も一般的です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-226">This error is also commonly seen if a firewall or proxy is not configured properly, causing the request headers to be rewritten.</span></span> <span data-ttu-id="8bc88-227">ソリューションでは、ファイアウォールまたはプロキシでポート 80 が有効になっていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8bc88-227">The solution is to make sure that port 80 is enabled on the firewall or proxy.</span></span>

### <a name="unexpected-response-code-500"></a><span data-ttu-id="8bc88-228">"予期しない応答コード: 500"</span><span class="sxs-lookup"><span data-stu-id="8bc88-228">"Unexpected response code: 500"</span></span>

<span data-ttu-id="8bc88-229">このエラーは、アプリケーションで使用される .NET framework のバージョンが Web.Config で指定されたバージョンと一致しない場合に発生する可能性があります。この解決策では、.NET 4.5 がアプリケーションの設定と、Web.Config ファイルの両方で使用されることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="8bc88-229">This error may occur if the version of .NET framework used in the application does not match the version specified in Web.Config. The solution is to verify that .NET 4.5 is used in both the application settings and the Web.Config file.</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="8bc88-230">"TypeError: &lt;hubType&gt;が定義されていません"エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-230">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="8bc88-231">このエラーが発生する呼び出し`MapSignalR`が正しく行われない。</span><span class="sxs-lookup"><span data-stu-id="8bc88-231">This error will result if the call to `MapSignalR` is not made properly.</span></span> <span data-ttu-id="8bc88-232">参照してください[SignalR ミドルウェアを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-232">See [How to register SignalR Middleware and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="8bc88-233">JsonSerializationException がユーザー コードによってハンドルされませんでした。</span><span class="sxs-lookup"><span data-stu-id="8bc88-233">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="8bc88-234">メソッドに送信するパラメーターには、シリアル化不可能な (のような種類ファイル ハンドルやデータベース接続) にはが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-234">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="8bc88-235">使用 (またはセキュリティのためのシリアル化の理由から)、クライアントに送信したくない、サーバー側オブジェクトにメンバーを使用する必要がある場合、`JSONIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="8bc88-235">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="8bc88-236">"プロトコル エラー: 不明なトランスポート"エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-236">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="8bc88-237">このエラーは、クライアントは、SignalR を使用するトランスポートをサポートしていない場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-237">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="8bc88-238">参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)についてを SignalR でブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-238">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="8bc88-239">「JavaScript Hub プロキシ生成は無効になっています。」</span><span class="sxs-lookup"><span data-stu-id="8bc88-239">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="8bc88-240">このエラーが発生`DisableJavaScriptProxies`で動的に生成されたプロキシへの参照を含めているときに設定されている`signalr/hubs`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-240">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="8bc88-241">手動でプロキシを作成する方法の詳細については、次を参照してください。 [、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-241">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="8bc88-242">"接続 ID の形式が正しくありません"または「アクティブな SignalR 接続中に、ユーザー id を変更できません」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-242">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="8bc88-243">このエラーは、認証が使用されていて、クライアントがログアウトしている接続を停止する前に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-243">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="8bc88-244">ソリューションは、クライアントをログアウトする前に、SignalR 接続を停止するためです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-244">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="8bc88-245">"エラーをキャッチしませんでした: SignalR: jQuery が見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="8bc88-245">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="8bc88-246">JQuery が SignalR.js ファイルの前に参照されていることを確認してください。"エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-246">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="8bc88-247">SignalR JavaScript クライアントには、jQuery を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-247">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="8bc88-248">JQuery への参照が使用されるパスが有効であることと jQuery への参照が SignalR への参照の前に、正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-248">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="8bc88-249">"TypeError をキャッチしませんでしたプロパティを読み取ることができません '&lt;プロパティ&gt;' 未定義の"エラー。</span><span class="sxs-lookup"><span data-stu-id="8bc88-249">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="8bc88-250">このエラーは、jQuery またはハブ プロキシが適切に参照されていないことから発生します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-250">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="8bc88-251">JQuery、およびハブ プロキシへの参照が使用されるパスが有効であることと jQuery への参照がハブ プロキシへの参照の前に、正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-251">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="8bc88-252">ハブ プロキシへの参照を既定値は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-252">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="8bc88-253">**ハブ プロキシを正しく参照する HTML クライアント側コード**</span><span class="sxs-lookup"><span data-stu-id="8bc88-253">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="8bc88-254">「ユーザー コードによってハンドルされませんでした。 RuntimeBinderException」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-254">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="8bc88-255">このエラーが発生するときの正しくないオーバー ロード`Hub.On`を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-255">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="8bc88-256">メソッドの戻り値が、戻り値の型がジェネリック型パラメーターとして指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-256">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="8bc88-257">**(生成されたプロキシは) なしのクライアントに定義されたメソッド**</span><span class="sxs-lookup"><span data-stu-id="8bc88-257">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="8bc88-258">接続 ID が一貫性のあるか、ページの読み込みの間で接続が切断</span><span class="sxs-lookup"><span data-stu-id="8bc88-258">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="8bc88-259">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-259">This behavior is by design.</span></span> <span data-ttu-id="8bc88-260">ハブ オブジェクトが、page オブジェクトにホストされているため、ハブは、ページが更新されると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-260">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="8bc88-261">複数ページ アプリケーションは、ページの読み込みの間の一貫性ができるように、ユーザーと接続 Id 間の関連付けを維持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-261">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="8bc88-262">接続 Id は、サーバーのいずれかに格納できる、`ConcurrentDictionary`オブジェクトまたはデータベース。</span><span class="sxs-lookup"><span data-stu-id="8bc88-262">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="8bc88-263">「値を null にすることはできません」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-263">"Value cannot be null" error</span></span>

<span data-ttu-id="8bc88-264">省略可能なパラメーターを使用してサーバー側メソッドは現在サポートされていません。省略可能なパラメーターを省略すると、メソッドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-264">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="8bc88-265">詳細については、次を参照してください。[省略可能なパラメーター](https://github.com/SignalR/SignalR/issues/324)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-265">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="8bc88-266">"Firefox でサーバーへの接続を確立できません&lt;アドレス&gt;"Firebug のエラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-266">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="8bc88-267">WebSocket トランスポートのネゴシエーションは失敗し、別のトランスポートが代わりに使用される場合、Firebug でこのエラー メッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-267">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="8bc88-268">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-268">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="8bc88-269">.NET クライアント アプリケーションで「リモート証明書は、検証手順に従った有効な」エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-269">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="8bc88-270">場合は、サーバーでは、要求が行われる前に、接続に x509certificate を追加することができますし、カスタム クライアント証明書が必要です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-270">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="8bc88-271">接続を使用して、証明書を追加`Connection.AddClientCertificate`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-271">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="8bc88-272">接続の認証のタイムアウト後のドロップします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-272">Connection drops after authentication times out</span></span>

<span data-ttu-id="8bc88-273">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-273">This behavior is by design.</span></span> <span data-ttu-id="8bc88-274">接続がアクティブになったときに、認証の資格情報を変更することはできません。資格情報を更新するには、接続を停止および再起動して必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-274">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="8bc88-275">OnConnected は jQuery Mobile を使用する場合に 2 回呼び出されます</span><span class="sxs-lookup"><span data-stu-id="8bc88-275">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="8bc88-276">jQuery Mobile の`initializePage`関数は、再実行するには、各ページのスクリプトは、2 番目の接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-276">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="8bc88-277">この問題の解決策は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-277">Solutions for this issue include:</span></span>

- <span data-ttu-id="8bc88-278">JQuery Mobile、JavaScript ファイルの前にへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-278">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="8bc88-279">無効にする、`initializePage`関数を設定して`$.mobile.autoInitializePage = false`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-279">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="8bc88-280">接続を開始する前に初期化を完了してページを待機します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-280">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="8bc88-281">サーバー送信イベントを使用する Silverlight アプリケーションでメッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-281">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="8bc88-282">Silverlight でイベントを送信するサーバーを使用するときに、メッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-282">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="8bc88-283">長いポーリングが代わりに使用するには、接続を開始するときに、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-283">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="8bc88-284">プロトコルを永久にフレーム「アクセス許可が拒否されました」を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-284">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="8bc88-285">これは、説明されている既知の問題[ここ](https://github.com/SignalR/SignalR/issues/1963)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-285">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="8bc88-286">この現象は、最新の JQuery ライブラリ; を使用して表示することがあります。回避策では、JQuery 1.8.2 にアプリをダウン グレードします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-286">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a><span data-ttu-id="8bc88-287">"InvalidOperationException: 有効な web ソケット リクエストではありません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-287">"InvalidOperationException: Not a valid web socket request.</span></span>

<span data-ttu-id="8bc88-288">このエラーは、WebSocket プロトコルを使用すると、ネットワーク プロキシには、要求ヘッダーを変更する場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-288">This error may occur if the WebSocket protocol is used, but the network proxy is modifying the request headers.</span></span> <span data-ttu-id="8bc88-289">ソリューションでは、ポート 80 で WebSocket を許可するプロキシを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-289">The solution is to configure the proxy to allow WebSocket on port 80.</span></span>

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a><span data-ttu-id="8bc88-290">"例外:&lt;メソッド名&gt;メソッドを解決できませんでした"クライアントがサーバーでメソッドを呼び出すと</span><span class="sxs-lookup"><span data-stu-id="8bc88-290">"Exception: &lt;method name&gt; method could not be resolved" when client calls method on server</span></span>

<span data-ttu-id="8bc88-291">このエラーは検出できません。 配列などの JSON ペイロードのデータ型を使用することがあります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-291">This error can result from using data types that cannot be discovered in a JSON payload, such as Array.</span></span> <span data-ttu-id="8bc88-292">回避策では、IList などの JSON で検出することのあるデータ型を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-292">The workaround is to use a data type that is discoverable by JSON, such as IList.</span></span> <span data-ttu-id="8bc88-293">詳細については、次を参照してください。 [.NET クライアント配列パラメーターを持つハブ メソッドを呼び出すことができません](https://github.com/SignalR/SignalR/issues/2672)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-293">For more information, see [.NET Client unable to call hub methods with array parameters](https://github.com/SignalR/SignalR/issues/2672).</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="8bc88-294">コンパイル、およびサーバー側エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-294">Compilation and server-side errors</span></span>

 <span data-ttu-id="8bc88-295">次のセクションには、コンパイラとサーバー側のランタイム エラーの考えられる解決策が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8bc88-295">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="8bc88-296">ハブ インスタンスへの参照が null</span><span class="sxs-lookup"><span data-stu-id="8bc88-296">Reference to Hub instance is null</span></span>

<span data-ttu-id="8bc88-297">ハブ インスタンスは接続ごとに作成される、ためにできませんインスタンスを作成するハブのコードで自分でします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-297">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="8bc88-298">ハブ自体の外部からのハブのメソッドを呼び出すを参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)のハブ コンテキストへの参照を取得する方法です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-298">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="8bc88-299">HTTPContext.Current.Session が null です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-299">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="8bc88-300">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-300">This behavior is by design.</span></span> <span data-ttu-id="8bc88-301">双方向のメッセージング中断は、セッションの状態を有効にするため、SignalR は ASP.NET セッション状態をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-301">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="8bc88-302">オーバーライドする適切なメソッドはありません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-302">No suitable method to override</span></span>

<span data-ttu-id="8bc88-303">以前のドキュメントまたはブログからのコードを使用している場合は、このエラーを表示することがあります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-303">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="8bc88-304">変更されたか推奨されなくなりましたがメソッドの名前を参照していないことを確認してください (と同様に`OnConnectedAsync`)。</span><span class="sxs-lookup"><span data-stu-id="8bc88-304">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="8bc88-305">HostContextExtensions.WebSocketServerUrl が null です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-305">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="8bc88-306">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-306">This behavior is by design.</span></span> <span data-ttu-id="8bc88-307">このメンバーは推奨されておらずは使用できません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-307">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="8bc88-308">「'Signalr.hubs' という名前のルートは既にルートのコレクションに」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-308">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="8bc88-309">場合は、このエラーが発生する`MapSignalR`は、アプリケーションによって 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-309">This error will be seen if `MapSignalR` is called twice by your application.</span></span> <span data-ttu-id="8bc88-310">いくつかの呼び出しの例のアプリケーション`MapSignalR`直接スタートアップ クラスで他のユーザーに呼び出しラッパー クラスです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-310">Some example applications call `MapSignalR` directly in the Startup class; others make the call in a wrapper class.</span></span> <span data-ttu-id="8bc88-311">あるアプリケーションで実行しない両方を確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-311">Ensure that your application does not do both.</span></span>

### <a name="websocket-is-not-used"></a><span data-ttu-id="8bc88-312">WebSocket は使用されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-312">WebSocket is not used</span></span>

<span data-ttu-id="8bc88-313">サーバーとクライアントが WebSocket の要件を満たしていることを確認した場合 (に一覧表示、[サポートされているプラットフォーム](../getting-started/supported-platforms.md)ドキュメント)、サーバーで WebSocket を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-313">If you have verified that your server and clients meet the requirements for WebSocket (listed in the [Supported Platforms](../getting-started/supported-platforms.md) document), you will need to enable WebSocket on your server.</span></span> <span data-ttu-id="8bc88-314">これを行うための手順を参照して[ここ](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-314">Instructions for doing this can be found [here](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

### <a name="connection-is-undefined"></a><span data-ttu-id="8bc88-315">$.connection が定義されていません</span><span class="sxs-lookup"><span data-stu-id="8bc88-315">$.connection is undefined</span></span>

<span data-ttu-id="8bc88-316">このエラーは、ページ上のスクリプトが正常に読み込まれていないかあるハブ プロキシが到達できないか、正しくアクセスしているを示します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-316">This error indicates that either the scripts on a page are not being loaded properly, or the hub proxy is not reachable or is being accessed incorrectly.</span></span> <span data-ttu-id="8bc88-317">ページ上のスクリプト参照がプロジェクトに読み込まれたスクリプトに対応していると、サーバーが実行されている場合、/signalr/hubs をブラウザーでアクセスできることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-317">Verify that the script references on your page correspond to the scripts loaded in your project, and that /signalr/hubs can be accessed in a browser when the server is running.</span></span>

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a><span data-ttu-id="8bc88-318">動的な式のコンパイルに必要な 1 つまたは複数の型が見つかりません</span><span class="sxs-lookup"><span data-stu-id="8bc88-318">One or more types required to compile a dynamic expression cannot be found</span></span>

<span data-ttu-id="8bc88-319">このエラーには、ことを示します、`Microsoft.CSharp`ライブラリがありません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-319">This error indicates that the `Microsoft.CSharp` library is missing.</span></span> <span data-ttu-id="8bc88-320">追加、**アセンブリ -&gt;Framework**タブです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-320">Add it in the **Assemblies-&gt;Framework** tab.</span></span>

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a><span data-ttu-id="8bc88-321">Visual Basic または厳密に型指定されたハブ; Clients.Caller から呼び出し元の状態にアクセスできません。「型 'Task (Of Object)' を 'String' 型からの変換が正しくありません」エラー</span><span class="sxs-lookup"><span data-stu-id="8bc88-321">Caller state cannot be accessed from Clients.Caller in Visual Basic or in a strongly typed hub; "Conversion from type 'Task(Of Object)' to type 'String' is not valid" error</span></span>

<span data-ttu-id="8bc88-322">Visual Basic または厳密に型指定されたハブの呼び出し元の状態にアクセスするには、使用、`Clients.CallerState`の代わりに (SignalR 2.1 で導入された) プロパティ`Clients.Caller`です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-322">To access caller state in Visual Basic or in a strongly typed hub, use the `Clients.CallerState` property (introduced in SignalR 2.1) instead of `Clients.Caller`.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="8bc88-323">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-323">Visual Studio issues</span></span>

<span data-ttu-id="8bc88-324">このセクションでは、Visual Studio で発生する問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-324">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="8bc88-325">ソリューション エクスプ ローラーでスクリプト ドキュメント ノードが表示されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-325">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="8bc88-326">チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで、[スクリプト ドキュメント] ノードに直接します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-326">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="8bc88-327">このノードは、JavaScript デバッガーによって生成され、Internet Explorer; 内のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合、ノードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-327">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="8bc88-328">JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。</span><span class="sxs-lookup"><span data-stu-id="8bc88-328">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="8bc88-329">SignalR では、Visual Studio 2008 またはそれ以前は機能しません</span><span class="sxs-lookup"><span data-stu-id="8bc88-329">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="8bc88-330">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-330">This behavior is by design.</span></span> <span data-ttu-id="8bc88-331">SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降で SignalR アプリケーションを開発することが必要です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-331">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span> <span data-ttu-id="8bc88-332">SignalR のサーバー コンポーネントには、.NET Framework 4.5 が必要です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-332">The server component of SignalR requires .NET Framework 4.5.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="8bc88-333">IIS の問題</span><span class="sxs-lookup"><span data-stu-id="8bc88-333">IIS issues</span></span>

<span data-ttu-id="8bc88-334">このセクションには、インターネット インフォメーション サービスで問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8bc88-334">This section contains issues with Internet Information Services.</span></span>

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a><span data-ttu-id="8bc88-335">IIS ではなく Visual Studio 開発サーバーで、SignalR の動作します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-335">SignalR works on Visual Studio development server, but not in IIS</span></span>

<span data-ttu-id="8bc88-336">IIS 7.0、7.5、SignalR はサポートされますが、拡張子のない Url を追加する必要がありますをサポートします。</span><span class="sxs-lookup"><span data-stu-id="8bc88-336">SignalR is supported on IIS 7.0 and 7.5, but support for extensionless URLs must be added.</span></span> <span data-ttu-id="8bc88-337">拡張子のない Url のサポートを追加するを参照してください[https://support.microsoft.com/kb/980368。](https://support.microsoft.com/kb/980368)</span><span class="sxs-lookup"><span data-stu-id="8bc88-337">To add support for extensionless URLs, see [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)</span></span>

<span data-ttu-id="8bc88-338">SignalR には、ASP.NET (ASP.NET がインストールされていない IIS で既定で) サーバーにインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bc88-338">SignalR requires ASP.NET to be installed on the server (ASP.NET is not installed on IIS by default).</span></span> <span data-ttu-id="8bc88-339">ASP.NET をインストールするを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-339">To install ASP.NET, see [ASP.NET Downloads](https://www.asp.net/downloads).</span></span>

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a><span data-ttu-id="8bc88-340">Microsoft Azure を発行します。</span><span class="sxs-lookup"><span data-stu-id="8bc88-340">Microsoft Azure issues</span></span>

<span data-ttu-id="8bc88-341">このセクションには、Microsoft Azure での問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8bc88-341">This section contains issues with Microsoft Azure.</span></span>

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a><span data-ttu-id="8bc88-342">FileLoadException Azure ワーカー ロールで SignalR をホストする場合</span><span class="sxs-lookup"><span data-stu-id="8bc88-342">FileLoadException when hosting SignalR in an Azure Worker Role</span></span>

<span data-ttu-id="8bc88-343">SignalR Azure ワーカー ロールでホストしている例外になる可能性があります"ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Owin、バージョン 2.0.0.0 を ="です。</span><span class="sxs-lookup"><span data-stu-id="8bc88-343">Hosting SignalR in an Azure Worker Role might result in the exception "Could not load file or assembly 'Microsoft.Owin, Version=2.0.0.0".</span></span> <span data-ttu-id="8bc88-344">これは、NuGet; の既知の問題バインド リダイレクトは、Azure ワーカー ロール プロジェクトでは自動的に追加されません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-344">This is a known issue with NuGet; Binding redirects are not added automatically in Azure Worker Role projects.</span></span> <span data-ttu-id="8bc88-345">これを解決するには、バインド リダイレクトを手動で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="8bc88-345">To fix this, you can add the binding redirects manually.</span></span> <span data-ttu-id="8bc88-346">次の行を追加、`app.config`のワーカー ロール プロジェクトのファイルです。</span><span class="sxs-lookup"><span data-stu-id="8bc88-346">Add the following lines to the `app.config` file for your Worker Role project.</span></span>

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="8bc88-347">メッセージがトピック名を変更した後、Azure のバック プレーンから受信されていません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-347">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="8bc88-348">Azure バック プレーンで使用されるトピックは内部的に保持されます。ユーザー構成可能であるこれらの目的ではありません。</span><span class="sxs-lookup"><span data-stu-id="8bc88-348">The topics used by the Azure backplane are maintained internally; they are not intended to be user-configurable.</span></span>
