---
title: "ASP.NET Core でのアプリケーション部分"
author: ardalis
description: "アプリのリソースに対する abstrations は、アプリケーションの構成要素を使用して、検出またはアセンブリからの機能の読み込みを避けるためにアプリを構成する方法を説明します。"
keywords: "ASP.NET Core、アプリケーションの部分、アプリの一部"
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a260675e7461105d4f6a0c61fd13971663c268f2
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="af588-104">ASP.NET Core でのアプリケーション部分</span><span class="sxs-lookup"><span data-stu-id="af588-104">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="af588-105">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af588-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="af588-106">*アプリケーション パーツ*MVC のコント ローラー、コンポーネントの表示と同様に機能元となる、アプリケーションのリソースを抽象化は、またはタグ ヘルパーを検出することがあります。</span><span class="sxs-lookup"><span data-stu-id="af588-106">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="af588-107">アプリケーション パーツの 1 つの例は、アセンブリ参照と公開型およびコンパイルの参照をカプセル化するの AssemblyPart です。</span><span class="sxs-lookup"><span data-stu-id="af588-107">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="af588-108">*機能のプロバイダー* ASP.NET Core MVC アプリの機能を設定するアプリケーション部分と連携します。</span><span class="sxs-lookup"><span data-stu-id="af588-108">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="af588-109">アプリケーション パーツの主なユース ケースが検出 (または読み込みを回避する) にアプリケーションを構成できるようにするアセンブリから MVC 機能します。</span><span class="sxs-lookup"><span data-stu-id="af588-109">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="af588-110">アプリケーション パーツの概要</span><span class="sxs-lookup"><span data-stu-id="af588-110">Introducing Application Parts</span></span>

