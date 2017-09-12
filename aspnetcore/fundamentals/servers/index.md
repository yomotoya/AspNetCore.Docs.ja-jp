---
title: "ASP.NET Core での Web サーバーの実装"
author: tdykstra
description: "ASP.NET Core の Web サーバーである Kestrel と WebListener の概要を示します。 いずれかを選択する方法と、リバース プロキシ サーバーでいずれかを使用するタイミングに関するガイダンスを提供します。"
keywords: "ASP.NET Core, IServer, Web サーバー, Kestrel, WebListener, リバース プロキシ"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 17124f1ef181a4f1572d9375ae8cd27ce8845016
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core での Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)

ASP.NET Core アプリケーションは、インプロセス HTTP サーバー実装を使用して実行されます。 サーバー実装では HTTP 要求をリッスンし、`HttpContext` に構成された[要求機能](https://docs.microsoft.com/aspnet/core/fundamentals/request-features)のセットとしてアプリケーションに公開します。

ASP.NET Core には次の 2 つのサーバー実装が用意されています。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) は、クロスプラットフォームの非同期 I/O ライブラリである [libuv](https://github.com/libuv/libuv) に基づくクロスプラットフォームの HTTP サーバーです。

* [HTTP.sys](httpsys.md) は、[Http.Sys カーネル ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づく Windows 専用の HTTP サーバーです。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) は、クロスプラットフォームの非同期 I/O ライブラリである [libuv](https://github.com/libuv/libuv) に基づくクロスプラットフォームの HTTP サーバーです。

* [WebListener](weblistener.md) は、[Http.Sys カーネル ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づく Windows 専用の HTTP サーバーです。

---

## <a name="kestrel"></a>Kestrel

Kestrel は、ASP.NET Core の新しいプロジェクト テンプレートに既定で含まれる Web サーバーです。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

いずれの構成 &mdash; リバース プロキシ サーバーがある場合とない場合 &mdash; も、Kestrel が内部ネットワークにのみ公開される場合にも使用できます。

Kestrel とリバース プロキシを併用するタイミングについては、[Kestrel の概要](kestrel.md)に関するページを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリケーションが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

アプリケーションをインターネットに公開する場合は、*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。 以下の図のように、リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

エッジ展開 (インターネットからのトラフィックに公開される) でリバース プロキシを使用する最も重要な理由は、セキュリティです。 1.x バージョンの Kestrel には、攻撃に対する防御の全装備が十分ではありません。 これには、適切なタイムアウト、サイズ制限、および同時接続の制限などが含まれます (ただし、これらに限定されない)。

Kestrel とリバース プロキシを併用するタイミングについては、[Kestrel の概要](kestrel.md)に関するページを参照してください。

---

IIS、Nginx、または Apache を Kestrel や[カスタム サーバー実装](#custom-servers)なしで使用することはできません。 ASP.NET Core は、プラットフォーム間で一貫して動作できるように、独自のプロセスで実行するように設計されています。 IIS、Nginx、および Apache では独自のスタートアップ プロセスと環境が指示されます。それらを直接使用するには、ASP.NET Core はそれぞれのニーズに適応する必要があります。 Kestrel などの Web サーバー実装を使用することで、ASP.NET Core はスタートアップ プロセスと環境を制御できます。 したがって、ASP.NET Core を IIS、Nginx、または Apache に適応するのではなく、Kestrel に要求をプロキシするようにこれらの Web サーバーを設定します。 このようにすれば、展開場所に関係なく、`Program.Main` および `Startup` クラスを本質的に同じにすることができます。

### <a name="iis-with-kestrel"></a>IIS と Kestrel

ASP.NET Core のリバース プロキシとして IIS または IIS Express を使用する場合、ASP.NET Core アプリケーションは IIS ワーカー プロセスとは別のプロセスで実行されます。 IIS プロセスでは、リバース プロキシの関係を調整するために特別な IIS モジュールが実行されます。  これは *ASP.NET Core モジュール*です。 ASP.NET Core モジュールの主な機能は、ASP.NET Core アプリケーションを開始し、クラッシュ時に再始動し、HTTP トラフィックを転送することです。 詳細については、[ASP.NET Core モジュール](aspnet-core-module.md)に関するページを参照してください。 

### <a name="nginx-with-kestrel"></a>Nginx と Kestrel

Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、[Linux 運用環境への発行](../../publishing/linuxproduction.md)に関するページを参照してください。

### <a name="apache-with-kestrel"></a>Apache と Kestrel

Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、[リバース プロキシとしての Apache Web サーバーの使用](../../publishing/apache-proxy.md)に関するページを参照してください。

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。 アプリをインターネットに公開し、Kestrel でサポートされない HTTP.sys 機能が必要なシナリオの場合、HTTP.sys を使用できます。 

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

HTTP.sys は、内部ネットワークにのみ公開されるアプリケーションにも使用できます。 

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

内部ネットワークのシナリオの場合、最適なパフォーマンスを得るには、一般に Kestrel が推奨されますが、一部のシナリオでは、HTTP.sys のみで提供される機能を使用することもできます。 HTTP.sys の機能については、[HTTP.sys](httpsys.md) に関するページを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x では、HTTP.sys は WebListener と呼ばれます。 Windows で ASP.NET Core アプリを実行する場合、アプリをインターネットに公開する際に IIS を使用できないシナリオで WebListener を代わりに使用できます。

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

Kestrel でサポートされない WebListener の機能が必要な場合は、内部ネットワークにのみ公開されるアプリケーションでも Kestrel の代わりに WebListener を使用できます。 

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

内部ネットワークのシナリオの場合、最適なパフォーマンスを得るには、一般に Kestrel が推奨されますが、一部のシナリオでは、WebListener のみで提供される機能を使用することもできます。 WebListener の機能については、[WebListener](weblistener.md) に関するページを参照してください。

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>ASP.NET Core サーバー インフラストラクチャに関する注意事項

`Startup` クラスの `Configure` メソッドで使用可能な [`IApplicationBuilder`](https://docs.microsoft.com/aspnet/core/api) は、[`IFeatureCollection`](https://docs.microsoft.com/aspnet/core/api) 型の `ServerFeatures` プロパティを公開します。 Kestrel と WebListener はどちらも [`IServerAddressesFeature`](https://docs.microsoft.com/aspnet/core/api) という 1 つの機能しか公開しませんが、異なるサーバー実装では追加の機能が公開される可能性があります。

`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。

## <a name="custom-servers"></a>カスタム サーバー

組み込みサーバーがニーズに合わない場合は、カスタム サーバー実装を作成できます。 [Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](../owin.md)を参照してください。 アプリケーションで必要な機能インターフェイスのみを実装するのは自由です。ただし、少なくとも [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) をサポートする必要があります。

## <a name="next-steps"></a>次のステップ

詳細については、次のリソースを参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel と IIS](aspnet-core-module.md)
- [Kestrel と Nginx](../../publishing/linuxproduction.md)
- [Kestrel と Apache](../../publishing/apache-proxy.md)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel と IIS](aspnet-core-module.md)
- [Kestrel と Nginx](../../publishing/linuxproduction.md)
- [Kestrel と Apache](../../publishing/apache-proxy.md)
- [WebListener](weblistener.md)

---
