---
title: "ASP.NET Core への Kestrel Web サーバーの実装"
author: tdykstra
description: "libuv に基づく ASP.NET Core 用のクロスプラット フォーム Web サーバーである Kestrel を紹介します。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core への Kestrel Web サーバーの実装の概要

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Stephen Halter](https://twitter.com/halter73)

Kestrel は、[libuv](https://github.com/libuv/libuv) (クロスプラットフォームの非同期 I/O ライブラリ) に基づくクロスプラット フォームの [ASP.NET Core 用 Web サーバー](index.md)です。 Kestrel は、ASP.NET Core のプロジェクト テンプレートに既定で含まれる Web サーバーです。 

Kestrel は、次の機能をサポートします。

  * HTTPS
  * [Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード
  * Nginx の背後にある高パフォーマンスの UNIX ソケット 

kestrel は、.NET Core がサポートするすべてのプラットフォームおよびバージョンでサポートされます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[2.x のサンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[1.x のサンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kestrel とリバース プロキシを使用するタイミング

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

いずれの構成 &mdash; リバース プロキシ サーバーがある場合とない場合 &mdash; も、Kestrel が内部ネットワークにのみ公開される場合にも使用できます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

アプリケーションをインターネットに公開する場合は、*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

リバース プロキシは、セキュリティ上の理由からエッジ展開 (インターネットからのトラフィックに公開される) で必要となります。 1.x バージョンの Kestrel には、攻撃に対する防御の全装備が十分ではありません。 これには、適切なタイムアウト、サイズ制限、および同時接続の制限などが含まれます (ただし、これらに限定されない)。

---

リバース プロキシを必要とするシナリオは、単一のサーバー上で実行される同じ IP とポートを共有する複数のアプリケーションがある場合となります。 このシナリオは Kestrel では直接機能しません。Kestrel では、複数のプロセス間で同じ IP とポートを共有する方法がサポートされていないからです。 ポートをリッスンするように Kestrel を構成すると、Kestrel はホスト ヘッダーに関係なく、そのポートでのすべてのトラフィックを処理します。 ポートを共有できるリバース プロキシは、一意の IP とポートで Kestrel に転送される必要があります。

リバース プロキシ サーバーが必要なくても、次に示すような他の理由により、これを使用することをお勧めします。

* 公開されるセキュリティ、外部からの領域を制限することができます。
* 構成および防御の層を必要に応じて追加します。
* 既存のインフラストラクチャとより適切に統合できる場合があります。
* 負荷分散と SSL のセットアップを簡略化します。 リバース プロキシ サーバーのみで SSL 証明書が必要です。このサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリケーション サーバーと通信することができます。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core アプリで Kestrel を使用する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) パッケージは、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれています。

ASP.NET Core プロジェクト テンプレートは既定では Kestrel を使用します。 *Program.cs* 内のテンプレート コードは `CreateDefaultBuilder` を呼び出し、これによってバックグラウンドで [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) が呼び出されます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Kestrel オプションを構成する必要がある場合は、次の例で示すように *Program.cs* で `UseKestrel` を呼び出します。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージをインストールします。

`Main` メソッド内の `WebHostBuilder` で [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 拡張メソッドを呼び出して、必要な [Kestrel オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)を指定します (次のセクションを参照)。

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel オプション

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel Web サーバーには、インターネットに接続する展開で特に有効な制約構成オプションがいくつかあります。 設定できる制限のいくつかを次に示します。

- クライアントの最大接続数
- 要求本文の最大サイズ
- 要求本文の最小レート

[KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) クラスの `Limits` プロパティで上記の制約およびその他の制約を設定します。 `Limits` プロパティは、[KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) クラスのインスタンスを保持します。 

**クライアントの最大接続数**

次のコードを使用することで、アプリケーション全体に対して同時に開かれる TCP 接続の最大数を設定できます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

HTTP または HTTPS から別のプロトコルにアップグレードされた接続については別個の制限があります (WebSockets 要求に関する制限など)。 接続がアップグレードされた後、それは `MaxConcurrentConnections` 制限に対してカウントされません。 

接続の最大数は既定で無制限 (null) です。

**要求本文の最大サイズ**

既定での要求本文の最大サイズは、30,000,000 バイトです。これはおよそ約 28.6 MB になります。 

ASP.NET Core MVC アプリでの制限を上書きする方法としては、アクション メソッドに対して [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 属性を使用することをお勧めします。

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

次の例では、アプリケーション全体を対象にしてすべての要求に対する制約を構成する方法を示しています。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

ミドルウェア内の特定の要求に関する設定を上書きすることができます。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
アプリケーションで要求の読み取りが開始された後、要求に対する制限を構成しようとすると、例外がスローされます。 `MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを知らせてくれる `IsReadOnly` プロパティがあります。

**要求本文の最小レート**

kestrel はデータが指定のレート (バイト数/秒) で着信しているかどうかを毎秒チェックします。 レートが最小値を下回った場合は接続がタイムアウトになります。猶予期間とは、クライアントによって送信速度を最低ラインまで引き上げられるのを、Kestrel が待機する時間のことです。この期間中、レートはチェックされません。 猶予期間により、TCP のスロースタートのため最初にデータを低速で送信する接続がドロップされるのを回避できます。

既定の最小レートは 240 バイト/秒であり、5 秒の猶予時間が設定されています。

最小レートは応答にも適用されます。 要求制限と応答制限を設定するコードは、プロパティ名およびインターフェイス名に `RequestBody` または `Response` が使用されることを除けば同じです。 

次の例では、*Program.cs* で最小データ レートを構成する方法を示します。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

ミドルウェアで要求レートを構成することができます。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

その他の Kestrel オプションについては、次のクラスを参照してください。

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kestrel オプションについては、「[KestrelServerOptions クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)」を参照してください。

---

### <a name="endpoint-configuration"></a>エンドポイントの構成

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

既定では ASP.NET Core は `http://localhost:5000` にバインドされます。 `KestrelServerOptions` で `Listen` メソッドまたは `ListenUnixSocket` メソッドを呼び出すことにより、Kestrel がリッスンする URL プレフィックスおよびポートを構成します  (`UseUrls`、`urls` コマンドライン引数、および ASPNETCORE_URLS 環境変数も機能しますが、[この記事で後述](#useurls-limitations)する制限があります)。

**TCP ソケットにバインドする**

`Listen` メソッドは TCP ソケットにバインドします。オプションのラムダにより、SSL 証明書を構成することができます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

この例では、[ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) を使用して特定のエンドポイントに対して SSL を構成する方法を示しています。 同じ API を使用して、特定のエンドポイントについて、その他の Kestrel 設定を構成できます。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**UNIX ソケットにバインドする**

次の例に示すように、Nginx ではパフォーマンスの向上のために UNIX ソケットをリッスンすることができます。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**ポート 0**

ポート番号 0 を指定すると、Kestrel は使用可能なポートに動的にバインドします。 次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls の制限事項**

エンドポイントを構成するには、`UseUrls` メソッドを呼び出すか、`urls` コマンドライン引数または ASPNETCORE_URLS 環境変数を使用します。 コードを Kestrel 以外のサーバーで機能させる場合は、これらのメソッドが有用です。 ただし、次の制限事項に注意してください。

* これらのメソッドで SSL を使用することはできません。
* `Listen` メソッドと `UseUrls` を両方とも使用する場合、`Listen` エンドポイントが `UseUrls` エンドポイントを上書きします。

**IIS 用のエンドポイントの構成**

IIS を使用する場合、`Listen` または `UseUrls` のいずれかを呼び出して設定したバインディングが、IIS 用の URL バインディングによって上書きされます。 詳細については、「[Introduction to ASP.NET Core Module](aspnet-core-module.md)」(ASP.NET Core モジュールの概要) を参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

既定では ASP.NET Core は `http://localhost:5000` にバインドされます。 `UseUrls` 拡張メソッド、`urls` コマンドライン引数、または ASP.NET Core 構成システムを使用して、Kestrel がリッスンする URL プレフィックスおよびポートを構成することができます。 これらのメソッドの詳細については、[ホスティング](../../fundamentals/hosting.md)に関するページを参照してください。 IIS をリバース プロキシとして使用する場合の URL バインディングの動作方法については、[ASP.NET Core モジュール](aspnet-core-module.md)に関するページを参照してください。 

---

### <a name="url-prefixes"></a>URL プレフィックス

`UseUrls` を呼び出すか、`urls` コマンドライン引数または ASPNETCORE_URLS 環境変数を使用する場合、URL プレフィックスは次のいずれかの形式となります。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

HTTP URL プレフィックスのみが無効です。`UseUrls` を使用して URL バインディングを構成する場合、Kestrel によって SSL はサポートされません。

* IPv4 アドレスとポート番号

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 は、すべての IPv4 アドレスにバインドする特別なケースです。


* IPv6 アドレスとポート番号

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  IPv6 の [::] は IPv4 の 0.0.0.0 に相当します。


* ホスト名とポート番号

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  ホスト名、*、および + は特別ではありません。 認識された IP アドレスまたは "localhost" ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。 異なるホスト名を同じポート上の異なる ASP.NET Core アプリケーションにバインドする必要がある場合は、[HTTP.sys](httpsys.md) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。

* "Localhost" 名とポート番号、またはループバック IP とポート番号

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。 要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。 他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4 アドレスとポート番号

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 は、すべての IPv4 アドレスにバインドする特別なケースです。


* IPv6 アドレスとポート番号

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  IPv6 の [::] は IPv4 の 0.0.0.0 に相当します。


* ホスト名とポート番号

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  ホスト名、\*、および + は特別ではありません。 認識された IP アドレスまたは "localhost" ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。 異なるホスト名を同じポート上の異なる ASP.NET Core アプリケーションにバインドする必要がある場合は、[WebListener](weblistener.md) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。

* "Localhost" 名とポート番号、またはループバック IP とポート番号

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。 要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。 他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。 

* UNIX ソケット

  ```
  http://unix:/run/dan-live.sock  
  ```

**ポート 0**

ポート番号 0 を指定すると、Kestrel は使用可能なポートに動的にバインドします。 ポート 0 へのバインドは、`localhost` 名を除き、任意のホスト名または ID で許可されます。

次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL 用の URL プレフィックス**

`UseHttps` 拡張メソッドを呼び出す場合は、次に示すように URL プレフィックスと `https:` を必ず含めます。

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
> 同じポート上で、HTTPS と HTTP をホストすることはできません。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [2.x のサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel ソース コード](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [1.x のサンプリ アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel ソース コード](https://github.com/aspnet/KestrelHttpServer)

---
