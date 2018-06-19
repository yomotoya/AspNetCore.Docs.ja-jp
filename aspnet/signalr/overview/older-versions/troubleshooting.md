---
uid: signalr/overview/older-versions/troubleshooting
title: SignalR のトラブルシューティング (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: この記事では、SignalR アプリケーションを開発の一般的な問題について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505821"
---
<a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="5a1dd-103">SignalR のトラブルシューティング (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="5a1dd-103">SignalR Troubleshooting (SignalR 1.x)</span></span>
====================
<span data-ttu-id="5a1dd-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5a1dd-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="5a1dd-105">このドキュメントでは、SignalR の一般的な問題のトラブルシューティングについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-105">This document describes common troubleshooting issues with SignalR.</span></span>


<span data-ttu-id="5a1dd-106">このドキュメントには、次のセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="5a1dd-107">サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="5a1dd-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="5a1dd-108">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="5a1dd-109">コンパイル、およびサーバー側エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="5a1dd-110">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="5a1dd-111">インターネット インフォメーション サービスの問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="5a1dd-112">Azure の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="5a1dd-113">サイレント モードで失敗のクライアントとサーバー間でメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="5a1dd-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="5a1dd-114">このセクションでは、メソッドの呼び出しがわかりやすいエラー メッセージを表示せずに失敗するには、クライアントとサーバー間の考えられる原因について説明します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="5a1dd-115">SignalR のアプリケーションで、サーバー情報がありません。 クライアントを実装する方法の詳細サーバーでは、クライアント メソッドを呼び出すとメソッドの名前とパラメーターのデータは、クライアントに送信されます、サーバーが指定した形式で存在する場合にのみ、メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="5a1dd-116">一致するメソッドが検出されないクライアントでは、何も起こりません、サーバー上のエラー メッセージは発生しません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="5a1dd-117">が呼び出されたクライアント メソッドをさらに調査するには、どのような呼び出しを表示するハブの start メソッドが、サーバーから送信されて呼び出しの前にログ記録をオンにすることができません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="5a1dd-118">JavaScript アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (JavaScript クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="5a1dd-119">.NET クライアント アプリケーションでのログ記録を有効にするのを参照してください。[クライアント側のログ記録 (.NET クライアントのバージョン) を有効にする方法](../guide-to-the-api/hubs-api-guide-net-client.md#logging)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="5a1dd-120">スペル ミスがあるメソッド、不適切なメソッドのシグネチャまたは不適切なハブ名</span><span class="sxs-lookup"><span data-stu-id="5a1dd-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="5a1dd-121">名前または呼び出されたメソッドのシグネチャが一致しない場合、クライアントに適切なメソッド、呼び出しは失敗します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="5a1dd-122">サーバーによって呼び出されるメソッドの名前が、クライアントのメソッドの名前と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="5a1dd-123">SignalR による camel 形式のメソッドを使用するハブ プロキシの作成も、JavaScript では、そのため、メソッドと呼ばれます`SendMessage`サーバーでが呼び出されます`sendMessage`クライアント プロキシでします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="5a1dd-124">使用する場合、`HubName`サーバー側コードで属性を使用する名前が、クライアントで、ハブの作成に使用する名前と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="5a1dd-125">使用しない場合、`HubName`属性、JavaScript クライアントのハブの名前が camel 形式の ChatHub ではなく chatHub などであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="5a1dd-126">クライアント上の重複したメソッド名</span><span class="sxs-lookup"><span data-stu-id="5a1dd-126">Duplicate method name on client</span></span>

<span data-ttu-id="5a1dd-127">大文字小文字によってのみとは異なるクライアントのメソッドの重複がないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="5a1dd-128">場合は、クライアント アプリケーションがある呼び出されるメソッド`sendMessage`、いないという名前のメソッドもを確認して`SendMessage`もします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="5a1dd-129">クライアントで JSON パーサーが不足しています。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="5a1dd-130">SignalR には、JSON パーサーは、サーバーとクライアント間の呼び出しをシリアル化に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="5a1dd-131">クライアントには、Internet Explorer 7) などの組み込み JSON パーサーが割り当てられていないは、アプリケーションで 1 つ含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="5a1dd-132">JSON パーサーをダウンロードする[ここ](http://nuget.org/packages/json2)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="5a1dd-133">ハブと PersistentConnection 構文を混在させる</span><span class="sxs-lookup"><span data-stu-id="5a1dd-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="5a1dd-134">SignalR が 2 つの通信モデルを使用します。 ハブおよび PersistentConnections です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="5a1dd-135">これら 2 つの通信のモデルを呼び出すための構文は、クライアント コードで異なります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="5a1dd-136">サーバー コードに、ハブを追加した場合は、適切なハブの構文を使用してすべてのクライアント コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="5a1dd-137">**JavaScript クライアント コード、JavaScript クライアントで、PersistentConnection を作成します。**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="5a1dd-138">**JavaScript クライアント コードで、Javascript クライアント ハブ プロキシを作成します。**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="5a1dd-139">**C# サーバー コード、PersistentConnection にルートをマップします。**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="5a1dd-140">**複数のアプリケーションがある場合、ハブ、または複数のハブにルートをマップ c# サーバー コード**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-140">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="5a1dd-141">サブスクリプションが追加される前に開始した接続</span><span class="sxs-lookup"><span data-stu-id="5a1dd-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="5a1dd-142">プロキシ サーバーから呼び出すことができるメソッドが追加される前に、ハブの接続が開始されている場合、メッセージは受信されません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="5a1dd-143">次の JavaScript コードは、ハブを正しく開始できません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="5a1dd-144">**ハブのメッセージを受信することはできません。 JavaScript クライアント コードが正しくないです。**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="5a1dd-145">代わりに、開始を呼び出す前に、メソッドのサブスクリプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="5a1dd-146">**正しくハブにサブスクリプションを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="5a1dd-147">ハブ プロキシのメソッド名がありません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="5a1dd-148">クライアントで、サーバーで定義されているメソッドにサブスクライブを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="5a1dd-149">メソッドを定義して、サーバー、場合でも、クライアント プロキシを引き続き追加できます必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="5a1dd-150">次の方法でクライアントのプロキシ メソッドを追加することができます (注メソッドに追加される、`client`ハブ、いないハブ直接のメンバー)。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="5a1dd-151">**ハブ プロキシのメソッドを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="5a1dd-152">ハブまたはハブのメソッドがパブリックとして宣言されていません</span><span class="sxs-lookup"><span data-stu-id="5a1dd-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="5a1dd-153">クライアントに表示する、ハブの実装とメソッドとして宣言されなければなりません`public`です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="5a1dd-154">別のアプリケーションからハブにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-154">Accessing hub from a different application</span></span>

<span data-ttu-id="5a1dd-155">SignalR ハブは、SignalR クライアントを実装するアプリケーションを介してのみアクセスできることができます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="5a1dd-156">SignalR は、その他の通信ライブラリ (SOAP または WCF web サービスなどです。) と相互運用することはできません。ターゲット プラットフォームの利用可能な SignalR クライアントがない場合は、サーバーのエンドポイントを直接アクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="5a1dd-157">手動でデータをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-157">Manually serializing data</span></span>

<span data-ttu-id="5a1dd-158">SignalR は自動的に JSON シリアル化に使用、メソッド パラメーター-がのしなくても自分で行います。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="5a1dd-159">リモートのハブ メソッドをクライアントの OnDisconnected 関数には実行されません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="5a1dd-160">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-160">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-161">ときに`OnDisconnected`が呼び出されると、ハブが既に入力、`Disconnected`が許可されていないさらにハブ メソッドを呼び出せる状態です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="5a1dd-162">**C# サーバー コードを正しく OnDisconnected イベント内のコードを実行**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="5a1dd-163">接続の制限に達しました</span><span class="sxs-lookup"><span data-stu-id="5a1dd-163">Connection limit reached</span></span>

<span data-ttu-id="5a1dd-164">を Windows 7 などのクライアント オペレーティング システムで、フル バージョンの IIS を使用する場合は、10 接続制限が適用されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="5a1dd-165">クライアントの OS を使用して、IIS Express 代わりに使用してこの制限を回避します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="5a1dd-166">ドメイン間の接続が正しく設定されていません</span><span class="sxs-lookup"><span data-stu-id="5a1dd-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="5a1dd-167">接続がドメインを越えた場合 (対象の SignalR URL がホストするページと同じドメインに接続) が正しくセットアップされていない、エラーが発生せず、接続が失敗します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="5a1dd-168">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="5a1dd-169">.NET クライアントで作業していない NTLM (Active Directory) を使用して接続</span><span class="sxs-lookup"><span data-stu-id="5a1dd-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="5a1dd-170">ドメインのセキュリティを使用する .NET クライアント アプリケーション内の接続には、接続が正しく構成されていない場合があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="5a1dd-171">SignalR を使用して、ドメイン環境では、次のように、必要な接続プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="5a1dd-172">**C# クライアントを実装するコード接続の資格情報**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="5a1dd-173">その他の接続の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-173">Other connection issues</span></span>

<span data-ttu-id="5a1dd-174">このセクションでは、原因と解決策の特定の現象または接続中に発生するエラー メッセージについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="5a1dd-175">「開始にはデータを送信する前に呼び出す必要があります」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="5a1dd-176">接続を開始する前に、コードが SignalR オブジェクトを参照している場合、このエラーは発生一般的です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="5a1dd-177">ワイヤアップ ハンドラーなどのメソッドの呼び出しに、サーバーで定義されている必要があります追加することが、接続が完了した後にします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="5a1dd-178">なおへの呼び出し`Start`はその前に呼び出しを実行することが後にコードが完了するために非同期。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="5a1dd-179">接続が完全に開始した後にハンドラーを追加する最善の方法は、start メソッドにパラメーターとして渡されるコールバック関数には。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="5a1dd-180">**正しく SignalR オブジェクトを参照するイベント ハンドラーを追加する JavaScript クライアント コード**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="5a1dd-181">このエラーは、SignalR オブジェクトが参照されているときに、接続が停止した場合にも表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="5a1dd-182">「301 が完全に移動しました」または「302 は一時的に移動されました」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="5a1dd-183">このエラーは、プロジェクトには、SignalR で、自動的に作成されたプロキシには影響をという名前のフォルダーが含まれている場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="5a1dd-184">このエラーを避けるためには、使用しないという名前のフォルダー`SignalR`アプリケーション、または有効にするプロキシの自動生成をオフにします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="5a1dd-185">参照してください[、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="5a1dd-186">.NET または Silverlight クライアントで「403 アクセス不可」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="5a1dd-187">このエラーは、ドメイン間の通信が正しく有効化されませんクロス ドメイン環境で発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="5a1dd-188">ドメイン間の通信を有効にする方法については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="5a1dd-189">Silverlight クライアントで、ドメイン間の接続を確立する、次を参照してください。 [Silverlight クライアントからドメイン間の接続](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="5a1dd-190">「404 見つかりません」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-190">"404 Not Found" error</span></span>

<span data-ttu-id="5a1dd-191">この問題のいくつかの原因があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-191">There are several causes for this issue.</span></span> <span data-ttu-id="5a1dd-192">次のすべてを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-192">Verify all of the following:</span></span>

- <span data-ttu-id="5a1dd-193">**ハブ プロキシ アドレスの参照が正しくフォーマットされていません:** このエラーは生成されたハブ プロキシのアドレスへの参照が正しくフォーマットされていない場合も表示されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="5a1dd-194">ハブのアドレスへの参照が正しく行われたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="5a1dd-195">参照してください[を動的に生成されたプロキシを参照する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="5a1dd-196">**ハブ ルートを追加する前に、アプリケーションへのルートの追加:** 場合、アプリケーションでは、他のルートを使用することを確認追加された最初のルートへの呼び出し`MapHubs`です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="5a1dd-197">「500 内部サーバー エラー」</span><span class="sxs-lookup"><span data-stu-id="5a1dd-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="5a1dd-198">これは、さまざまな原因の可能性がある非常に一般的なエラーです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="5a1dd-199">エラーの詳細は、サーバーのイベント ログに記録する必要があります。 またはサーバーのデバッグから確認できます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="5a1dd-200">サーバーの詳細なエラーを有効にして、詳細なエラー情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="5a1dd-201">詳細については、次を参照してください。[ハブ クラスのエラーを処理する方法](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="5a1dd-202">"TypeError: &lt;hubType&gt;が定義されていません"エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="5a1dd-203">このエラーが発生する呼び出し`MapHubs`が正しく行われない。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="5a1dd-204">参照してください[SignalR のルートを登録し、SignalR のオプションを構成する方法](../guide-to-the-api/hubs-api-guide-server.md#route)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="5a1dd-205">JsonSerializationException がユーザー コードによってハンドルされませんでした。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="5a1dd-206">メソッドに送信するパラメーターには、シリアル化不可能な (のような種類ファイル ハンドルやデータベース接続) にはが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="5a1dd-207">使用 (またはセキュリティのためのシリアル化の理由から)、クライアントに送信したくない、サーバー側オブジェクトにメンバーを使用する必要がある場合、`JSONIgnore`属性。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="5a1dd-208">"プロトコル エラー: 不明なトランスポート"エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="5a1dd-209">このエラーは、クライアントは、SignalR を使用するトランスポートをサポートしていない場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="5a1dd-210">参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)についてを SignalR でブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="5a1dd-211">「JavaScript Hub プロキシ生成は無効になっています。」</span><span class="sxs-lookup"><span data-stu-id="5a1dd-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="5a1dd-212">このエラーが発生`DisableJavaScriptProxies`で動的に生成されたプロキシへの参照を含めているときに設定されている`signalr/hubs`です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="5a1dd-213">手動でプロキシを作成する方法の詳細については、次を参照してください。 [、生成されたプロキシとそれが何を](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="5a1dd-214">"接続 ID の形式が正しくありません"または「アクティブな SignalR 接続中に、ユーザー id を変更できません」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="5a1dd-215">このエラーは、認証が使用されていて、クライアントがログアウトしている接続を停止する前に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="5a1dd-216">ソリューションは、クライアントをログアウトする前に、SignalR 接続を停止するためです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="5a1dd-217">"エラーをキャッチしませんでした: SignalR: jQuery が見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="5a1dd-218">JQuery が SignalR.js ファイルの前に参照されていることを確認してください。"エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="5a1dd-219">SignalR JavaScript クライアントには、jQuery を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="5a1dd-220">JQuery への参照が使用されるパスが有効であることと jQuery への参照が SignalR への参照の前に、正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="5a1dd-221">"TypeError をキャッチしませんでしたプロパティを読み取ることができません '&lt;プロパティ&gt;' 未定義の"エラー。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="5a1dd-222">このエラーは、jQuery またはハブ プロキシが適切に参照されていないことから発生します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="5a1dd-223">JQuery、およびハブ プロキシへの参照が使用されるパスが有効であることと jQuery への参照がハブ プロキシへの参照の前に、正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="5a1dd-224">ハブ プロキシへの参照を既定値は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="5a1dd-225">**ハブ プロキシを正しく参照する HTML クライアント側コード**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="5a1dd-226">「ユーザー コードによってハンドルされませんでした。 RuntimeBinderException」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="5a1dd-227">このエラーが発生するときの正しくないオーバー ロード`Hub.On`を使用します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="5a1dd-228">メソッドの戻り値が、戻り値の型がジェネリック型パラメーターとして指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="5a1dd-229">**(生成されたプロキシは) なしのクライアントに定義されたメソッド**</span><span class="sxs-lookup"><span data-stu-id="5a1dd-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="5a1dd-230">接続 ID が一貫性のあるか、ページの読み込みの間で接続が切断</span><span class="sxs-lookup"><span data-stu-id="5a1dd-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="5a1dd-231">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-231">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-232">ハブ オブジェクトが、page オブジェクトにホストされているため、ハブは、ページが更新されると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="5a1dd-233">複数ページ アプリケーションは、ページの読み込みの間の一貫性ができるように、ユーザーと接続 Id 間の関連付けを維持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="5a1dd-234">接続 Id は、サーバーのいずれかに格納できる、`ConcurrentDictionary`オブジェクトまたはデータベース。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="5a1dd-235">「値を null にすることはできません」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-235">"Value cannot be null" error</span></span>

<span data-ttu-id="5a1dd-236">省略可能なパラメーターを使用してサーバー側メソッドは現在サポートされていません。省略可能なパラメーターを省略すると、メソッドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="5a1dd-237">詳細については、次を参照してください。[省略可能なパラメーター](https://github.com/SignalR/SignalR/issues/324)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="5a1dd-238">"Firefox でサーバーへの接続を確立できません&lt;アドレス&gt;"Firebug のエラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="5a1dd-239">WebSocket トランスポートのネゴシエーションは失敗し、別のトランスポートが代わりに使用される場合、Firebug でこのエラー メッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="5a1dd-240">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="5a1dd-241">.NET クライアント アプリケーションで「リモート証明書は、検証手順に従った有効な」エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="5a1dd-242">場合は、サーバーでは、要求が行われる前に、接続に x509certificate を追加することができますし、カスタム クライアント証明書が必要です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="5a1dd-243">接続を使用して、証明書を追加`Connection.AddClientCertificate`です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="5a1dd-244">接続の認証のタイムアウト後のドロップします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="5a1dd-245">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-245">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-246">接続がアクティブになったときに、認証の資格情報を変更することはできません。資格情報を更新するには、接続を停止および再起動して必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="5a1dd-247">OnConnected は jQuery Mobile を使用する場合に 2 回呼び出されます</span><span class="sxs-lookup"><span data-stu-id="5a1dd-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="5a1dd-248">jQuery Mobile の`initializePage`関数は、再実行するには、各ページのスクリプトは、2 番目の接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="5a1dd-249">この問題の解決策は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="5a1dd-250">JQuery Mobile、JavaScript ファイルの前にへの参照が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="5a1dd-251">無効にする、`initializePage`関数を設定して`$.mobile.autoInitializePage = false`です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="5a1dd-252">接続を開始する前に初期化を完了してページを待機します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="5a1dd-253">サーバー送信イベントを使用する Silverlight アプリケーションでメッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="5a1dd-254">Silverlight でイベントを送信するサーバーを使用するときに、メッセージが遅延します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="5a1dd-255">長いポーリングが代わりに使用するには、接続を開始するときに、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="5a1dd-256">プロトコルを永久にフレーム「アクセス許可が拒否されました」を使用します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="5a1dd-257">これは、説明されている既知の問題[ここ](https://github.com/SignalR/SignalR/issues/1963)です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="5a1dd-258">この現象は、最新の JQuery ライブラリ; を使用して表示することがあります。回避策では、JQuery 1.8.2 にアプリをダウン グレードします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="5a1dd-259">コンパイル、およびサーバー側エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="5a1dd-260">次のセクションには、コンパイラとサーバー側のランタイム エラーの考えられる解決策が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="5a1dd-261">ハブ インスタンスへの参照が null</span><span class="sxs-lookup"><span data-stu-id="5a1dd-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="5a1dd-262">ハブ インスタンスは接続ごとに作成される、ためにできませんインスタンスを作成するハブのコードで自分でします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="5a1dd-263">ハブ自体の外部からのハブのメソッドを呼び出すを参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)のハブ コンテキストへの参照を取得する方法です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="5a1dd-264">HTTPContext.Current.Session が null です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="5a1dd-265">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-265">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-266">双方向のメッセージング中断は、セッションの状態を有効にするため、SignalR は ASP.NET セッション状態をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="5a1dd-267">オーバーライドする適切なメソッドはありません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-267">No suitable method to override</span></span>

<span data-ttu-id="5a1dd-268">以前のドキュメントまたはブログからのコードを使用している場合は、このエラーを表示することがあります。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="5a1dd-269">変更されたか推奨されなくなりましたがメソッドの名前を参照していないことを確認してください (と同様に`OnConnectedAsync`)。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="5a1dd-270">HostContextExtensions.WebSocketServerUrl が null です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="5a1dd-271">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-271">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-272">このメンバーは推奨されておらずは使用できません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="5a1dd-273">「'Signalr.hubs' という名前のルートは既にルートのコレクションに」エラー</span><span class="sxs-lookup"><span data-stu-id="5a1dd-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="5a1dd-274">場合は、このエラーが発生する`MapHubs`は、アプリケーションによって 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="5a1dd-275">いくつかの呼び出しの例のアプリケーション`MapHubs`ラッパー クラスでの呼び出しを行う他のユーザーのグローバルなアプリケーション ファイルに直接できます。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="5a1dd-276">あるアプリケーションで実行しない両方を確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="5a1dd-277">Visual Studio の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-277">Visual Studio issues</span></span>

<span data-ttu-id="5a1dd-278">このセクションでは、Visual Studio で発生する問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="5a1dd-279">ソリューション エクスプ ローラーでスクリプト ドキュメント ノードが表示されません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="5a1dd-280">チュートリアルの一部を実行するデバッグ中にソリューション エクスプ ローラーで、[スクリプト ドキュメント] ノードに直接します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="5a1dd-281">このノードは、JavaScript デバッガーによって生成され、Internet Explorer; 内のブラウザー クライアントのデバッグ中にのみ表示されます。Chrome または Firefox を使用する場合、ノードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="5a1dd-282">JavaScript デバッガーも実行されません、Silverlight デバッガーなど、別のクライアントのデバッガーが実行されている場合。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="5a1dd-283">SignalR では、Visual Studio 2008 またはそれ以前は機能しません</span><span class="sxs-lookup"><span data-stu-id="5a1dd-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="5a1dd-284">この動作は意図されたものです。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-284">This behavior is by design.</span></span> <span data-ttu-id="5a1dd-285">SignalR には、.NET Framework 4 以降が必要です。これは、Visual Studio 2010 以降で SignalR アプリケーションを開発することが必要です。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="5a1dd-286">IIS の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-286">IIS issues</span></span>

<span data-ttu-id="5a1dd-287">このセクションには、インターネット インフォメーション サービスで問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="5a1dd-288">MapHubs 呼び出し後に、web サイトがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="5a1dd-289">この問題は、SignalR の最新のバージョンで修正されました。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="5a1dd-290">NuGet を使用して、インストールを更新することによって、SignalR の最新のリリース バージョンを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="5a1dd-291">Azure の問題</span><span class="sxs-lookup"><span data-stu-id="5a1dd-291">Azure issues</span></span>

<span data-ttu-id="5a1dd-292">このセクションには、Microsoft Azure での問題が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="5a1dd-293">メッセージがトピック名を変更した後、Azure のバック プレーンから受信されていません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="5a1dd-294">Azure バック プレーンで使用されるトピックは、ユーザー構成可能にするものではありません。</span><span class="sxs-lookup"><span data-stu-id="5a1dd-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
