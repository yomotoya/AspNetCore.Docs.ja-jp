---
title: ASP.NET Core での依存関係の挿入
author: guardrex
description: ASP.NET Core で依存関係の挿入を実装する方法とそれを使う方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: b9c322e56c0902c2a78bbbf2563dd01ce79fdc9a
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927898"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="1e6aa-103">ASP.NET Core での依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="1e6aa-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="1e6aa-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1e6aa-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1e6aa-105">ASP.NET Core では依存関係の挿入 (DI) ソフトウェア設計パターンがサポートされています。これは、クラスとその依存関係の間で[制御の反転 (IoC)](https://deviq.com/inversion-of-control/) を実現するための手法です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](https://deviq.com/inversion-of-control/) between classes and their dependencies.</span></span>

<span data-ttu-id="1e6aa-106">MVC コントローラー内部における依存関係の挿入に固有の情報については、<xref:mvc/controllers/dependency-injection> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="1e6aa-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="1e6aa-108">依存関係の挿入の概要</span><span class="sxs-lookup"><span data-stu-id="1e6aa-108">Overview of dependency injection</span></span>

<span data-ttu-id="1e6aa-109">"*依存関係*" とは、他のオブジェクトが必要とする任意のオブジェクトのことです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="1e6aa-110">アプリ内の他のクラスが依存している、次の `WriteMessage` メソッドを備えた `MyDependency` クラスを調べます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1e6aa-111">クラスで `WriteMessage` メソッドを使用できるようにするために、`MyDependency` クラスのインスタンスを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="1e6aa-112">`MyDependency` クラスは `IndexModel` クラスの依存関係です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="1e6aa-113">クラスで `WriteMessage` メソッドを使用できるようにするために、`MyDependency` クラスのインスタンスを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="1e6aa-114">`MyDependency` クラスは `HomeController` クラスの依存関係です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="1e6aa-115">このクラスは `MyDependency` のインスタンスを作成し、これに直接依存しています。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="1e6aa-116">コードの依存関係 (前の例など) には問題が含まれ、次の理由から回避する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="1e6aa-117">`MyDependency` を別の実装で置き換えるには、クラスを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="1e6aa-118">`MyDependency` が依存関係を含んでいる場合、これらはクラスによって構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="1e6aa-119">複数のクラスが `MyDependency` に依存している大規模なプロジェクトでは、構成コードがアプリ全体に分散するようになります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="1e6aa-120">このような実装では、単体テストを行うことが困難です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="1e6aa-121">アプリはモックまたはスタブの `MyDependency` クラスを使用する必要がありますが、この方法では不可能です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="1e6aa-122">依存関係の挿入は、次の方法によってこれらの問題に対応します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="1e6aa-123">依存関係の実装を抽象化するための、インターフェイスの使用。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="1e6aa-124">サービス コンテナー内の依存関係の登録。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="1e6aa-125">ASP.NET Core には、組み込みのサービス コンテナー [IServiceProvider](/dotnet/api/system.iserviceprovider) が用意されています。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="1e6aa-126">サービスはアプリの `Startup.ConfigureServices` メソッドに登録されています。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="1e6aa-127">サービスを使用するクラスのコンストラクターへの、サービスの "*挿入*"。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="1e6aa-128">依存関係のインスタンスの作成、およびインスタンスが不要になったときの廃棄の役割を、フレームワークが担当します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="1e6aa-129">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)では、サービスがアプリに提供するメソッドが `IMyDependency` インターフェイスによって定義されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-130">このインターフェイスは、具象型 `MyDependency` によって実装されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-131">`MyDependency` はそのコンストラクター内で [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) を要求します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="1e6aa-132">依存関係の挿入をチェーン形式で使用することは、一般的ではありません。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="1e6aa-133">次に、要求されたそれぞれの依存関係が、それ自身の依存関係を要求します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="1e6aa-134">コンテナーによってグラフ内の依存関係が解決され、完全に解決されたサービスが返されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="1e6aa-135">解決する必要がある依存関係の集合的なセットは、通常、"*依存関係ツリー*"、"*依存関係グラフ*"、または "*オブジェクト グラフ*" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="1e6aa-136">`IMyDependency` と `ILogger<TCategoryName>` をサービス コンテナーに登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="1e6aa-137">`IMyDependency` は `Startup.ConfigureServices` に登録されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1e6aa-138">`ILogger<TCategoryName>` はログ記録の抽象化インフラストラクチャによって登録されます。したがって、これは、フレームワークによって既定で登録される[フレームワークが提供するサービス](#framework-provided-services)です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="1e6aa-139">サンプル アプリにおいて、`IMyDependency` サービスは `MyDependency` 具象型を使用して登録されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="1e6aa-140">登録によって、サービスの有効期間が 1 つの要求の有効期間に範囲設定されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="1e6aa-141">[サービスの有効期間](#service-lifetimes)については、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="1e6aa-142">各 `services.Add{SERVICE_NAME}` 拡張メソッドは、サービスを追加 (および場合によっては構成) します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="1e6aa-143">たとえば、`services.AddMvc()` はサービスの Razor Pages と必須の MVC を追加します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="1e6aa-144">アプリをこの規則に従わせることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="1e6aa-145">拡張メソッドを [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 名前空間に配置して、サービス登録のグループをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="1e6aa-146">サービスのコンストラクターでプリミティブ (`string` など) が必要な場合は、[構成](xref:fundamentals/configuration/index)や[オプション パターン](xref:fundamentals/configuration/options)を使ってプリミティブを挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="1e6aa-147">クラスのコンストラクター経由でサービスのインスタンスが要求されます。サービスはプライベート フィールドに割り当てられて使用されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="1e6aa-148">このフィールドは、クラス全体で必要に応じてサービスにアクセスするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="1e6aa-149">サンプル アプリでは、`IMyDependency` インスタンスが要求され、サービスの `WriteMessage` メソッドを呼び出すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="1e6aa-150">フレームワークが提供するサービス</span><span class="sxs-lookup"><span data-stu-id="1e6aa-150">Framework-provided services</span></span>

<span data-ttu-id="1e6aa-151">`Startup.ConfigureServices` メソッドでは、Entity Framework Core や ASP.NET Core MVC のようなプラットフォーム機能など、アプリが使うサービスを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="1e6aa-152">最初に、`ConfigureServices` に提供される `IServiceCollection` では、次のサービスが定義されています ([ホストの構成方法](xref:fundamentals/host/index)に依存します)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="1e6aa-153">サービスの種類</span><span class="sxs-lookup"><span data-stu-id="1e6aa-153">Service Type</span></span> | <span data-ttu-id="1e6aa-154">有効期間</span><span class="sxs-lookup"><span data-stu-id="1e6aa-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="1e6aa-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="1e6aa-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="1e6aa-156">一時的</span><span class="sxs-lookup"><span data-stu-id="1e6aa-156">Transient</span></span> |
| [<span data-ttu-id="1e6aa-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="1e6aa-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="1e6aa-158">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-158">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="1e6aa-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="1e6aa-160">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-160">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="1e6aa-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="1e6aa-162">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-162">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="1e6aa-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="1e6aa-164">一時的</span><span class="sxs-lookup"><span data-stu-id="1e6aa-164">Transient</span></span> |
| [<span data-ttu-id="1e6aa-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="1e6aa-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="1e6aa-166">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-166">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="1e6aa-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="1e6aa-168">一時的</span><span class="sxs-lookup"><span data-stu-id="1e6aa-168">Transient</span></span> |
| [<span data-ttu-id="1e6aa-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="1e6aa-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="1e6aa-170">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-170">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="1e6aa-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="1e6aa-172">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-172">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="1e6aa-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="1e6aa-174">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-174">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="1e6aa-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="1e6aa-176">一時的</span><span class="sxs-lookup"><span data-stu-id="1e6aa-176">Transient</span></span> |
| [<span data-ttu-id="1e6aa-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="1e6aa-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="1e6aa-178">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-178">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="1e6aa-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="1e6aa-180">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-180">Singleton</span></span> |
| [<span data-ttu-id="1e6aa-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="1e6aa-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="1e6aa-182">シングルトン</span><span class="sxs-lookup"><span data-stu-id="1e6aa-182">Singleton</span></span> |

<span data-ttu-id="1e6aa-183">サービス (および必要であればサービスが依存するサービス) を登録するためにサービス コレクションの拡張メソッドを使用できる場合は、1 つの `Add{SERVICE_NAME}` 拡張メソッドを使用してそのサービスが必要とするすべてのサービスを登録することが規則です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="1e6aa-184">次のコードは、拡張メソッド [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)、[AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity)、[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) を使用して、コンテナーに追加のサービスを追加する方法の例です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="1e6aa-185">詳細については、API のドキュメントの「[ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection)」(ServiceCollection クラス) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="1e6aa-186">サービスの有効期間</span><span class="sxs-lookup"><span data-stu-id="1e6aa-186">Service lifetimes</span></span>

<span data-ttu-id="1e6aa-187">登録される各サービスの適切な有効期間を選択します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="1e6aa-188">ASP.NET Core サービスは、次の有効期間で構成できます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="1e6aa-189">**一時的**</span><span class="sxs-lookup"><span data-stu-id="1e6aa-189">**Transient**</span></span>

<span data-ttu-id="1e6aa-190">有効期間が一時的のサービスは、要求されるたびに作成されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="1e6aa-191">この有効期間は、軽量でステートレスのサービスに最適です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="1e6aa-192">**スコープ**</span><span class="sxs-lookup"><span data-stu-id="1e6aa-192">**Scoped**</span></span>

<span data-ttu-id="1e6aa-193">有効期間がスコープのサービスは、要求ごとに 1 回作成されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="1e6aa-194">ミドルウェアでスコープ サービスを使用している場合、サービスを `Invoke` または `InvokeAsync` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="1e6aa-195">コンストラクターを使用して挿入すると、サービスがシングルトンのように動作するよう強制されるので、コンストラクターを使用した挿入は行わないでください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="1e6aa-196">詳細については、「<xref:fundamentals/middleware/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="1e6aa-197">**シングルトン**</span><span class="sxs-lookup"><span data-stu-id="1e6aa-197">**Singleton**</span></span>

<span data-ttu-id="1e6aa-198">有効期間がシングルトンのサービスは、最初に要求されたときに作成されます (または、`ConfigureServices` が実行されて、サービス登録でインスタンスが指定された場合)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="1e6aa-199">以降の要求は、すべて同じインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="1e6aa-200">アプリをシングルトンで動作させる必要がある場合は、サービス コンテナーによるサービスの有効期間の管理を許可することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="1e6aa-201">クラス内のオブジェクトの有効期間を管理するために、シングルトンの設計パターンを実装してユーザー コードを提供しないでください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="1e6aa-202">シングルトンからスコープ サービスを解決するのは危険です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="1e6aa-203">後続の要求を処理する際に、サービスが正しくない状態になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="1e6aa-204">コンストラクターの挿入の動作</span><span class="sxs-lookup"><span data-stu-id="1e6aa-204">Constructor injection behavior</span></span>

<span data-ttu-id="1e6aa-205">サービスは 2 つのメカニズムによって解決できます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="1e6aa-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; 依存関係の挿入コンテナーにサービスを登録することなくオブジェクトの作成を許可します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="1e6aa-207">`ActivatorUtilities` はユーザー側の抽象化 (タグ ヘルパー、MVC コントローラー、SignalR ハブ、モデル バインダーなど) で使用されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, SignalR Hubs, and model binders.</span></span>

<span data-ttu-id="1e6aa-208">コンストラクターは、依存関係の挿入によって提供されない引数を受け取ることができますが、引数は既定値を割り当てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="1e6aa-209">`IServiceProvider` または `ActivatorUtilities` によってサービスを解決する場合、コンストラクターの挿入には "*パブリック*" コンストラクターが必要です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="1e6aa-210">`ActivatorUtilities` によってサービスを解決する場合、コンストラクターの挿入に必要なことは、該当するコンストラクターが 1 つだけ存在することです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="1e6aa-211">コンストラクターのオーバーロードはサポートされていますが、依存関係の挿入によってすべての引数を設定できるオーバーロードは 1 つしか存在できません。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="1e6aa-212">Entity Framework コンテキスト</span><span class="sxs-lookup"><span data-stu-id="1e6aa-212">Entity Framework contexts</span></span>

<span data-ttu-id="1e6aa-213">Entity Framework コンテキストは、スコープの有効期間を使ってサービス コンテナーに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="1e6aa-214">これは、データベース コンテキストを登録する際に、[AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) メソッドへの呼び出しを使用して自動的に処理されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="1e6aa-215">データベース コンテキストを使用するサービスは、スコープの有効期間も使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="1e6aa-216">有効期間と登録のオプション</span><span class="sxs-lookup"><span data-stu-id="1e6aa-216">Lifetime and registration options</span></span>

<span data-ttu-id="1e6aa-217">有効期間と登録のオプションの違いを示すために、タスクを一意の識別子 `OperationId` を備えた操作として表す、次のインターフェイスについて考えます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="1e6aa-218">次のインターフェイスに対して操作のサービスの有効期間がどのように構成されているかに応じて、コンテナーは、クラスに要求されたときに、サービスの同じインスタンスか別のインスタンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-219">インターフェイスは `Operation` クラス内で実装されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="1e6aa-220">指定されない場合、`Operation` コンストラクターは GUID を生成します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-221">`OperationService` が登録されます。これは、その他の `Operation` 型のそれぞれに依存します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="1e6aa-222">依存関係の挿入を経由して `OperationService` が要求される場合、これは各サービスの新しいインスタンスか、依存関係のあるサービスの有効期間に基づく既存のインスタンスを受信します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="1e6aa-223">要求時に一時的なサービスを作成する場合、`IOperationTransient` サービスの `OperationsId` と `OperationService` の `OperationsId` は異なります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="1e6aa-224">`OperationService` は `IOperationTransient` クラスの新しいインスタンスを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="1e6aa-225">新しいインスタンスは異なる `OperationsId` を生成します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="1e6aa-226">要求ごとにスコープ サービスを作成する場合、要求内で `IOperationScoped` サービスの `OperationsId` は `OperationService` のものと同じになります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="1e6aa-227">要求間で、両方のサービスは異なる `OperationsId` 値を共有します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="1e6aa-228">シングルトンとシングルトン インスタンスのサービスが 1 回作成され、すべての要求とすべてのサービス間で使用される場合、`OperationsId` はすべてのサービス要求の間で一定です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-229">`Startup.ConfigureServices` では、各型が名前で指定されている有効期間に従ってコンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="1e6aa-230">`IOperationSingletonInstance` サービスは、`Guid.Empty` の既知の ID を持った特定のインスタンスを使用しています。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="1e6aa-231">この型が使用されるタイミングは明らかです (その GUID はすべてゼロです)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1e6aa-232">サンプル アプリでは、個々の要求内、および個々の要求間におけるオブジェクトの有効期間が示されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="1e6aa-233">サンプル アプリの `IndexModel` には、各種類の `IOperation` 型と `OperationService` が必要です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="1e6aa-234">次に、ページ モデル クラスとサービスのすべての `OperationId` 値が、プロパティの割り当てを通じてページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="1e6aa-235">サンプル アプリでは、個々の要求内、および個々の要求間におけるオブジェクトの有効期間が示されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="1e6aa-236">サンプル アプリには、各種類の `IOperation` 型と `OperationService` を要求する `OperationsController` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="1e6aa-237">`Index` アクションは、サービスの `OperationId` 値の表示のために、サービスを `ViewBag` に設定します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1e6aa-238">2 つの要求の結果を、以下の 2 つの出力に示します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="1e6aa-239">**最初の要求:**</span><span class="sxs-lookup"><span data-stu-id="1e6aa-239">**First request:**</span></span>

<span data-ttu-id="1e6aa-240">コントローラーの操作:</span><span class="sxs-lookup"><span data-stu-id="1e6aa-240">Controller operations:</span></span>

<span data-ttu-id="1e6aa-241">一時的: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="1e6aa-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="1e6aa-242">スコープ: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="1e6aa-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="1e6aa-243">シングルトン: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1e6aa-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1e6aa-244">インスタンス: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1e6aa-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1e6aa-245">`OperationService` の操作:</span><span class="sxs-lookup"><span data-stu-id="1e6aa-245">`OperationService` operations:</span></span>

<span data-ttu-id="1e6aa-246">一時的: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="1e6aa-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="1e6aa-247">スコープ: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="1e6aa-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="1e6aa-248">シングルトン: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1e6aa-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1e6aa-249">インスタンス: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1e6aa-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1e6aa-250">**2 番目の要求:**</span><span class="sxs-lookup"><span data-stu-id="1e6aa-250">**Second request:**</span></span>

<span data-ttu-id="1e6aa-251">コントローラーの操作:</span><span class="sxs-lookup"><span data-stu-id="1e6aa-251">Controller operations:</span></span>

<span data-ttu-id="1e6aa-252">一時的: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="1e6aa-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="1e6aa-253">スコープ: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="1e6aa-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="1e6aa-254">シングルトン: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1e6aa-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1e6aa-255">インスタンス: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1e6aa-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1e6aa-256">`OperationService` の操作:</span><span class="sxs-lookup"><span data-stu-id="1e6aa-256">`OperationService` operations:</span></span>

<span data-ttu-id="1e6aa-257">一時的: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="1e6aa-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="1e6aa-258">スコープ: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="1e6aa-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="1e6aa-259">シングルトン: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1e6aa-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1e6aa-260">インスタンス: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1e6aa-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1e6aa-261">要求内および要求間で、どの `OperationId` 値が変化しているかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="1e6aa-262">"*一時的な*" オブジェクトは常に異なります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-262">*Transient* objects are always different.</span></span> <span data-ttu-id="1e6aa-263">最初の要求と 2 番目の要求の一時的な `OperationId` 値は、`OperationService` 操作と要求間の両方に対して異なります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="1e6aa-264">それぞれのサービスと要求に対して、新しいインスタンスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="1e6aa-265">"*スコープ*" オブジェクトは、要求内では同じですが、要求間では異なります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="1e6aa-266">"*シングルトン*" オブジェクトは、`Operation` インスタンスが `ConfigureServices` で提供されるかどうかに関係なく、すべてのオブジェクトとすべての要求について同じです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="1e6aa-267">main からサービスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="1e6aa-267">Call services from main</span></span>

<span data-ttu-id="1e6aa-268">[IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) を使用して [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) を作成し、アプリのスコープ内のスコープ サービスを解決します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="1e6aa-269">この方法は、起動時に初期化タスクを実行するために、スコープ サービスにアクセスするのに便利です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="1e6aa-270">次の例では、`Program.Main` で `MyScopedService` のコンテキストを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="1e6aa-271">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="1e6aa-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1e6aa-272">アプリが開発環境で実行されている場合、既定のサービス プロバイダーは次を確認するためのチェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="1e6aa-273">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="1e6aa-274">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="1e6aa-275">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="1e6aa-276">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="1e6aa-277">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="1e6aa-278">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="1e6aa-279">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="1e6aa-280">詳細については、「<xref:fundamentals/host/web-host#scope-validation>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="1e6aa-281">要求サービス</span><span class="sxs-lookup"><span data-stu-id="1e6aa-281">Request Services</span></span>

<span data-ttu-id="1e6aa-282">`HttpContext` からの ASP.NET Core 要求内で使用可能なサービスは、[HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) コレクションを通じて公開されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="1e6aa-283">要求サービスは、アプリの一部として構成および要求されるサービスを表します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="1e6aa-284">オブジェクトで依存関係を指定すると、これらは `ApplicationServices` ではなく `RequestServices` で検出された型で満たされます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="1e6aa-285">一般に、アプリから直接これらのプロパティを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="1e6aa-286">代わりに、クラスのコンストラクターを介してクラスに必要な型を要求し、フレームワークに依存関係を挿入させます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="1e6aa-287">このようにすると、クラスのテストが簡単になります (「[テストとデバッグ](xref:test/index)」のトピックを参照)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="1e6aa-288">コンストラクターのパラメーターとして依存関係を要求し、`RequestServices` コレクションにアクセスするようにします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="1e6aa-289">依存関係の挿入のためのサービスの設計</span><span class="sxs-lookup"><span data-stu-id="1e6aa-289">Design services for dependency injection</span></span>

<span data-ttu-id="1e6aa-290">ベスト プラクティスは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-290">Best practices are to:</span></span>

* <span data-ttu-id="1e6aa-291">各依存関係を取得するために依存関係の挿入を使用するようサービスを設計します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="1e6aa-292">ステートフルな静的メソッドの呼び出しを回避します ([静的な結び付き](https://deviq.com/static-cling/)と呼ばれる方法です)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="1e6aa-293">サービス内部で依存関係のあるクラスを直接インスタンス化することを回避します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="1e6aa-294">直接のインスタンス化は、コードの固有の実装につながります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="1e6aa-295">[オブジェクト指向設計の SOLID 原則](https://deviq.com/solid/)に従うことで、小さく、十分に要素に分割された、テストが簡単なアプリのクラスを実現できる可能性が自然と高くなります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="1e6aa-296">クラスに含まれる挿入される依存関係が多すぎるように見える場合、これは通常、クラスが担当する役割が多すぎて、[単一責任の原則 (SRP)](https://deviq.com/single-responsibility-principle/) に違反していることのサインです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](https://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="1e6aa-297">責任の一部を新しいクラスに移動することにより、クラスのリファクタリングを試みます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="1e6aa-298">Razor Pages のページ モデル クラスと MVC コントローラー クラスは、UI の問題に集中する必要があることに留意します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="1e6aa-299">ビジネス ルールとデータ アクセスの実装の詳細は、これらの[個別の問題](https://deviq.com/separation-of-concerns/)に適したクラスに分離する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](https://deviq.com/separation-of-concerns/).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="1e6aa-300">サービスの破棄</span><span class="sxs-lookup"><span data-stu-id="1e6aa-300">Disposal of services</span></span>

<span data-ttu-id="1e6aa-301">コンテナーは、作成する `IDisposable` 型の `Dispose` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="1e6aa-302">ユーザー コードによってコンテナーに追加されたインスタンスは、自動的に破棄されません。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="1e6aa-303">ASP.NET Core 1.0 のコンテナーは、コンテナーで作成しなかったものも含めて、"*すべての*" `IDisposable` に対して破棄を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="1e6aa-304">既定のサービス コンテナーの置換</span><span class="sxs-lookup"><span data-stu-id="1e6aa-304">Default service container replacement</span></span>

<span data-ttu-id="1e6aa-305">組み込みのサービス コンテナーは、フレームワークと、ほとんどのコンシューマー アプリのニーズに対応することを意図したものです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-305">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="1e6aa-306">それがサポートしない特定の機能が必要な場合は、組み込みのコンテナーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-306">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="1e6aa-307">組み込みのコンテナーにない、サードパーティのコンテナーでサポートされている一部の機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-307">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="1e6aa-308">プロパティの挿入</span><span class="sxs-lookup"><span data-stu-id="1e6aa-308">Property injection</span></span>
* <span data-ttu-id="1e6aa-309">名前に基づく挿入</span><span class="sxs-lookup"><span data-stu-id="1e6aa-309">Injection based on name</span></span>
* <span data-ttu-id="1e6aa-310">子コンテナー</span><span class="sxs-lookup"><span data-stu-id="1e6aa-310">Child containers</span></span>
* <span data-ttu-id="1e6aa-311">有効期間のカスタム管理</span><span class="sxs-lookup"><span data-stu-id="1e6aa-311">Custom lifetime management</span></span>
* <span data-ttu-id="1e6aa-312">遅延初期化に対する `Func<T>` のサポート</span><span class="sxs-lookup"><span data-stu-id="1e6aa-312">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="1e6aa-313">アダプターをサポートするいくつかのコンテナーの一覧については、「[Dependency Injection readme.md file](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection)」 (Dependency Injection readme.md ファイル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-313">See the [Dependency Injection readme.md file](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="1e6aa-314">次のサンプルでは、[Autofac](https://autofac.org/) で組み込みのコンテナーが置き換えられています。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-314">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="1e6aa-315">適切なコンテナー パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-315">Install the appropriate container package(s):</span></span>

    * [<span data-ttu-id="1e6aa-316">Autofac</span><span class="sxs-lookup"><span data-stu-id="1e6aa-316">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
    * [<span data-ttu-id="1e6aa-317">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="1e6aa-317">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="1e6aa-318">`Startup.ConfigureServices` でコンテナーを構成して、`IServiceProvider` を返します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-318">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="1e6aa-319">サード パーティのコンテナーを使用するには、`Startup.ConfigureServices` で `IServiceProvider` が返される必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-319">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="1e6aa-320">`DefaultModule` で Autofac を構成します。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-320">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="1e6aa-321">実行時には、Autofac を使って、型の解決と依存関係の挿入が行われます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-321">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="1e6aa-322">ASP.NET Core での Autofac の使用について詳しくは、[Autofac のドキュメント](https://docs.autofac.org/en/latest/integration/aspnetcore.html)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-322">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="1e6aa-323">スレッド セーフ</span><span class="sxs-lookup"><span data-stu-id="1e6aa-323">Thread safety</span></span>

<span data-ttu-id="1e6aa-324">シングルトン サービスは、スレッド セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-324">Singleton services need to be thread safe.</span></span> <span data-ttu-id="1e6aa-325">シングルトン サービスに一時的サービスへの依存関係がある場合、シングルトンによる一時的サービスの使い方によっては、一時的サービスもスレッド セーフであることが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-325">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="1e6aa-326">1 つのサービスのファクトリ メソッド (例: [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__) に対する 2 番目の引数) をスレッド セーフにする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-326">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="1e6aa-327">型 (`static`) のコンストラクターのように、1 つのスレッドによって 1 回呼び出されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-327">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="1e6aa-328">推奨事項</span><span class="sxs-lookup"><span data-stu-id="1e6aa-328">Recommendations</span></span>

<span data-ttu-id="1e6aa-329">依存関係の挿入を使うときは、次の推奨事項に留意してください。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-329">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="1e6aa-330">データと構成をサービス コンテナーに直接格納しないようにします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-330">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="1e6aa-331">たとえば、通常、ユーザーのショッピング カートはサービス コンテナーに追加しません。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-331">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="1e6aa-332">構成では、[オプション パターン](xref:fundamentals/configuration/options)を使う必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-332">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="1e6aa-333">同様に、他のオブジェクトへのアクセスを許可するためだけに存在する "データ ホルダー" オブジェクトは避ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-333">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="1e6aa-334">可能な場合は、実際のアイテムを依存関係の挿入経由で要求することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-334">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="1e6aa-335">サービスへの静的なアクセスを回避します (たとえば、他所で使用するための [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) の静的な型指定)。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-335">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="1e6aa-336">サービス ロケーター パターンの使用を回避します (たとえば [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice))。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-336">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="1e6aa-337">`HttpContext` への静的なアクセスを回避します (たとえば [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext))。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-337">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="1e6aa-338">どのような推奨事項であっても、それを無視する必要がある状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-338">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="1e6aa-339">例外はまれです&mdash;ほとんどがフレームワーク自体の内の特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-339">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="1e6aa-340">依存関係の挿入は静的/グローバル オブジェクト アクセス パターンの*代替機能*です。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-340">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="1e6aa-341">静的オブジェクト アクセスと併用した場合、依存関係の挿入のメリットを実現できない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e6aa-341">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e6aa-342">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1e6aa-342">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="1e6aa-343">依存関係の挿入による ASP.NET Core でのクリーンなコードの作成 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="1e6aa-343">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="1e6aa-344">コンテナー管理アプリケーションの設計、前段階: コンテナーはどこに属していますか</span><span class="sxs-lookup"><span data-stu-id="1e6aa-344">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="1e6aa-345">明示的な依存関係の原則</span><span class="sxs-lookup"><span data-stu-id="1e6aa-345">Explicit Dependencies Principle</span></span>](https://deviq.com/explicit-dependencies-principle/)
* [<span data-ttu-id="1e6aa-346">制御の反転コンテナーと依存関係の挿入パターン (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="1e6aa-346">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="1e6aa-347">new は接着剤である (コードを特定の実装に "接着" する)</span><span class="sxs-lookup"><span data-stu-id="1e6aa-347">New is Glue ("gluing" code to a particular implementation)</span></span>](https://ardalis.com/new-is-glue)
