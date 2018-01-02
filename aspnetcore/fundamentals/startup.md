---
title: "ASP.NET Core でのアプリケーションの起動"
author: ardalis
description: "ASP.NET Core でスタートアップ クラスがサービスとアプリケーションの要求パイプラインを構成する方法を検出します。"
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core でのアプリケーションの起動

によって[Steve Smith](https://ardalis.com)、 [Tom Dykstra](https://github.com/tdykstra)、および[Luke Latham](https://github.com/guardrex)

`Startup`クラスは、サービスとアプリケーションの要求パイプラインを構成します。

## <a name="the-startup-class"></a>スタートアップ クラス

ASP.NET Core アプリで使用する、`Startup`というクラス`Startup`慣例です。 `Startup`クラス。

* 必要に応じて含めることができます、 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)アプリのサービスを構成する方法です。
* 含める必要があります、[構成](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)メソッドをアプリの要求処理パイプラインを作成します。

`ConfigureServices`および`Configure`アプリの起動時にランタイムによって呼び出されます。

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

指定して、`Startup`クラス、 [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)メソッド。

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup`クラス コンス トラクターは、ホストによって定義された依存関係を受け取ります。 一般的な用途[依存性の注入](xref:fundamentals/dependency-injection)に、`Startup`クラスは、挿入する[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)環境によってサービスを構成します。

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

挿入する代わりに`IHostingStartup`規則ベースのアプローチを使用することです。 アプリを個別に定義できます`Startup`さまざまな環境のためのクラス (たとえば、 `StartupDevelopment`)、実行時に適切なスタートアップ クラスが選択されているとします。 ある名前サフィックスが、現在の環境と一致するクラスが優先順位します。 アプリ開発環境で実行され、両方が含まれる場合、`Startup`クラスおよび`StartupDevelopment`クラス、`StartupDevelopment`クラスを使用します。 詳細については、次を参照してください。[複数の環境で作業](xref:fundamentals/environments#startup-conventions)です。

について詳しく学習する`WebHostBuilder`を参照してください、[ホスティング](xref:fundamentals/hosting)トピックです。 起動中にエラーを処理する方法については、次を参照してください。[スタートアップ例外処理](xref:fundamentals/error-handling#startup-exception-handling)です。

## <a name="the-configureservices-method"></a>ConfigureServices メソッド

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)メソッドは。

* 任意。
* 前に、web ホストによって呼び出される、`Configure`アプリのサービスを構成する方法です。
* ここで[構成オプション](xref:fundamentals/configuration/index)規約によって設定されます。

サービスをサービス コンテナーに追加することが可能になります内およびアプリ内で、`Configure`メソッドです。 サービスは、によって解決[依存性の注入](xref:fundamentals/dependency-injection)またはから[IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)です。

Web ホストが前にいくつかのサービスを構成することがあります`Startup`メソッドが呼び出されます。 詳細については、[ホスティング](xref:fundamentals/hosting)トピックです。 

大幅なセットアップを必要とする機能がある`Add[Service]`拡張メソッド[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection)です。 一般的な web アプリは、Entity Framework、Id、および MVC 用のサービスを登録します。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>スタートアップで使用できるサービス

Web ホストに使用できるいくつかのサービスを提供する、`Startup`クラスのコンス トラクターです。 アプリを使用して追加のサービスを追加する`ConfigureServices`です。 ホストとアプリの両方のサービスを利用しで`Configure`およびアプリケーション全体でします。

## <a name="the-configure-method"></a>Configure メソッド

[構成](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)メソッドは、アプリが HTTP 要求に応答する方法を指定するために使用します。 追加することで、要求パイプラインが構成されている[ミドルウェア](xref:fundamentals/middleware)コンポーネントを[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)インスタンス。 `IApplicationBuilder`使用できる、`Configure`メソッドは、サービス コンテナーに登録されていません。 ホストを作成、`IApplicationBuilder`に直接渡されますと`Configure`([参照ソース](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。

[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)開発者例外ページでは、サポート、パイプラインを構成する[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的ファイル、および ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

各`Use`拡張メソッドは、要求パイプラインにミドルウェア コンポーネントを追加します。 インスタンス、`UseMvc`拡張メソッドを追加、[ルーティング ミドルウェア](xref:fundamentals/routing)要求パイプラインを構成および[MVC](xref:mvc/overview)既定のハンドラーとして。

追加サービスなど、`IHostingEnvironment`と`ILoggerFactory`、メソッド シグネチャ内でも指定することがあります。 指定した場合、使用可能な場合にその他のサービスが挿入されたします。

使用する方法の詳細についての`IApplicationBuilder`を参照してください[ミドルウェア](xref:fundamentals/middleware)です。

## <a name="convenience-methods"></a>便利なメソッド

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices)と[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure)便利なメソッドを指定する代わりに使用できます、`Startup`クラスです。 複数回呼び出す`ConfigureServices`互いに追加します。 複数回呼び出す`Configure`最後のメソッド呼び出しを使用します。

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>スタートアップ フィルター

使用して[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter)ミドルウェアを構成するには、先頭または末尾のアプリの[構成](#the-configure-method)ミドルウェア パイプライン。 `IStartupFilter`ミドルウェアが前に、または後に開始またはアプリの要求処理パイプラインの最後のライブラリによって追加されたミドルウェアが実行されるようにするのに便利です。

`IStartupFilter`1 つのメソッドを実装する[構成](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure)、受信を返す、`Action<IApplicationBuilder>`です。 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)アプリの要求パイプラインを構成するクラスを定義します。 詳細については、次を参照してください。 [IApplicationBuilder のミドルウェア パイプラインを作成する](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder)です。

各`IStartupFilter`要求パイプライン内の 1 つまたは複数の middlewares を実装します。 フィルターは、サービス コンテナーに追加された順序で呼び出されます。 フィルターを追加する前にミドルウェアまたは後、次のフィルターに制御を渡すこと、したがって、追加の先頭または末尾にあるアプリケーション パイプラインにします。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) とミドルウェアを登録する方法を示します`IStartupFilter`です。 サンプル アプリには、クエリ文字列パラメーターから、オプションの値を設定するミドルウェアが含まれています。

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware`で構成された、`RequestSetOptionsStartupFilter`クラス。

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter`でサービス コンテナーに登録されて`ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

