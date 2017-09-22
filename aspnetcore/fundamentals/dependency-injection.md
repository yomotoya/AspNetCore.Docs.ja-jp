---
title: "ASP.NET Core の依存関係の挿入"
author: ardalis
description: "ASP.NET Core が依存関係の挿入を実装する方法とその使用方法について説明します。"
keywords: "ASP.NET Core、依存関係の挿入、di"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5dfebeddb3a9394fd1faf798e029a9312201089
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a><span data-ttu-id="5a4d8-104">ASP.NET Core の依存関係の挿入の概要</span><span class="sxs-lookup"><span data-stu-id="5a4d8-104">Introduction to Dependency Injection in ASP.NET Core</span></span>

<a name=fundamentals-dependency-injection></a>

<span data-ttu-id="5a4d8-105">によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="5a4d8-105">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="5a4d8-106">ASP.NET Core は、まったく新たに設計をサポートし、依存関係の挿入を活用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-106">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="5a4d8-107">ASP.NET Core アプリケーションは、スタートアップ クラス内のメソッドを挿入することによってサービスを組み込みフレームワークを利用でき、挿入もアプリケーション サービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-107">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="5a4d8-108">ASP.NET Core によって提供される既定のサービス コンテナーは、最小限の機能が設定され、その他のコンテナーを置き換えるものではありませんを提供します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-108">The default services container provided by ASP.NET Core provides a minimal feature set and is not intended to replace other containers.</span></span>

