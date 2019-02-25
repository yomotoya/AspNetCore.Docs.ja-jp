---
title: ASP.NET Core でのコントローラーへの依存関係の挿入
author: ardalis
description: ASP.NET Core の MVC コントローラーが、ASP.NET Core でそれらのコンストラクターと依存関係の挿入を使用して、明示的にそれらの依存関係を要求する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9d9d0a68927da62fad8df72c868eaf4b8ada440d
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410272"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="92e0d-103">ASP.NET Core でのコントローラーへの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="92e0d-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="92e0d-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="92e0d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="92e0d-105">ASP.NET Core の MVC コントローラーは、それらのコンストラクターを使用して明示的にそれらの依存関係を要求する必要があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="92e0d-106">場合によっては、個々のコントローラーのアクションがサービスを必要とし、コントローラー レベルで要求しても意味がないことがあります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="92e0d-107">この場合、アクション メソッドのパラメーターとしてサービスを挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="92e0d-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="92e0d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="92e0d-109">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="92e0d-109">Dependency Injection</span></span>

<span data-ttu-id="92e0d-110">ASP.NET Core には[依存関係の挿入](../../fundamentals/dependency-injection.md)の組み込みのサポートがあり、アプリケーションを簡単にテストして維持することができます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="92e0d-111">コンストラクターの挿入</span><span class="sxs-lookup"><span data-stu-id="92e0d-111">Constructor Injection</span></span>

<span data-ttu-id="92e0d-112">ASP.NET Core のコンストラクターに基づく依存関係の挿入の組み込みサポートは、MVC コントローラーを拡張します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="92e0d-113">単にサービスの種類をコンストラクター パラメーターとしてコントローラーに追加することで、ASP.NET Core は組み込みのサービス コンテナーを使用してその種類の解決を試みます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="92e0d-114">サービスの定義には、常にではありませんが、一般的にインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="92e0d-115">たとえば、アプリケーションに現在の時刻に依存するビジネス ロジックがある場合、時刻を取得するサービスを (ハードコーディングする代わりに) 挿入することができます。これにより設定された時刻を使用する実装でテストに合格させることができます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="92e0d-116">実行時にシステム クロックを使用するように、このようなインターフェイスを実装するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="92e0d-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="92e0d-117">こうすると、コントローラーでサービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="92e0d-118">このケースでは、いくつかのロジックを `HomeController` `Index` メソッドに追加し、時刻に基づいてユーザーにあいさつを表示します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="92e0d-119">アプリケーションを今すぐ実行した場合、エラーが発生する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="92e0d-120">このエラーは、`Startup` クラスの `ConfigureServices` メソッドでサービスを構成していないときに発生します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="92e0d-121">`IDateTime` の要求が `SystemDateTime` のインスタンスを使用して解決されるように指定するには、下のリストで強調表示された行を `ConfigureServices` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="92e0d-122">この特定のサービスは、いくつかの異なる有効期間オプション (`Transient`、`Scoped`、または`Singleton`) のいずれかを使用して実装することもできます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="92e0d-123">これらの範囲オプションがサービスの動作にどのように影響するかを理解するには、「[依存関係の挿入](../../fundamentals/dependency-injection.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="92e0d-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="92e0d-124">サービスが構成された後に、アプリケーションを実行して、ホーム ページに移動すると、予想どおりに時刻ベースのメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![サーバーの応答メッセージ](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="92e0d-126">コントローラーで依存関係を明示的に要求することによってコードをテストしやすくする方法については、[コントローラー ロジックのテスト](testing.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="92e0d-126">See [Test controller logic](testing.md) to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

<span data-ttu-id="92e0d-127">ASP.NET Core の組み込みの依存関係の挿入は、サービスを要求するクラスの 1 つのコンストラクターのみの使用をサポートします。</span><span class="sxs-lookup"><span data-stu-id="92e0d-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="92e0d-128">複数のコンストラクターを使用している場合、次のような例外が表示される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="92e0d-129">エラー メッセージが示しているように、1 つのコンストラクターを使用することでこの問題を修正できます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-129">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="92e0d-130">[既定の依存関係の挿入のコンテナーをサード パーティ製の実装に置き換える](xref:fundamentals/dependency-injection#default-service-container-replacement)こともできます。それらの多くは複数のコンストラクターをサポートします。</span><span class="sxs-lookup"><span data-stu-id="92e0d-130">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="92e0d-131">FromServices によるアクションの挿入</span><span class="sxs-lookup"><span data-stu-id="92e0d-131">Action Injection with FromServices</span></span>

<span data-ttu-id="92e0d-132">コントローラー内の複数のアクションのサービスが必要ない場合があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="92e0d-133">この場合、アクション メソッドにパラメーターとしてサービスを挿入するのが賢明です。</span><span class="sxs-lookup"><span data-stu-id="92e0d-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="92e0d-134">次に示すように、これは属性 `[FromServices]` でパラメーターをマークすることで行います。</span><span class="sxs-lookup"><span data-stu-id="92e0d-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="92e0d-135">コントローラーから設定へのアクセス</span><span class="sxs-lookup"><span data-stu-id="92e0d-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="92e0d-136">コントローラー内からアプリケーションまたは構成の設定へのアクセスが一般的なパターンです。</span><span class="sxs-lookup"><span data-stu-id="92e0d-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="92e0d-137">このアクセスでは、[構成](xref:fundamentals/configuration/index)に関するページで説明されているオプション パターンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="92e0d-138">一般的に、依存関係の挿入を使用してコントローラーから直接設定を要求しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="92e0d-139">`IOptions<T>` インスタンスを要求する方法がより適切です。`T` は必要な構成クラスです。</span><span class="sxs-lookup"><span data-stu-id="92e0d-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="92e0d-140">オプションのパターンを使用するには、次のようなオプションを表すクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="92e0d-141">その後で、オプションのモデルを使用するようにアプリケーションを構成し、構成クラスを `ConfigureServices` でサービス コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="92e0d-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="92e0d-142">上記のリストで、JSON 形式のファイルから設定を読み取るようにアプリケーションを構成しています。</span><span class="sxs-lookup"><span data-stu-id="92e0d-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="92e0d-143">上記のコメントが付けられたコードに示すように、コードで設定全体を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="92e0d-144">その他の構成オプションについては、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="92e0d-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="92e0d-145">厳密に型指定された構成オブジェクト (このケースでは `SampleWebSettings`) を指定し、それをサービスのコレクションに追加したら、`IOptions<T>` のインスタンス (このケースでは `IOptions<SampleWebSettings>`) を要求することによって、任意のコントローラーまたはアクション メソッドからそれを要求することができます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="92e0d-146">次のコードは、コントローラーから設定を要求する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="92e0d-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="92e0d-147">オプションのパターンに従うと、設定や構成を相互に分離し、さらにコントローラーが設定情報を見つける方法または場所を知る必要がないので、コントローラーを[関心の分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)に確実に従わせることができます。</span><span class="sxs-lookup"><span data-stu-id="92e0d-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="92e0d-148">また、コントローラー クラス内で設定クラスを直接インスタンス化することがないため、コントローラーの[単体テスト](testing.md)も容易になります。</span><span class="sxs-lookup"><span data-stu-id="92e0d-148">It also makes the controller easier to [unit test](testing.md), since there's no direct instantiation of settings classes within the controller class.</span></span>
