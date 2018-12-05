---
title: ASP.NET Core での Web サーバーの実装
author: guardrex
description: ASP.NET Core の Web サーバー Kestrel と HTTP.sys を検出します。 サーバーを選択する方法と、リバース プロキシ サーバーを使用するタイミングについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861357"
---
# <a name="web-server-implementations-in-aspnet-core"></a>ASP.NET Core での Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)

ASP.NET Core アプリは、インプロセス HTTP サーバー実装を使用して実行されます。 サーバー実装は HTTP 要求をリッスンし、<xref:Microsoft.AspNetCore.Http.HttpContext> に構成された[要求機能](xref:fundamentals/request-features)のセットとしてアプリに公開します。

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core には次のものが付属しています。

* [Kestrel サーバー](xref:fundamentals/servers/kestrel)は、既定のクロスプラットフォーム HTTP サーバーです。
* IIS HTTP サーバー (`IISHttpServer`) は、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)で使用される[インプロセス IIS サーバー](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)です。
* [HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。 ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core には次のものが付属しています。

* [Kestrel サーバー](xref:fundamentals/servers/kestrel)は、既定のクロスプラットフォーム HTTP サーバーです。
* [HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。 ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。

---

::: moniker-end

## <a name="kestrel"></a>Kestrel

Kestrel は、ASP.NET Core のプロジェクト テンプレートに含まれる既定の Web サーバーです。

::: moniker range=">= aspnetcore-2.0"

Kestrel は次のように使用できます。

* これ自体で、インターネットを含むネットワークから直接要求を処理するエッジ サーバーとして。
* [インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*と共に。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。 詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

アプリをインターネットに公開する場合は、Kestrel が[インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*を使用する必要があります。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

インターネットに直接公開される一般向けのエッジ サーバー展開でリバース プロキシを使用する最も重要な理由は、セキュリティです。 1.x バージョンの Kestrel はインターネットからの攻撃を防御する重要なセキュリティ機能を備えていません。 これには、適切なタイムアウト、要求サイズの制限、およびコンカレント接続の制限などが含まれます (ただし、これらに限定されない)。

詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

::: moniker-end

### <a name="iis-with-kestrel"></a>IIS と Kestrel

::: moniker range=">= aspnetcore-2.2"

[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用している場合、ASP.NET Core アプリは、IIS ワーカー プロセスと同じプロセス (*インプロセス* ホスティング モデル) または IIS ワーカー プロセスとは別のプロセス (*アウトプロセス* ホスティング モデル) のいずれかで実行されます。

[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)は、インプロセスの IIS HTTP サーバーまたはアウトプロセスの Kestrel サーバーのいずれかの間でネイティブの IIS 要求を処理するネイティブの IIS モジュールです。 詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core のリバース プロキシとして [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用する場合、ASP.NET Core アプリは IIS ワーカー プロセスとは別のプロセスで実行されます。 IIS プロセスで、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がリバース プロキシの関係を調整します。 ASP.NET Core モジュールの主な機能は、アプリを開始し、クラッシュ時にアプリを再始動し、HTTP トラフィックをアプリに転送することです。 詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。

::: moniker-end

### <a name="nginx-with-kestrel"></a>Nginx と Kestrel

Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、<xref:host-and-deploy/linux-nginx> を参照してください。

### <a name="apache-with-kestrel"></a>Apache と Kestrel

Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、<xref:host-and-deploy/linux-apache> を参照してください。

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。 最適なパフォーマンスを得るには、通常は Kestrel をお勧めします。 HTTP.sys は、アプリがインターネットに公開されていて、必要な機能が HTTP.sys でサポートされているものの、Kestrel ではサポートされていないシナリオで使用できます。 詳細については、「<xref:fundamentals/servers/httpsys>」を参照してください。

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

HTTP.sys は、内部ネットワークにのみ公開されるアプリにも使用できます。

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。 IIS がホストアプリで使用できないシナリオにおいて、Windows で ASP.NET Core アプリを実行する場合は、WebListener を代わりに使用できます。

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

内部ネットワークにのみ公開されるアプリで、必要な機能が WebListener でサポートされていて、Kestrel ではサポートされていない場合、Kestrel の代わりに WebListener も使用できます。 WebListener の情報については、[WebListener](xref:fundamentals/servers/weblistener) に関するページを参照してください。

![内部ネットワークと直接通信する WebListener](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>ASP.NET Core サーバー インフラストラクチャ

`Startup.Configure` メソッドで使用できる [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) は、[IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 型の [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) プロパティを公開します。 Kestrel および HTTP.sys (ASP.NET Core 1.x の WebListener) は、それぞれ単独の機能 [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) のみを公開しますが、サーバー実装が異なると追加機能が公開される場合があります。

`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。

## <a name="custom-servers"></a>カスタム サーバー

組み込みサーバーがアプリの要件に合わない場合は、カスタム サーバー実装を作成できます。 [Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](xref:fundamentals/owin)を参照してください。 実装を必要とするのは、アプリで使用される機能のインターフェイスのみです。ただし、少なくとも [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) はサポートされている必要があります。

## <a name="server-startup"></a>サーバーの起動

[Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/)、または [Visual Studio Code](https://code.visualstudio.com/) を使用する場合、サーバーはアプリが統合開発環境 (IDE) によって開始されたときに起動します。 Windows の Visual Studio では、[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)またはコンソールで、起動プロファイルを使用してアプリとサーバーを開始することができます。 Visual Studio Code では、CoreCLR デバッガーをアクティブ化する [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) によって、アプリとサーバーが開始します。 Visual Studio for Mac を使用する場合は、アプリとサーバーが [Mono の Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) によって開始します。

コマンド プロンプトからプロジェクトのフォルダーでアプリを起動する場合、[dotnet run](/dotnet/core/tools/dotnet-run) でアプリとサーバーが起動します (Kestrel および HTTP.sys のみ)。 この構成は、`Debug` (既定) または `Release` のどちらかに設定された `-c|--configuration` オプションによって指定されます。 起動プロファイルが *launchSettings.json* ファイルに存在する場合は、`--launch-profile <NAME>` オプションを使用して起動プロファイルを設定します (`Development`、`Production` など)。 詳細については、[dotnet run](/dotnet/core/tools/dotnet-run) と [.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)のトピックを参照してください。

## <a name="http2-support"></a>HTTP/2 のサポート

[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の展開シナリオでの ASP.NET Core でサポートされます。

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * オペレーティング システム
    * Windows Server 2016/Windows 10 以降&dagger;
    * OpenSSL 1.0.2 以降を使用した Linux (Ubuntu 16.04 以降など)
    * 将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。
  * ターゲット フレームワーク: .NET Core 2.2 以降
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 以降
  * ターゲット フレームワーク: HTTP.sys の展開には適用できません。
* [IIS (インプロセス)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 以降、IIS 10 以降
  * ターゲット フレームワーク: .NET Core 2.2 以降
* [IIS (アウトプロセス)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 以降、IIS 10 以降
  * 一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。
  * ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。

&dagger;Kestrel では、Windows Server 2012 R2 および Windows 8.1 上での HTTP/2 のサポートは制限されています。 サポートが制限されている理由は、これらのオペレーティング システムで使用できる TLS 暗号のスイートのリストが制限されているためです。 TLS 接続をセキュリティで保護するためには、楕円曲線デジタル署名アルゴリズム (ECDSA) を使用して生成した証明書が必要になる場合があります。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016/Windows 10 以降
  * ターゲット フレームワーク: HTTP.sys の展開には適用できません。
* [IIS (アウトプロセス)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016/Windows 10 以降、IIS 10 以降
  * 一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。
  * ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。

::: moniker-end

HTTP/2 接続では、[アプリケーション レイヤー プロトコルのネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) および TLS 1.2 以降を使用する必要があります。 詳細については、ご利用のサーバーの展開シナリオに関連するトピックを参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x については、<xref:fundamentals/servers/weblistener> を参照してください)
