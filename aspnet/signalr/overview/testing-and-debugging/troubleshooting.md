---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: SignalR トラブルシューティング |Microsoft Docs
author: pfletcher
description: この記事では、SignalR アプリケーションの開発に関する一般的な問題について説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: e41061f0310c021b10dc6667a5c3297788213b0a
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287954"
---
<a name="signalr-troubleshooting"></a><span data-ttu-id="d2d47-103">SignalR トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="d2d47-103">SignalR Troubleshooting</span></span>
====================
<span data-ttu-id="d2d47-104">提供者: [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d2d47-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d2d47-105">このドキュメントでは、SignalR を使って一般的な問題のトラブルシューティングについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-105">This document describes common troubleshooting issues with SignalR.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d2d47-106">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="d2d47-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="d2d47-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d2d47-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="d2d47-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d2d47-108">.NET 4.5</span></span>
> - <span data-ttu-id="d2d47-109">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="d2d47-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d2d47-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="d2d47-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="d2d47-111">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="d2d47-112">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="d2d47-112">Questions and comments</span></span>
>
> <span data-ttu-id="d2d47-113">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="d2d47-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d2d47-114">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="d2d47-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="d2d47-115">このドキュメントには、次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d2d47-115">This document contains the following sections.</span></span>

- [<span data-ttu-id="d2d47-116">サイレント モードでが失敗したクライアントとサーバー間のメソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="d2d47-116">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="d2d47-117">停止したクライアントを検出するためにピンポン IIS websocket を構成します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-117">Configuring IIS websockets to ping/pong to detect a dead client</span></span>](#pong)
- [<span data-ttu-id="d2d47-118">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-118">Other connection issues</span></span>](#other)
- [<span data-ttu-id="d2d47-119">コンパイルとサーバー側のエラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-119">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="d2d47-120">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-120">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="d2d47-121">インターネット インフォメーション サービスの問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-121">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="d2d47-122">Microsoft Azure を発行します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-122">Microsoft Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="d2d47-123">サイレント モードでが失敗したクライアントとサーバー間のメソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="d2d47-123">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="d2d47-124">このセクションでは、意味のあるエラー メッセージを表示せずに失敗するには、クライアントとサーバー間のメソッド呼び出しの考えられる原因について説明します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-124">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="d2d47-125">SignalR アプリケーションで、サーバーに関する情報がない。 クライアントを実装する方法サーバーは、クライアント メソッドを呼び出しとメソッドの名前とパラメーターのデータがクライアントに送信される、メソッドが実行されるは、サーバーが指定した形式である場合にのみ。</span><span class="sxs-lookup"><span data-stu-id="d2d47-125">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="d2d47-126">一致するメソッドが検出されないクライアントでは、何も起こりません、サーバー上のエラー メッセージが発生しなかった場合は。</span><span class="sxs-lookup"><span data-stu-id="d2d47-126">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="d2d47-127">クライアント メソッドが呼び出されない作業をさらに調査するには、どのような呼び出しを表示するハブの start メソッドは、サーバーから送信される呼び出しの前にログ記録にできます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-127">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="d2d47-128">JavaScript アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (JavaScript クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-128">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="d2d47-129">.NET クライアント アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (.NET クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-net-client.md#logging)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-129">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="d2d47-130">スペルの正しくないメソッド、不適切なメソッドのシグネチャ、または不適切なハブの名前</span><span class="sxs-lookup"><span data-stu-id="d2d47-130">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="d2d47-131">名前または呼び出されたメソッドのシグネチャが一致しない場合、クライアント上の適切なメソッド、呼び出しは失敗します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-131">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="d2d47-132">メソッド名がサーバーによって呼び出されますが、クライアント上のメソッドの名前と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-132">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="d2d47-133">また、SignalR は camel 形式のメソッドを使用するハブ プロキシを作成します。 JavaScript では、そのため、メソッドと呼ばれます`SendMessage`サーバーでが呼び出されます`sendMessage`クライアント プロキシで。</span><span class="sxs-lookup"><span data-stu-id="d2d47-133">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="d2d47-134">使用する場合、`HubName`サーバー側コードで属性を使用する名前が、クライアントで、ハブの作成に使用する名前と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-134">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="d2d47-135">使用しない場合、`HubName`属性、JavaScript クライアント内でハブの名前が ChatHub ではなく chatHub など、キャメル形式で表記であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-135">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="d2d47-136">クライアント上のメソッド名が重複しています</span><span class="sxs-lookup"><span data-stu-id="d2d47-136">Duplicate method name on client</span></span>

<span data-ttu-id="d2d47-137">大文字小文字によってのみとは異なるクライアントで重複するメソッドがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-137">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="d2d47-138">場合は、クライアント アプリケーションがある呼び出されるメソッド`sendMessage`、いないというメソッドもを確認して`SendMessage`もします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-138">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="d2d47-139">クライアントで不足している JSON のパーサー</span><span class="sxs-lookup"><span data-stu-id="d2d47-139">Missing JSON parser on the client</span></span>

<span data-ttu-id="d2d47-140">SignalR では、JSON パーサーは、サーバーとクライアント間の呼び出しをシリアル化するために必要です。</span><span class="sxs-lookup"><span data-stu-id="d2d47-140">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="d2d47-141">クライアントが (Internet Explorer 7 の場合) などの組み込み JSON パーサーを持っていない場合は、アプリケーションのいずれかに含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-141">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="d2d47-142">JSON パーサーをダウンロードする[ここ](http://nuget.org/packages/json2)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-142">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="d2d47-143">ハブおよび PersistentConnection 構文を混在させる</span><span class="sxs-lookup"><span data-stu-id="d2d47-143">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="d2d47-144">SignalR では、2 つの間の通信モデルを使用します。ハブおよび PersistentConnections します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-144">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="d2d47-145">これらの 2 つの通信モデルを呼び出すための構文は、クライアント コードで異なります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-145">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="d2d47-146">サーバー コードにハブを追加した場合は、すべてのクライアント コードは適切なハブの構文を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-146">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="d2d47-147">**JavaScript クライアント内で、PersistentConnection を作成する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-147">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="d2d47-148">**Javascript クライアント内でハブ プロキシを作成する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-148">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="d2d47-149">**C# サーバー コードを PersistentConnection にルートをマップします。**</span><span class="sxs-lookup"><span data-stu-id="d2d47-149">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="d2d47-150">**複数のアプリケーションがある場合、ハブ、または複数のハブ ルートをマップ c# サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-150">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="d2d47-151">サブスクリプションを追加する前に開始した接続</span><span class="sxs-lookup"><span data-stu-id="d2d47-151">Connection started before subscriptions are added</span></span>

<span data-ttu-id="d2d47-152">プロキシ サーバーから呼び出すことができるメソッドが追加される前に、ハブの接続が開始されると、メッセージが受信されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-152">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="d2d47-153">次の JavaScript コードは、ハブを正しく開始できません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-153">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="d2d47-154">**ハブのメッセージを受信することはできません。 JavaScript クライアント コードが正しくないです。**</span><span class="sxs-lookup"><span data-stu-id="d2d47-154">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="d2d47-155">代わりに、開始を呼び出す前に、メソッドのサブスクリプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-155">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="d2d47-156">**ハブに誤ってサブスクリプションを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-156">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="d2d47-157">ハブ プロキシのメソッド名がありません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-157">Missing method name on the hub proxy</span></span>

<span data-ttu-id="d2d47-158">サーバーで定義されたメソッドがクライアントで購読していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-158">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="d2d47-159">場合でも、サーバーは、メソッドを定義、クライアント プロキシにも追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-159">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="d2d47-160">メソッドは、次の方法でクライアント プロキシに追加できます (注、メソッドに追加される、`client`ハブのハブではなく直接のメンバー)。</span><span class="sxs-lookup"><span data-stu-id="d2d47-160">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="d2d47-161">**ハブ プロキシのメソッドを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-161">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="d2d47-162">ハブまたはハブ メソッドをパブリックとして宣言されていません</span><span class="sxs-lookup"><span data-stu-id="d2d47-162">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="d2d47-163">クライアントに表示される、ハブの実装とメソッドとして宣言する必要があります`public`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-163">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="d2d47-164">別のアプリケーションからハブへのアクセス</span><span class="sxs-lookup"><span data-stu-id="d2d47-164">Accessing hub from a different application</span></span>

<span data-ttu-id="d2d47-165">SignalR ハブは、SignalR クライアントを実装するアプリケーションからのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-165">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="d2d47-166">SignalR ことはできません (SOAP サービスまたは WCF web サービスです。) などの他の通信ライブラリとの相互運用します。ターゲット プラットフォームの使用可能な SignalR クライアントがない場合、サーバーのエンドポイントに直接アクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-166">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="d2d47-167">手動でデータをシリアル化</span><span class="sxs-lookup"><span data-stu-id="d2d47-167">Manually serializing data</span></span>

<span data-ttu-id="d2d47-168">SignalR は自動的に JSON シリアル化に使用、メソッド パラメーターであるのなら自分自身でするのに必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-168">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="d2d47-169">リモートのハブ メソッドをクライアント OnDisconnected 関数では実行されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-169">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="d2d47-170">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-170">This behavior is by design.</span></span> <span data-ttu-id="d2d47-171">ときに`OnDisconnected`が呼び出されると、ハブが既に入力、`Disconnected`によりさらにハブ メソッドを呼び出せる状態。</span><span class="sxs-lookup"><span data-stu-id="d2d47-171">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="d2d47-172">**OnDisconnected イベント内のコードを正しく実行 c# サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-172">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a><span data-ttu-id="d2d47-173">一貫性のタイミングで発生しない OnDisconnect</span><span class="sxs-lookup"><span data-stu-id="d2d47-173">OnDisconnect not firing at consistent times</span></span>

<span data-ttu-id="d2d47-174">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-174">This behavior is by design.</span></span> <span data-ttu-id="d2d47-175">ユーザーがアクティブな SignalR 接続に関するページから移動しようとすると、SignalR クライアントは、クライアント接続が停止されることをサーバーに通知するベストエフォートの試行を加えます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-175">When a user attempts to navigate away from a page with an active SignalR connection, the SignalR client will then make a best-effort attempt to notify the server that the client connection will be stopped.</span></span> <span data-ttu-id="d2d47-176">SignalR クライアントのベスト エフォート場合は、サーバーに到達する試行が失敗した後、構成可能な接続の破棄は、サーバー、`DisconnectTimeout`時点で、後で、`OnDisconnected`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-176">If the SignalR client's best-effort attempt fails to reach the server, the server will dispose of the connection after a configurable `DisconnectTimeout` later, at which time the `OnDisconnected` event will fire.</span></span> <span data-ttu-id="d2d47-177">試行が成功すると、SignalR クライアントのベスト エフォートである場合、`OnDisconnected`イベントは、すぐに発生します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-177">If the SignalR client's best-effort attempt is successful, the `OnDisconnected` event will fire immediately.</span></span>

<span data-ttu-id="d2d47-178">設定の詳細について、`DisconnectTimeout`設定、表示[接続の有効期間イベントを処理します。DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-178">For information on setting the `DisconnectTimeout` setting, see [Handling connection lifetime events: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).</span></span>

### <a name="connection-limit-reached"></a><span data-ttu-id="d2d47-179">接続の上限に達しました</span><span class="sxs-lookup"><span data-stu-id="d2d47-179">Connection limit reached</span></span>

<span data-ttu-id="d2d47-180">Windows 7 などのクライアント オペレーティング システムでの完全版の IIS を使用する場合は、10 接続制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-180">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="d2d47-181">クライアント OS を使用して、IIS Express 代わりに使用してこの制限を回避します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-181">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="d2d47-182">ドメイン間の接続が正しく設定されていません</span><span class="sxs-lookup"><span data-stu-id="d2d47-182">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="d2d47-183">ドメイン間の接続の場合 (対象の SignalR URL が、ホスティング ページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-183">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="d2d47-184">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-184">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="d2d47-185">.NET クライアントで機能しない NTLM (Active Directory) を使用して接続</span><span class="sxs-lookup"><span data-stu-id="d2d47-185">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="d2d47-186">ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続が失敗する場合は、接続が正しく構成されていません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-186">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="d2d47-187">SignalR を使用して、ドメイン環境で、次のように、必要な接続プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-187">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="d2d47-188">**接続の資格情報を実装する c# クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-188">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a><span data-ttu-id="d2d47-189">停止したクライアントを検出するためにピンポン IIS websocket を構成します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-189">Configuring IIS websockets to ping/pong to detect a dead client</span></span>

<span data-ttu-id="d2d47-190">SignalR のサーバーがわからないかどうか、クライアントが切れているか、つまりから基になるための websocket 接続の失敗を通知に依存、`OnClose`コールバック。</span><span class="sxs-lookup"><span data-stu-id="d2d47-190">SignalR servers don't know if the client is dead or not and they rely on notification from the underlying websocket for connection failures, that is, the `OnClose` callback.</span></span> <span data-ttu-id="d2d47-191">この問題を 1 つのソリューションでは、IIS websocket ping/pong な作業を構成します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-191">One solution to this problem is to configure IIS websockets to do the ping/pong for you.</span></span> <span data-ttu-id="d2d47-192">これにより、予期せず中断の場合、接続が閉じられます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-192">This ensures that your connection will close if it breaks unexpectedly.</span></span> <span data-ttu-id="d2d47-193">詳細については、次を参照してください。[この stackoverflow の投稿](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-193">For more information see [this stackoverflow post](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).</span></span>

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="d2d47-194">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-194">Other connection issues</span></span>

<span data-ttu-id="d2d47-195">このセクションでは、原因と解決策の特定の現象または接続中に発生するエラー メッセージについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-195">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="d2d47-196">「スタートにはデータを送信する前に呼び出す必要があります」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-196">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="d2d47-197">このエラーは、接続を開始する前に、コードが SignalR オブジェクトを参照している場合によく見られます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-197">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="d2d47-198">ハンドラーなどのワイヤアップがメソッドの呼び出し、サーバーで定義されている必要があります追加されること、接続が完了した後。</span><span class="sxs-lookup"><span data-stu-id="d2d47-198">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="d2d47-199">なおへの呼び出し`Start`は前に、呼び出しを実行することが後のコードが完了するために非同期でします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-199">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="d2d47-200">接続が完全に開始した後、ハンドラーを追加する最善の方法は、start メソッドにパラメーターとして渡されるコールバック関数には。</span><span class="sxs-lookup"><span data-stu-id="d2d47-200">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="d2d47-201">**正しく SignalR オブジェクトを参照するイベント ハンドラーを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-201">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="d2d47-202">このエラーは、SignalR オブジェクトはまだ参照されているときに、接続が停止した場合にも表示されます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-202">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="d2d47-203">「301 は完全に移動されました」または「302 が一時的に移動されました」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-203">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="d2d47-204">このエラーは、プロジェクトには、SignalR で、自動的に作成されたプロキシが干渉をという名前のフォルダーが含まれている場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-204">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="d2d47-205">このエラーを回避するために使用しないでくださいという名前のフォルダー`SignalR`アプリケーション、または有効にする自動プロキシの生成をオフにします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-205">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="d2d47-206">参照してください[、生成されたプロキシとは何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)の詳細。</span><span class="sxs-lookup"><span data-stu-id="d2d47-206">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="d2d47-207">.NET または Silverlight クライアントに「403 アクセス不可」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-207">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="d2d47-208">このエラーは、ドメイン間の通信が正しく有効化しないクロス ドメイン環境で発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-208">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="d2d47-209">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-209">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="d2d47-210">Silverlight クライアントでのドメイン間の接続を確立するを参照してください。 [Silverlight クライアントからのドメインを越えた接続](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-210">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="d2d47-211">「404 見つかりません」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-211">"404 Not Found" error</span></span>

<span data-ttu-id="d2d47-212">この問題のいくつかの原因があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-212">There are several causes for this issue.</span></span> <span data-ttu-id="d2d47-213">次のすべてを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-213">Verify all of the following:</span></span>

- <span data-ttu-id="d2d47-214">**ハブ プロキシのアドレス リファレンス形式が正しくありません。** このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合によく見られます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-214">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="d2d47-215">ハブ アドレスへの参照が正しく行われたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-215">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="d2d47-216">参照してください[動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-216">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="d2d47-217">**ハブ ルートを追加する前にアプリケーションへのルートの追加。** アプリケーションでは、他のルートを使用する場合、最初のルートが追加の呼び出しの確認`MapSignalR`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-217">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapSignalR`.</span></span>
- <span data-ttu-id="d2d47-218">**IIS 7 または 7.5、更新プログラムがないを使用して、拡張子のない Url:** IIS 7 または 7.5 を使用して、サーバーにハブの定義へのアクセスを提供できるように、拡張子のない Url の更新プログラムを必要`/signalr/hubs`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-218">**Using IIS 7 or 7.5 without the update for extensionless URLs:** Using IIS 7 or 7.5 requires an update for extensionless URLs so that the server can provide access to the hub definitions at `/signalr/hubs`.</span></span> <span data-ttu-id="d2d47-219">更新プログラムが見つかります[ここ](https://support.microsoft.com/kb/980368)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-219">The update can be found [here](https://support.microsoft.com/kb/980368).</span></span>
- <span data-ttu-id="d2d47-220">**IIS のキャッシュ期限切れであるか、または壊れています。** キャッシュの内容が有効期限が切れていないことを確認するには、キャッシュをクリアする PowerShell ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-220">**IIS cache out of date or corrupt:** To verify that the cache contents are not out of date, enter the following command in a PowerShell window to clear the cache:</span></span>

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a><span data-ttu-id="d2d47-221">「500 内部サーバー エラー」</span><span class="sxs-lookup"><span data-stu-id="d2d47-221">"500 Internal Server Error"</span></span>

<span data-ttu-id="d2d47-222">これは、さまざまな原因の可能性がある非常に一般的なエラーです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-222">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="d2d47-223">エラーの詳細については、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグを確認できます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-223">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="d2d47-224">サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-224">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="d2d47-225">詳細については、次を参照してください。[ハブ クラス内のエラーの処理方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-225">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

<span data-ttu-id="d2d47-226">ファイアウォールまたはプロキシが正しく構成されていない、書き換え要求ヘッダーの原因の場合、このエラーは発生も一般的です。</span><span class="sxs-lookup"><span data-stu-id="d2d47-226">This error is also commonly seen if a firewall or proxy is not configured properly, causing the request headers to be rewritten.</span></span> <span data-ttu-id="d2d47-227">ソリューションでは、ファイアウォールまたはプロキシのポート 80 が有効であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-227">The solution is to make sure that port 80 is enabled on the firewall or proxy.</span></span>

### <a name="unexpected-response-code-500"></a><span data-ttu-id="d2d47-228">"予期しない応答コード。500"</span><span class="sxs-lookup"><span data-stu-id="d2d47-228">"Unexpected response code: 500"</span></span>

<span data-ttu-id="d2d47-229">このエラーは、アプリケーションで使用される .NET framework のバージョンが Web.Config で指定されたバージョンと一致しない場合に発生する可能性があります。このソリューションでは、.NET 4.5 がアプリケーションの設定と、Web.Config ファイルの両方で使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-229">This error may occur if the version of .NET framework used in the application does not match the version specified in Web.Config. The solution is to verify that .NET 4.5 is used in both the application settings and the Web.Config file.</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="d2d47-230">"TypeError: &lt;hubType&gt;が定義されていません"エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-230">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="d2d47-231">このエラーが発生する呼び出し`MapSignalR`が正しく行われていません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-231">This error will result if the call to `MapSignalR` is not made properly.</span></span> <span data-ttu-id="d2d47-232">参照してください[SignalR ミドルウェアを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-232">See [How to register SignalR Middleware and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="d2d47-233">JsonSerializationException がユーザー コードでハンドルされませんでした。</span><span class="sxs-lookup"><span data-stu-id="d2d47-233">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="d2d47-234">パラメーター、メソッドに送信するには、シリアル化できない型 (ファイル ハンドル、データベース接続など) が含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-234">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="d2d47-235">使用 (またはセキュリティのためのシリアル化の理由から)、クライアントに送信したくないのサーバー側オブジェクトにメンバーを使用する必要がある場合、`JSONIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="d2d47-235">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="d2d47-236">"プロトコル エラー。不明なトランスポートは"エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-236">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="d2d47-237">このエラーは、クライアントは SignalR を使用するトランスポートをサポートしていない場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-237">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="d2d47-238">参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)についてを SignalR でブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-238">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="d2d47-239">「JavaScript ハブ プロキシの生成が無効になっています。」</span><span class="sxs-lookup"><span data-stu-id="d2d47-239">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="d2d47-240">このエラーが発生`DisableJavaScriptProxies`で動的に生成されたプロキシへの参照を含むも中に設定されている`signalr/hubs`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-240">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="d2d47-241">プロキシを手動で作成する方法の詳細については、次を参照してください。 [、生成されたプロキシとは何が](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-241">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="d2d47-242">「接続 ID は形式が正しくありません」または「ユーザー id は、SignalR のアクティブな接続中に変更できません」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-242">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="d2d47-243">このエラーは、認証を使用して、接続が停止する前に、クライアントがログアウトした場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-243">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="d2d47-244">ソリューションでは、クライアントをログアウトする前に SignalR 接続を停止します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-244">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="d2d47-245">"エラーをキャッチできません。SignalR: jQuery が見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="d2d47-245">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="d2d47-246">SignalR.js ファイルの前に jQuery が参照されていることを確認してください"のエラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-246">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="d2d47-247">SignalR JavaScript クライアントでは、jQuery を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-247">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="d2d47-248">JQuery への参照が使用されるパスが有効であるおよび SignalR への参照を前に jQuery への参照が正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-248">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="d2d47-249">"TypeError をキャッチできません。プロパティを読み取ることができません '&lt;プロパティ&gt;' 未定義の"エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-249">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="d2d47-250">このエラーは、jQuery またはハブ プロキシを適切に参照されていないことから発生します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-250">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="d2d47-251">JQuery、およびハブ プロキシへの参照が使用されるパスが有効であると、ハブ プロキシへの参照を前に jQuery への参照が正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-251">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="d2d47-252">ハブ プロキシを既定の参照は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-252">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="d2d47-253">**ハブ プロキシを正しく参照する HTML クライアント側のコード**</span><span class="sxs-lookup"><span data-stu-id="d2d47-253">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="d2d47-254">「RuntimeBinderException はユーザー コードで未処理でした」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-254">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="d2d47-255">このエラーが発生する場合の正しくないオーバー ロード`Hub.On`使用されます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-255">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="d2d47-256">メソッドの戻り値の場合、戻り値の型がジェネリック型パラメーターとして指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-256">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="d2d47-257">**(生成されたプロキシは) を使用せず、クライアントで定義されたメソッド**</span><span class="sxs-lookup"><span data-stu-id="d2d47-257">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="d2d47-258">接続 ID が一貫性のあるか、ページ読み込みの間で接続が切断</span><span class="sxs-lookup"><span data-stu-id="d2d47-258">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="d2d47-259">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-259">This behavior is by design.</span></span> <span data-ttu-id="d2d47-260">ハブ オブジェクトは、page オブジェクトでホストされている、ため、ハブは、ページが更新されると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-260">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="d2d47-261">複数ページのアプリケーションは、ページ読み込みの間の一貫性になるように、ユーザーと接続 Id 間の関連付けを維持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-261">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="d2d47-262">接続 Id は、いずれかで、サーバーに格納できる、`ConcurrentDictionary`オブジェクトまたはデータベース。</span><span class="sxs-lookup"><span data-stu-id="d2d47-262">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="d2d47-263">「値を null にすることはできません」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-263">"Value cannot be null" error</span></span>

<span data-ttu-id="d2d47-264">省略可能なパラメーターを持つサーバー側のメソッドは現在サポートされていません。省略可能なパラメーターを省略した場合、メソッドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-264">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="d2d47-265">詳細については、次を参照してください。[省略可能なパラメーター](https://github.com/SignalR/SignalR/issues/324)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-265">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="d2d47-266">"Firefox でサーバーへの接続を確立できません&lt;アドレス&gt;"Firebug でのエラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-266">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="d2d47-267">このエラー メッセージは、WebSocket トランスポートのネゴシエーションは失敗し、他のトランスポートが代わりに使用される場合、Firebug で確認できます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-267">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="d2d47-268">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-268">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="d2d47-269">.NET クライアント アプリケーションで「リモート証明書が検証の手順に従って無効です」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-269">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="d2d47-270">場合は、サーバーでは、要求が行われる前に、接続に x509certificate を追加することができますし、カスタムのクライアント証明書が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2d47-270">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="d2d47-271">接続を使用して、証明書を追加`Connection.AddClientCertificate`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-271">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="d2d47-272">認証のタイムアウト後の接続を削除します</span><span class="sxs-lookup"><span data-stu-id="d2d47-272">Connection drops after authentication times out</span></span>

<span data-ttu-id="d2d47-273">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-273">This behavior is by design.</span></span> <span data-ttu-id="d2d47-274">接続がアクティブになったときに、認証資格情報を変更することはできません。資格情報を更新するには、接続を停止および再起動してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-274">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="d2d47-275">OnConnected が jQuery Mobile を使用する場合に 2 回呼び出されます</span><span class="sxs-lookup"><span data-stu-id="d2d47-275">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="d2d47-276">Mobile を jQuery`initializePage`関数は、再実行するには、各ページのスクリプトは、2 番目の接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-276">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="d2d47-277">この問題のソリューションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-277">Solutions for this issue include:</span></span>

- <span data-ttu-id="d2d47-278">JQuery Mobile、JavaScript ファイルへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-278">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="d2d47-279">無効にする、`initializePage`関数を設定して`$.mobile.autoInitializePage = false`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-279">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="d2d47-280">接続を開始する前に初期化を完了してページを待機します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-280">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="d2d47-281">サーバー送信イベントを使用して Silverlight アプリケーションでメッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-281">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="d2d47-282">Silverlight でイベントを送信するサーバーを使用する場合、メッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-282">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="d2d47-283">長い代わりに使用するポーリングを強制するには、接続を開始するときに、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-283">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="d2d47-284">プロトコルのフレーム「アクセス許可が拒否されました」を使用して永久に</span><span class="sxs-lookup"><span data-stu-id="d2d47-284">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="d2d47-285">これは既知の問題で説明されている[ここ](https://github.com/SignalR/SignalR/issues/1963)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-285">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="d2d47-286">この現象は、最新 JQuery ライブラリを使用して表示する可能性があります。回避策では、JQuery 1.8.2 にアプリケーションをダウン グレードします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-286">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a><span data-ttu-id="d2d47-287">"InvalidOperationException:有効な web ソケット要求されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-287">"InvalidOperationException: Not a valid web socket request.</span></span>

<span data-ttu-id="d2d47-288">このエラーは、WebSocket プロトコルを使用すると、ネットワーク プロキシが要求ヘッダーを変更する場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-288">This error may occur if the WebSocket protocol is used, but the network proxy is modifying the request headers.</span></span> <span data-ttu-id="d2d47-289">ソリューションでは、ポート 80 で WebSocket を許可するプロキシを構成します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-289">The solution is to configure the proxy to allow WebSocket on port 80.</span></span>

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a><span data-ttu-id="d2d47-290">"例外:&lt;メソッド名&gt;メソッドを解決できませんでした"クライアントがサーバーでメソッドを呼び出すと</span><span class="sxs-lookup"><span data-stu-id="d2d47-290">"Exception: &lt;method name&gt; method could not be resolved" when client calls method on server</span></span>

<span data-ttu-id="d2d47-291">このエラーは、配列などの JSON ペイロードを検出できないデータ型の使用から発生します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-291">This error can result from using data types that cannot be discovered in a JSON payload, such as Array.</span></span> <span data-ttu-id="d2d47-292">回避策では、IList などの JSON で検出可能なデータ型を使用します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-292">The workaround is to use a data type that is discoverable by JSON, such as IList.</span></span> <span data-ttu-id="d2d47-293">詳細については、次を参照してください。 [.NET クライアントの配列パラメーターを持つハブ メソッドを呼び出すことができません](https://github.com/SignalR/SignalR/issues/2672)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-293">For more information, see [.NET Client unable to call hub methods with array parameters](https://github.com/SignalR/SignalR/issues/2672).</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="d2d47-294">コンパイルとサーバー側のエラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-294">Compilation and server-side errors</span></span>

 <span data-ttu-id="d2d47-295">次のセクションには、コンパイラとサーバー側のランタイム エラーの考えられる解決策が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d2d47-295">The following section contains possible solutions to compiler and server-side runtime errors.</span></span>

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="d2d47-296">ハブ インスタンスへの参照が null</span><span class="sxs-lookup"><span data-stu-id="d2d47-296">Reference to Hub instance is null</span></span>

<span data-ttu-id="d2d47-297">接続ごとにハブ インスタンスが作成された後できませんインスタンスを作成するハブのコードで自分でします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-297">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="d2d47-298">ハブ自体では、外部からハブのメソッドを呼び出すを参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)のハブ コンテキストへの参照を取得する方法。</span><span class="sxs-lookup"><span data-stu-id="d2d47-298">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="d2d47-299">HTTPContext.Current.Session が null</span><span class="sxs-lookup"><span data-stu-id="d2d47-299">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="d2d47-300">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-300">This behavior is by design.</span></span> <span data-ttu-id="d2d47-301">双方向メッセージング中断は、セッション状態を有効にするため、SignalR は ASP.NET セッション状態をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-301">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="d2d47-302">オーバーライドする適切なメソッドはありません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-302">No suitable method to override</span></span>

<span data-ttu-id="d2d47-303">以前のドキュメントまたはブログからのコードを使用している場合は、このエラーを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-303">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="d2d47-304">変更または非推奨とされているメソッドの名前を参照していないことを確認します (など`OnConnectedAsync`)。</span><span class="sxs-lookup"><span data-stu-id="d2d47-304">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="d2d47-305">HostContextExtensions.WebSocketServerUrl が null</span><span class="sxs-lookup"><span data-stu-id="d2d47-305">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="d2d47-306">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-306">This behavior is by design.</span></span> <span data-ttu-id="d2d47-307">このメンバーは非推奨とされますは使用できません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-307">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="d2d47-308">「'Signalr.hubs' という名前のルートは既にルート コレクションがいます」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-308">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="d2d47-309">場合、このエラーが発生する`MapSignalR`は、アプリケーションによって 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-309">This error will be seen if `MapSignalR` is called twice by your application.</span></span> <span data-ttu-id="d2d47-310">いくつかの例のアプリケーション呼び出し`MapSignalR`直接スタートアップ クラスで他のユーザー呼び出しを行うラッパー クラスにします。</span><span class="sxs-lookup"><span data-stu-id="d2d47-310">Some example applications call `MapSignalR` directly in the Startup class; others make the call in a wrapper class.</span></span> <span data-ttu-id="d2d47-311">アプリケーションはないこと両方を確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-311">Ensure that your application does not do both.</span></span>

### <a name="websocket-is-not-used"></a><span data-ttu-id="d2d47-312">WebSocket が使用されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-312">WebSocket is not used</span></span>

<span data-ttu-id="d2d47-313">サーバーとクライアントが WebSocket の要件を満たしていることを確認した場合 (記載、[サポートされているプラットフォーム](../getting-started/supported-platforms.md)ドキュメント)、サーバーで WebSocket を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-313">If you have verified that your server and clients meet the requirements for WebSocket (listed in the [Supported Platforms](../getting-started/supported-platforms.md) document), you will need to enable WebSocket on your server.</span></span> <span data-ttu-id="d2d47-314">これを行うための手順を参照して[ここ](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-314">Instructions for doing this can be found [here](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

### <a name="connection-is-undefined"></a><span data-ttu-id="d2d47-315">$.connection が定義されていません</span><span class="sxs-lookup"><span data-stu-id="d2d47-315">$.connection is undefined</span></span>

<span data-ttu-id="d2d47-316">このエラーは、ページ上のスクリプトが正常に読み込まれていないまたはハブ プロキシに到達できないか、正しくアクセスしていることを示します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-316">This error indicates that either the scripts on a page are not being loaded properly, or the hub proxy is not reachable or is being accessed incorrectly.</span></span> <span data-ttu-id="d2d47-317">ページのスクリプト参照がプロジェクトに読み込まれたスクリプトに対応していると、サーバーが実行されているときに、/signalr/hubs をブラウザーでアクセスできることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-317">Verify that the script references on your page correspond to the scripts loaded in your project, and that /signalr/hubs can be accessed in a browser when the server is running.</span></span>

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a><span data-ttu-id="d2d47-318">動的な式のコンパイルに必要な 1 つまたは複数の種類が見つかりません</span><span class="sxs-lookup"><span data-stu-id="d2d47-318">One or more types required to compile a dynamic expression cannot be found</span></span>

<span data-ttu-id="d2d47-319">このエラーには、ことを示します、`Microsoft.CSharp`ライブラリがありません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-319">This error indicates that the `Microsoft.CSharp` library is missing.</span></span> <span data-ttu-id="d2d47-320">追加することで、**アセンブリ -&gt;Framework**  タブ。</span><span class="sxs-lookup"><span data-stu-id="d2d47-320">Add it in the **Assemblies-&gt;Framework** tab.</span></span>

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a><span data-ttu-id="d2d47-321">Visual Basic または厳密に型指定されたハブ; Clients.Caller から呼び出し元の状態にアクセスできません。「型 'Task (Of Object)' を 'String' を型に変換が無効です」エラー</span><span class="sxs-lookup"><span data-stu-id="d2d47-321">Caller state cannot be accessed from Clients.Caller in Visual Basic or in a strongly typed hub; "Conversion from type 'Task(Of Object)' to type 'String' is not valid" error</span></span>

<span data-ttu-id="d2d47-322">Visual basic、または厳密に型指定されたハブに呼び出し元の状態にアクセスするには、使用、 `Clients.CallerState` (SignalR 2.1 で導入) の代わりにプロパティ`Clients.Caller`します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-322">To access caller state in Visual Basic or in a strongly typed hub, use the `Clients.CallerState` property (introduced in SignalR 2.1) instead of `Clients.Caller`.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="d2d47-323">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-323">Visual Studio issues</span></span>

<span data-ttu-id="d2d47-324">このセクションでは、Visual Studio で発生する問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-324">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="d2d47-325">ソリューション エクスプ ローラーでスクリプト ドキュメントのノードが表示されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-325">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="d2d47-326">チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで [スクリプト ドキュメント] ノードに送る。</span><span class="sxs-lookup"><span data-stu-id="d2d47-326">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="d2d47-327">このノードは、JavaScript デバッガーによって生成され、Internet explorer のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合は、ノードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-327">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="d2d47-328">JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。</span><span class="sxs-lookup"><span data-stu-id="d2d47-328">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="d2d47-329">SignalR では、Visual Studio 2008 またはそれ以前は機能しません</span><span class="sxs-lookup"><span data-stu-id="d2d47-329">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="d2d47-330">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="d2d47-330">This behavior is by design.</span></span> <span data-ttu-id="d2d47-331">SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降に SignalR アプリケーションを開発することが必要です。</span><span class="sxs-lookup"><span data-stu-id="d2d47-331">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span> <span data-ttu-id="d2d47-332">SignalR のサーバー コンポーネントでは、.NET Framework 4.5 が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2d47-332">The server component of SignalR requires .NET Framework 4.5.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="d2d47-333">IIS の問題</span><span class="sxs-lookup"><span data-stu-id="d2d47-333">IIS issues</span></span>

<span data-ttu-id="d2d47-334">このセクションには、インターネット インフォメーション サービスの問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d2d47-334">This section contains issues with Internet Information Services.</span></span>

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a><span data-ttu-id="d2d47-335">Visual Studio 開発サーバーで、IIS ではなく、SignalR works</span><span class="sxs-lookup"><span data-stu-id="d2d47-335">SignalR works on Visual Studio development server, but not in IIS</span></span>

<span data-ttu-id="d2d47-336">IIS 7.0 および 7.5、SignalR がサポートされていますが、サポート拡張子のない Url を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-336">SignalR is supported on IIS 7.0 and 7.5, but support for extensionless URLs must be added.</span></span> <span data-ttu-id="d2d47-337">拡張子のない Url のサポートを追加するを参照してください。 [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)</span><span class="sxs-lookup"><span data-stu-id="d2d47-337">To add support for extensionless URLs, see [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)</span></span>

<span data-ttu-id="d2d47-338">SignalR では、ASP.NET (ASP.NET がインストールされていない IIS で既定で) サーバーにインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2d47-338">SignalR requires ASP.NET to be installed on the server (ASP.NET is not installed on IIS by default).</span></span> <span data-ttu-id="d2d47-339">ASP.NET をインストールするを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-339">To install ASP.NET, see [ASP.NET Downloads](https://www.asp.net/downloads).</span></span>

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a><span data-ttu-id="d2d47-340">Microsoft Azure を発行します。</span><span class="sxs-lookup"><span data-stu-id="d2d47-340">Microsoft Azure issues</span></span>

<span data-ttu-id="d2d47-341">このセクションには、Microsoft Azure での問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d2d47-341">This section contains issues with Microsoft Azure.</span></span>

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a><span data-ttu-id="d2d47-342">FileLoadException Azure ワーカー ロールで SignalR をホストする場合</span><span class="sxs-lookup"><span data-stu-id="d2d47-342">FileLoadException when hosting SignalR in an Azure Worker Role</span></span>

<span data-ttu-id="d2d47-343">例外で、Azure Worker ロールで SignalR をホストしている可能性があります"ファイルまたはアセンブリを読み込むことができません ' Microsoft.Owin、バージョン 2.0.0.0 を ="。</span><span class="sxs-lookup"><span data-stu-id="d2d47-343">Hosting SignalR in an Azure Worker Role might result in the exception "Could not load file or assembly 'Microsoft.Owin, Version=2.0.0.0".</span></span> <span data-ttu-id="d2d47-344">これは、NuGet の既知の問題バインド リダイレクトは、Azure ワーカー ロール プロジェクトでは自動的に追加されません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-344">This is a known issue with NuGet; Binding redirects are not added automatically in Azure Worker Role projects.</span></span> <span data-ttu-id="d2d47-345">これを解決するには、バインド リダイレクトを手動で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="d2d47-345">To fix this, you can add the binding redirects manually.</span></span> <span data-ttu-id="d2d47-346">次の行を追加、`app.config`のワーカー ロール プロジェクトのファイル。</span><span class="sxs-lookup"><span data-stu-id="d2d47-346">Add the following lines to the `app.config` file for your Worker Role project.</span></span>

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="d2d47-347">メッセージがトピック名を変更した後は Azure のバック プレーンを介して受信されていません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-347">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="d2d47-348">Azure のバック プレーンで使用されるトピックが内部的に保持されます。ユーザー構成可能にするものではありません。</span><span class="sxs-lookup"><span data-stu-id="d2d47-348">The topics used by the Azure backplane are maintained internally; they are not intended to be user-configurable.</span></span>
