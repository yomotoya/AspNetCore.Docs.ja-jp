---
title: ASP.NET Core でのアプリケーションのスタートアップ
author: tdykstra
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: 362186be6feeeefeca3c56688ee6420de5fb9659
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468625"
---
# <a name="app-startup-in-aspnet-core"></a>ASP.NET Core でのアプリケーションのスタートアップ

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex)、[Steve Smith](https://ardalis.com)

`Startup` クラスはサービスとアプリケーションの要求パイプラインを構成します。

## <a name="the-startup-class"></a>Startup クラス

ASP.NET Core アプリケーションでは `Startup` クラスが使用されています。このクラスは規約に従って `Startup` と名前が付けられています。 `Startup` クラス:

* 必要に応じて <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> メソッドを含め、アプリの*サービス*を構成することができます。 サービスとは、アプリ機能を提供する再利用可能なコンポーネントです。 サービスは `ConfigureServices` で構成され (&mdash;*登録*と表現されることもあります&mdash;)、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) または <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> を介してアプリ全体で利用されます。
* アプリの要求処理パイプラインを作成するために <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> メソッドを含めます。

`ConfigureServices` と `Configure` はアプリケーションの起動時にランタイムによって呼び出されます。

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

アプリの`Startup`ホスト[がビルドされるとき、](xref:fundamentals/index#host) クラスがアプリに指定されます。 `Program` クラスのホスト ビルダーで `Build` が呼び出されるとき、アプリのホストがビルドされます。 `Startup` クラスは通常、ホスト ビルダーで [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) メソッドを呼び出すことで指定されます。

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

ホストには、`Startup` クラス コンストラクターで使用できるサービスが用意されています。 アプリケーションから `ConfigureServices` 経由でサービスが追加されます。 これで、ホスト サービスとアプリケーション サービスの両方が `Configure` 内とアプリ全体で使用できるようになります。

`Startup` クラスへの[依存関係挿入](xref:fundamentals/dependency-injection)の一般的な用途は、以下を挿入する場合です。

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> (環境別にサービスを構成するため)。
* <xref:Microsoft.Extensions.Configuration.IConfiguration> (構成を読み取るため)。
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> (`Startup.ConfigureServices` でロガーを作成するため)。

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

`IHostingEnvironment` を挿入する代わりに、規約ベースのアプローチがあります。 アプリケーションの環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスが定義されると、実行時に適切な `Startup` クラスが選択されます。 名前のサフィックスが現在の環境と一致するクラスが優先されます。 アプリケーションが Development 環境で実行され、`Startup` クラスと `StartupDevelopment` クラスの両方が含まれている場合は、`StartupDevelopment` クラスが使用されます。 詳細については、「[Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods)」(複数の環境の使用) を参照してください。

ホストの詳細については、「[ホスト](xref:fundamentals/index#host)」を参照してください。 スタートアップ時のエラー処理については、「[Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling)」(スタートアップ例外処理) を参照してください。

## <a name="the-configureservices-method"></a>ConfigureServices メソッド

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> メソッド:

* 任意。
* アプリケーションのサービスを構成する `Configure` メソッドの前にホストによって呼び出されます。
* 規約によって[構成オプション](xref:fundamentals/configuration/index)が設定されます。

一般的なパターンとしては、すべての `Add{Service}` メソッドを呼び出した後にすべての `services.Configure{Service}` メソッドを呼び出します。 たとえば、[Identity サービスの構成](xref:security/authentication/identity#pw)に関する記事をご覧ください。

ホストでは、`Startup` メソッドが呼び出される前にいくつかのサービスを構成することができます。 詳細については、「[ホスト](xref:fundamentals/index#host)」を参照してください。

多くの設定が必要な機能には、<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上の `Add{Service}` 拡張メソッドがあります。 一般的な ASP.NET Core アプリは、Entity Framework、Identity、および MVC のサービスを登録します。

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

サービス コンテナーにサービスを追加すると、アプリケーション内と `Configure` メソッド内でサービスを利用できるようになります。 サービスは[依存関係の挿入](xref:fundamentals/dependency-injection)または <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> によって解決されます。

`SetCompatibilityVersion` について詳しくは、「[SetCompatibilityVersion](xref:mvc/compatibility-version)」をご覧ください。

## <a name="the-configure-method"></a>Configure メソッド

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> メソッドは、アプリケーションが HTTP 要求にどのように応答するかを指定するために使用されます。 要求パイプラインは、[ミドルウェア](xref:fundamentals/middleware/index) コンポーネントを <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> インスタンスに追加することで構成されます。 `IApplicationBuilder` は `Configure` メソッドから使用できますが、サービス コンテナーに登録されていません。 ホスティングによって `IApplicationBuilder` が作成され、`Configure` に直接渡されます。

[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)では、次をサポートするパイプラインが構成されます。

* [開発者例外ページ](xref:fundamentals/error-handling#developer-exception-page)
* [例外ハンドラー](xref:fundamentals/error-handling#exception-handler-page)
* [HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [HTTPS リダイレクト](xref:security/enforcing-ssl)
* [静的ファイル](xref:fundamentals/static-files)
* [一般データ保護規制 (GDPR)](xref:security/gdpr)
* ASP.NET Core [MVC](xref:mvc/overview) と [Razor ページ](xref:razor-pages/index)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

各 `Use` 拡張メソッドによって、要求パイプラインに 1 つまたは複数のミドルウェア コンポーネントが追加されます。 たとえば、`UseMvc` 拡張メソッドでは、[ルーティング ミドルウェア](xref:fundamentals/routing)が要求パイプラインに追加され、既定のハンドラーとして [MVC](xref:mvc/overview) が構成されます。

要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。 ショート サーキットがミドルウェアのチェーンで発生しない場合、各ミドルウェアには、クライアントに送信される前に要求を処理する 2 度目の機会があります。

また、`IHostingEnvironment` や `ILoggerFactory` などの追加サービスを `Configure` メソッド シグネチャで指定することもできます。 指定すると、(使用可能なサービスの場合) 追加サービスが挿入されます。

`IApplicationBuilder` の使用方法およびミドルウェアの処理の順序については、「<xref:fundamentals/middleware/index>」を参照してください。

## <a name="convenience-methods"></a>簡易メソッド

`Startup` クラスを使用せず、サービスと要求処理パイプラインを構成するには、ホスト ビルダーで便利なメソッド、`ConfigureServices` と `Configure` を呼び出します。 `ConfigureServices` の複数回の呼び出しでは、互いに追加されます。 `Configure` メソッドが複数回呼び出された場合、最後の `Configure` 呼び出しが使用されます。

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a>スタートアップ フィルターを使用した Startup の拡張

<xref:Microsoft.AspNetCore.Hosting.IStartupFilter> を使用して、アプリケーションの [Configure](#the-configure-method) ミドルウェア パイプラインの先頭または末尾でミドルウェアを構成します。 `IStartupFilter` は、アプリケーションの要求処理パイプラインの先頭または末尾で、ライブラリによってミドルウェアが追加される前または後にミドルウェアを確実に実行する場合に便利です。

`IStartupFilter` では、`Action<IApplicationBuilder>` を受け取って返す 1 つのメソッド <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> が実装されています。 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> で、アプリケーションの要求パイプラインを構成するクラスを定義します。 詳細については、「[Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)」(IApplicationBuilder を使用したミドルウェア パイプラインの作成) を参照してください。

各 `IStartupFilter` は、要求パイプラインで 1 つまたは複数のミドルウェアを実装します。 フィルターは、サービス コンテナーに追加された順に呼び出されます。 フィルターは、コントロールを次のフィルターに渡す前または後にミドルウェアを追加できるため、アプリケーション パイプラインの先頭または末尾に追加されます。

`IStartupFilter` でミドルウェアを登録する方法を次の例に示します。

`RequestSetOptionsMiddleware` ミドルウェアによって、クエリ文字列パラメーターからオプション値が設定されます。

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` は `RequestSetOptionsStartupFilter` クラスで構成されています。

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` は <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> のサービス コンテナーに登録され、`Startup` クラスの外側から `Startup` を補強します。

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

`option` のクエリ文字列パラメーターを指定すると、ミドルウェアは MVC ミドルウェアが応答をレンダリングする前に値の割り当てを処理します。

![レンダリングされたインデックス ページを表示するブラウザー ウィンドウ。 クエリ文字列パラメーターと 'From Middleware' に設定されたオプションの値を含むページの要求に基づいて、Option の値は 'From Middleware' とレンダリングされます。](startup/_static/index.png)

ミドルウェアの実行順序は、`IStartupFilter` の登録順に設定されています。

* 複数の `IStartupFilter` の実装が、同じオブジェクトとやり取りする可能性があります。 順序が重要な場合は、ミドルウェアの実行順序に合わせて `IStartupFilter` サービスの登録順序を指定してください。
* `IStartupFilter` に登録された他のアプリケーション ミドルウェアの前または後に実行される `IStartupFilter` の実装が 1 つまたは複数あるミドルウェアを、ライブラリで追加することができます。 ライブラリの `IStartupFilter` によって追加されたミドルウェアの前に `IStartupFilter` ミドルウェアを呼び出すには、ライブラリがサービス コンテナーに追加される前にサービスの登録を配置します。 後で呼び出すには、ライブラリが追加される後にサービスの登録を配置します。

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>外部アセンブリからの起動時に構成を追加する

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。 詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* [ホスト](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
