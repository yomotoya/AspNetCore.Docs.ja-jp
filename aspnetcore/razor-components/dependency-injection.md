---
title: Razor のコンポーネントの依存関係の挿入
author: guardrex
description: アプリの Blazor と剃刀コンポーネントがコンポーネントに挿入することによってサービスを使用する方法を参照してください。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515486"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="afd5f-103">Razor のコンポーネントの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="afd5f-103">Razor Components dependency injection</span></span>

<span data-ttu-id="afd5f-104">によって[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="afd5f-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="afd5f-105">Razor のコンポーネントをサポートしています[依存関係の注入 (DI)](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="afd5f-106">アプリでは、コンポーネントに挿入することによって、組み込みのサービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="afd5f-107">アプリも定義し、カスタム サービスを登録し、DI を使用して、アプリ全体で使用できるように。</span><span class="sxs-lookup"><span data-stu-id="afd5f-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="afd5f-108">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="afd5f-108">Dependency injection</span></span>

<span data-ttu-id="afd5f-109">DI は、中央の場所に構成されているサービスにアクセスするための手法です。</span><span class="sxs-lookup"><span data-stu-id="afd5f-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="afd5f-110">これは、Razor コンポーネント アプリで役立ちます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="afd5f-111">呼ばれる、多くのコンポーネント間で、サービス クラスの 1 つのインスタンスを共有する*シングルトン*サービス。</span><span class="sxs-lookup"><span data-stu-id="afd5f-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="afd5f-112">参照の抽象化を使用して、サービスの具象クラスからコンポーネントを分離します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="afd5f-113">たとえば、インターフェイス`IDataAccess`アプリ内のデータにアクセスするためです。</span><span class="sxs-lookup"><span data-stu-id="afd5f-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="afd5f-114">インターフェイスを実装する具象によって`DataAccess`クラスし、アプリのサービス コンテナーにサービスとして登録します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="afd5f-115">コンポーネントが受信する DI を使用する場合、`IDataAccess`コンポーネントの実装が具象型に結合されていません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="afd5f-116">おそらく、実装は、単体テストでモック実装にはスワップできます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="afd5f-117">詳細については、「 <xref:fundamentals/dependency-injection> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="afd5f-118">DI にサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-118">Add services to DI</span></span>

<span data-ttu-id="afd5f-119">新しいアプリを作成すると、確認、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="afd5f-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="afd5f-120">`ConfigureServices`メソッドに渡されますが、 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>、サービスの記述子オブジェクトの一覧 (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>)。</span><span class="sxs-lookup"><span data-stu-id="afd5f-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="afd5f-121">サービスは、サービスのコレクションにサービスの記述子を提供することによって追加されます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="afd5f-122">次の例では、概念を`IDataAccess`インターフェイスとその具体的な実装`DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="afd5f-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="afd5f-123">サービスは、有効期間の次の表に示すように構成できます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="afd5f-124">有効期間</span><span class="sxs-lookup"><span data-stu-id="afd5f-124">Lifetime</span></span> | <span data-ttu-id="afd5f-125">説明</span><span class="sxs-lookup"><span data-stu-id="afd5f-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="afd5f-126">DI を作成、*単一インスタンス*のサービス。</span><span class="sxs-lookup"><span data-stu-id="afd5f-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="afd5f-127">必要とするすべてのコンポーネントを`Singleton`サービスが同じサービスのインスタンスを受信します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="afd5f-128">コンポーネントがのインスタンスを取得するたびに、`Transient`サービスは受信サービスのコンテナーから、*新しいインスタンス*サービスの。</span><span class="sxs-lookup"><span data-stu-id="afd5f-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="afd5f-129">クライアント側 Blazor 現在 DI スコープの概念はありません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="afd5f-130">`Scoped` ように動作`Singleton`します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="afd5f-131">ただし、ASP.NET Core Razor のコンポーネントのサポート、`Scoped`有効期間。</span><span class="sxs-lookup"><span data-stu-id="afd5f-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="afd5f-132">Razor のコンポーネントでは、スコープ化されたサービスの登録は、接続に制限されます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="afd5f-133">このためを使用してスコープを持つサービスの現在のユーザーにスコープする必要がありますサービス現在の目的は、クライアント側で実行する場合でも、ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="afd5f-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="afd5f-134">DI システムは、ASP.NET Core で DI システムに基づきます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="afd5f-135">詳細については、「 <xref:fundamentals/dependency-injection> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="afd5f-136">既定のサービス</span><span class="sxs-lookup"><span data-stu-id="afd5f-136">Default services</span></span>

<span data-ttu-id="afd5f-137">既定のサービスは、アプリのサービスのコレクションに自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="afd5f-138">サービス</span><span class="sxs-lookup"><span data-stu-id="afd5f-138">Service</span></span> | <span data-ttu-id="afd5f-139">説明</span><span class="sxs-lookup"><span data-stu-id="afd5f-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="afd5f-140">HTTP 要求の送信と URI (シングルトン) で識別されるリソースから HTTP 応答を受信するメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="afd5f-141">注意のこのインスタンス`HttpClient`バック グラウンドで HTTP トラフィックを処理するため、ブラウザーを使用します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="afd5f-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress)アプリのベース URI プレフィックスが自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="afd5f-143">`HttpClient` クライアント側 Blazor アプリにのみ提供されます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="afd5f-144">JavaScript ランタイムの呼び出しをディスパッチできますのインスタンスを表します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="afd5f-145">詳細については、「 <xref:razor-components/javascript-interop> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="afd5f-146">Uri およびナビゲーションの状態 (シングルトン) を操作するためのヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="afd5f-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="afd5f-147">`IUriHelper` Blazor と剃刀コンポーネントの両方のアプリに提供されます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="afd5f-148">既定のテンプレートによって追加された既定のサービス プロバイダーではなく、カスタム サービス プロバイダーを使用することになります。</span><span class="sxs-lookup"><span data-stu-id="afd5f-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="afd5f-149">カスタム サービス プロバイダーは、表に示す既定のサービスを自動的に提供しません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="afd5f-150">カスタム サービス プロバイダーを使用して、テーブルのようにサービスを必要とする場合は、新しいサービス プロバイダーに必要なサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="afd5f-151">要求では、コンポーネント サービス</span><span class="sxs-lookup"><span data-stu-id="afd5f-151">Request a service in a component</span></span>

<span data-ttu-id="afd5f-152">サービスがサービス コレクションに追加された後を使用して、コンポーネントの Razor テンプレートに、サービスを挿入、 [ @inject ](xref:mvc/views/razor#section-4) Razor ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="afd5f-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="afd5f-153">`@inject` 2 つのパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="afd5f-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="afd5f-154">型名:挿入するサービスの型。</span><span class="sxs-lookup"><span data-stu-id="afd5f-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="afd5f-155">プロパティ名:挿入された app service の受信プロパティの名前。</span><span class="sxs-lookup"><span data-stu-id="afd5f-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="afd5f-156">プロパティが手動で作成を必要としないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="afd5f-157">コンパイラは、プロパティを作成します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-157">The compiler creates the property.</span></span>

<span data-ttu-id="afd5f-158">詳細については、「 <xref:mvc/views/dependency-injection> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="afd5f-159">複数回使用`@inject`さまざまなサービスを挿入するステートメント。</span><span class="sxs-lookup"><span data-stu-id="afd5f-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="afd5f-160">次の例は、`@inject` を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="afd5f-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="afd5f-161">実装するサービス`Services.IDataAccess`コンポーネントのプロパティには挿入`DataRepository`します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="afd5f-162">コードがのみを使用する方法に注意してください、`IDataAccess`抽象化します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="afd5f-163">内部的には、生成されたプロパティ (`DataRepository`) で修飾されたが、`InjectAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="afd5f-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="afd5f-164">通常、この属性は、直接使用されません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="afd5f-165">基底クラスはコンポーネントに必要な場合に挿入されたプロパティは、基底クラスに必要なも`InjectAttribute`手動で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="afd5f-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="afd5f-166">基本クラスから派生したコンポーネントで、`@inject`ディレクティブは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="afd5f-167">`InjectAttribute`の基本クラスで十分です。</span><span class="sxs-lookup"><span data-stu-id="afd5f-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="afd5f-168">サービスの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="afd5f-168">Dependency injection in services</span></span>

<span data-ttu-id="afd5f-169">複雑なサービスには、その他のサービスが必要です。</span><span class="sxs-lookup"><span data-stu-id="afd5f-169">Complex services might require additional services.</span></span> <span data-ttu-id="afd5f-170">前の例では、`DataAccess`必要があります、`HttpClient`既定のサービスです。</span><span class="sxs-lookup"><span data-stu-id="afd5f-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="afd5f-171">`@inject` (または`InjectAttribute`) はサービスで使用するために使用できません。</span><span class="sxs-lookup"><span data-stu-id="afd5f-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="afd5f-172">*コンス トラクターの挿入*代わりに使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="afd5f-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="afd5f-173">必要なサービスを追加するには、サービスのコンス トラクターにパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="afd5f-174">依存関係の挿入は、サービスを作成するときに、コンス トラクターで必要し、するにはそれに応じて、サービスを認識します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="afd5f-175">コンス トラクターの挿入の前提条件:</span><span class="sxs-lookup"><span data-stu-id="afd5f-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="afd5f-176">1 つのコンス トラクター引数はすべてによって満たすことが依存関係の挿入が必要があります。</span><span class="sxs-lookup"><span data-stu-id="afd5f-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="afd5f-177">既定値を指定する場合、DI でカバーされない追加のパラメーターが許可されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="afd5f-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="afd5f-178">該当するコンス トラクターがある必要があります*パブリック*します。</span><span class="sxs-lookup"><span data-stu-id="afd5f-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="afd5f-179">該当するコンス トラクターの 1 つだけあります。</span><span class="sxs-lookup"><span data-stu-id="afd5f-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="afd5f-180">DI は、あいまいさが発生した場合、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="afd5f-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="afd5f-181">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="afd5f-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
