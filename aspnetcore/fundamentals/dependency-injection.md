---
title: ASP.NET Core での依存関係の挿入
author: ardalis
description: ASP.NET Core で依存関係の挿入を実装する方法とそれを使う方法について説明します。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 14c3d464773fe78a563a27776bfcd124c22df134
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566959"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="e1350-103">ASP.NET Core での依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="e1350-103">Dependency injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="e1350-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="e1350-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="e1350-105">ASP.NET Core は、依存関係の挿入をサポートして利用するようにまったく新しく設計されています。</span><span class="sxs-lookup"><span data-stu-id="e1350-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="e1350-106">ASP.NET Core アプリケーションは、組み込みフレームワーク サービスをスタートアップ クラスのメソッドに挿入することによってサービスを利用でき、アプリケーション サービスも挿入用に構成することができます。</span><span class="sxs-lookup"><span data-stu-id="e1350-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="e1350-107">ASP.NET Core によって提供される既定のサービス コンテナーは、最小限の機能セットを提供するものであり、他のコンテナーの置き換えは意図されていません。</span><span class="sxs-lookup"><span data-stu-id="e1350-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="e1350-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e1350-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="e1350-109">依存関係の挿入とは</span><span class="sxs-lookup"><span data-stu-id="e1350-109">What is dependency injection?</span></span>

<span data-ttu-id="e1350-110">依存関係の挿入 (DI) とは、オブジェクトとそのコラボレーター (依存関係) との間の疎結合を実現するための手法です。</span><span class="sxs-lookup"><span data-stu-id="e1350-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="e1350-111">クラスがそのアクションを実行するために必要なオブジェクトは、コラボレーターを直接インスタンス化したり、静的参照を使ったりするのではなく、それ以外の何らかの方法でクラスに提供されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="e1350-112">ほとんどの場合、クラスはコンストラクターを使って依存関係を宣言することで、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従うことができます。</span><span class="sxs-lookup"><span data-stu-id="e1350-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="e1350-113">この方法は、"コンストラクターの挿入" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e1350-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="e1350-114">クラスが DI を考慮して設計されていると、コラボレーターに対して直接的なハード コーディングされた依存関係を持たないので、より弱い結合になります。</span><span class="sxs-lookup"><span data-stu-id="e1350-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="e1350-115">これは[依存関係の逆転の原則](http://deviq.com/dependency-inversion-principle/)に従うものです。この原則は、"*高レベルのモジュールは低レベルのモジュールに依存してはならず、どちらも抽象化に依存する必要がある*" というものです。</span><span class="sxs-lookup"><span data-stu-id="e1350-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="e1350-116">特定の実装を参照する代わりに、クラスはクラスの構築時に提供される抽象化 (通常は `interfaces`) に要求します。</span><span class="sxs-lookup"><span data-stu-id="e1350-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="e1350-117">依存関係をインターフェイスに抽出し、これらのインターフェイスの実装をパラメーターとして提供することも、[戦略設計パターン](http://deviq.com/strategy-design-pattern/)の例です。</span><span class="sxs-lookup"><span data-stu-id="e1350-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="e1350-118">システムが DI を使うように設計されていて、多くのクラスがコンストラクター (またはプロパティ) を使って依存関係を要求する場合は、これらのクラスとそれに関連付けられている依存関係を作成する専用のクラスを用意すると便利です。</span><span class="sxs-lookup"><span data-stu-id="e1350-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="e1350-119">このようなクラスは、"*コンテナー*"、あるいはさらに具体的に[制御の反転 (IoC)](http://deviq.com/inversion-of-control/) コンテナーまたは依存関係の挿入 (DI) コンテナーと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e1350-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="e1350-120">コンテナーは基本的に、要求された型のインスタンスを提供するファクトリです。</span><span class="sxs-lookup"><span data-stu-id="e1350-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="e1350-121">特定の型が依存関係を持つものとして宣言されていて、コンテナーが依存関係の型を提供するように構成されている場合、コンテナーは要求されたインスタンスの作成の一環として依存関係を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1350-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="e1350-122">これにより、複雑な依存関係グラフをクラスに提供でき、ハード コーディングされたオブジェクトの構築は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e1350-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="e1350-123">依存関係を含むオブジェクトを作成するだけでなく、コンテナーは通常、アプリケーション内でのオブジェクトの有効期間を管理します。</span><span class="sxs-lookup"><span data-stu-id="e1350-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="e1350-124">ASP.NET Core には既定でコンストラクターの挿入をサポートする簡単な組み込みコンテナーが含まれ (`IServiceProvider` インターフェイスによって表されます)、ASP.NET は DI によって特定のサービスを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="e1350-125">ASP.NET のコンテナーでは、それが管理する型を "*サービス*" と呼びます。</span><span class="sxs-lookup"><span data-stu-id="e1350-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="e1350-126">これ以降この記事では、"*サービス*" は ASP.NET Core の IoC コンテナーによって管理される型を指します。</span><span class="sxs-lookup"><span data-stu-id="e1350-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="e1350-127">組み込みコンテナーのサービスは、アプリケーションの `Startup` クラスの `ConfigureServices` メソッドで構成します。</span><span class="sxs-lookup"><span data-stu-id="e1350-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="e1350-128">Martin Fowler は、「[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html)」(制御の反転コンテナーと依存関係の挿入パターン) で広範な記事を書いています。</span><span class="sxs-lookup"><span data-stu-id="e1350-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="e1350-129">Microsoft のパターンとプラクティスにも[依存関係の挿入](https://msdn.microsoft.com/library/hh323705.aspx)に関する優れた説明があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e1350-130">この記事では、すべての ASP.NET アプリケーションに適用される依存関係の挿入について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1350-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="e1350-131">MVC コントローラーでの依存関係の挿入については、「[Dependency injection into controllers](../mvc/controllers/dependency-injection.md)」(コントローラーへの依存関係の挿入) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e1350-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="e1350-132">コンストラクターの挿入の動作</span><span class="sxs-lookup"><span data-stu-id="e1350-132">Constructor injection behavior</span></span>

