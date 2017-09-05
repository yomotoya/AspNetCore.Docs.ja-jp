---
title: "ASP.NET Core の kestrel web サーバーの実装"
author: tdykstra
description: "Libuv に基づいて ASP.NET Core の Kestrel、クロスプラット フォームの web サーバーを紹介します。"
keywords: "ASP.NET Core、Kestrel、libuv、url のプレフィックス"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451a548403c8fa0ed2befeb6969a3ee28fe34790
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core の Kestrel web サーバーの実装の概要

によって[Tom Dykstra](http://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、および[Stephen Halter](https://twitter.com/halter73)

Kestrel はクロスプラット フォーム[ASP.NET core web server](index.md)に基づいて[libuv](https://github.com/libuv/libuv)プラットフォーム間の非同期 I/O ライブラリです。 Kestrel は、既定では ASP.NET Core プロジェクト テンプレートに含まれている web サーバーです。 

Kestrel には、次の機能がサポートされています。

  * HTTPS
  * 有効にするために使用する不透明なアップグレード[Websocket](https://github.com/aspnet/websockets)
  * Nginx の背後にある高パフォーマンスの Unix ソケット 

すべてのプラットフォームと .NET Core をサポートするバージョンでは、kestrel をサポートします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

[表示または 2.x のサンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[表示または 1.x のサンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>リバース プロキシで Kestrel を使用する場合

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

Kestrel を使用するには、単独またはで、*リバース プロキシ サーバー*IIS、Nginx、Apache などです。 リバース プロキシ サーバーでは、インターネットから HTTP 要求を受け取り、いくつかの予備処理後に Kestrel に転送します。

![Kestrel がリバース プロキシ サーバーを使用せず、インターネットに直接通信します。](kestrel/_static/kestrel-to-internet2.png)

![Kestrel が IIS、Nginx、Apache などのリバース プロキシ サーバー経由でインターネットといない直接通信します。](kestrel/_static/kestrel-to-internet.png)

いずれかの構成&mdash;リバース プロキシ サーバーの有無&mdash;Kestrel が内部ネットワークにのみ公開される場合にも使用できます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションが内部ネットワークからのみ要求を受け入れる場合は、単独で Kestrel を使用することができます。

![Kestrel が内部ネットワークに直接通信します。](kestrel/_static/kestrel-to-internal.png)

インターネット アプリケーションを公開する場合は、IIS、Nginx、またはとして Apache を使用する必要があります、*リバース プロキシ サーバー*です。 リバース プロキシ サーバーでは、インターネットから HTTP 要求を受け取り、いくつかの予備処理後に Kestrel に転送します。

![Kestrel が IIS、Nginx、Apache などのリバース プロキシ サーバー経由でインターネットといない直接通信します。](kestrel/_static/kestrel-to-internet.png)

リバース プロキシは、セキュリティ上の理由のエッジの展開 (インターネットからのトラフィックに対して公開) 必要があります。 1.x バージョン Kestrel の攻撃に対する防御の完全な補数がありません。 これが含まれていますが、適切なタイムアウト、サイズの制限、および同時接続の制限に限定されません。

---

リバース プロキシを必要とするシナリオ、同じ IP と 1 台のサーバーで実行されているポートを共有する複数のアプリケーションがある場合です。 それでも Kestrel で直接 Kestrel が同じ IP と複数のプロセス間でポート共有をサポートしていないためです。 ポートでリッスンするように Kestrel を構成するときに、ホスト ヘッダーに関係なく、そのポートのすべてのトラフィックを処理します。 リバース プロキシ ポートを共有できる、Kestrel 一意の IP とポートでに転送する必要があります。

場合でも、リバース プロキシ サーバーは必要ありません、いずれかの方法と、他の理由により、適切な選択可能性があります。

* 公開されている領域を制限することができます。
* 省略可能な追加の構成と多層の層を提供します。
* 既存のインフラストラクチャをより適切に統合可能性があります。
* これには、負荷分散と SSL のセットアップが簡略化します。 リバース プロキシ サーバーのみが、SSL 証明書を必要と、そのサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリケーション サーバーと通信できます。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core アプリケーションで Kestrel を使用する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)にパッケージが含まれる、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)です。

ASP.NET Core プロジェクト テンプレートは、既定では Kestrel を使用します。 *Program.cs*、テンプレート コードの呼び出し`CreateDefaultBuilder`、どの呼び出し[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)バック グラウンドでします。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Kestrel オプションを構成する必要がある場合は、呼び出す`UseKestrel`で*Program.cs*次の例で示すようにします。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

インストール、 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージです。

呼び出す、 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[Kestrel オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)する必要がある、次のセクションで示すようにします。

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel オプション

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

Kestrel web サーバーには、インターネットに接続された環境で特に便利な制約の構成オプションがあります。 次に設定できる値の範囲を示します。

- 最大クライアント接続
- 最大要求本文のサイズ
- 最小の要求本文データ レート

これらの制約と内の他に設定する、`Limits`のプロパティ、 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)クラスです。 `Limits`プロパティのインスタンスを保持する、 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)クラスです。 

**最大クライアント接続**

次のコード全体のアプリケーションの同時実行の開いている TCP 接続の最大数を設定できます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

(たとえば、Websocket 要求など) 上の別のプロトコルに HTTP または HTTPS からアップグレードした接続に対して個別の制限があります。 接続がアップグレードされた後に照らし合わせてカウントされるされていない、`MaxConcurrentConnections`制限します。 

接続の最大数は、既定で無制限 (null) です。

**最大要求本文のサイズ**

既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。 

ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

ミドルウェア内で特定の要求の設定を上書きできます。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。 `IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。

**最小の要求本文データ レート**

データが送信される場合で指定したレートでバイト数/秒、kestrel は毎秒をチェックします。 速度が、最小値を下回る場合、接続がタイムアウトしました。Kestrel は、最小; まで、送信速度を向上させるクライアントを時間は、猶予期間はレートは、この期間中はチェックされません。 猶予期間により、TCP 速度の遅い起動のための低速なレートで最初にデータを送信する接続を削除しないでください。

既定の最小間隔は、240 バイト/秒、5 秒の猶予時間です。

最短間隔は、応答にも適用されます。 要求の制限と応答の上限を設定するコードがあることを除いて同じ`RequestBody`または`Response`プロパティおよびインターフェイスの名前でします。 

最小限のデータ間隔を構成する方法を示す例を次に示します*Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

ミドルウェア内で 1 回の要求レートを構成することができます。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

その他の Kestrel オプションについては、次のクラスを参照してください。

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kestrel オプションについては、次を参照してください。 [KestrelServerOptions クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)です。

---

### <a name="endpoint-configuration"></a>エンドポイントの構成

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

既定では ASP.NET Core にバインド`http://localhost:5000`です。 URL プレフィックスと Kestrel を呼び出すことによってリッスン用のポートを構成する`Listen`または`ListenUnixSocket`メソッド`KestrelServerOptions`です。 (`UseUrls`、`urls`コマンドラインの引数とも作業 ASPNETCORE_URLS 環境変数は、説明した制限がある[この記事で後述](#useurls-limitations))。

**TCP ソケットをバインドします。**

`Listen`メソッドは、TCP ソケットをバインドし、オプション ラムダでは、SSL 証明書を構成することができます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

どのようにこの例 SSL の構成、特定のエンドポイントを使用してに注意してください。 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)です。 同じ API を使用すると、特定のエンドポイントの場合は、その他の Kestrel 設定を構成します。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Unix ソケットにバインドします。**

