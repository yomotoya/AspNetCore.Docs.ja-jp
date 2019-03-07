---
title: ASP.NET Core の基礎
author: rick-anderson
description: ASP.NET Core アプリの構築に関する基本概念について学習します。
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d4d03-103">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="d4d03-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d4d03-104">この記事では、ASP.NET Core アプリの開発方法を理解するための主要なトピックをまとめています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="d4d03-105">Startup クラス</span><span class="sxs-lookup"><span data-stu-id="d4d03-105">The Startup class</span></span>

<span data-ttu-id="d4d03-106">`Startup` クラスとは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="d4d03-107">アプリで必要なすべてのサービスが構成されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="d4d03-108">要求を処理するパイプラインが定義されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="d4d03-109">サービスを構成 (または*登録*) するコードが `Startup.ConfigureServices` メソッドに追加されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d4d03-110">*サービス*とは、アプリが使用するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="d4d03-111">たとえば、Entity Framework Core コンテキスト オブジェクトはサービスです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="d4d03-112">`Startup.Configure` メソッドには、要求を処理するパイプラインを構成するコードが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="d4d03-113">このパイプラインは、一連の*ミドルウェア* コンポーネントとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="d4d03-114">たとえば、ミドルウェアは、静的ファイルに対する要求を処理したり、HTTPS に HTTP 要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="d4d03-115">各ミドルウェアは `HttpContext` に非同期操作を実行してから、パイプラインの次のミドルウェアを呼び出すか、要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4d03-116">`Startup` クラスの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