<span data-ttu-id="e1350-133">コンストラクターの挿入には、対象のコンストラクターが "*パブリック*" である必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="e1350-134">そうでない場合、アプリは `InvalidOperationException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="e1350-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="e1350-135">A suitable constructor for type 'YourType' couldn't be located.</span><span class="sxs-lookup"><span data-stu-id="e1350-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="e1350-136">Ensure the type is concrete and services are registered for all parameters of a public constructor. (型 'YourType' の適切なコンストラクターが見つかりませんでした。型が具象であること、およびサービスがパブリック コンストラクターのすべてのパラメーターに登録されていることを確認してください。)</span><span class="sxs-lookup"><span data-stu-id="e1350-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>

<span data-ttu-id="e1350-137">コンストラクターの挿入では、該当するコンストラクターがただ 1 つだけ存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="e1350-138">コンストラクターのオーバーロードはサポートされていますが、依存関係の挿入によってすべての引数を設定できるオーバーロードは 1 つしか存在できません。</span><span class="sxs-lookup"><span data-stu-id="e1350-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="e1350-139">複数のコンストラクターが存在する場合、アプリは `InvalidOperationException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="e1350-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="e1350-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span><span class="sxs-lookup"><span data-stu-id="e1350-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="e1350-141">There should only be one applicable constructor. (型 'YourType' には、特定の引数の型をすべて受け付けるコンストラクターが複数存在します。該当するコンストラクターは 1 つだけでなければなりません。)</span><span class="sxs-lookup"><span data-stu-id="e1350-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="e1350-142">コンストラクターは、依存関係の挿入によって提供されない引数を受け入れることができますが、既定値をサポートしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="e1350-143">例:</span><span class="sxs-lookup"><span data-stu-id="e1350-143">For example:</span></span>

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

## <a name="using-framework-provided-services"></a><span data-ttu-id="e1350-144">フレームワークが提供するサービスの使用</span><span class="sxs-lookup"><span data-stu-id="e1350-144">Using framework-provided services</span></span>

