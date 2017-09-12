---
title: "コント ローラーに依存関係の挿入"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: b83bd4a24ccf7e90e9df06d6a8e229a2d5c6699a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="b309b-103">コント ローラーに依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="b309b-103">Dependency injection into controllers</span></span>

<a name=dependency-injection-controllers></a>

<span data-ttu-id="b309b-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b309b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b309b-105">ASP.NET Core の MVC コント ローラーには、そのコンス トラクターを使用して明示的にその依存関係を要求する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b309b-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="b309b-106">場合によっては、個々 のコント ローラーのアクションは、サービスを必要があり、ことはできません、コント ローラー レベルを要求する合理的です。</span><span class="sxs-lookup"><span data-stu-id="b309b-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="b309b-107">この場合、アクション メソッドのパラメーターとしてサービスを挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="b309b-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

[<span data-ttu-id="b309b-108">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="b309b-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)

## <a name="dependency-injection"></a><span data-ttu-id="b309b-109">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="b309b-109">Dependency Injection</span></span>

<span data-ttu-id="b309b-110">依存関係の挿入がこれに続く手法、[依存関係の逆転原則](http://deviq.com/dependency-inversion-principle/)、疎結合モジュールで構成されるアプリケーションにすることができます。</span><span class="sxs-lookup"><span data-stu-id="b309b-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="b309b-111">ASP.NET Core はの組み込みサポート[依存性の注入](../../fundamentals/dependency-injection.md)を簡単にアプリケーションをテストし、維持します。</span><span class="sxs-lookup"><span data-stu-id="b309b-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="b309b-112">コンス トラクター インジェクション</span><span class="sxs-lookup"><span data-stu-id="b309b-112">Constructor Injection</span></span>

<span data-ttu-id="b309b-113">MVC コント ローラーにコンス トラクターに基づく依存関係の挿入の ASP.NET Core の組み込みサポートを拡張します。</span><span class="sxs-lookup"><span data-stu-id="b309b-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="b309b-114">単にサービスの種類を追加すると、コンス トラクター パラメーターとして、コント ローラーに、ASP.NET Core をサービス コンテナーでそのビルドを使用してその型の解決を試みます。</span><span class="sxs-lookup"><span data-stu-id="b309b-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="b309b-115">サービスは、常にではありませんが、通常、定義のインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b309b-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="b309b-116">たとえば、アプリケーションに現在の時刻に依存するビジネス ロジックがある場合、テストの設定された時刻を使用する実装を渡すことを許可する時間 (代わりにハードコーディングする)、サービスを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="b309b-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

<span data-ttu-id="b309b-117">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="b309b-117">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span></span>


<span data-ttu-id="b309b-118">単純では実行時に、システム クロックを使用するように、このようなインターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="b309b-118">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

<span data-ttu-id="b309b-119">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="b309b-119">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span></span>


<span data-ttu-id="b309b-120">こうすると、サービス、コント ローラーに使用できます。</span><span class="sxs-lookup"><span data-stu-id="b309b-120">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="b309b-121">いくつかのロジックを追加しましたここで、 `HomeController` `Index`時刻に基づいて、ユーザーにするとあいさつ文を表示するメソッド。</span><span class="sxs-lookup"><span data-stu-id="b309b-121">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

<span data-ttu-id="b309b-122">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span><span class="sxs-lookup"><span data-stu-id="b309b-122">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span></span>

<span data-ttu-id="b309b-123">今すぐアプリケーションを実行する場合ほとんどの場合、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="b309b-123">If we run the application now, we will most likely encounter an error:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="b309b-124">このエラーでサービスが構成されていないときに発生、`ConfigureServices`メソッドで、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="b309b-124">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="b309b-125">要求を指定する`IDateTime`のインスタンスを使用して解決する必要がある`SystemDateTime`には、以下のリストで強調表示された行を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b309b-125">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

<span data-ttu-id="b309b-126">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span><span class="sxs-lookup"><span data-stu-id="b309b-126">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="b309b-127">この特定のサービスは、さまざまな有効期間オプションがいくつかのいずれかを使用して実装することも (`Transient`、 `Scoped`、または`Singleton`)。</span><span class="sxs-lookup"><span data-stu-id="b309b-127">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="b309b-128">参照してください[依存性の注入](../../fundamentals/dependency-injection.md)方法、サービスの動作に影響はこれらの各スコープ オプションを理解します。</span><span class="sxs-lookup"><span data-stu-id="b309b-128">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="b309b-129">サービスが構成されると、アプリケーションを実行し、ホーム ページに移動する必要がありますメッセージを表示、時間ベース期待どおりに。</span><span class="sxs-lookup"><span data-stu-id="b309b-129">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![サーバーの応答メッセージ](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="b309b-131">参照してください[テスト コント ローラー ロジック](testing.md)の依存関係を明示的に要求する方法について[http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)コント ローラーに簡単にコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="b309b-131">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="b309b-132">ASP.NET Core の組み込みの依存関係の挿入は、サービスを要求するクラスの 1 つのコンス トラクターのみを含めることがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="b309b-132">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="b309b-133">複数のコンス トラクターを使っている場合、例外を示すを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b309b-133">If you have more than one constructor, you may get an exception stating:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="b309b-134">エラー メッセージが示しているとは、コンス トラクターは 1 つだけを持つこの問題を修正することができます。</span><span class="sxs-lookup"><span data-stu-id="b309b-134">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="b309b-135">こともできます[サード パーティ製の実装で既定の依存関係の挿入のサポートを置き換えます](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)、多くの複数のコンス トラクターをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b309b-135">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="b309b-136">FromServices による操作性の注入</span><span class="sxs-lookup"><span data-stu-id="b309b-136">Action Injection with FromServices</span></span>

<span data-ttu-id="b309b-137">場合があります、コント ローラー内のサービスを 1 つ以上のアクションは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="b309b-137">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="b309b-138">この場合が賢明をアクション メソッドにパラメーターとして、サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="b309b-138">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="b309b-139">これは、属性を持つパラメーターをマークすることで`[FromServices]`次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="b309b-139">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

<span data-ttu-id="b309b-140">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span><span class="sxs-lookup"><span data-stu-id="b309b-140">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span></span>

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="b309b-141">コント ローラーから設定へのアクセス</span><span class="sxs-lookup"><span data-stu-id="b309b-141">Accessing Settings from a Controller</span></span>

<span data-ttu-id="b309b-142">コント ローラー内からアプリケーションまたは構成の設定へのアクセスは、一般的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="b309b-142">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="b309b-143">このアクセスで説明されているオプション パターンを使用する必要があります[構成](../../fundamentals/configuration.md)です。</span><span class="sxs-lookup"><span data-stu-id="b309b-143">This access should use the Options pattern described in [configuration](../../fundamentals/configuration.md).</span></span> <span data-ttu-id="b309b-144">一般にする必要がありますいないを要求する設定の依存関係の挿入を使用して、コント ローラーから直接。</span><span class="sxs-lookup"><span data-stu-id="b309b-144">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="b309b-145">より適切な方法は、要求、`IOptions<T>`インスタンス、場所`T`必要がある構成クラスです。</span><span class="sxs-lookup"><span data-stu-id="b309b-145">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="b309b-146">オプションのパターンを使用するには、ように、オプションを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b309b-146">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

<span data-ttu-id="b309b-147">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="b309b-147">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span></span>

<span data-ttu-id="b309b-148">オプションのモデルを使用し、構成、クラス、サービスでコレクションに追加するアプリケーションを構成する必要があります`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b309b-148">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

<span data-ttu-id="b309b-149">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span><span class="sxs-lookup"><span data-stu-id="b309b-149">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="b309b-150">上記の一覧で、アプリケーションが JSON 形式のファイルから設定を読み取る構成を行っています。</span><span class="sxs-lookup"><span data-stu-id="b309b-150">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="b309b-151">上記のコメントが付けられたコードに示すようには、コードで完全設定を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="b309b-151">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="b309b-152">参照してください[構成](../../fundamentals/configuration.md)詳細構成オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="b309b-152">See [Configuration](../../fundamentals/configuration.md) for further configuration options.</span></span>

<span data-ttu-id="b309b-153">厳密に型指定された構成オブジェクトを指定すると、(この場合、 `SampleWebSettings`) され、追加サービスのコレクションに要求できますが、コント ローラーまたはアクション メソッドからのインスタンスを要求することによって`IOptions<T>`(この場合、 `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="b309b-153">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="b309b-154">次のコードは、コント ローラーから、設定を要求 1 つの方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="b309b-154">The following code shows how one would request the settings from a controller:</span></span>

<span data-ttu-id="b309b-155">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span><span class="sxs-lookup"><span data-stu-id="b309b-155">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span></span>

<span data-ttu-id="b309b-156">設定と構成、互いから分離するのには、オプションのパターンに従うと、コント ローラーは、次のことを確認[関心の分離](http://deviq.com/separation-of-concerns/)方法や場所を知る必要があるため、設定が見つかりません情報です。</span><span class="sxs-lookup"><span data-stu-id="b309b-156">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="b309b-157">また、コント ローラーの単体テストを簡単に[テスト コント ローラー ロジック](testing.md)があるため、ありません[静的窓](http://deviq.com/static-cling/)またはコント ローラー クラス内で設定クラスの直接インスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="b309b-157">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
