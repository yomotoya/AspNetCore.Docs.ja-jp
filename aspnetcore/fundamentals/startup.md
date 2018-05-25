---
title: ASP.NET Core でのアプリケーションのスタートアップ
author: ardalis
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: a61f78b2d0e5c6c171a26690fcce256462a82508
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core でのアプリケーションのスタートアップ

作成者: [Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex)

`Startup` クラスはサービスとアプリケーションの要求パイプラインを構成します。

## <a name="the-startup-class"></a>Startup クラス

ASP.NET Core アプリケーションでは `Startup` クラスが使用されています。このクラスは規約に従って `Startup` と名前が付けられています。 `Startup` クラスには次の特徴があります。

* 必要に応じて [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドを含め、アプリケーションのサービスを構成することができます。
* アプリケーションの要求処理パイプラインを作成するために [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドを含める必要があります。

`ConfigureServices` と `Configure` はアプリケーションの起動時にランタイムによって呼び出されます。

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

[WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) メソッドで `Startup` クラスを指定します。

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` クラス コンストラクターは、ホストに定義されている依存関係を受け入れます。 `Startup` クラスへの[依存関係挿入](xref:fundamentals/dependency-injection)の一般的な用途は、以下を挿入する場合です。

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) (環境別にサービスを構成するため)。
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) (スタートアップ時にアプリケーションを構成するため)。

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

`IHostingEnvironment` を挿入する代わりに、規約ベースのアプローチがあります。 アプリケーションでは、環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスを定義することができます。実行時に適切な startup クラスが選択されます。 名前のサフィックスが現在の環境と一致するクラスが優先されます。 アプリケーションが Development 環境で実行され、`Startup` クラスと `StartupDevelopment` クラスの両方が含まれている場合は、`StartupDevelopment` クラスが使用されます。 詳細については、「[Use multiple environments](xref:fundamentals/environments#startup-conventions)」(複数の環境の使用) を参照してください。

`WebHostBuilder` の詳細については、[ホスティング](xref:fundamentals/hosting)に関するトピックを参照してください。 スタートアップ時のエラー処理については、「[Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling)」(スタートアップ例外処理) を参照してください。

## <a name="the-configureservices-method"></a>ConfigureServices メソッド

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドの特徴:

* Optional
* アプリケーションのサービスを構成する `Configure` メソッドの前に、Web ホストによって呼び出されます。
* 規約によって[構成オプション](xref:fundamentals/configuration/index)が設定されます。

サービス コンテナーにサービスを追加すると、アプリケーション内と `Configure` メソッド内でサービスを利用できるようになります。 サービスは、[依存関係挿入](xref:fundamentals/dependency-injection)を介して、または [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) から解決されます。

Web ホストでは、`Startup` メソッドが呼び出される前にいくつかのサービスを構成することができます。 詳細については、[ホスティング](xref:fundamentals/hosting)に関するトピックを参照してください。

多くの設定が必要な機能には、[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上の `Add[Service]` 拡張メソッドがあります。 一般的な Web アプリケーションは、Entity Framework、Identity、および MVC のサービスを登録します。

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>ASP.NET Core MVC の SetCompatibilityVersion 

`SetCompatibilityVersion` メソッドを使用すると、ASP.NET MVC Core 2.1 以降に導入されている、互換性に影響する重大な変更をオプトインまたはオプトアウトすることができます。 互換性に影響する可能性のあるこれらの重大な変更は、ほとんどの場合、MVC サブシステムの動作方法と、ランタイムで**ユーザーのコード**が呼び出される方法についてです。 オプトインした場合、最新の動作と ASP.NET Core の最新の動作を得ることができます。

次のコードにより、互換性モードは ASP.NET Core 2.1 に設定されます。

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

最新のバージョンを使用して (`CompatibilityVersion.Version_2_1`) アプリケーションをテストすることをお勧めします。 ほとんどのアプリケーションには、最新のバージョンを使用しても、互換性に影響する重大な変更はないと見込まれます。 

`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すアプリケーションは、ASP.NET Core 2.1 MVC およびそれ以降の 2. バージョンで実装されている、動作が中断する可能性がある変更から保護されています。 この保護とは、次のとおりです。

* 2.1 以降のすべての変更には該当しません。これは、MVC サブシステムの ASP.NET Core ランタイムの互換性に影響する可能性のある重大な変更のみを対象としています。
* 次のメジャー バージョンには適用されません。

`SetCompatibilityVersion` を呼び出さ**ない** ASP.NET Core 2.1 およびそれ以降の 2.x アプリケーションの既定の互換性は、2.0 の互換性です。 つまり、`SetCompatibilityVersion` を呼び出さないことは、`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すことと同じです。

次のコードは、以下の動作を除き、互換性モードを ASP.NET Core 2.1 に設定します。

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

互換性に影響する重大な変更があったアプリでは、適切な互換性スイッチを使用することにより、次が得られます。

* 最新のリリースを使用し、互換性に影響する特定の重大な変更をオプトアウトできます。
* アプリが最新の変更に対応するよう、更新を行う時間を得られます。

[MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) クラス ソースのコメントには、変更があった個所とそれらの改善が多くのユーザーに与えるメリットについて説明しています。

将来的に、[ASP.NET Core 3.0 バージョン](https://github.com/aspnet/Home/wiki/Roadmap)がリリースされます。 3.0 バージョンでは、互換性スイッチによってサポートされている古い動作は削除されます。 これらの正の変更は、ほぼすべてのユーザーにとってメリットとなると感じています。 これらの変更を現在導入することにより、ほとんどのアプリでは今メリット享受でき、他ではアプリケーションをアップデートする時間を得られます。

::: moniker-end

## <a name="services-available-in-startup"></a>Startup で使用できるサービス

Web ホストには、`Startup` クラス コンストラクターに使用できるサービスがいくつか用意されています。 アプリケーションから `ConfigureServices` 経由でサービスが追加されます。 これで、ホスト サービスとアプリケーション サービスの両方が `Configure` 内とアプリケーション全体で使用できるようになります。

## <a name="the-configure-method"></a>Configure メソッド

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドは、アプリケーションが HTTP 要求にどのように応答するかを指定するために使用されます。 要求パイプラインは、[ミドルウェア](xref:fundamentals/middleware/index) コンポーネントを [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) インスタンスに追加することで構成されます。 `IApplicationBuilder` は `Configure` メソッドから使用できますが、サービス コンテナーに登録されていません。 ホスティングによって `IApplicationBuilder` が作成され、`Configure` ([参照元](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)) に直接渡されます。

[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)では、開発者の例外ページ、[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的ファイル、および ASP.NET MVC をサポートするパイプラインを構成します。

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

各 `Use` 拡張メソッドで、要求パイプラインにミドルウェア コンポーネントを追加します。 たとえば、`UseMvc` 拡張メソッドでは、[ルーティング ミドルウェア](xref:fundamentals/routing)を要求パイプラインに追加し、既定のハンドラーとして [MVC](xref:mvc/overview) を構成します。

要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。 ショート サーキットがミドルウェアのチェーンで発生しない場合、各ミドルウェアには、クライアントに送信される前に要求を処理する 2 度目の機会があります。

また、`IHostingEnvironment` や `ILoggerFactory` などの追加サービスをメソッド シグネチャで指定することもできます。 指定すると、(使用可能なサービスの場合) 追加サービスが挿入されます。

`IApplicationBuilder` の使用方法およびミドルウェアの処理の順序については、「[ミドルウェア](xref:fundamentals/middleware/index)」を参照してください。

## <a name="convenience-methods"></a>簡易メソッド

`Startup` クラスを指定する代わりに、[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) および [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 簡易メソッドを使用できます。 `ConfigureServices` の複数回の呼び出しでは、互いに追加されます。 `Configure` の複数回の呼び出しでは、最後のメソッド呼び出しが使用されます。

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Startup フィルター

[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) を使用して、アプリケーションのミドルウェア [Configure](#the-configure-method) パイプラインの先頭または末尾でミドルウェアを構成します。 `IStartupFilter` は、アプリケーションの要求処理パイプラインの先頭または末尾で、ライブラリによってミドルウェアが追加される前または後にミドルウェアを確実に実行する場合に便利です。

`IStartupFilter` は、`Action<IApplicationBuilder>` を受け取って返す 1 つのメソッド [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure) を実装しています。 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) で、アプリケーションの要求パイプラインを構成するクラスを定義します。 詳細については、「[Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)」(IApplicationBuilder を使用したミドルウェア パイプラインの作成) を参照してください。

各 `IStartupFilter` は、要求パイプラインで 1 つまたは複数のミドルウェアを実装します。 フィルターは、サービス コンテナーに追加された順に呼び出されます。 フィルターは、コントロールを次のフィルターに渡す前または後にミドルウェアを追加できるため、アプリケーション パイプラインの先頭または末尾に追加されます。

この[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) は、ミドルウェアを `IStartupFilter` に登録する方法を示しています。 このサンプル アプリケーションには、クエリ文字列パラメーターからオプション値を設定するミドルウェアが含まれています。

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` は `RequestSetOptionsStartupFilter` クラスで構成されています。

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` は `ConfigureServices` のサービス コンテナーに登録されています。

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

`option` のクエリ文字列パラメーターを指定すると、ミドルウェアは MVC ミドルウェアが応答をレンダリングする前に値の割り当てを処理します。

![レンダリングされたインデックス ページを表示するブラウザー ウィンドウ。 クエリ文字列パラメーターと 'From Middleware' に設定されたオプションの値を含むページの要求に基づいて、Option の値は 'From Middleware' とレンダリングされます。](startup/_static/index.png)

ミドルウェアの実行順序は、`IStartupFilter` の登録順に設定されています。

* 複数の `IStartupFilter` の実装が、同じオブジェクトとやり取りする可能性があります。 順序が重要な場合は、ミドルウェアの実行順序に合わせて `IStartupFilter` サービスの登録順序を指定してください。
* `IStartupFilter` に登録された他のアプリケーション ミドルウェアの前または後に実行される `IStartupFilter` の実装が 1 つまたは複数あるミドルウェアを、ライブラリで追加することができます。 ライブラリの `IStartupFilter` によって追加されたミドルウェアの前に `IStartupFilter` ミドルウェアを呼び出すには、ライブラリがサービス コンテナーに追加される前にサービスの登録を配置します。 後で呼び出すには、ライブラリが追加される後にサービスの登録を配置します。

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>外部アセンブリからの起動時に構成を追加する

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) の実装により、アプリの `Startup` クラスの外部にある外部アセンブリからの起動時に拡張機能をアプリに追加できるようになります。 詳細については、「[Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration)」(外部アセンブリからアプリを拡張する) を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* [ホスティング](xref:fundamentals/hosting)
* [複数の環境の使用](xref:fundamentals/environments)
* [ミドルウェア](xref:fundamentals/middleware/index)
* [ログ](xref:fundamentals/logging/index)
* [構成](xref:fundamentals/configuration/index)
* [StartupLoader クラス: FindStartupType メソッド (参照元)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