<span data-ttu-id="af588-111">MVC アプリからその機能を読み込む[アプリケーション パーツ](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)です。</span><span class="sxs-lookup"><span data-stu-id="af588-111">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="af588-112">具体的には、 [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)クラスがアセンブリで補助されているアプリケーションの部分を表します。</span><span class="sxs-lookup"><span data-stu-id="af588-112">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="af588-113">検出し、コント ローラー、コンポーネントの表示、タグ ヘルパーの場合は、razor コンパイル ソースなどの MVC 機能を読み込むには、これらのクラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="af588-113">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="af588-114">[ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) MVC アプリに、アプリケーションの部分と使用可能な機能のプロバイダーを追跡します。</span><span class="sxs-lookup"><span data-stu-id="af588-114">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="af588-115">操作できますが、`ApplicationPartManager`で`Startup`MVC を構成する場合。</span><span class="sxs-lookup"><span data-stu-id="af588-115">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="af588-116">既定では、MVC は依存関係ツリーを検索および (でも他のアセンブリ) コント ローラーを検索します。</span><span class="sxs-lookup"><span data-stu-id="af588-116">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="af588-117">(たとえば、コンパイル時に参照されていないプラグイン) から任意のアセンブリを読み込むには、アプリケーションの部分を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="af588-117">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="af588-118">アプリケーションの部分で構成を使用することができます*回避*コント ローラーで、特定のアセンブリまたは場所を検索します。</span><span class="sxs-lookup"><span data-stu-id="af588-118">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="af588-119">部分 (またはアセンブリ) を利用できますがアプリに変更することによってを制御する、`ApplicationParts`のコレクション、`ApplicationPartManager`です。</span><span class="sxs-lookup"><span data-stu-id="af588-119">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="af588-120">内のエントリの順序、`ApplicationParts`コレクションは重要ではありません。</span><span class="sxs-lookup"><span data-stu-id="af588-120">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="af588-121">完全に構成することが重要、`ApplicationPartManager`コンテナー内のサービスの構成を使用する前にします。</span><span class="sxs-lookup"><span data-stu-id="af588-121">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="af588-122">たとえば、完全に設定します、`ApplicationPartManager`を呼び出す前に`AddControllersAsServices`です。</span><span class="sxs-lookup"><span data-stu-id="af588-122">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="af588-123">ためには、失敗していることを意味をコント ローラー アプリケーション パーツの追加後にメソッドの呼び出しは影響しません (は登録されないサービスとして) アプリケーションの不適切な bevavior する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="af588-123">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="af588-124">使用したくないコント ローラーを含むアセンブリがあればを削除してから、 `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="af588-124">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="af588-125">プロジェクトのアセンブリとその依存アセンブリに加えて、`ApplicationPartManager`のパーツが含まれます`Microsoft.AspNetCore.Mvc.TagHelpers`と`Microsoft.AspNetCore.Mvc.Razor`既定です。</span><span class="sxs-lookup"><span data-stu-id="af588-125">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="af588-126">アプリケーション機能のプロバイダー</span><span class="sxs-lookup"><span data-stu-id="af588-126">Application Feature Providers</span></span>

<span data-ttu-id="af588-127">アプリケーション機能のプロバイダーは、アプリケーション部分を調べるし、これらのパーツの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="af588-127">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="af588-128">次の MVC 機能の組み込み機能のプロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="af588-128">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="af588-129">コントローラー</span><span class="sxs-lookup"><span data-stu-id="af588-129">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="af588-130">メタデータの参照</span><span class="sxs-lookup"><span data-stu-id="af588-130">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="af588-131">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="af588-131">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="af588-132">コンポーネントの表示</span><span class="sxs-lookup"><span data-stu-id="af588-132">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="af588-133">機能プロバイダーを継承`IApplicationFeatureProvider<T>`ここで、`T`機能の種類です。</span><span class="sxs-lookup"><span data-stu-id="af588-133">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="af588-134">MVC の機能の種類のいずれかのプロバイダーが上に示した独自の機能を実装することができます。</span><span class="sxs-lookup"><span data-stu-id="af588-134">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="af588-135">機能プロバイダーでの順序、`ApplicationPartManager.FeatureProviders`以降のプロバイダーは、以前のプロバイダーによって実行されたアクションに対処できるため、コレクションが重要ですが、できます。</span><span class="sxs-lookup"><span data-stu-id="af588-135">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="af588-136">例: 汎用コント ローラーの機能</span><span class="sxs-lookup"><span data-stu-id="af588-136">Sample: Generic controller feature</span></span>

<span data-ttu-id="af588-137">既定では、ASP.NET Core MVC には、汎用的なコント ローラーが無視されます (たとえば、 `SomeController<T>`)。</span><span class="sxs-lookup"><span data-stu-id="af588-137">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="af588-138">このサンプルは、既定のプロバイダーの後に実行し、型の指定されたリストの汎用的なコント ローラー インスタンスを追加するコント ローラー機能プロバイダーを使用して (で定義されている`EntityTypes.Types`)。</span><span class="sxs-lookup"><span data-stu-id="af588-138">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="af588-139">エンティティの種類:</span><span class="sxs-lookup"><span data-stu-id="af588-139">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="af588-140">機能プロバイダーを追加`Startup`:</span><span class="sxs-lookup"><span data-stu-id="af588-140">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="af588-141">既定では、ルーティングに使用されるジェネリック コント ローラー名と形式にする*GenericController'1 [ウィジェット]*の代わりに*ウィジェット*です。</span><span class="sxs-lookup"><span data-stu-id="af588-141">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="af588-142">次の属性を使用して、コント ローラーによって使用されるジェネリック型に対応する名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="af588-142">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="af588-143">`GenericController`クラス。</span><span class="sxs-lookup"><span data-stu-id="af588-143">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="af588-144">結果、一致するルートが要求されたときに:</span><span class="sxs-lookup"><span data-stu-id="af588-144">The result, when a matching route is requested:</span></span>

![サンプル アプリからの出力例を読み取ると、'こんにちはジェネリック Sproket コント ローラーからです '](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="af588-146">例: 表示の使用可能な機能</span><span class="sxs-lookup"><span data-stu-id="af588-146">Sample: Display available features</span></span>

<span data-ttu-id="af588-147">反復処理できる使用可能なデータが設定された機能をアプリに要求することによって、`ApplicationPartManager`を通じて[依存性の注入](../../fundamentals/dependency-injection.md)と適切な機能のインスタンスを作成使用します。</span><span class="sxs-lookup"><span data-stu-id="af588-147">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="af588-148">出力例:</span><span class="sxs-lookup"><span data-stu-id="af588-148">Example output:</span></span>

![サンプル アプリからの出力例](app-parts/_static/available-features.png)
