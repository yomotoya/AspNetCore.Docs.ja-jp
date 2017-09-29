---
title: "ASP.NET Core でのアプリケーションの起動"
author: ardalis
description: "ASP.NET Core でスタートアップ クラスをについて説明します。"
keywords: "ASP.NET Core、スタートアップ時に、構成する方法、ConfigureServices メソッド"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 94db2ff530b5de7fe357cfb591d09b984cb248f9
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core でのアプリケーションの起動

によって[Steve Smith](https://ardalis.com/)と[Tom Dykstra](https://github.com/tdykstra/)

`Startup`クラスは、サービスとアプリケーションの要求パイプラインを構成します。 

## <a name="the-startup-class"></a>スタートアップ クラス

ASP.NET Core アプリケーションが必要な`Startup`というクラス`Startup`慣例です。 スタートアップ クラス名を指定する、`Main`プログラムの[WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)メソッドです。 参照してください[ホスティング](xref:fundamentals/hosting)について詳しく学習する`WebHostBuilder`、前に実行される`Startup`です。

独立したを定義することができます`Startup`クラスのさまざまな環境、および適切ないずれかが実行時に選択されます。 指定した場合`startupAssembly`で、 [WebHost 構成](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host)オプション をホストしているか、またはそのスタートアップ アセンブリの読み込みおよび検索、`Startup`または`Startup[Environment]`型です。 クラスを名前サフィックスと一致する現在の環境は優先順位を付けるために、アプリを実行、*開発*環境では、両方が含まれると、`Startup`と`StartupDevelopment`クラス、`StartupDevelopment`クラスになります使用されます。 参照してください[FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs)で`StartupLoader`と[複数の環境で作業](environments.md#startup-conventions)です。

またを定義し、固定`Startup`呼び出すことにより、環境に関係なく使用されるクラス`UseStartup<TStartup>`です。 この方法をお勧めします。

`Startup`クラスのコンス トラクターがから提供される依存関係を受け入れることができる[依存性の注入](xref:fundamentals/dependency-injection)です。 一般的な方法は、使用する`IHostingEnvironment`を設定する[構成](xref:fundamentals/configuration)ソース。

`Startup`クラスを含める必要があります、`Configure`メソッドを必要に応じて含めることができます、`ConfigureServices`メソッド、アプリケーションの起動時にどちらもと呼ばれます。 クラスを含めることも[環境固有のバージョンをこれらのメソッドの](xref:fundamentals/environments#startup-conventions)します。 `ConfigureServices`、存在する場合は、前に呼び出されます`Configure`です。

について学習[アプリケーションの起動中に例外を処理](xref:fundamentals/error-handling#startup-exception-handling)です。

## <a name="the-configureservices-method"></a>ConfigureServices メソッド

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_)メソッドは省略可能です。 が、このオプションを使用すると、関数が呼び出される前に、 `Configure` web ホストでのメソッドです。 Web ホストが前にいくつかのサービスを構成することがあります``Startup``メソッドが呼び出される (を参照してください[ホスト](xref:fundamentals/hosting))。 慣例により、[構成オプション](xref:fundamentals/configuration)はこのメソッドで設定します。

大量のセットアップが必要な機能がある`Add[Service]`拡張メソッド[IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection)です。 既定の web サイト テンプレートからこの例は、Entity Framework、Id、および MVC 用のサービスを使用するアプリを構成します。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

サービスをサービス コンテナーに追加することが可能になります経由でのアプリケーション内で[依存性の注入](xref:fundamentals/dependency-injection)です。

## <a name="services-available-in-startup"></a>スタートアップで使用できるサービス

ASP.NET Core 依存関係の挿入は、アプリケーションの起動中にサービスを提供します。 パラメーターとして、適切なインターフェイスを含めることによってこれらのサービスを要求することができます、`Startup`クラスのコンス トラクターまたはその`Configure`メソッドです。 `ConfigureServices`メソッドはわずかな`IServiceCollection`パラメーター (ただし、追加のパラメーターは必要ありませんので、このコレクションから取得するサービスが登録されていることができます)。

によって要求された通常のサービスの一部を次に示します`Startup`メソッド。

* コンス トラクターで: `IHostingEnvironment`、`ILogger<Startup>`
* `ConfigureServices`:`IServiceCollection`
* `Configure`: `IApplicationBuilder`、 `IHostingEnvironment`、`ILoggerFactory`

によって追加されたすべてのサービス、 ``WebHostBuilder`` ``ConfigureServices``によって要求されるメソッド、``Startup``クラスのコンス トラクターまたはその``Configure``メソッドです。 使用して`WebHostBuilder`中にいずれかのサービスを提供する必要がある`Startup`メソッドです。

## <a name="the-configure-method"></a>Configure メソッド

`Configure`メソッドは、ASP.NET アプリケーションが HTTP 要求に応答する方法を指定するために使用します。 追加することで、要求パイプラインが構成されている[ミドルウェア](middleware.md)コンポーネントを`IApplicationBuilder`依存性の注入が提供するインスタンス。

既定の web サイト テンプレートから次の例でいくつかの拡張メソッドを使用して、サポート、パイプラインを構成する[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的なファイル、ASP.NET MVC、および Id。

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

各`Use`拡張メソッドを追加、[ミドルウェア](xref:fundamentals/middleware)コンポーネントを要求パイプライン。 インスタンス、`UseMvc`拡張メソッドを追加、[ルーティング](routing.md)要求パイプラインにミドルウェアを構成および[MVC](xref:mvc/overview)既定のハンドラーとして。

使用する方法の詳細についての`IApplicationBuilder`を参照してください[ミドルウェア](xref:fundamentals/middleware)です。

などの他のサービス`IHostingEnvironment`と`ILoggerFactory`メソッド シグネチャ内で指定することもありますいる場合にこれらのサービスが表示されます[挿入](dependency-injection.md)可能な場合です。 

## <a name="additional-resources"></a>その他のリソース

* [複数の環境の使用](xref:fundamentals/environments)
* [ミドルウェア](xref:fundamentals/middleware)
* [ログ](xref:fundamentals/logging)
* [構成](xref:fundamentals/configuration)