ときのクエリ文字列パラメーター`option`は提供された場合、ミドルウェアが MVC ミドルウェアが応答を表示する前に、値の割り当てを処理します。

![ブラウザー ウィンドウが表示するインデックス ページを表示します。 'からミドルウェア' ベースのクエリ文字列パラメーターと 'からミドルウェア' に設定するオプションの値を持つページを要求するのには、オプションの値が表示されます。](startup/_static/index.png)

ミドルウェアの実行順序がの順序で設定されている`IStartupFilter`登録。

* 複数`IStartupFilter`実装が同じのオブジェクトと対話することがあります。 順序付けが重要な場合は、注文、 `IStartupFilter` middlewares を実行する順序と一致する登録のサービスを提供します。
* ライブラリは追加する 1 つまたは複数のミドルウェア`IStartupFilter`前に、または後に登録されている他のアプリのミドルウェアを実行している実装`IStartupFilter`です。 呼び出す、`IStartupFilter`ライブラリのによって追加されたミドルウェアの前にミドルウェア`IStartupFilter`ライブラリは、サービス コンテナーに追加される前に、サービスの登録を配置します。 後でこれを呼び出すには、ライブラリを追加した後、サービスの登録を置きます。

## <a name="additional-resources"></a>その他の技術情報

* [ホスティング](xref:fundamentals/hosting)
* [複数の環境の使用](xref:fundamentals/environments)
* [ミドルウェア](xref:fundamentals/middleware)
* [ログ](xref:fundamentals/logging/index)
* [構成](xref:fundamentals/configuration/index)
* [StartupLoader クラス: FindStartupType メソッド (参照元)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