この例で示すようには、Nginx、によるパフォーマンスの向上の Unix ソケットでリッスンできます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**ポート 0**

ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。 次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls の制限事項**

エンドポイントを構成するには呼び出すことによって、`UseUrls`メソッドまたはを使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数。 これらのメソッドは Kestrel 以外のサーバーを使用するコードを作成する場合に便利です。 ただし、これらの制限に注意してください。

* これらのメソッドで SSL を使用することはできません。
* 両方を使用する場合、`Listen`メソッドおよび`UseUrls`、`Listen`エンドポイントを上書き、`UseUrls`エンドポイント。

**IIS のエンドポイントの構成**

IIS の URL のバインディングがいずれかを呼び出して設定するすべてのバインドを上書き IIS を使用する場合`Listen`または`UseUrls`です。 詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](aspnet-core-module.md)です。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

既定では ASP.NET Core にバインド`http://localhost:5000`です。 URL プレフィックスおよび Kestrel を使用してリッスンするポートを構成することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数、または ASP.NET Core の構成システムです。 これらのメソッドの詳細については、次を参照してください。[ホスティング](../../fundamentals/hosting.md)です。 リバース プロキシとして IIS を使用するときの URL のバインドの動作方法の詳細については、次を参照してください。 [ASP.NET Core モジュール](aspnet-core-module.md)です。 

---

### <a name="url-prefixes"></a>URL プレフィックス

呼び出す場合`UseUrls`か使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数、URL プレフィックス可能で、次の形式のいずれか。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

HTTP URL プレフィックスのみが無効です。Kestrel が SSL をサポートしていないを使用してバインディングを URL を構成するときに`UseUrls`です。

* ポート番号を含む IPv4 アドレス

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。


* ポート番号を含む IPv6 アドレス

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。


* ポート番号を持つホスト名

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  ホスト名、* と +, ではありません。 認識されている IP アドレスまたは"localhost"ではないものは、すべての IPv4 および IPv6 ip アドレスにバインドされます。 別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[HTTP.sys](httpsys.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。

* ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。 いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。 かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 はサポートされていないためによく)、Kestrel 警告をログに記録します。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* ポート番号を含む IPv4 アドレス

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。


* ポート番号を含む IPv6 アドレス

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。


* ポート番号を持つホスト名

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  ホスト名、 \*、および特殊ないない + です。 認識される IP アドレスまたは"localhost"がないものは、すべての IPv4 および IPv6 ip アドレスにバインドします。 別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[WebListener](weblistener.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。

* ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。 いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。 かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 はサポートされていないためによく)、Kestrel 警告をログに記録します。 

* Unix ソケット

  ```
  http://unix:/run/dan-live.sock  
  ```

**ポート 0**

ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。 ホスト名または IP を除く任意のポート 0 へのバインドは許可されて`localhost`名。

次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL 用の URL のプレフィックス**

持つ URL プレフィックスを含めてください`https:`を呼び出す場合は、`UseHttps`拡張メソッドを次のようにします。

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> 同じポートでは、HTTPS と HTTP をホストすることはできません。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>次のステップ

詳細については、次のリソースを参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

* [2.x 用のサンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel ソース コード](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [1.x 用のサンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel ソース コード](https://github.com/aspnet/KestrelHttpServer)

---