<span data-ttu-id="e1350-145">`Startup` クラスの `ConfigureServices` メソッドでは、Entity Framework Core や ASP.NET Core MVC といったプラットフォーム機能など、アプリケーションが使うサービスを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="e1350-146">最初に、`ConfigureServices` に提供される `IServiceCollection` では、次のサービスが定義されています ([ホストの構成方法](xref:fundamentals/host/index)に依存します)。</span><span class="sxs-lookup"><span data-stu-id="e1350-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="e1350-147">サービスの種類</span><span class="sxs-lookup"><span data-stu-id="e1350-147">Service Type</span></span> | <span data-ttu-id="e1350-148">有効期間</span><span class="sxs-lookup"><span data-stu-id="e1350-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="e1350-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="e1350-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="e1350-150">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-150">Singleton</span></span> |
| [<span data-ttu-id="e1350-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="e1350-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="e1350-152">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-152">Singleton</span></span> |
| [<span data-ttu-id="e1350-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e1350-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="e1350-154">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-154">Singleton</span></span> |
| [<span data-ttu-id="e1350-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="e1350-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="e1350-156">一時的</span><span class="sxs-lookup"><span data-stu-id="e1350-156">Transient</span></span> |
| [<span data-ttu-id="e1350-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="e1350-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="e1350-158">一時的</span><span class="sxs-lookup"><span data-stu-id="e1350-158">Transient</span></span> |
| [<span data-ttu-id="e1350-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e1350-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="e1350-160">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-160">Singleton</span></span> |
| [<span data-ttu-id="e1350-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="e1350-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="e1350-162">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-162">Singleton</span></span> |
| [<span data-ttu-id="e1350-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="e1350-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="e1350-164">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-164">Singleton</span></span> |
| [<span data-ttu-id="e1350-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="e1350-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="e1350-166">一時的</span><span class="sxs-lookup"><span data-stu-id="e1350-166">Transient</span></span> |
| [<span data-ttu-id="e1350-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="e1350-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="e1350-168">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-168">Singleton</span></span> |
| [<span data-ttu-id="e1350-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="e1350-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="e1350-170">一時的</span><span class="sxs-lookup"><span data-stu-id="e1350-170">Transient</span></span> |
| [<span data-ttu-id="e1350-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="e1350-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="e1350-172">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-172">Singleton</span></span> |
| [<span data-ttu-id="e1350-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="e1350-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="e1350-174">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-174">Singleton</span></span> |
| [<span data-ttu-id="e1350-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e1350-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="e1350-176">シングルトン</span><span class="sxs-lookup"><span data-stu-id="e1350-176">Singleton</span></span> |

<span data-ttu-id="e1350-177">`AddDbContext`、`AddIdentity`、`AddMvc` などの複数の拡張メソッドを使ってコンテナーにサービスを追加する方法の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e1350-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="e1350-178">ASP.NET によって提供される機能とミドルウェア (MVC など) は、単一の Add<*サービス名*> 拡張メソッドを使って、その機能に必要なすべてのサービスを登録する規則に従います。</span><span class="sxs-lookup"><span data-stu-id="e1350-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

> [!TIP]
> <span data-ttu-id="e1350-179">`Startup` メソッドでは、パラメーター リストを使って、フレームワークによって提供される特定のサービスを要求できます。詳細については、[アプリケーションの起動](startup.md)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e1350-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-services"></a><span data-ttu-id="e1350-180">サービスの登録</span><span class="sxs-lookup"><span data-stu-id="e1350-180">Registering services</span></span>

<span data-ttu-id="e1350-181">次のようにして、独自のアプリケーション サービスを登録できます。</span><span class="sxs-lookup"><span data-stu-id="e1350-181">You can register your own application services as follows.</span></span> <span data-ttu-id="e1350-182">最初のジェネリック型は、コンテナーに要求する型 (通常はインターフェイス) を表します。</span><span class="sxs-lookup"><span data-stu-id="e1350-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="e1350-183">2 番目のジェネリック型は、コンテナーによってインスタンス化されてそのような要求を満たすために使われる具象型を表します。</span><span class="sxs-lookup"><span data-stu-id="e1350-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="e1350-184">各 `services.Add<ServiceName>` 拡張メソッドは、サービスを追加 (および場合によっては構成) します。</span><span class="sxs-lookup"><span data-stu-id="e1350-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="e1350-185">たとえば、`services.AddMvc()` は MVC が必要とするサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1350-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="e1350-186">この規則に従い、拡張メソッドを `Microsoft.Extensions.DependencyInjection` 名前空間に配置して、サービス登録のグループをカプセル化することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1350-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="e1350-187">抽象型を、それを要求するすべてのオブジェクトに対して個別にインスタンス化される具象サービスにマップするには、`AddTransient` メソッドを使います。</span><span class="sxs-lookup"><span data-stu-id="e1350-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="e1350-188">これは、サービスの "*有効期間*" と呼ばれます。有効期間の他のオプションについては後で説明します。</span><span class="sxs-lookup"><span data-stu-id="e1350-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="e1350-189">登録するサービスごとに適切な有効期間を選ぶことが重要です。</span><span class="sxs-lookup"><span data-stu-id="e1350-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="e1350-190">要求するクラスごとに、サービスの新しいインスタンスを提供する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="e1350-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="e1350-191">特定の Web 要求全体で、1 つのインスタンスを使う必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="e1350-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="e1350-192">それとも、アプリケーションの有効期間を通して 1 つのインスタンスを使う必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="e1350-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="e1350-193">この記事のサンプルには、文字名を表示する `CharactersController` という名前の簡単なコントローラーがあります。</span><span class="sxs-lookup"><span data-stu-id="e1350-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="e1350-194">その `Index` メソッドは、アプリケーションに格納されている文字の現在のリストを表示し、存在しない場合はいくつかの文字でコレクションを初期化します。</span><span class="sxs-lookup"><span data-stu-id="e1350-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="e1350-195">このアプリケーションは、永続化のために Entity Framework Core と `ApplicationDbContext` クラスを使いますが、いずれもコントローラーですぐにわかるようにはなっていません。</span><span class="sxs-lookup"><span data-stu-id="e1350-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="e1350-196">代わりに、特定のデータ アクセス メカニズムがインターフェイス `ICharacterRepository` の背後に抽象化されており、それは[リポジトリ パターン](http://deviq.com/repository-pattern/)に従います。</span><span class="sxs-lookup"><span data-stu-id="e1350-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="e1350-197">`ICharacterRepository` のインスタンスがコンストラクターによって要求されてプライベート フィールドに割り当てられ、必要に応じて文字へのアクセスに使われます。</span><span class="sxs-lookup"><span data-stu-id="e1350-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="e1350-198">`ICharacterRepository` では、コントローラーが `Character` インスタンスを操作するときに必要な 2 つのメソッドが定義されています。</span><span class="sxs-lookup"><span data-stu-id="e1350-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="e1350-199">このインターフェイスは、実行時に使われる具象型 `CharacterRepository` で実装されています。</span><span class="sxs-lookup"><span data-stu-id="e1350-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="e1350-200">`CharacterRepository` クラスでの DI の使い方は一般的なモデルであり、"リポジトリ" やデータ アクセス クラスだけでなく、すべてのアプリケーション サービスで従うことができます。</span><span class="sxs-lookup"><span data-stu-id="e1350-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="e1350-201">`CharacterRepository` はコンストラクターで `ApplicationDbContext` を要求していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1350-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="e1350-202">このように、要求された各依存関係がさらにそれ自身の依存関係を要求するチェーン形式で依存関係の挿入が使われるのは、珍しいことではありません。</span><span class="sxs-lookup"><span data-stu-id="e1350-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="e1350-203">コンテナーは、グラフ内のすべての依存関係を解決し、完全に解決されたサービスを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="e1350-204">要求されたオブジェクト、それが要求するすべてのオブジェクト、さらにそれらが要求するすべてのオブジェクトを作成する処理は、"*オブジェクト グラフ*" と呼ばれることがあります。</span><span class="sxs-lookup"><span data-stu-id="e1350-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="e1350-205">同様に、解決する必要がある依存関係の集合的なセットは、通常、"*依存関係ツリー*" または "*依存関係グラフ*" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e1350-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="e1350-206">この場合、`ICharacterRepository` と `ApplicationDbContext` の両方が、`Startup` の `ConfigureServices` でサービス コンテナーに登録されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="e1350-207">`ApplicationDbContext` は、拡張メソッド `AddDbContext<T>` を呼び出して構成します。</span><span class="sxs-lookup"><span data-stu-id="e1350-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="e1350-208">次のコードでは、`CharacterRepository` 型の登録を示します。</span><span class="sxs-lookup"><span data-stu-id="e1350-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="e1350-209">Entity Framework コンテキストは、`Scoped` 有効期間を使ってサービス コンテナーに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="e1350-210">上で示したようにヘルパー メソッドを使っている場合は、これは自動的に処理されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="e1350-211">Entity Framework を利用するリポジトリは、同じ有効期間を使う必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

> [!WARNING]
> <span data-ttu-id="e1350-212">注意しなければならない最大の危険は、シングルトンからの `Scoped` サービスの解決です。</span><span class="sxs-lookup"><span data-stu-id="e1350-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="e1350-213">このような場合、後続の要求を処理するときに、サービスが正しくない状態になっている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="e1350-214">依存関係のあるサービスは、コンテナーに登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="e1350-215">サービスのコンストラクターでプリミティブ (`string` など) が必要な場合は、[構成](xref:fundamentals/configuration/index)と[オプション パターン](xref:fundamentals/configuration/options)を使ってこれを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="e1350-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="e1350-216">サービスの有効期間と登録のオプション</span><span class="sxs-lookup"><span data-stu-id="e1350-216">Service lifetimes and registration options</span></span>

<span data-ttu-id="e1350-217">ASP.NET サービスは、次の有効期間で構成できます。</span><span class="sxs-lookup"><span data-stu-id="e1350-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="e1350-218">**一時的**</span><span class="sxs-lookup"><span data-stu-id="e1350-218">**Transient**</span></span>

<span data-ttu-id="e1350-219">有効期間が一時的のサービスは、要求されるたびに作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="e1350-220">この有効期間は、軽量でステートレスのサービスに最適です。</span><span class="sxs-lookup"><span data-stu-id="e1350-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="e1350-221">**スコープ**</span><span class="sxs-lookup"><span data-stu-id="e1350-221">**Scoped**</span></span>

<span data-ttu-id="e1350-222">有効期間がスコープのサービスは、要求ごとに 1 回作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-222">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="e1350-223">ミドルウェアでスコープ サービスを使用している場合、サービスを `Invoke` または `InvokeAsync` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1350-223">If you're using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="e1350-224">コンストラクターを使用して挿入すると、サービスがシングルトンのように動作するよう強制されるので、コンストラクターを使用した挿入は行わないでください。</span><span class="sxs-lookup"><span data-stu-id="e1350-224">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span>

<span data-ttu-id="e1350-225">**シングルトン**</span><span class="sxs-lookup"><span data-stu-id="e1350-225">**Singleton**</span></span>

<span data-ttu-id="e1350-226">有効期間がシングルトンのサービスは、サービスが初めて要求されたとき (または、インスタンスを指定する場合は `ConfigureServices` が実行されたとき) に作成され、後続のすべての要求で同じインスタンスが使われます。</span><span class="sxs-lookup"><span data-stu-id="e1350-226">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="e1350-227">アプリケーションでシングルトン動作が必要な場合は、シングルトン設計パターンを実装して自分でクラス内のオブジェクトの有効期間を管理するのではなく、サービス コンテナーにサービスの有効期間の管理を任せることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1350-227">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="e1350-228">サービスは、複数の方法でコンテナーに登録できます。</span><span class="sxs-lookup"><span data-stu-id="e1350-228">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="e1350-229">使う具象型を指定することにより特定の型でサービスの実装を登録する方法は既に示しました。</span><span class="sxs-lookup"><span data-stu-id="e1350-229">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="e1350-230">さらに、ファクトリを指定することができます。ファクトリは、必要なときにインスタンスを作成するために使われます。</span><span class="sxs-lookup"><span data-stu-id="e1350-230">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="e1350-231">3 番目の方法は、使う型のインスタンスを直接指定する方法です。この場合、コンテナーはインスタンスの作成を試みません (インスタンスの破棄も行いません)。</span><span class="sxs-lookup"><span data-stu-id="e1350-231">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="e1350-232">これらの有効期間と登録オプションの違いを示すため、1 つまたは複数のタスクを一意の識別子 `OperationId` を持つ "*操作*" として表す簡単なインターフェイスについて考えます。</span><span class="sxs-lookup"><span data-stu-id="e1350-232">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="e1350-233">このサービスの有効期間の構成方法に応じて、コンテナーはサービスの同じインスタンスまたは異なるインスタンスを要求元のクラスに提供します。</span><span class="sxs-lookup"><span data-stu-id="e1350-233">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="e1350-234">要求されている有効期間がはっきりわかるように、有効期間オプションごとに 1 つの型を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1350-234">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="e1350-235">これらのインターフェイスを、1 つのクラス `Operation` を使って実装します。このクラスは、コンストラクターで `Guid` を受け取り、提供されない場合は新しい `Guid` を使います。</span><span class="sxs-lookup"><span data-stu-id="e1350-235">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="e1350-236">次に、`ConfigureServices` では、各型が名前で指定されている有効期間に従ってコンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-236">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="e1350-237">`IOperationSingletonInstance` サービスは既知の ID `Guid.Empty` を持つ特定のインスタンスを使っているので、この型が使われているときは明確にわかります (その Guid はすべて 0 になります)。</span><span class="sxs-lookup"><span data-stu-id="e1350-237">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="e1350-238">また、他の各 `Operation` 型に依存する `OperationService` も登録してあるので、操作の種類ごとに、このサービスがコントローラーと同じインスタンスを取得しているか、または新しいインスタンスかが、要求内ではっきりわかります。</span><span class="sxs-lookup"><span data-stu-id="e1350-238">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="e1350-239">このサービスが行うことは、依存関係をビューに表示できるように、依存関係をプロパティとして公開することだけです。</span><span class="sxs-lookup"><span data-stu-id="e1350-239">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="e1350-240">アプリケーションに対する 1 つの要求内でのオブジェクトの有効期間および異なる個別の要求間でのオブジェクトの有効期間を示すため、サンプルには、各種類の `IOperation` 型と `OperationService` を要求する `OperationsController` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e1350-240">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="e1350-241">その後、`Index` アクションは、すべてのコント ローラーおよびサービスの `OperationId` の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="e1350-241">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="e1350-242">ここで、このコントローラー アクションに対して 2 つの異なる要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e1350-242">Now two separate requests are made to this controller action:</span></span>

![最初の要求での Transient、Scoped、Singleton、Instance の各コントローラーと OperationService の操作の操作 ID の値 (GUID) が表示されている、Microsoft Edge で実行する DependencyInjectionSample Web アプリケーションの操作ビュー。](dependency-injection/_static/lifetimes_request1.png)

![2 番目の要求の操作 ID の値が表示されている操作ビュー。](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="e1350-245">要求内および要求間で変化している `OperationId` の値を確認してください。</span><span class="sxs-lookup"><span data-stu-id="e1350-245">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="e1350-246">*Transient* オブジェクトは常に異なります。コントローラーごとおよびサービスごとに、新しいインスタンスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-246">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="e1350-247">*Scoped* オブジェクトは、要求内では同じですが、異なる要求間では異なります</span><span class="sxs-lookup"><span data-stu-id="e1350-247">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="e1350-248">*Singleton* オブジェクトは、インスタンスが `ConfigureServices` で提供されるかどうかに関係なく、すべてのオブジェクトとすべての要求について同じです</span><span class="sxs-lookup"><span data-stu-id="e1350-248">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="resolve-a-scoped-service-within-the-application-scope"></a><span data-ttu-id="e1350-249">アプリケーション スコープ内のスコープ サービスを解決する</span><span class="sxs-lookup"><span data-stu-id="e1350-249">Resolve a scoped service within the application scope</span></span>

<span data-ttu-id="e1350-250">[IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) を使用して [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) を作成し、アプリのスコープ内のスコープ サービスを解決します。</span><span class="sxs-lookup"><span data-stu-id="e1350-250">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="e1350-251">この方法は、起動時に初期化タスクを実行するために、スコープ サービスにアクセスするのに便利です。</span><span class="sxs-lookup"><span data-stu-id="e1350-251">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="e1350-252">次の例では、`Program.Main` で `MyScopedService` のコンテキストを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e1350-252">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

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

## <a name="scope-validation"></a><span data-ttu-id="e1350-253">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="e1350-253">Scope validation</span></span>

<span data-ttu-id="e1350-254">アプリが ASP.NET Core 2.0 以降の開発環境で実行されている場合、既定のサービス プロバイダーは次を確認するチェックを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1350-254">When the app is running in the Development environment on ASP.NET Core 2.0 or later, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="e1350-255">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="e1350-255">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="e1350-256">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="e1350-256">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="e1350-257">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-257">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="e1350-258">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-258">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="e1350-259">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-259">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="e1350-260">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-260">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="e1350-261">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="e1350-261">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="e1350-262">詳細については、[Web ホストのトピックのスコープ検証](xref:fundamentals/host/web-host#scope-validation)に関する説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1350-262">For more information, see [Scope validation in the Web Host topic](xref:fundamentals/host/web-host#scope-validation).</span></span>

## <a name="request-services"></a><span data-ttu-id="e1350-263">要求サービス</span><span class="sxs-lookup"><span data-stu-id="e1350-263">Request Services</span></span>

<span data-ttu-id="e1350-264">`HttpContext` からの ASP.NET 要求内で使用可能なサービスは、`RequestServices` コレクションを通じて公開されます。</span><span class="sxs-lookup"><span data-stu-id="e1350-264">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![要求のサービス コンテナーへのアクセスを提供する IServiceProvider を要求サービスが取得または設定することを示す、HttpContext 要求サービスの Intellisense コンテキスト ダイアログ。](dependency-injection/_static/request-services.png)

<span data-ttu-id="e1350-266">要求サービスは、アプリケーションの一部として構成および要求するサービスを表します。</span><span class="sxs-lookup"><span data-stu-id="e1350-266">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="e1350-267">オブジェクトで依存関係を指定すると、これらは `ApplicationServices` ではなく `RequestServices` で検出された型で満たされます。</span><span class="sxs-lookup"><span data-stu-id="e1350-267">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="e1350-268">一般に、これらのプロパティを直接使ってはいけません。代わりに、クラスのコンストラクターを使ってクラスで必要な型を要求し、フレームワークにこれらの依存関係を挿入させます。</span><span class="sxs-lookup"><span data-stu-id="e1350-268">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="e1350-269">このようにすると、クラスはテストしやすくなり (「[テストとデバッグ](xref:test/index)」を参照)、より弱い結合になります。</span><span class="sxs-lookup"><span data-stu-id="e1350-269">This yields classes that are easier to test (see [Test and debug](xref:test/index)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="e1350-270">コンストラクターのパラメーターとして依存関係を要求し、`RequestServices` コレクションにアクセスするようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-270">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-services-for-dependency-injection"></a><span data-ttu-id="e1350-271">依存関係の挿入のためのサービスの設計</span><span class="sxs-lookup"><span data-stu-id="e1350-271">Designing services for dependency injection</span></span>

<span data-ttu-id="e1350-272">依存関係の挿入を使ってコラボレーターを取得するように、サービスを設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-272">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="e1350-273">つまり、ステートフルな静的メソッドの呼び出し ([静的な結び付き](http://deviq.com/static-cling/)として知られるコードの状態になります)、およびサービス内での依存クラスの直接インスタンス化は、使わないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-273">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="e1350-274">型をインスタンス化するか、それとも依存関係の挿入を使って要求するか迷ったときは、"[new は密接な結び付きを生み出す](https://ardalis.com/new-is-glue)" という格言を思い出すと役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="e1350-274">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="e1350-275">[オブジェクト指向設計の SOLID 原則](http://deviq.com/solid/)に従うことで、クラスは必然的に小さく、十分に考慮された、簡単にテストできるものになる可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="e1350-275">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="e1350-276">クラスで挿入される依存関係が多すぎる場合はどうすればよいでしょうか。</span><span class="sxs-lookup"><span data-stu-id="e1350-276">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="e1350-277">これは一般に、クラスで行おうとしていることが多すぎるサインであり、SRP ([単一責任の原則](http://deviq.com/single-responsibility-principle/)) に違反する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-277">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="e1350-278">責任の一部を新しいクラスに移動することにより、クラスをリファクタリングできるかどうか検討します。</span><span class="sxs-lookup"><span data-stu-id="e1350-278">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="e1350-279">`Controller` クラスは UI に関することに集中する必要があり、ビジネス ルールとデータ アクセスの実装の詳細は[問題の分離](http://deviq.com/separation-of-concerns/)に適したクラスに維持する必要があることに留意してください。</span><span class="sxs-lookup"><span data-stu-id="e1350-279">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="e1350-280">特にデータ アクセスに関しては、`DbContext` をコントローラーに挿入できます (`ConfigureServices` のサービス コンテナーに EF を追加した場合)。</span><span class="sxs-lookup"><span data-stu-id="e1350-280">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="e1350-281">`DbContext` を直接挿入するより、データベースへのリポジトリ インターフェイスを使う方が好まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-281">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="e1350-282">インターフェイスを使ってデータ アクセス ロジックを 1 か所にカプセル化すると、データベースを変更するときに変更する必要がある場所の数を最少にできます。</span><span class="sxs-lookup"><span data-stu-id="e1350-282">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="e1350-283">サービスの破棄</span><span class="sxs-lookup"><span data-stu-id="e1350-283">Disposing of services</span></span>

<span data-ttu-id="e1350-284">コンテナーは、作成する `IDisposable` 型の `Dispose` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e1350-284">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="e1350-285">ただし、手動でインスタンスをコンテナーに追加した場合は、破棄されません。</span><span class="sxs-lookup"><span data-stu-id="e1350-285">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="e1350-286">例:</span><span class="sxs-lookup"><span data-stu-id="e1350-286">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="e1350-287">バージョン 1.0 のコンテナーは、コンテナーで作成しなかったものも含めて、"*すべての*" `IDisposable` に対して破棄を呼び出しました。</span><span class="sxs-lookup"><span data-stu-id="e1350-287">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="e1350-288">既定のサービス コンテナーの置き換え</span><span class="sxs-lookup"><span data-stu-id="e1350-288">Replacing the default services container</span></span>

<span data-ttu-id="e1350-289">組み込みのサービス コンテナーは、フレームワークと、それを基に構築されたほとんどのコンシューマー アプリケーションの、基本的なニーズに対応することを意図したものです。</span><span class="sxs-lookup"><span data-stu-id="e1350-289">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="e1350-290">ただし、開発者は、組み込みのコンテナーを好みのコンテナーで置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="e1350-290">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="e1350-291">通常、`ConfigureServices` メソッドは `void` を返しますが、`IServiceProvider` を返すようにシグネチャを変更した場合、別のコンテナーを構成して返すことができます。</span><span class="sxs-lookup"><span data-stu-id="e1350-291">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="e1350-292">.NET で使うことができる多くの IOC コンテナーがあります。</span><span class="sxs-lookup"><span data-stu-id="e1350-292">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="e1350-293">この例では、[Autofac](https://autofac.org/) パッケージを使います。</span><span class="sxs-lookup"><span data-stu-id="e1350-293">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="e1350-294">最初に、適切なコンテナー パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1350-294">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="e1350-295">次に、`ConfigureServices` でコンテナーを構成して、`IServiceProvider` を返します。</span><span class="sxs-lookup"><span data-stu-id="e1350-295">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

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
> <span data-ttu-id="e1350-296">サードパーティの DI コンテナーを使うときは、`void` ではなく `IServiceProvider` を返すように、`ConfigureServices` を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-296">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="e1350-297">最後に、`DefaultModule` で Autofac を通常どおり構成します。</span><span class="sxs-lookup"><span data-stu-id="e1350-297">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="e1350-298">実行時には、Autofac を使って、型の解決と依存関係の挿入が行われます。</span><span class="sxs-lookup"><span data-stu-id="e1350-298">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="e1350-299">[Autofac と ASP.NET Core の使用については、こちらをご覧ください](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。</span><span class="sxs-lookup"><span data-stu-id="e1350-299">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="e1350-300">スレッド セーフ</span><span class="sxs-lookup"><span data-stu-id="e1350-300">Thread safety</span></span>

<span data-ttu-id="e1350-301">シングルトン サービスは、スレッド セーフである必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-301">Singleton services need to be thread safe.</span></span> <span data-ttu-id="e1350-302">シングルトン サービスに一時的サービスへの依存関係がある場合、シングルトンによる一時的サービスの使い方によっては、一時的サービスもスレッド セーフであることが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-302">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="e1350-303">推奨事項</span><span class="sxs-lookup"><span data-stu-id="e1350-303">Recommendations</span></span>

<span data-ttu-id="e1350-304">依存関係の挿入を使うときは、次の推奨事項に留意してください。</span><span class="sxs-lookup"><span data-stu-id="e1350-304">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="e1350-305">DI は、複雑な依存関係のあるオブジェクトのためのものです。</span><span class="sxs-lookup"><span data-stu-id="e1350-305">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="e1350-306">コントローラー、サービス、アダプター、リポジトリはすべて、DI に追加される可能性があるオブジェクトの例です。</span><span class="sxs-lookup"><span data-stu-id="e1350-306">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="e1350-307">データと構成は、DI に直接格納しないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-307">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="e1350-308">たとえば、通常、ユーザーのショッピング カートをサービス コンテナーに追加してはいけません。</span><span class="sxs-lookup"><span data-stu-id="e1350-308">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="e1350-309">構成では、[オプション パターン](xref:fundamentals/configuration/options)を使う必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-309">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="e1350-310">同様に、他のオブジェクトへのアクセスを許可するためだけに存在する "データ ホルダー" オブジェクトは避ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-310">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="e1350-311">可能な場合は、実際に必要なアイテムを DI 経由で要求することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1350-311">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="e1350-312">サービスへの静的なアクセスを行わないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-312">Avoid static access to services.</span></span>

* <span data-ttu-id="e1350-313">サービスの場所をアプリケーション コード内に記述しないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-313">Avoid service location in your application code.</span></span>

* <span data-ttu-id="e1350-314">`HttpContext` への静的なアクセスを行わないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1350-314">Avoid static access to `HttpContext`.</span></span>

<span data-ttu-id="e1350-315">どのような推奨事項でも、無視する必要がある状況が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e1350-315">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="e1350-316">例外はまれであることがわかっており、ほとんどはフレームワーク自体内での非常に特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="e1350-316">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="e1350-317">依存関係の挿入は静的/グローバル オブジェクト アクセス パターンの*代替機能*です。</span><span class="sxs-lookup"><span data-stu-id="e1350-317">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="e1350-318">静的オブジェクト アクセスと併用した場合、DI のメリットを実現することはできません。</span><span class="sxs-lookup"><span data-stu-id="e1350-318">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1350-319">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e1350-319">Additional resources</span></span>

* [<span data-ttu-id="e1350-320">ビューへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="e1350-320">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="e1350-321">コントローラーへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="e1350-321">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="e1350-322">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="e1350-322">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="e1350-323">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="e1350-323">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="e1350-324">テストとデバッグ</span><span class="sxs-lookup"><span data-stu-id="e1350-324">Test and debug</span></span>](xref:test/index)
* [<span data-ttu-id="e1350-325">ファクトリ ベースのミドルウェアのアクティブ化</span><span class="sxs-lookup"><span data-stu-id="e1350-325">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="e1350-326">依存関係の挿入による ASP.NET Core でのクリーンなコードの作成 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="e1350-326">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="e1350-327">コンテナー管理アプリケーションの設計、前段階: コンテナーはどこに属していますか</span><span class="sxs-lookup"><span data-stu-id="e1350-327">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="e1350-328">明示的な依存関係の原則</span><span class="sxs-lookup"><span data-stu-id="e1350-328">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)
* <span data-ttu-id="e1350-329">[制御の反転コンテナーと依存関係の挿入パターン](https://www.martinfowler.com/articles/injection.html) (Fowler)</span><span class="sxs-lookup"><span data-stu-id="e1350-329">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
