---
title: "ASP.NET Core の基礎"
author: rick-anderson
description: "ASP.NET Core アプリケーションの構築に関する基本概念について説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: be37df7789354ac4ce8e373a1560366be157ffa5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core の基礎

ASP.NET Core アプリケーションは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHost.CreateDefaultBuilder` を呼び出します。 ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。 前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。 ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。 [HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。 `UseStartup` については、次のセクションで詳しく説明します。

`IWebHostBuilder` (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。 これらのメソッドの一部には、HTTP.sys でアプリをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` が含まれています。 `Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHostBuilder` を使用します。 ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。 前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。 [WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。 `UseStartup` については、次のセクションで詳しく説明します。

`WebHostBuilder` では、IIS および IIS Express でホストするための `UseIISIntegration` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` を含む、省略可能な多くのメソッドが提供されます。 `Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。

---

## <a name="startup"></a>スタートアップ

`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` クラスでは、要求処理パイプラインを定義し、アプリで必要なサービスが構成されます。 `Startup` クラスはパブリックであり、次のメソッドを含む必要があります。

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` は、アプリで使用される[サービス](#dependency-injection-services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。 `Configure` は、要求パイプラインの[ミドルウェア](xref:fundamentals/middleware/index)を定義します。

詳細については、[アプリケーションの起動](xref:fundamentals/startup)に関するページを参照してください。

## <a name="content-root"></a>コンテンツ ルート

コンテンツ ルートは、ビュー、[Razor ページ](xref:mvc/razor-pages/index)、および静的資産など、アプリで使用されるコンテンツへの基本パスです。 既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルのアプリケーション基本パスと同じです。

## <a name="web-root"></a>Web ルート

アプリの Web ルートは、CSS、JavaScript、イメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。

## <a name="dependency-injection-services"></a>依存性の注入 (サービス)

サービスは、アプリでの一般的な使用を想定されているコンポーネントです。 サービスは[依存性の注入 (DI)](xref:fundamentals/dependency-injection) を介して使用できます。 ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (**I**nversion **o**f **C**ontrol、IoC) コンテナーが含まれます。 必要に応じて、既定のネイティブ コンテナーを置き換えることができます。 その疎結合の利点に加え、DI はアプリ全体でサービス ([ログ](xref:fundamentals/logging/index)など) を使用できるようにします。

詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。

## <a name="middleware"></a>ミドルウェア

ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware/index)を使用して要求パイプラインを構成します。 ASP.NET Core ミドルウェアは `HttpContext` で非同期ロジックを実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。 "XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出して追加されます。

ASP.NET Core には、豊富な組み込みミドルウェアのセットが含まれます。

* [静的ファイル](xref:fundamentals/static-files)
* [ルーティング](xref:fundamentals/routing)
* [認証](xref:security/authentication/index)
* [応答圧縮ミドルウェア](xref:performance/response-compression)
* [URL リライト ミドルウェア](xref:fundamentals/url-rewriting)

ASP.NET Core アプリで [OWIN](http://owin.org) ベースのミドルウェアを使用することができます。また、独自のカスタム ミドルウェアを作成することもできます。

詳細については、[ミドルウェア](xref:fundamentals/middleware/index)と [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) に関するページを参照してください。

## <a name="environments"></a>環境

"開発" や "実稼働" などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数を使用して設定できます。

詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。

## <a name="configuration"></a>構成

ASP.NET Core は、名前と値のペアに基づく構成モデルを使用します。 この構成モデルは `System.Configuration` または *web.config* には基づきません。構成は、構成プロバイダーの順序付けされたセットから設定を取得します。 組み込みの構成プロバイダーではさまざまなファイル形式 (XML、JSON、INI) と環境変数がサポートされ、これにより環境ベースの構成が可能になります。 独自のカスタム構成プロバイダーを作成することもできます。

詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。

## <a name="logging"></a>ログの記録

ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。 組み込みプロバイダーは、1 つまたは複数の送信先へのログの送信をサポートします。 サード パーティ製のログ記録フレームワークを使用できます。

[ログ](xref:fundamentals/logging/index)

## <a name="error-handling"></a>エラー処理

ASP.NET Core には、開発者の例外ページ、カスタム エラー ページ、静的ステータス コード ページ、起動時の例外処理を含む、アプリのエラー処理の機能が組み込まれています。

詳細については、[エラー処理](xref:fundamentals/error-handling)に関するページを参照してください。

## <a name="routing"></a>ルーティング

ASP.NET Core は、ルート ハンドラーにアプリ要求をルーティングする機能を提供します。

詳細については、[ルーティング](xref:fundamentals/routing)に関するページを参照してください。

## <a name="file-providers"></a>ファイル プロバイダー

ASP.NET Core は、プラットフォーム間でファイルを操作するための共通のインターフェイスを提供するファイル プロバイダーの使用により、ファイル システムへのアクセスを抽象化します。

詳細については、[ファイル プロバイダー](xref:fundamentals/file-providers)に関するページを参照してください。

## <a name="static-files"></a>静的ファイル

静的ファイルのミドルウェアは、HTML、CSS、画像、JavaScript などの静的ファイルを処理します。

詳細については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページを参照してください。

## <a name="hosting"></a>ホスト

ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。

詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。

## <a name="session-and-application-state"></a>セッションとアプリケーションの状態

セッション状態は ASP.NET Core の機能で、これを使用することでユーザーが Web アプリを参照中にユーザー データを保存して格納することができます。

詳細については、[セッションとアプリケーション状態](xref:fundamentals/app-state)に関するページを参照してください。

## <a name="servers"></a>サーバー

ASP.NET Core のホスティング モデルは、直接要求をリッスンしません。 ホスティング モデルは HTTP サーバー実装に依存して要求をアプリに転送します。 転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。 ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。 Kestrel は多くの場合、[IIS](https://www.iis.net/) や [Nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。 Kestrel は、エッジ サーバーとして実行することができます。

詳細については、[サーバー](xref:fundamentals/servers/index)に関するページと、次のトピックを参照してください。

* [Kestrel](xref:fundamentals/servers/kestrel)
* [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>グローバリゼーションとローカリゼーション

ASP.NET Core で多言語の Web サイトを作成すると、より幅広い対象者がサイトにアクセスできるようになります。 ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。

詳細については、[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)に関するページを参照してください。

## <a name="request-features"></a>要求機能

HTTP の要求と応答に関連する Web サーバーの実装の詳細が、インターフェイスで定義されています。 これらのインターフェイスは、サーバーの実装とミドルウェアがアプリのホスティングのパイプラインを作成および変更するために使用します。

詳細については、 [要求機能](xref:fundamentals/request-features)に関するページを参照してください。

## <a name="background-tasks"></a>バックグラウンド タスク

バックグラウンド タスクは*ホストされるサービス*として実装されます。 ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。

詳細については、「[Background tasks with hosted services](xref:fundamentals/hosted-services)」(ホストされるタスクを使用するバックグラウンド タスク) を参照してください。

## <a name="open-web-interface-for-net-owin"></a>Open Web Interface for .NET (OWIN)

ASP.NET Core は、Open Web Interface for .NET (OWIN) をサポートします。 OWIN により、Web アプリを Web サーバーから切り離すことが可能になります。

詳細については、[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)に関するページを参照してください。

## <a name="websockets"></a>WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。 チャット、株価情報、ゲームなどのアプリ、さらに Web アプリでのリアルタイムな機能が必要とされるあらゆる場所で使用されます。 ASP.NET Core は、Web ソケットの機能をサポートします。

詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All メタパッケージ

ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。

* ASP.NET Core チームでサポートされるすべてのパッケージ。
* Entity Framework Core でサポートされるすべてのパッケージ。 
* ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。

詳細については、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に関するページを参照してください。

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core と .NET Framework ランタイム

ASP.NET Core アプリは、.NET Core または .NET Framework ランタイムを対象にすることができます。

詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。

## <a name="choose-between-aspnet-core-and-aspnet"></a>ASP.NET Core と ASP.NET の選択

ASP.NET Core と ASP.NET の選択の詳細については、「[ ASP.NET と ASP.NET Core の選択](xref:fundamentals/choose-between-aspnet-and-aspnetcore)」を参照してください。