<span data-ttu-id="d4d03-117">詳細については、[アプリの Startup](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-117">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d4d03-118">依存性の注入 (サービス)</span><span class="sxs-lookup"><span data-stu-id="d4d03-118">Dependency injection (services)</span></span>

<span data-ttu-id="d4d03-119">ASP.NET Core には、アプリのクラスが構成済みのサービスを利用できるようにする依存性の注入 (DI) フレームワークが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="d4d03-120">クラスのサービスのインスタンスを取得する 1 つの方法は、必要な型のパラメーターを使用したコンストラクターを作成することです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="d4d03-121">このパラメーターには、サービスの種類またはインターフェイスが可能です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="d4d03-122">DI システムは、実行時にこのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-122">The DI system provides the service at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4d03-123">Entity Framework Core コンテキスト オブジェクトを取得するために DI を使用するクラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="d4d03-124">強調表示されている行は、コンストラクターの挿入の例です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

<span data-ttu-id="d4d03-125">DI が組み込まれており、必要に応じてサードパーティ製の制御の反転 (IoC) コンテナーを組み込むことができるよう設計されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="d4d03-126">詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-126">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d4d03-127">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="d4d03-127">Middleware</span></span>

<span data-ttu-id="d4d03-128">要求を処理するパイプラインは、一連のミドルウェア コンポーネントとして構成されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="d4d03-129">各コンポーネントは `HttpContext` に非同期操作を実行してから、パイプラインの次のミドルウェアを呼び出すか、要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="d4d03-130">通常、ミドルウェア コンポーネントは、`Startup.Configure` メソッドのその `Use...` 拡張メソッドを呼び出してパイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="d4d03-131">たとえば、静的ファイルのレンダリングを有効にするには、`UseStaticFiles` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4d03-132">次の例の強調表示されているコードは、要求を処理するパイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

<span data-ttu-id="d4d03-133">ASP.NET Core にはミドルウェアのセットが豊富に組み込まれており、カスタム ミドルウェアをユーザーが記述できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="d4d03-134">詳細については、[ミドルウェア](xref:fundamentals/middleware/index)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-134">For more information, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="d4d03-135">ホスト</span><span class="sxs-lookup"><span data-stu-id="d4d03-135">The host</span></span>

<span data-ttu-id="d4d03-136">ASP.NET Core アプリは起動時に*ホスト*をビルドします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="d4d03-137">ホストとは、次などのアプリのすべてのリソースをカプセル化するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-137">The host is an object that encapsulates all of the the app's resources, such as:</span></span>

* <span data-ttu-id="d4d03-138">HTTP サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="d4d03-138">An HTTP server implementation</span></span>
* <span data-ttu-id="d4d03-139">ミドルウェア コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d4d03-139">Middleware components</span></span>
* <span data-ttu-id="d4d03-140">ログの記録</span><span class="sxs-lookup"><span data-stu-id="d4d03-140">Logging</span></span>
* <span data-ttu-id="d4d03-141">DI</span><span class="sxs-lookup"><span data-stu-id="d4d03-141">DI</span></span>
* <span data-ttu-id="d4d03-142">構成</span><span class="sxs-lookup"><span data-stu-id="d4d03-142">Configuration</span></span>

<span data-ttu-id="d4d03-143">アプリの相互依存するすべてのリソースを 1 つのオブジェクトに含める主な理由は、アプリの起動と正常なシャットダウンの制御の有効期間の管理のためです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="d4d03-144">ホストを作成するコードは、`Program.Main` にあり、[Builder パターン](https://wikipedia.org/wiki/Builder_pattern)に従っています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="d4d03-145">メソッドは、ホストの一部である各リソースを構成するために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="d4d03-146">ビルダー メソッドは、それをすべてまとめ、ホスト オブジェクトをインスタンス化するために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d4d03-147">ASP.NET Core 2.x では、Web アプリに Web Host (`WebHost` クラス) を使用します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-147">ASP.NET Core 2.x uses Web Host (the `WebHost` class) for web apps.</span></span> <span data-ttu-id="d4d03-148">このフレームワークは、次などのよく使用されるオプションと共にホストを設定する `CreateDefaultBuilder` を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-148">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d4d03-149">Web サーバーとして [Kestrel](#servers) を使用し、IIS の統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-149">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d4d03-150">構成を *appsettings.json*、環境変数、コマンド ライン引数およびその他のソースから読み込みます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-150">Load configuration from *appsettings.json*, environment variables, command line arguments, and other sources.</span></span>
* <span data-ttu-id="d4d03-151">ログ出力をコンソールとデバッグ プロバイダーに送ります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-151">Send logging output to the console and debug providers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="d4d03-152">ホストをビルドするサンプル コードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-152">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="d4d03-153">詳細については、[Web ホスト](xref:fundamentals/host/web-host)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-153">For more information, see [Web Host](xref:fundamentals/host/web-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d4d03-154">ASP.NET Core 3.0 では、Web ホスト (`WebHost` クラス) または汎用ホスト (`Host` クラス) を Web アプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-154">In ASP.NET Core 3.0, Web Host (`WebHost` class) or Generic Host (`Host` class) can be used in a web app.</span></span> <span data-ttu-id="d4d03-155">汎用ホストが推奨されますが、下位互換性のために Web ホストを使用することも可能です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-155">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="d4d03-156">このフレームワークは、次などのよく使用されるオプションと共にホストを設定する `CreateDefaultBuilder` メソッドと `ConfigureWebHostDefaults` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-156">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="d4d03-157">Web サーバーとして [Kestrel](#servers) を使用し、IIS の統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-157">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="d4d03-158">構成を *appsettings.json*、*appsettings.[EnvironmentName].json*、環境変数、コマンド ライン引数から読み込みます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-158">Load configuration from *appsettings.json*, *appsettings.[EnvironmentName].json*, environment variables, and command line arguments.</span></span>
* <span data-ttu-id="d4d03-159">ログ出力をコンソールとデバッグ プロバイダーに送ります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-159">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="d4d03-160">ホストをビルドするサンプル コードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-160">Here's sample code that builds a host.</span></span> <span data-ttu-id="d4d03-161">ホストを設定するメソッドとよく使用されるオプションが強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-161">The methods that set up the host with commonly used options are highlighted.</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="d4d03-162">詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)と [Web ホスト](xref:fundamentals/host/web-host)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-162">For more information, see [Generic Host](xref:fundamentals/host/generic-host) and [Web Host](xref:fundamentals/host/web-host)</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="d4d03-163">高度なホスト シナリオ</span><span class="sxs-lookup"><span data-stu-id="d4d03-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="d4d03-164">Web ホストは、他の種類の .NET アプリでは必要のない、HTTP サーバー実装を含めるよう設計されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-164">Web Host is designed to include an HTTP server implementation, which isn't needed for other kinds of .NET apps.</span></span> <span data-ttu-id="d4d03-165">2.1 以降では、汎用ホスト (`Host` クラス) は、ASP.NET Core アプリのみでなくすべての .NET Core アプリで使用できます。&mdash;</span><span class="sxs-lookup"><span data-stu-id="d4d03-165">Starting in 2.1, Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d4d03-166">汎用ホストでは、他の種類のアプリのログ記録、DI、構成、および有効期間管理などの横断的な機能で使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-166">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="d4d03-167">詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-167">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d4d03-168">汎用ホストは、ASP.NET Core アプリのみでなくすべての .NET Core アプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-168">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="d4d03-169">汎用ホストでは、他の種類のアプリのログ記録、DI、構成、および有効期間管理などの横断的な機能で使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-169">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="d4d03-170">詳細については、[汎用ホスト](xref:fundamentals/host/generic-host)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-170">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

<span data-ttu-id="d4d03-171">ホストを使用して、バックグラウンド タスクを実行することも可能です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="d4d03-172">詳細については、[バックグラウンド タスク](xref:fundamentals/host/hosted-services)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-172">For more information, see [Background tasks](xref:fundamentals/host/hosted-services).</span></span>

## <a name="servers"></a><span data-ttu-id="d4d03-173">サーバー</span><span class="sxs-lookup"><span data-stu-id="d4d03-173">Servers</span></span>

<span data-ttu-id="d4d03-174">ASP.NET Core アプリは、HTTP 要求をリッスンするために HTTP サーバー実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="d4d03-175">サーバーは、`HttpContext` に構成した[要求機能](xref:fundamentals/request-features)のセットとして、アプリへの要求を公開します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d4d03-176">Windows</span><span class="sxs-lookup"><span data-stu-id="d4d03-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="d4d03-177">ASP.NET Core では、次のサーバー実装が提供されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d4d03-178">*Kestrel* は、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d4d03-179">Kestrel は [IIS](https://www.iis.net/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d4d03-180">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d4d03-181">*IIS HTTP サーバー*は、IIS を使用する Windows のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="d4d03-182">このサーバーでは、ASP.NET Core アプリと IIS が同じプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="d4d03-183">*HTTP.sys* は、IIS とは一緒に使用しない Windows のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d4d03-184">macOS</span><span class="sxs-lookup"><span data-stu-id="d4d03-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="d4d03-185">ASP.NET Core は、*Kestrel* クロスプラットフォーム サーバーの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d4d03-186">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d4d03-187">Kestrel は [Nginx](https://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d4d03-188">Linux</span><span class="sxs-lookup"><span data-stu-id="d4d03-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="d4d03-189">ASP.NET Core は、*Kestrel* クロスプラットフォーム サーバーの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d4d03-190">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d4d03-191">Kestrel は [Nginx](https://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="d4d03-192">Windows</span><span class="sxs-lookup"><span data-stu-id="d4d03-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="d4d03-193">ASP.NET Core では、次のサーバー実装が提供されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="d4d03-194">*Kestrel* は、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="d4d03-195">Kestrel は [IIS](https://www.iis.net/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="d4d03-196">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="d4d03-197">*HTTP.sys* は、IIS とは一緒に使用しない Windows のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d4d03-198">macOS</span><span class="sxs-lookup"><span data-stu-id="d4d03-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="d4d03-199">ASP.NET Core は、*Kestrel* クロスプラットフォーム サーバーの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d4d03-200">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d4d03-201">Kestrel は [Nginx](https://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d4d03-202">Linux</span><span class="sxs-lookup"><span data-stu-id="d4d03-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="d4d03-203">ASP.NET Core は、*Kestrel* クロスプラットフォーム サーバーの実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="d4d03-204">Kestrel は、ASP.NET Core 2.0 以降で、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="d4d03-205">Kestrel は [Nginx](http://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="d4d03-206">詳細については、[サーバー](xref:fundamentals/servers/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-206">For more information, see [Servers](xref:fundamentals/servers/index).</span></span>

## <a name="configuration"></a><span data-ttu-id="d4d03-207">構成</span><span class="sxs-lookup"><span data-stu-id="d4d03-207">Configuration</span></span>

<span data-ttu-id="d4d03-208">ASP.NET Core は、構成プロバイダーの順序付けされたセットから、名前と値のペアの設定を取得する構成フレームワークとなります。</span><span class="sxs-lookup"><span data-stu-id="d4d03-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="d4d03-209">*.json* ファイル、*.xml* ファイル、環境変数、コマンドライン引数など、さまざまなソース用に構成プロバイダーが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="d4d03-210">独自のカスタム構成プロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="d4d03-211">たとえば、構成は *appsettings.json* と環境変数から取得したものであると指定できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="d4d03-212">このとき *ConnectionString* 値が要求されると、フレームワークはまず *appsettings.json* ファイルを参照します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="d4d03-213">値がそこにあり、しかし環境変数にもある場合、環境変数の値が優先されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="d4d03-214">ASP.NET Core には、パスワードなどの機密の構成データの管理に[シークレット マネージャー ツール](xref:security/app-secrets)が用意されています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d4d03-215">実稼働の機密情報には、[Azure Key Vault](/aspnet/core/security/key-vault-configuration) を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-215">For production secrets, we recommend [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span></span>

<span data-ttu-id="d4d03-216">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-216">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="options"></a><span data-ttu-id="d4d03-217">オプション</span><span class="sxs-lookup"><span data-stu-id="d4d03-217">Options</span></span>

<span data-ttu-id="d4d03-218">ASP.NET Core では、構成値の格納と取得に、可能な限り*オプション パターン*を使用します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="d4d03-219">オプション パターンではクラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="d4d03-220">たとえば、以下のコードでは WebSockets のオプションが設定されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="d4d03-221">詳細については、[オプション](xref:fundamentals/configuration/options)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-221">For more information, see [Options](xref:fundamentals/configuration/options).</span></span>

## <a name="environments"></a><span data-ttu-id="d4d03-222">環境</span><span class="sxs-lookup"><span data-stu-id="d4d03-222">Environments</span></span>

<span data-ttu-id="d4d03-223">*開発*、*ステージング*、および*実稼働*などの実行環境は ASP.NET Core の最上の概念です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="d4d03-224">アプリが実行している環境は、`ASPNETCORE_ENVIRONMENT` 環境変数を設定することにより指定できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d4d03-225">ASP.NET Core は、アプリの起動時にその環境変数を読み取り、その値を `IHostingEnvironment` 実装に格納します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="d4d03-226">この環境オブジェクトは、DI を介しアプリの任意の場所で使用されます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-226">The environment object is available anywhere in the app via DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4d03-227">`Startup` クラスの次のサンプル コードは、それが開発環境で実行された場合のみ、詳細なエラー情報を提供するようアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

<span data-ttu-id="d4d03-228">詳細については、[環境](xref:fundamentals/environments)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-228">For more information, see [Environments](xref:fundamentals/environments).</span></span>

## <a name="logging"></a><span data-ttu-id="d4d03-229">ログの記録</span><span class="sxs-lookup"><span data-stu-id="d4d03-229">Logging</span></span>

<span data-ttu-id="d4d03-230">ASP.NET Core では、組み込みやサード パーティ製のさまざまなログ プロバイダーと連携するログ API がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="d4d03-231">利用可能なプロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-231">Available providers include the following:</span></span>

* <span data-ttu-id="d4d03-232">コンソール</span><span class="sxs-lookup"><span data-stu-id="d4d03-232">Console</span></span>
* <span data-ttu-id="d4d03-233">デバッグ</span><span class="sxs-lookup"><span data-stu-id="d4d03-233">Debug</span></span>
* <span data-ttu-id="d4d03-234">Event Tracing on Windows</span><span class="sxs-lookup"><span data-stu-id="d4d03-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="d4d03-235">Windows イベント ログ</span><span class="sxs-lookup"><span data-stu-id="d4d03-235">Windows Event Log</span></span>
* <span data-ttu-id="d4d03-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="d4d03-236">TraceSource</span></span>
* <span data-ttu-id="d4d03-237">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d4d03-237">Azure App Service</span></span>
* <span data-ttu-id="d4d03-238">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="d4d03-238">Azure Application Insights</span></span>

<span data-ttu-id="d4d03-239">DI からの `ILogger` オブジェクトの取得およびログ メソッドの呼び出しによってアプリのコードの任意の場所からログを記述します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4d03-240">コンストラクターの挿入とログ記録メソッドの呼び出しが強調表示されている `ILogger` オブジェクトを使用するサンプル コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

<span data-ttu-id="d4d03-241">`ILogger` インターフェイスは、ログ プロバイダーに任意の数のフィールドを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="d4d03-242">このフィールドは、一般的にメッセージの文字列を構築するために使用しますが、プロバイダーがデータ ストアに別のフィールドとして送信することも可能です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="d4d03-243">この機能は、[構造化ロギングとも呼ばれるセマンティック ロギング](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)をログ プロバイダーが実装するのを可能にします。</span><span class="sxs-lookup"><span data-stu-id="d4d03-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="d4d03-244">詳細については、[ログ記録](xref:fundamentals/logging/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-244">For more information, see [Logging](xref:fundamentals/logging/index).</span></span>

## <a name="routing"></a><span data-ttu-id="d4d03-245">ルーティング</span><span class="sxs-lookup"><span data-stu-id="d4d03-245">Routing</span></span>

<span data-ttu-id="d4d03-246">*ルート*とは、ハンドラーにマップされている URL のパターンです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="d4d03-247">このハンドラーは一般的には Razor Pages、MVC コントローラーのアクション メソッドまたはミドルウェアです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="d4d03-248">ASP.NET Core のルーティングでは、アプリで使用する URL を制御できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="d4d03-249">詳細については、[ルーティング](xref:fundamentals/routing)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-249">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="error-handling"></a><span data-ttu-id="d4d03-250">エラー処理</span><span class="sxs-lookup"><span data-stu-id="d4d03-250">Error handling</span></span>

<span data-ttu-id="d4d03-251">ASP.NET Core には、次などのエラー処理用の機能が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="d4d03-252">開発者例外ページ</span><span class="sxs-lookup"><span data-stu-id="d4d03-252">A developer exception page</span></span>
* <span data-ttu-id="d4d03-253">カスタム エラー ページ</span><span class="sxs-lookup"><span data-stu-id="d4d03-253">Custom error pages</span></span>
* <span data-ttu-id="d4d03-254">静的状態コード ページ</span><span class="sxs-lookup"><span data-stu-id="d4d03-254">Static status code pages</span></span>
* <span data-ttu-id="d4d03-255">起動時の例外処理</span><span class="sxs-lookup"><span data-stu-id="d4d03-255">Startup exception handling</span></span>

<span data-ttu-id="d4d03-256">詳細については、[エラー処理](xref:fundamentals/error-handling)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-256">For more information, see [Error handling](xref:fundamentals/error-handling).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a><span data-ttu-id="d4d03-257">HTTP 要求を行う</span><span class="sxs-lookup"><span data-stu-id="d4d03-257">Make HTTP requests</span></span>

<span data-ttu-id="d4d03-258">`HttpClient` インスタンスの作成に、`IHttpClientFactory` の実装を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="d4d03-259">ファクトリは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="d4d03-259">The factory:</span></span>

* <span data-ttu-id="d4d03-260">論理 `HttpClient` インスタンスの名前付けと構成を一元化します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="d4d03-261">たとえば、*github* クライアントを登録して、GitHub にアクセスするように構成できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="d4d03-262">既定のクライアントは、他の目的に登録できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="d4d03-263">複数のデリゲート ハンドラーを登録してチェーン化し、送信要求ミドルウェア パイプラインを構築するのをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="d4d03-264">このパターンは、ASP.NET Core での受信ミドルウェア パイプラインに似ています。</span><span class="sxs-lookup"><span data-stu-id="d4d03-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="d4d03-265">このパターンは、キャッシュ、エラー処理、シリアル化、ログ記録など、HTTP 要求に関する横断的関心事を管理するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="d4d03-266">一時的な障害処理用の人気のサードパーティ製ライブラリ、*Polly* と統合できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="d4d03-267">基になっている `HttpClientMessageHandler` インスタンスのプールと有効期間を管理し、`HttpClient` の有効期間を手動で管理するときに発生する一般的な DNS の問題を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="d4d03-268">ファクトリによって作成されたクライアントから送信されるすべての要求に対し、(*ILogger* によって) 構成可能なログ エクスペリエンスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-268">Adds a configurable logging experience (via *ILogger*) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="d4d03-269">詳細については、[HTTP 要求の実行](xref:fundamentals/http-requests)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-269">For more information, see [Make HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="content-root"></a><span data-ttu-id="d4d03-270">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="d4d03-270">Content root</span></span>

<span data-ttu-id="d4d03-271">このコンテンツ ルートは、その Razor ファイルなど、アプリで使用されるすべてのプライベート コンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="d4d03-272">既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="d4d03-273">[ホストの構築時](#host)には、別の場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d4d03-274">詳細については、[コンテンツ ルート](xref:fundamentals/host/web-host#content-root)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-274">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="d4d03-275">詳細については、[コンテンツ ルート](xref:fundamentals/host/generic-host#content-root)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-275">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="d4d03-276">Web ルート</span><span class="sxs-lookup"><span data-stu-id="d4d03-276">Web root</span></span>

<span data-ttu-id="d4d03-277">(*webroot* としても知られている) Web ルートは、CSS、JavaScript、およびイメージ ファイルなど、パブリックな静的リソースへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="d4d03-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d4d03-278">既定で、この静的ファイル ミドルウェアは、Web ルート ディレクトリ (とそのサブディレクトリ) のファイルにのみサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="d4d03-279">web ルートのパスの既定は、*\<コンテンツ ルート>/wwwroot* です。ただし、[ホストの構築](#host)時に、別の場所を指定することも可能です。</span><span class="sxs-lookup"><span data-stu-id="d4d03-279">The web root path defaults to *\<content root>/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="d4d03-280">Razor (*.cshtml*) のファイルの場合、チルダとスラッシュ `~/` が webroot を指します。</span><span class="sxs-lookup"><span data-stu-id="d4d03-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="d4d03-281">`~/` で始まるパスは仮想パスと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="d4d03-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="d4d03-282">詳しくは、[静的ファイル](xref:fundamentals/static-files)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d4d03-282">For more information, see [Static files](xref:fundamentals/static-files).</span></span>
