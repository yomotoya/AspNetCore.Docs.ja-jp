---
title: Redis ASP.NET Core SignalR スケール アウト バック プレーン
author: tdykstra
description: ASP.NET Core SignalR アプリのスケール アウトを有効にする Redis バック プレーンを設定する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52452985"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>ASP.NET Core SignalR スケール アウトの Redis のバック プレーンを設定します。

によって[Andrew Stanton-nurse](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)、および[Tom Dykstra](https://github.com/tdykstra)、

この記事には、セットアップの SignalR に固有の側面がについて説明します、 [Redis](https://redis.io/) ASP.NET Core SignalR アプリのスケーリングに使用するサーバー。

## <a name="set-up-a-redis-backplane"></a>Redis のバック プレーンを設定します。

* Redis サーバーをデプロイします。

  運用環境で使用、Redis バック プレーンは、オンプレミスのインフラストラクチャに対してのみお勧めします。 待機時間を最小限に抑えるには、Redis サーバーは、SignalR のアプリと同じデータ センターでなければなりません。 SignalR アプリが Azure クラウドで実行している場合は、Redis のバック プレーンではなく Azure SignalR サービスを推奨します。 開発用の Azure Redis Cache Service を使用し、環境をテストできます。 詳細については、次のリソースを参照してください。

  * <xref:signalr/scale>
  * [Redis のドキュメント](https://redis.io/)
  * [Azure Redis Cache のドキュメント](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* SignalR アプリでは、インストール、 `Microsoft.AspNetCore.SignalR.Redis` NuGet パッケージ。

* `Startup.ConfigureServices`メソッドを呼び出します`AddRedis`後`AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* 必要に応じてオプションを構成します。
 
  接続文字列またはほとんどのオプションを設定することができます、 [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)オブジェクト。 指定されたオプション`ConfigurationOptions`接続文字列に設定されたポリシーに優先します。

  次の例では、オプションを設定する方法を示しています、`ConfigurationOptions`オブジェクト。 この例プレフィックスを追加しますチャネル複数のアプリが同じ Redis インスタンスを共有できるように、次の手順で説明したようです。

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  上記のコードで`options.Configuration`接続文字列で指定された内容で初期化されます。

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* SignalR アプリでは、インストール、 `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet パッケージ。

* `Startup.ConfigureServices`メソッドを呼び出します`AddStackExchangeRedis`後`AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* 必要に応じてオプションを構成します。
 
  接続文字列またはほとんどのオプションを設定することができます、 [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)オブジェクト。 指定されたオプション`ConfigurationOptions`接続文字列に設定されたポリシーに優先します。

  次の例では、オプションを設定する方法を示しています、`ConfigurationOptions`オブジェクト。 この例プレフィックスを追加しますチャネル複数のアプリが同じ Redis インスタンスを共有できるように、次の手順で説明したようです。

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  上記のコードで`options.Configuration`接続文字列で指定された内容で初期化されます。

  Redis のオプションについては、次を参照してください。、 [StackExchange Redis ドキュメント](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)します。

::: moniker-end

* SignalR の複数のアプリを 1 つの Redis サーバーを使用している場合は、SignalR アプリごとに異なるチャネル プレフィックスを使用します。

  別のチャネルのプレフィックスを使用する他のユーザーからの 1 つの SignalR アプリを分離するチャネルのプレフィックスを設定します。 異なるプレフィックスを割り当てない場合、すべての独自のクライアントに 1 つのアプリから送信されたメッセージは、バック プレーンとしての Redis サーバーを使用するすべてのアプリのすべてのクライアントに送られます。

* ソフトウェアを分散スティッキー セッションをサーバー ファームの負荷を構成します。 その方法に関するドキュメントのいくつかの例を次に示します。

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Redis サーバーのエラー

Redis サーバーがダウンしたときに、SignalR は、メッセージは配信されませんを示す例外をスローします。 いくつかの一般的な例外メッセージ:

* *失敗した書き込みメッセージ*
* *'MethodName' のハブ メソッドの呼び出しに失敗しました*
* *Redis に接続できませんでした。*

SignalR では、サーバーが再度起動するときに送信するメッセージをバッファーしません。 Redis サーバーの停止中に送信されたメッセージは失われます。

SignalR では、Redis サーバーが再び使用可能なときに自動的に再接続します。

### <a name="custom-behavior-for-connection-failures"></a>接続エラーのカスタム動作

Redis 接続の失敗イベントを処理する方法を示す例を次に示します。

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

## <a name="clustering"></a>クラスタリング

クラスタ リングは、複数の Redis サーバーを使用して高可用性を実現するためのメソッドです。 正式にクラスタ リングはサポートされていませんが、機能があります。

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* <xref:signalr/scale>
* [Redis のドキュメント](https://redis.io/documentation)
* [StackExchange Redis ドキュメント](https://stackexchange.github.io/StackExchange.Redis/)
* [Azure Redis Cache のドキュメント](https://docs.microsoft.com/en-us/azure/redis-cache/)
