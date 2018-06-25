---
title: ASP.NET Core でのコントローラーへの依存関係の挿入
author: ardalis
description: ASP.NET Core の MVC コントローラーが、ASP.NET Core でそれらのコンストラクターと依存関係の挿入を使用して、明示的にそれらの依存関係を要求する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 23c91a4363223a135c50ceca51e6af22ed69fe3b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276452"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="f6c9f-103">ASP.NET Core でのコントローラーへの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="f6c9f-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="f6c9f-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f6c9f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f6c9f-105">ASP.NET Core の MVC コントローラーは、それらのコンストラクターを使用して明示的にそれらの依存関係を要求する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="f6c9f-106">場合によっては、個々のコントローラーのアクションがサービスを必要とし、コントローラー レベルで要求しても意味がないことがあります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="f6c9f-107">この場合、アクション メソッドのパラメーターとしてサービスを挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="f6c9f-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="f6c9f-109">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="f6c9f-109">Dependency Injection</span></span>

<span data-ttu-id="f6c9f-110">依存関係の挿入は、[依存関係逆転の原則](http://deviq.com/dependency-inversion-principle/)に従う手法であり、弱く結合されたモジュールでアプリケーションを構成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="f6c9f-111">ASP.NET Core には[依存関係の挿入](../../fundamentals/dependency-injection.md)の組み込みのサポートがあり、アプリケーションを簡単にテストして維持することができます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="f6c9f-112">コンストラクターの挿入</span><span class="sxs-lookup"><span data-stu-id="f6c9f-112">Constructor Injection</span></span>

<span data-ttu-id="f6c9f-113">ASP.NET Core のコンストラクターに基づく依存関係の挿入の組み込みサポートは、MVC コントローラーを拡張します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="f6c9f-114">単にサービスの種類をコンストラクター パラメーターとしてコントローラーに追加することで、ASP.NET Core は組み込みのサービス コンテナーを使用してその種類の解決を試みます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="f6c9f-115">サービスの定義には、常にではありませんが、一般的にインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="f6c9f-116">たとえば、アプリケーションに現在の時刻に依存するビジネス ロジックがある場合、時刻を取得するサービスを (ハードコーディングする代わりに) 挿入することができます。これにより設定された時刻を使用する実装でテストに合格させることができます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="f6c9f-117">実行時にシステム クロックを使用するように、このようなインターフェイスを実装するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="f6c9f-118">こうすると、コントローラーでサービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="f6c9f-119">このケースでは、いくつかのロジックを `HomeController` `Index` メソッドに追加し、時刻に基づいてユーザーにあいさつを表示します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="f6c9f-120">アプリケーションを今すぐ実行した場合、エラーが発生する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="f6c9f-121">このエラーは、`Startup` クラスの `ConfigureServices` メソッドでサービスを構成していないときに発生します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="f6c9f-122">`IDateTime` の要求が `SystemDateTime` のインスタンスを使用して解決されるように指定するには、下のリストで強調表示された行を `ConfigureServices` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="f6c9f-123">この特定のサービスは、いくつかの異なる有効期間オプション (`Transient`、`Scoped`、または`Singleton`) のいずれかを使用して実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="f6c9f-124">これらの範囲オプションがサービスの動作にどのように影響するかを理解するには、「[依存関係の挿入](../../fundamentals/dependency-injection.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="f6c9f-125">サービスが構成された後に、アプリケーションを実行して、ホーム ページに移動すると、予想どおりに時刻ベースのメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![サーバーの応答メッセージ](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="f6c9f-127">コントローラーで依存関係 ([http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)) を明示的に要求することによってコードをテストしやすくする方法については、[コントローラー ロジックのテスト](testing.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="f6c9f-128">ASP.NET Core の組み込みの依存関係の挿入は、サービスを要求するクラスの 1 つのコンストラクターのみの使用をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="f6c9f-129">複数のコンストラクターを使用している場合、次のような例外が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="f6c9f-130">エラー メッセージが示しているように、コンストラクターを 1 つだけにすることでこの問題を修正することができます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="f6c9f-131">[既定の依存関係の挿入のサポートをサード パーティ製の実装に置き換えることもできます](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)。それらの多くは複数のコンストラクターをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="f6c9f-132">FromServices によるアクションの挿入</span><span class="sxs-lookup"><span data-stu-id="f6c9f-132">Action Injection with FromServices</span></span>

<span data-ttu-id="f6c9f-133">コントローラー内の複数のアクションのサービスが必要ない場合があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="f6c9f-134">この場合、アクション メソッドにパラメーターとしてサービスを挿入するのが賢明です。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="f6c9f-135">次に示すように、これは属性 `[FromServices]` でパラメーターをマークすることで行います。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="f6c9f-136">コントローラーから設定へのアクセス</span><span class="sxs-lookup"><span data-stu-id="f6c9f-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="f6c9f-137">コントローラー内からアプリケーションまたは構成の設定へのアクセスが一般的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="f6c9f-138">このアクセスでは、[構成](xref:fundamentals/configuration/index)に関するページで説明されているオプション パターンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="f6c9f-139">一般的に、依存関係の挿入を使用してコントローラーから直接設定を要求しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="f6c9f-140">`IOptions<T>` インスタンスを要求する方法がより適切です。`T` は必要な構成クラスです。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="f6c9f-141">オプションのパターンを使用するには、次のようなオプションを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="f6c9f-142">その後で、オプションのモデルを使用するようにアプリケーションを構成し、構成クラスを `ConfigureServices` でサービス コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="f6c9f-143">上記のリストで、JSON 形式のファイルから設定を読み取るようにアプリケーションを構成しています。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="f6c9f-144">上記のコメントが付けられたコードに示すように、コードで設定全体を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="f6c9f-145">その他の構成オプションについては、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="f6c9f-146">厳密に型指定された構成オブジェクト (このケースでは `SampleWebSettings`) を指定し、それをサービスのコレクションに追加したら、`IOptions<T>` のインスタンス (このケースでは `IOptions<SampleWebSettings>`) を要求することによって、任意のコントローラーまたはアクション メソッドからそれを要求することができます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="f6c9f-147">次のコードは、コントローラーから設定を要求する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="f6c9f-148">オプションのパターンに従うと、設定や構成を相互に分離し、さらにコントローラーが設定情報を見つける方法または場所を知る必要がないので、コントローラーを[関心の分離](http://deviq.com/separation-of-concerns/)に確実に従わせることができます。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="f6c9f-149">また、コントローラー クラス内での[静的な結合](http://deviq.com/static-cling/)または設定クラスの直接のインスタンス化がないので、コントローラーの単体テスト ([コントローラー ロジックのテスト](testing.md)) も容易になります。</span><span class="sxs-lookup"><span data-stu-id="f6c9f-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
