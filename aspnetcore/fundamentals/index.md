---
title: "ASP.NET Core の基礎"
author: rick-anderson
description: "この記事では、ASP.NET Core アプリケーションを構築する場合に理解しておく必要がある基本概念の概要を示します。"
keywords: "ASP.NET Core,基礎,概要"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>ASP.NET Core の基礎の概要

ASP.NET Core アプリケーションは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHost.CreateDefaultBuilder` を呼び出します。 ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。 前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。 ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。 [HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。 `UseStartup` については、次のセクションで詳しく説明します。

`IWebHostBuilder` (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。 これらのメソッドの一部には、HTTP.sys でアプリケーションをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` が含まれています。 `Build` および `Run` メソッドは、アプリケーションをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHostBuilder` を使用します。 ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。 前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。 [WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。 `UseStartup` については、次のセクションで詳しく説明します。

`WebHostBuilder` では、IIS および IIS Express でホストするための `UseIISIntegration` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` を含む、省略可能な多くのメソッドが提供されます。 `Build` および `Run` メソッドは、アプリケーションをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。

---

## <a name="startup"></a>スタートアップ

`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` クラスでは、要求処理パイプラインを定義し、アプリケーションで必要なサービスが構成されます。 `Startup` クラスはパブリックであり、次のメソッドを含む必要があります。

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` は、アプリケーションで使用される[サービス](#services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。

* `Configure` は、要求パイプラインで[ミドルウェア](xref:fundamentals/middleware)を定義します。

詳細については、[アプリケーションの起動](xref:fundamentals/startup)に関するページを参照してください。

## <a name="services"></a>サービス

サービスは、アプリケーションでの一般的な使用を想定されているコンポーネントです。 サービスは[依存性の注入](xref:fundamentals/dependency-injection) (DI) を介して使用できます。 ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (IoC) コンテナーが含まれます。 ネイティブ コンテナーは任意のコンテナーに置き換えることができます。 その疎結合の利点に加え、DI はアプリケーション全体でサービスを使用できるようにします。 たとえば、[ログ](xref:fundamentals/logging)はアプリケーション全体で使用できます。

詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。

## <a name="middleware"></a>ミドルウェア

ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware)を使用して要求パイプラインを構成します。 ASP.NET Core ミドルウェアは `HttpContext` で非同期ロジックを実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。 "XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出して追加されます。

ASP.NET Core には、豊富な組み込みミドルウェアのセットが付属しています。

* [静的ファイル](xref:fundamentals/static-files)

* [ルーティング](xref:fundamentals/routing)

* [認証](xref:security/authentication/index)

ASP.NET Core で [OWIN](http://owin.org) ベースのミドルウェアを使用することができます。また、独自のカスタム ミドルウェアを作成することもできます。

詳細については、[ミドルウェア](xref:fundamentals/middleware)と [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) に関するページを参照してください。

## <a name="servers"></a>サーバー

ASP.NET Core ホスティング モデルは、要求を直接リッスンするのではなく、HTTP サーバー実装を介して要求をアプリケーションに転送します。 転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。 アプリケーションはこのセットを `HttpContext` に構成します。 ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。 Kestrel は通常、[IIS](https://www.iis.net/) や [nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。

詳細については、[サーバー](xref:fundamentals/servers/index)と[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。

## <a name="content-root"></a>コンテンツ ルート

コンテンツ ルートは、ビュー、[Razor ページ](xref:mvc/razor-pages/index)、および静的資産など、アプリで使用されるコンテンツへの基本パスです。 既定では、コンテンツ ルートは、アプリケーションをホストする実行可能ファイルのアプリケーション基本パスと同じです。 コンテンツ ルートの別の場所は `WebHostBuilder` で指定されます。

## <a name="web-root"></a>Web ルート

アプリケーションの Web ルートは、CSS、JavaScript、およびイメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。 既定では、静的ファイル ミドルウェアは、Web ルート ディレクトリとそのサブディレクトリからのファイルのみを提供します。 詳細については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページを参照してください。 Web ルート パスは既定で */wwwroot* に設定されますが、`WebHostBuilder` を使用して別の場所を指定できます。

## <a name="configuration"></a>構成

ASP.NET Core は、単純な名前と値のペアを処理するために新しい構成モデルを使用します。 この新しい構成モデルは `System.Configuration` や *web.config* に基づくものではなく、構成プロバイダーの順序付けされたセットから取得します。 組み込みの構成プロバイダーではさまざまなファイル形式 (XML、JSON、INI) と環境変数がサポートされ、これにより環境ベースの構成が可能になります。 独自のカスタム構成プロバイダーを作成することもできます。

詳細については、[構成](xref:fundamentals/configuration)に関するページを参照してください。

## <a name="environments"></a>環境

"開発" や "実稼働" などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数を使用して設定できます。

詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core と .NET Framework ランタイム

ASP.NET Core アプリケーションは、.NET Core または .NET Framework ランタイムを対象にすることができます。 詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。

## <a name="additional-information"></a>追加情報

次のトピックも参照してください。

- [エラー処理](xref:fundamentals/error-handling)
- [ファイル プロバイダー](xref:fundamentals/file-providers)
- [グローバライズとローカライズ](xref:fundamentals/localization)
- [ログ](xref:fundamentals/logging)
- [アプリケーション状態の管理](xref:fundamentals/app-state)