[<span data-ttu-id="5a4d8-109">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="5a4d8-109">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5a4d8-110">依存関係の挿入とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-110">What is Dependency Injection?</span></span>

<span data-ttu-id="5a4d8-111">依存性の注入 (DI) は、オブジェクトとの共同作業者、または依存関係の間の疎結合を実現するための手法です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-111">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="5a4d8-112">、の共同作業者を直接インスタンス化または静的参照を使用して、ではなく、クラスは、そのアクションを実行する必要があるオブジェクトは、なんらかの方法でクラスに提供されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-112">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="5a4d8-113">クラスに従うように、コンス トラクターを使用して、その依存関係を宣言する、ほとんどの場合、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-113">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="5a4d8-114">この方法は、「コンス トラクター インジェクション」と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-114">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="5a4d8-115">クラスは、注意 DI でデザインされます、ときよりは疎結合されて共同作業者に直接、ハード コーディングされた依存関係があるないためです。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-115">When classes are designed with DI in mind, they are more loosely coupled because they do not have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="5a4d8-116">次のように、[依存関係の逆転原則](http://deviq.com/dependency-inversion-principle/)、ことを示している*「高レベルのモジュールは低レベルのモジュールに依存しないように; 抽象化の両方を利用する必要があります」。*</span><span class="sxs-lookup"><span data-stu-id="5a4d8-116">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules should not depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="5a4d8-117">クラスが抽象化を要求する特定の実装を参照するには、代わりに (通常`interfaces`) クラスを作成するときに提供されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-117">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="5a4d8-118">インターフェイスへの依存関係を抽出し、これらのインターフェイスのパラメーターとして実装することはの例ではまた、[戦略の設計パターン](http://deviq.com/strategy-design-pattern/)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-118">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="5a4d8-119">DI を使用する、システムの設計時に、コンス トラクター (またはプロパティ) を使用して、その依存関係を要求する多くのクラスが関連付けられている依存関係とこれらのクラスを作成するのには専用のクラスをするおくと便利です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-119">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="5a4d8-120">これらのクラスと呼びます*コンテナー*、または具体的には、[制御の反転 (IoC)](http://deviq.com/inversion-of-control/)や依存関係の挿入 (DI) コンテナーのコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-120">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="5a4d8-121">コンテナーとは、そこから要求された型のインスタンスを提供するファクトリを本質的にします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-121">A container is essentially a factory that is responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="5a4d8-122">場合は、指定された型は、依存関係があるし、依存関係の種類を提供するコンテナーが構成されていることを宣言しましたが、要求されたインスタンスの作成の一環として依存関係が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-122">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="5a4d8-123">これにより、複雑な依存関係グラフは、ハード コーディングされたオブジェクトの構築が必要ないクラスに提供できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-123">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="5a4d8-124">オブジェクトをその依存関係を作成するだけでなくコンテナーは通常、アプリケーション内でオブジェクトの有効期間を管理します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-124">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="5a4d8-125">ASP.NET Core に簡単なビルトイン コンテナーが含まれています (によって表される、`IServiceProvider`インターフェイス) 既定では、コンス トラクターの挿入をサポートして、ASP.NET により、特定のサービスが DI を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-125">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="5a4d8-126">ASP です。NET のコンテナーとして管理されます。 その型を指す*services*です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-126">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="5a4d8-127">この記事の残りの部分全体にわたって*services*は ASP.NET Core IoC コンテナーで管理されている型を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-127">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="5a4d8-128">組み込みのコンテナーのサービスを構成する、`ConfigureServices`で、アプリケーションの`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-128">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-129">Martin ファウラー上の広範な資料が書き込ま[逆転のコントロール コンテナーとの依存関係挿入パターン](https://www.martinfowler.com/articles/injection.html)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-129">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="5a4d8-130">最適な説明も Microsoft Patterns and Practices[依存性の注入](https://msdn.microsoft.com/library/hh323705.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-130">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-131">この記事では、すべての ASP.NET アプリケーションに適用される、依存関係の挿入がについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-131">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="5a4d8-132">MVC コント ローラー内の依存関係の挿入は、「[依存関係挿入とコント ローラー](../mvc/controllers/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-132">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="5a4d8-133">コンス トラクター インジェクションの動作</span><span class="sxs-lookup"><span data-stu-id="5a4d8-133">Constructor Injection Behavior</span></span>

<span data-ttu-id="5a4d8-134">コンス トラクターの挿入は、対象のコンス トラクターがある必要があります*パブリック*です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-134">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="5a4d8-135">それ以外の場合、アプリがスローされます、 `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-135">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="5a4d8-136">型 '' の適切なコンス トラクターが見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-136">A suitable constructor for type 'YourType' could not be located.</span></span> <span data-ttu-id="5a4d8-137">具体的な型とサービスに登録されて、パブリック コンス トラクターのすべてのパラメーターを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-137">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>


<span data-ttu-id="5a4d8-138">コンス トラクターの挿入では、その 1 つだけ適用可能なコンス トラクターの存在が必要です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-138">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="5a4d8-139">コンス トラクター オーバー ロードがサポートされますが、引数のすべてで満足できる依存関係の挿入のみ 1 つのオーバー ロードが存在できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-139">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="5a4d8-140">1 つ以上存在する場合、アプリがスローされます、 `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-140">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="5a4d8-141">型 '' には、すべての指定した引数型を受け付ける複数のコンス トラクターが検出されました。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-141">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="5a4d8-142">適用可能なコンス トラクターの 1 つだけあります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-142">There should only be one applicable constructor.</span></span>

<span data-ttu-id="5a4d8-143">コンス トラクターは、依存関係の挿入では提供されない引数を受け入れることができますが、これらの既定値をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-143">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="5a4d8-144">例:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-144">For example:</span></span>

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a><span data-ttu-id="5a4d8-145">Framework が提供するサービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-145">Using Framework-Provided Services</span></span>

<span data-ttu-id="5a4d8-146">`ConfigureServices`メソッドで、`Startup`クラスは Entity Framework Core および ASP.NET Core MVC などのプラットフォーム機能を含む、アプリケーションで使用するサービスの定義を担当します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-146">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="5a4d8-147">最初に、`IServiceCollection`に提供される`ConfigureServices`が定義されている次のサービス (に応じて[ホストの構成方法](xref:fundamentals/hosting))。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-147">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="5a4d8-148">サービスの種類</span><span class="sxs-lookup"><span data-stu-id="5a4d8-148">Service Type</span></span> | <span data-ttu-id="5a4d8-149">有効期間</span><span class="sxs-lookup"><span data-stu-id="5a4d8-149">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="5a4d8-150">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="5a4d8-150">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="5a4d8-151">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-151">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-152">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="5a4d8-152">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="5a4d8-153">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-153">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-154">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5a4d8-154">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="5a4d8-155">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-155">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-156">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="5a4d8-156">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="5a4d8-157">一時的なもの</span><span class="sxs-lookup"><span data-stu-id="5a4d8-157">Transient</span></span> |
| [<span data-ttu-id="5a4d8-158">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="5a4d8-158">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="5a4d8-159">一時的なもの</span><span class="sxs-lookup"><span data-stu-id="5a4d8-159">Transient</span></span> |
| [<span data-ttu-id="5a4d8-160">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5a4d8-160">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="5a4d8-161">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-161">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-162">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="5a4d8-162">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="5a4d8-163">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-163">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-164">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="5a4d8-164">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="5a4d8-165">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-165">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-166">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="5a4d8-166">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="5a4d8-167">一時的なもの</span><span class="sxs-lookup"><span data-stu-id="5a4d8-167">Transient</span></span> |
| [<span data-ttu-id="5a4d8-168">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="5a4d8-168">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="5a4d8-169">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-169">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-170">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5a4d8-170">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="5a4d8-171">一時的なもの</span><span class="sxs-lookup"><span data-stu-id="5a4d8-171">Transient</span></span> |
| [<span data-ttu-id="5a4d8-172">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="5a4d8-172">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="5a4d8-173">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-173">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-174">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="5a4d8-174">Microsoft.AspNetCore.Hosting.IStartup</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="5a4d8-175">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-175">Singleton</span></span> |
| [<span data-ttu-id="5a4d8-176">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="5a4d8-176">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="5a4d8-177">シングルトン</span><span class="sxs-lookup"><span data-stu-id="5a4d8-177">Singleton</span></span> |

<span data-ttu-id="5a4d8-178">以下のように拡張メソッドの番号を使用してコンテナーに追加のサービスを追加する方法の例は、 `AddDbContext`、 `AddIdentity`、および`AddMvc`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-178">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="5a4d8-179">機能と ASP.NET、MVC などによって提供されるミドルウェアを 1 つの追加を使用する規則に従って*ServiceName*その機能に必要なサービスをすべて登録する拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-179">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

>[!TIP]
> <span data-ttu-id="5a4d8-180">内の特定のフレームワークに用意されているサービスを要求する`Startup`パラメーター リストで使用してメソッドを参照してください[アプリケーションの起動](startup.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-180">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-your-own-services"></a><span data-ttu-id="5a4d8-181">独自のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-181">Registering Your Own Services</span></span>

<span data-ttu-id="5a4d8-182">次のように、独自のアプリケーション サービスを登録できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-182">You can register your own application services as follows.</span></span> <span data-ttu-id="5a4d8-183">最初のジェネリック型では、コンテナーから要求される型 (通常はインターフェイス) を表します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-183">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="5a4d8-184">2 番目のジェネリック型では、コンテナーによってインスタンス化してこのような要求を満たすために使用される具体的な型を表します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-184">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="5a4d8-185">各`services.Add<ServiceName>`拡張メソッドを追加 (および可能性のある構成) サービス。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-185">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="5a4d8-186">たとえば、 `services.AddMvc()` MVC が必要とするサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-186">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="5a4d8-187">拡張メソッドを配置することをこの規則に従うお勧め、`Microsoft.Extensions.DependencyInjection`名前空間には、サービス登録のグループをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-187">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="5a4d8-188">`AddTransient`抽象型を必要とするすべてのオブジェクトとは別にインスタンス化される具体的なサービスにマップするメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-188">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="5a4d8-189">これは、サービスと呼ばれます*有効期間*、追加の有効期間オプションは次に説明します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-189">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="5a4d8-190">ことが重要、各サービスを登録するための適切な有効期間を選択します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-190">It is important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="5a4d8-191">それを要求する各クラスに、サービスの新しいインスタンスを提供しますか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-191">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="5a4d8-192">指定された web 要求全体で、1 つのインスタンスを使用する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-192">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="5a4d8-193">または、アプリケーションの有効期間の 1 つのインスタンスを使用します必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-193">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="5a4d8-194">この記事のサンプルと呼ばれる文字名を表示する単純なコント ローラーが`CharactersController`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-194">In the sample for this article, there is a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="5a4d8-195">その`Index`メソッドは、現在のアプリケーションで格納されている文字の一覧を表示しが存在しない場合に、いくつかの文字を使用して、コレクションを初期化します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-195">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="5a4d8-196">このアプリケーションは、Entity Framework のコアを使用していますが、なおと`ApplicationDbContext`クラスなしですが、コント ローラーで明らかなもの、永続化します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-196">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that is apparent in the controller.</span></span> <span data-ttu-id="5a4d8-197">特定のデータ アクセスのメカニズムが代わりに、インターフェイスの背後に抽象化されて`ICharacterRepository`、どの次のように、[リポジトリ パターン](http://deviq.com/repository-pattern/)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-197">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="5a4d8-198">インスタンス`ICharacterRepository`コンス トラクターを使用して要求し、必要に応じて文字へのアクセスを使用して、プライベート フィールドに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-198">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="5a4d8-199">`ICharacterRepository`コント ローラーを使用する必要がある 2 つのメソッドを定義`Character`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-199">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="5a4d8-200">このインターフェイスは、具象型で実装に`CharacterRepository`、それが実行時に使用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-200">This interface is in turn implemented by a concrete type, `CharacterRepository`, that is used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-201">DI 方法を使用、`CharacterRepository`クラスは、すべての「リポジトリ」] または [データ アクセス クラスではなく、アプリケーション サービスを行うことができる汎用モデル。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-201">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="5a4d8-202">なお`CharacterRepository`要求、`ApplicationDbContext`のコンス トラクターにします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-202">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="5a4d8-203">連鎖的に、次のように要求された依存関係は、独自の依存関係をさらに要求するごとに使用する依存関係の挿入の異常なことはできません。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-203">It is not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="5a4d8-204">コンテナーは、すべてのグラフ内の依存関係を解決し、完全に解決済みのサービスを返すことを担当します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-204">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-205">要求されたオブジェクトとすべてのオブジェクトを必要としてすべての必要なものは、オブジェクトの作成とも呼ば、*オブジェクト グラフ*です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-205">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="5a4d8-206">同様に、解決する必要がある依存関係の集合を通常呼びます、*依存関係ツリー*または*依存関係グラフ*です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-206">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="5a4d8-207">この場合、両方`ICharacterRepository`さらに`ApplicationDbContext`でサービス コンテナーに登録されている必要があります`ConfigureServices`で`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-207">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="5a4d8-208">`ApplicationDbContext`拡張メソッドの呼び出しで構成されている`AddDbContext<T>`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-208">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="5a4d8-209">次のコードの登録を示しています、`CharacterRepository`型です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-209">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="5a4d8-210">Entity Framework コンテキストは、サービスを使用してコンテナーに追加するか、`Scoped`有効期間。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-210">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="5a4d8-211">これは、処理の完了に自動的に上記のように、これらのヘルパー メソッドを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-211">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="5a4d8-212">Entity Framework を使用すると、リポジトリは、同じ有効期間を使用してください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-212">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

>[!WARNING]
> <span data-ttu-id="5a4d8-213">注意しなければならないメイン危険性を解決する、`Scoped`単一サービスです。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-213">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="5a4d8-214">このような場合は後続の要求を処理するときに、サービスが正しくない状態を持つこと可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-214">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="5a4d8-215">依存関係のあるサービスをコンテナーに登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-215">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="5a4d8-216">かどうか、サービスのコンス トラクターが必要です、プリミティブなど、`string`を使用してこれを挿入できるか、[パターンと構成オプション](configuration.md)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-216">If a service's constructor requires a primitive, such as a `string`, this can be injected by using the [options pattern and configuration](configuration.md).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="5a4d8-217">サービスの有効期間と登録オプション</span><span class="sxs-lookup"><span data-stu-id="5a4d8-217">Service Lifetimes and Registration Options</span></span>

<span data-ttu-id="5a4d8-218">ASP.NET サービスは、次の有効期間で構成できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-218">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="5a4d8-219">**一時的なもの**</span><span class="sxs-lookup"><span data-stu-id="5a4d8-219">**Transient**</span></span>

<span data-ttu-id="5a4d8-220">一時的な有効期間サービスは、要求されるたびに作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-220">Transient lifetime services are created each time they are requested.</span></span> <span data-ttu-id="5a4d8-221">この有効期間は軽量のステートレスなサービスに最適です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-221">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="5a4d8-222">**スコープ**</span><span class="sxs-lookup"><span data-stu-id="5a4d8-222">**Scoped**</span></span>

<span data-ttu-id="5a4d8-223">有効期間のスコープを持つサービスは、要求あたり 1 回作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-223">Scoped lifetime services are created once per request.</span></span>

<span data-ttu-id="5a4d8-224">**シングルトン**</span><span class="sxs-lookup"><span data-stu-id="5a4d8-224">**Singleton**</span></span>

<span data-ttu-id="5a4d8-225">シングルトン有効期間サービスが要求される最初の時に作成されます (または`ConfigureServices`インスタンスがありますを指定する場合に実行) し、すべての後続の要求が同じインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-225">Singleton lifetime services are created the first time they are requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="5a4d8-226">アプリケーションは、シングルトン動作を必要とする場合は、シングルトン デザイン パターンを実装して、自分でクラスのオブジェクトの有効期間を管理するのではなく勧めサービス コンテナー サービスの有効期間を管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-226">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="5a4d8-227">サービスは、いくつかの方法で、コンテナーに登録することができます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-227">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="5a4d8-228">使用する具体的な種類を指定して型を指定して、サービスの実装を登録する方法が既にあります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-228">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="5a4d8-229">さらに、ファクトリを指定できます、し、要求時にインスタンスの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-229">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="5a4d8-230">3 番目のアプローチでは、直接使用するには型のインスタンスをコンテナーの場合は、インスタンスを作成することはありません試みます (も、インスタンスが破棄されます) を指定します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-230">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="5a4d8-231">これらの有効期間と登録のオプションの違いを示すためには、1 つまたは複数のタスクを表す単純なインターフェイスを検討してください、*操作*一意の識別子を持つ`OperationId`します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-231">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="5a4d8-232">このサービスの有効期間を構成して方法に応じて、コンテナーは要求元のクラスにサービスの同じまたは別のインスタンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-232">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="5a4d8-233">オフにする有効期間が要求されているように、お lifetime オプションごとに 1 つの型が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-233">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="5a4d8-234">1 つのクラスを使用してこれらのインターフェイスを実装して`Operation`を受け入れる、`Guid`コンス トラクター、または使用して、新しい`Guid`がない場合。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-234">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="5a4d8-235">次に、 `ConfigureServices`、各種類は、名前付きの有効期間に従って、コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-235">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="5a4d8-236">なお、`IOperationSingletonInstance`サービスが特定のインスタンスを使用して既知の ID を持つ`Guid.Empty`のため、この型を (その Guid はすべて 0 になります) を使用するとき、明確になります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-236">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="5a4d8-237">登録されていること、`OperationService`他のそれぞれに依存する`Operation`ことができるように、要求内でチェック ボックスをオフこのサービスが、コント ローラー、または操作の種類ごとに、新しいものと同じインスタンスを取得するかどうかを入力します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-237">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="5a4d8-238">このサービスはすべて、ビューが表示されるように、その依存関係をプロパティとして公開します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-238">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="5a4d8-239">内でのアプリケーションに個別の個々 の要求間およびオブジェクトの有効期間を示すためには、サンプルが含まれています、`OperationsController`各種類の要求を`IOperation`型だけでなく、`OperationService`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-239">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="5a4d8-240">`Index`すべてのコント ローラーのおよびサービスのアクションで、表示`OperationId`値。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-240">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="5a4d8-241">今すぐこのコント ローラーのアクションを次の 2 つの個別の要求が行われます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-241">Now two separate requests are made to this controller action:</span></span>

![一時的なスコープ ベース、シングルトン、およびインスタンスのコント ローラーおよび最初の要求でサービス操作の操作の操作の ID 値 (GUID) を示す Microsoft Edge で実行されている依存関係の挿入のサンプル web アプリケーションの操作ビューです。](dependency-injection/_static/lifetimes_request1.png)

![操作は、2 番目の要求の操作の ID 値が表示を表示します。](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="5a4d8-244">確認のうち、`OperationId`および要求間で、要求内に値が異なります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-244">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="5a4d8-245">*一時的な*オブジェクトが異なる常に以外の新しいインスタンスはすべてのコント ローラーおよびすべてのサービスに提供します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-245">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="5a4d8-246">*スコープ*オブジェクトは、異なる要求間で異なるが、要求内で同じです。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-246">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="5a4d8-247">*シングルトン*オブジェクトは、すべてのオブジェクトとすべての要求で同じ (インスタンスがで提供されるかどうかに関係なく`ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="5a4d8-247">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="request-services"></a><span data-ttu-id="5a4d8-248">サービスを要求します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-248">Request Services</span></span>

<span data-ttu-id="5a4d8-249">ASP.NET 内で使用可能なサービスを要求`HttpContext`を通じて公開される、`RequestServices`コレクション。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-249">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![HttpContext 要求サービス Intellisense 要求サービスを取得または要求のサービス コンテナーへのアクセスを提供する IServiceProvider を設定することを示すダイアログ コンテキスト。](dependency-injection/_static/request-services.png)

<span data-ttu-id="5a4d8-251">サービスを要求では、サービスを構成して、アプリケーションの一部として要求を表します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-251">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="5a4d8-252">オブジェクトは、依存関係を指定するときに、これらがで検出された型で満たされます。`RequestServices`ではなく、`ApplicationServices`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-252">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="5a4d8-253">一般に、クラスのコンス トラクターを使用して必要なクラス型を要求することで、フレームワークのこれらの依存関係を挿入できるようにすることは、直接、これらのプロパティを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-253">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="5a4d8-254">簡単にテストされるクラスが生成されます。 (を参照してください[テスト](../testing/index.md)) より疎結合されたとします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-254">This yields classes that are easier to test (see [Testing](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-255">アクセスにコンス トラクターのパラメーターとしての依存関係の要求を必要に応じて、`RequestServices`コレクション。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-255">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-your-services-for-dependency-injection"></a><span data-ttu-id="5a4d8-256">依存関係の挿入で、サービスの設計</span><span class="sxs-lookup"><span data-stu-id="5a4d8-256">Designing Your Services For Dependency Injection</span></span>

<span data-ttu-id="5a4d8-257">共同作業者を取得する依存関係の挿入を使用するサービスを設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-257">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="5a4d8-258">つまり、ステートフルな静的メソッドの呼び出しは使用しない (と呼ばれるコード匂いが発生する[静的窓](http://deviq.com/static-cling/)) と、サービス内の依存クラスを直接インスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-258">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="5a4d8-259">やすくなり、語句を注意してください[は、New はグルー](https://ardalis.com/new-is-glue)型のインスタンスを作成するか、依存関係の挿入を使用して要求するかを選択するときに、します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-259">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="5a4d8-260">に従って、[実線の基本原則のオブジェクト指向設計](http://deviq.com/solid/)クラスが必然的に小さく、十分に考慮された、簡単にテストする傾向があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-260">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="5a4d8-261">場合、クラスが多すぎる依存関係が挿入されていることになる傾向がありますを検索しますか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-261">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="5a4d8-262">これは、クラスが多すぎる、実行しようとし、SRP - に違反する可能性がありますが、サインインでは、通常、[単一責任の原則](http://deviq.com/single-responsibility-principle/)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-262">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="5a4d8-263">新しいクラスにその負荷の一部を移動することによって、クラスをリファクタリングすることができるかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-263">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="5a4d8-264">注意してください、`Controller`クラス必要があるに重点を置いて UI の問題、これらに適切なクラスにビジネス ルールとデータ アクセスの実装の詳細を保持する必要がありますので[に関する注意事項を区切る](http://deviq.com/separation-of-concerns/)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-264">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="5a4d8-265">データ アクセスに関する具体的には、挿入することできます、 `DbContext` 、コント ローラーに (と仮定した場合にある services コンテナに追加した EF `ConfigureServices`)。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-265">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="5a4d8-266">一部の開発者がデータベースへのリポジトリ インターフェイスの使用を好むを挿入することではなく、`DbContext`直接です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-266">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="5a4d8-267">1 か所でアクセス ロジックのデータをカプセル化するインターフェイスを使用して、データベースが変更されたときに変更する必要がどのようにさまざまな場所が最小化できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-267">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="5a4d8-268">サービスの破棄</span><span class="sxs-lookup"><span data-stu-id="5a4d8-268">Disposing of services</span></span>

<span data-ttu-id="5a4d8-269">コンテナーが呼び出す`Dispose`の`IDisposable`型が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-269">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="5a4d8-270">ただし場合は、自分自身を追加するインスタンスのコンテナーに、これは破棄されません。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-270">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="5a4d8-271">例:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-271">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="5a4d8-272">バージョン 1.0 の場合で、コンテナーはで dispose を呼び出す*すべて* `IDisposable` 、作成していないものを含むオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-272">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it did not create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="5a4d8-273">既定のサービス コンテナーを置き換える</span><span class="sxs-lookup"><span data-stu-id="5a4d8-273">Replacing the default services container</span></span>

<span data-ttu-id="5a4d8-274">フレームワークと上に構築されたほとんどのコンシューマー アプリケーションの基本的なニーズに対応するものでは、組み込みのサービス コンテナー。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-274">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="5a4d8-275">ただし、開発者は、推奨されるコンテナーで組み込みのコンテナーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-275">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="5a4d8-276">`ConfigureServices`メソッドは通常を返します`void`を返すシグネチャが変更された場合、 `IServiceProvider`、別のコンテナーを構成し、返されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-276">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="5a4d8-277">多くの IOC コンテナーは .NET を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-277">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="5a4d8-278">この例では、 [Autofac](https://autofac.org/)パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-278">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="5a4d8-279">最初に、適切なコンテナーのパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-279">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="5a4d8-280">次に、構成内のコンテナー`ConfigureServices`を返すと、 `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-280">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

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

> [!NOTE]
> <span data-ttu-id="5a4d8-281">サード パーティ製 DI コンテナーを使用して、変更する必要あります`ConfigureServices`を返すように`IServiceProvider`の代わりに`void`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-281">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="5a4d8-282">通常どおり Autofac を最後に、構成`DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="5a4d8-282">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="5a4d8-283">実行時に、Autofac 型を解決して、依存関係の挿入に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-283">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="5a4d8-284">[Autofac および ASP.NET Core の使用に関する詳細については](http://docs.autofac.org/en/latest/integration/aspnetcore.html)します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-284">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="5a4d8-285">スレッド セーフ</span><span class="sxs-lookup"><span data-stu-id="5a4d8-285">Thread safety</span></span>

<span data-ttu-id="5a4d8-286">シングルトン サービスは、スレッド セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-286">Singleton services need to be thread safe.</span></span> <span data-ttu-id="5a4d8-287">一時的なサービスのシングルトン サービスに依存した場合、一時的なサービスもスレッドによって、単一の使用方法に応じてセーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-287">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it’s used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="5a4d8-288">推奨事項</span><span class="sxs-lookup"><span data-stu-id="5a4d8-288">Recommendations</span></span>

<span data-ttu-id="5a4d8-289">依存関係の挿入を使用する場合は、次の推奨事項に留意してください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-289">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="5a4d8-290">DI では、複雑な依存関係のあるオブジェクト用です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-290">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="5a4d8-291">コント ローラー、サービス、アダプター、およびリポジトリ di が追加されるオブジェクトのすべての例に示します。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-291">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="5a4d8-292">DI で直接データと構成を保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-292">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="5a4d8-293">たとえば、ユーザーのショッピング カートは、通常サービス コンテナーに追加べきではありません。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-293">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="5a4d8-294">構成を使用する必要があります、[オプション モデル](configuration.md#options-config-objects)です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-294">Configuration should use the [Options Model](configuration.md#options-config-objects).</span></span> <span data-ttu-id="5a4d8-295">同様に、その他のオブジェクトへのアクセスを許可するだけに存在する「データ ホルダー」オブジェクトは避けてください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-295">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="5a4d8-296">可能であれば、DI、経由で必要な実際のアイテムを要求することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-296">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="5a4d8-297">静的なサービスにアクセスしないようにします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-297">Avoid static access to services.</span></span>

* <span data-ttu-id="5a4d8-298">アプリケーション コードでサービスの場所は避けてください。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-298">Avoid service location in your application code.</span></span>

* <span data-ttu-id="5a4d8-299">静的なアクセスを防ぐ`HttpContext`です。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-299">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4d8-300">推奨事項のすべてのセットと同様に 1 つを無視する必要がある状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-300">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="5a4d8-301">例外はまれで--framework 自体内のほとんどの場合、非常に特殊なケースが見つかりました。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-301">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="5a4d8-302">依存関係の挿入は、*代替*静的/グローバル オブジェクト アクセスのパターンにします。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-302">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="5a4d8-303">静的オブジェクトのアクセス権を持つ混在している場合は、DI のメリットを実現することはできません。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-303">You will not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a4d8-304">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5a4d8-304">Additional Resources</span></span>

* [<span data-ttu-id="5a4d8-305">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="5a4d8-305">Application Startup</span></span>](startup.md)

* [<span data-ttu-id="5a4d8-306">テスト</span><span class="sxs-lookup"><span data-stu-id="5a4d8-306">Testing</span></span>](../testing/index.md)

* [<span data-ttu-id="5a4d8-307">依存関係の挿入 (MSDN) での ASP.NET Core でクリーンなコードの記述</span><span class="sxs-lookup"><span data-stu-id="5a4d8-307">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [<span data-ttu-id="5a4d8-308">コンテナー管理アプリケーションの設計、前段階: が属しているコンテナーしますか。</span><span class="sxs-lookup"><span data-stu-id="5a4d8-308">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [<span data-ttu-id="5a4d8-309">明示的な依存関係の原則</span><span class="sxs-lookup"><span data-stu-id="5a4d8-309">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)

* <span data-ttu-id="5a4d8-310">[コントロール コンテナーとの依存関係挿入パターンの反転](https://www.martinfowler.com/articles/injection.html)(ファウラー)</span><span class="sxs-lookup"><span data-stu-id="5a4d8-310">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
