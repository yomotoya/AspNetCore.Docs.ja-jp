---
title: Redis ASP.NET Core SignalR スケール アウト バック プレーン
author: bradygaster
description: ASP.NET Core SignalR アプリのスケール アウトを有効にする Redis バック プレーンを設定する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: 9d2a942dba6abe669126efee7f2b3cdd6560658e
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012644"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="4bd44-103">ASP.NET Core SignalR スケール アウトの Redis のバック プレーンを設定します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="4bd44-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)、および[Tom Dykstra](https://github.com/tdykstra)、</span><span class="sxs-lookup"><span data-stu-id="4bd44-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="4bd44-105">この記事には、セットアップの SignalR に固有の側面がについて説明します、 [Redis](https://redis.io/) ASP.NET Core SignalR アプリのスケーリングに使用するサーバー。</span><span class="sxs-lookup"><span data-stu-id="4bd44-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="4bd44-106">Redis のバック プレーンを設定します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="4bd44-107">Redis サーバーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4bd44-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="4bd44-108">運用環境で使用、Redis のバック プレーンは SignalR アプリと同じデータ センターでの実行時にのみお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4bd44-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="4bd44-109">それ以外の場合、ネットワーク待機時間には、パフォーマンスが低下します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="4bd44-110">SignalR アプリが Azure クラウドで実行している場合は、Redis のバック プレーンではなく Azure SignalR サービスを推奨します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="4bd44-111">開発用の Azure Redis Cache Service を使用し、環境をテストできます。</span><span class="sxs-lookup"><span data-stu-id="4bd44-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="4bd44-112">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4bd44-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="4bd44-113">Redis のドキュメント</span><span class="sxs-lookup"><span data-stu-id="4bd44-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="4bd44-114">Azure Redis Cache のドキュメント</span><span class="sxs-lookup"><span data-stu-id="4bd44-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="4bd44-115">SignalR アプリでは、インストール、 `Microsoft.AspNetCore.SignalR.Redis` NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="4bd44-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="4bd44-116">(も、`Microsoft.AspNetCore.SignalR.StackExchangeRedis`パッケージ化、ASP.NET Core 2.2 以降であることができます)。</span><span class="sxs-lookup"><span data-stu-id="4bd44-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="4bd44-117">`Startup.ConfigureServices`メソッドを呼び出します`AddRedis`後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="4bd44-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="4bd44-118">必要に応じてオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="4bd44-119">接続文字列またはほとんどのオプションを設定することができます、 [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4bd44-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="4bd44-120">指定されたオプション`ConfigurationOptions`接続文字列に設定されたポリシーに優先します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="4bd44-121">次の例では、オプションを設定する方法を示しています、`ConfigurationOptions`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4bd44-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="4bd44-122">この例プレフィックスを追加しますチャネル複数のアプリが同じ Redis インスタンスを共有できるように、次の手順で説明したようです。</span><span class="sxs-lookup"><span data-stu-id="4bd44-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="4bd44-123">上記のコードで`options.Configuration`接続文字列で指定された内容で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="4bd44-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="4bd44-124">SignalR アプリケーションでは、次の NuGet パッケージのいずれかをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4bd44-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="4bd44-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` -StackExchange.Redis 2.X.X に依存します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="4bd44-126">これは、ASP.NET Core 2.2 以降に推奨されるパッケージです。</span><span class="sxs-lookup"><span data-stu-id="4bd44-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="4bd44-127">`Microsoft.AspNetCore.SignalR.Redis` -StackExchange.Redis 1.X.X に依存します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="4bd44-128">このパッケージは、ASP.NET Core 3.0 に発送されません。</span><span class="sxs-lookup"><span data-stu-id="4bd44-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="4bd44-129">`Startup.ConfigureServices`メソッドを呼び出します`AddStackExchangeRedis`後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="4bd44-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="4bd44-130">必要に応じてオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="4bd44-131">接続文字列またはほとんどのオプションを設定することができます、 [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4bd44-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="4bd44-132">指定されたオプション`ConfigurationOptions`接続文字列に設定されたポリシーに優先します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="4bd44-133">次の例では、オプションを設定する方法を示しています、`ConfigurationOptions`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4bd44-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="4bd44-134">この例プレフィックスを追加しますチャネル複数のアプリが同じ Redis インスタンスを共有できるように、次の手順で説明したようです。</span><span class="sxs-lookup"><span data-stu-id="4bd44-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="4bd44-135">上記のコードで`options.Configuration`接続文字列で指定された内容で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="4bd44-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="4bd44-136">Redis のオプションについては、次を参照してください。、 [StackExchange Redis ドキュメント](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="4bd44-137">SignalR の複数のアプリを 1 つの Redis サーバーを使用している場合は、SignalR アプリごとに異なるチャネル プレフィックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="4bd44-138">別のチャネルのプレフィックスを使用する他のユーザーからの 1 つの SignalR アプリを分離するチャネルのプレフィックスを設定します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="4bd44-139">異なるプレフィックスを割り当てない場合、すべての独自のクライアントに 1 つのアプリから送信されたメッセージは、バック プレーンとしての Redis サーバーを使用するすべてのアプリのすべてのクライアントに送られます。</span><span class="sxs-lookup"><span data-stu-id="4bd44-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="4bd44-140">ソフトウェアを分散スティッキー セッションをサーバー ファームの負荷を構成します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="4bd44-141">その方法に関するドキュメントのいくつかの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="4bd44-142">IIS</span><span class="sxs-lookup"><span data-stu-id="4bd44-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="4bd44-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="4bd44-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="4bd44-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="4bd44-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="4bd44-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="4bd44-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="4bd44-146">Redis サーバーのエラー</span><span class="sxs-lookup"><span data-stu-id="4bd44-146">Redis server errors</span></span>

<span data-ttu-id="4bd44-147">Redis サーバーがダウンしたときに、SignalR は、メッセージは配信されませんを示す例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="4bd44-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="4bd44-148">いくつかの一般的な例外メッセージ:</span><span class="sxs-lookup"><span data-stu-id="4bd44-148">Some typical exception messages:</span></span>

* <span data-ttu-id="4bd44-149">*失敗した書き込みメッセージ*</span><span class="sxs-lookup"><span data-stu-id="4bd44-149">*Failed writing message*</span></span>
* <span data-ttu-id="4bd44-150">*'MethodName' のハブ メソッドの呼び出しに失敗しました*</span><span class="sxs-lookup"><span data-stu-id="4bd44-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="4bd44-151">*Redis に接続できませんでした。*</span><span class="sxs-lookup"><span data-stu-id="4bd44-151">*Connection to Redis failed*</span></span>

<span data-ttu-id="4bd44-152">SignalR では、サーバーが再度起動するときに送信するメッセージをバッファーしません。</span><span class="sxs-lookup"><span data-stu-id="4bd44-152">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="4bd44-153">Redis サーバーの停止中に送信されたメッセージは失われます。</span><span class="sxs-lookup"><span data-stu-id="4bd44-153">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="4bd44-154">SignalR では、Redis サーバーが再び使用可能なときに自動的に再接続します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-154">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="4bd44-155">接続エラーのカスタム動作</span><span class="sxs-lookup"><span data-stu-id="4bd44-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="4bd44-156">Redis 接続の失敗イベントを処理する方法を示す例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="4bd44-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="clustering"></a><span data-ttu-id="4bd44-157">クラスタリング</span><span class="sxs-lookup"><span data-stu-id="4bd44-157">Clustering</span></span>

<span data-ttu-id="4bd44-158">クラスタ リングは、複数の Redis サーバーを使用して高可用性を実現するためのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="4bd44-158">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="4bd44-159">正式にクラスタ リングはサポートされていませんが、機能があります。</span><span class="sxs-lookup"><span data-stu-id="4bd44-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bd44-160">次の手順</span><span class="sxs-lookup"><span data-stu-id="4bd44-160">Next steps</span></span>

<span data-ttu-id="4bd44-161">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4bd44-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="4bd44-162">Redis のドキュメント</span><span class="sxs-lookup"><span data-stu-id="4bd44-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="4bd44-163">StackExchange Redis ドキュメント</span><span class="sxs-lookup"><span data-stu-id="4bd44-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="4bd44-164">Azure Redis Cache のドキュメント</span><span class="sxs-lookup"><span data-stu-id="4bd44-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
