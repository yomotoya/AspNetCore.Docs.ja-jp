---
title: "アプリケーション モデルの操作"
author: ardalis
description: 
keywords: "ASP.NET Core、ASP.NET Core MVC アプリケーション モデル"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 18046389becd17135ff831e71e700244d48552d3
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="5669e-103">アプリケーション モデルの操作</span><span class="sxs-lookup"><span data-stu-id="5669e-103">Working with the Application Model</span></span>

<span data-ttu-id="5669e-104">によって[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="5669e-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="5669e-105">ASP.NET Core MVC 定義、*アプリケーション モデル*MVC アプリのコンポーネントを表すです。</span><span class="sxs-lookup"><span data-stu-id="5669e-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="5669e-106">読み取りおよび MVC 要素の動作を変更するには、このモデルを操作できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="5669e-107">既定では、MVC はクラスは、コント ローラーであると見なされます、これらのクラスにする方法は、アクション、およびパラメーターとルーティングの動作方法を決定する特定の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="5669e-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="5669e-108">ニーズに合わせて、アプリケーションの独自の規則を作成し、グローバル、または属性として適用するには、この動作をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="5669e-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="5669e-109">モデルおよびプロバイダー</span><span class="sxs-lookup"><span data-stu-id="5669e-109">Models and Providers</span></span>

<span data-ttu-id="5669e-110">ASP.NET Core MVC アプリケーションのモデルには、抽象インターフェイスと、MVC アプリケーションを記述する具体的な実装クラスの両方が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5669e-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="5669e-111">このモデルでは、MVC アプリケーションのコント ローラー、アクション、アクション パラメーターのルート、および既定の規則に従ってフィルターの検出の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="5669e-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="5669e-112">アプリケーション モデルを使用して、既定の MVC 動作から別の規則に従って、アプリを変更できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="5669e-113">パラメーター、名前、ルート、およびフィルターはすべてデータとして使用構成アクションおよびコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="5669e-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="5669e-114">ASP.NET Core MVC アプリケーションのモデルには、次のような構造があります。</span><span class="sxs-lookup"><span data-stu-id="5669e-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="5669e-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="5669e-115">ApplicationModel</span></span>
    * <span data-ttu-id="5669e-116">コント ローラー (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="5669e-116">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="5669e-117">アクション (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="5669e-117">Actions (ActionModel)</span></span>
            * <span data-ttu-id="5669e-118">パラメーター (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="5669e-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="5669e-119">モデルの各レベルが、共通のアクセス`Properties`コレクション、および下位レベルにアクセスして、階層の上位レベルで設定されたプロパティ値を上書きします。</span><span class="sxs-lookup"><span data-stu-id="5669e-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="5669e-120">プロパティに保存される、`ActionDescriptor.Properties`アクションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="5669e-121">要求を処理しているときに任意のプロパティを追加または変更、規則からアクセスできます`ActionContext.ActionDescriptor.Properties`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="5669e-122">-アクションごとに、フィルターやモデル バインダーなどを構成する優れた方法としては、プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="5669e-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="5669e-123">`ActionDescriptor.Properties`コレクションはスレッド セーフはな (書き込み) のアプリのスタートアップが完了します。</span><span class="sxs-lookup"><span data-stu-id="5669e-123">The `ActionDescriptor.Properties` collection is not thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="5669e-124">規則は、このコレクションにデータを安全に追加する最善の方法です。</span><span class="sxs-lookup"><span data-stu-id="5669e-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="5669e-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="5669e-125">IApplicationModelProvider</span></span>

<span data-ttu-id="5669e-126">ASP.NET Core MVC によって定義された、プロバイダーのパターンを使用して、アプリケーション モデルの読み込み、 [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider)インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="5669e-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="5669e-127">ここでいくつかの方法の内部実装の詳細は、このプロバイダーが機能します。</span><span class="sxs-lookup"><span data-stu-id="5669e-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="5669e-128">高度なトピックでは、-、アプリケーション モデルを活用するほとんどのアプリは規則で作業してこれを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5669e-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="5669e-129">実装、`IApplicationModelProvider`インターフェイス「ラップ」、各実装通話で`OnProvidersExecuting`に基づいて昇順でその`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="5669e-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="5669e-130">`OnProvidersExecuted`メソッドが逆の順序で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="5669e-131">フレームワークでは、いくつかのプロバイダーを定義します。</span><span class="sxs-lookup"><span data-stu-id="5669e-131">The framework defines several providers:</span></span>

<span data-ttu-id="5669e-132">最初 (`Order=-1000`)。</span><span class="sxs-lookup"><span data-stu-id="5669e-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="5669e-133">(`Order=-990`)。</span><span class="sxs-lookup"><span data-stu-id="5669e-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="5669e-134">どの 2 つのプロバイダーの値が同じ順序`Order`と呼ばれますが、定義されていませんし、したがってが依存していません。</span><span class="sxs-lookup"><span data-stu-id="5669e-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore should not be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="5669e-135">`IApplicationModelProvider`フレームワークの作成者を拡張するための高度な概念がします。</span><span class="sxs-lookup"><span data-stu-id="5669e-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="5669e-136">一般に、アプリは、規則を使用する必要があり、フレームワークは、プロバイダーを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5669e-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="5669e-137">重要な違いは、プロバイダーが規則の前に常に実行することです。</span><span class="sxs-lookup"><span data-stu-id="5669e-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="5669e-138">`DefaultApplicationModelProvider` ASP.NET Core MVC で使用される既定の動作の多くを確立します。</span><span class="sxs-lookup"><span data-stu-id="5669e-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="5669e-139">その役割は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5669e-139">Its responsibilities include:</span></span>

* <span data-ttu-id="5669e-140">グローバル フィルターをコンテキストに追加します。</span><span class="sxs-lookup"><span data-stu-id="5669e-140">Adding global filters to the context</span></span>
* <span data-ttu-id="5669e-141">コント ローラーのコンテキストに追加</span><span class="sxs-lookup"><span data-stu-id="5669e-141">Adding controllers to the context</span></span>
* <span data-ttu-id="5669e-142">アクション、コント ローラーのパブリック メソッドの追加</span><span class="sxs-lookup"><span data-stu-id="5669e-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="5669e-143">コンテキストにアクション メソッドのパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="5669e-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="5669e-144">ルート アクションとその他の属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="5669e-144">Applying route and other attributes</span></span>

<span data-ttu-id="5669e-145">いくつかの組み込みのビヘイビアーはによって実装される、`DefaultApplicationModelProvider`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="5669e-146">このプロバイダーは、構築するため、 [ `ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)を参照する順番[ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)、 [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)、および[`ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="5669e-146">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="5669e-147">`DefaultApplicationModelProvider`クラスは、内部のフレームワーク実装の詳細は、今後変更したりできます。</span><span class="sxs-lookup"><span data-stu-id="5669e-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="5669e-148">`AuthorizationApplicationModelProvider`に関連付けられている動作を適用する役割が、`AuthorizeFilter`と`AllowAnonymousFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="5669e-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="5669e-149">[これらの属性の詳細について](https://docs.microsoft.com/aspnet/core/security/authorization/simple)です。</span><span class="sxs-lookup"><span data-stu-id="5669e-149">[Learn more about these attributes](https://docs.microsoft.com/aspnet/core/security/authorization/simple).</span></span>

<span data-ttu-id="5669e-150">`CorsApplicationModelProvider`に関連付けられている動作を実装、`IEnableCorsAttribute`と`IDisableCorsAttribute`、および`DisableCorsAuthorizationFilter`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="5669e-151">[詳細については、CORS](https://docs.microsoft.com/aspnet/core/security/cors)です。</span><span class="sxs-lookup"><span data-stu-id="5669e-151">[Learn more about CORS](https://docs.microsoft.com/aspnet/core/security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="5669e-152">規則</span><span class="sxs-lookup"><span data-stu-id="5669e-152">Conventions</span></span>

<span data-ttu-id="5669e-153">アプリケーション モデルでは、モデル全体またはプロバイダーをオーバーライドするよりもモデルの動作をカスタマイズする簡単な方法を提供する規則の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="5669e-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="5669e-154">これらの抽象化は、アプリの動作を変更することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5669e-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="5669e-155">規則のカスタマイズを動的に適用するコードを記述するための手段です。</span><span class="sxs-lookup"><span data-stu-id="5669e-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="5669e-156">中に[フィルター](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters)フレームワークの動作を変更する手段を提供のカスタマイズでは、アプリ全体が一緒にワイヤード方法を制御できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-156">While [filters](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="5669e-157">次の規則を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="5669e-158">MVC のオプションを追加することによって、または実装することで、規則が適用される`Attribute`s とコント ローラー、アクション、またはアクションのパラメーターに適用する (に似て[ `Filters` ](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters))。</span><span class="sxs-lookup"><span data-stu-id="5669e-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters)).</span></span> <span data-ttu-id="5669e-159">フィルターとは異なり規則は各要求の一部ではなく、アプリの起動時にのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="5669e-160">例: ApplicationModel を変更します。</span><span class="sxs-lookup"><span data-stu-id="5669e-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="5669e-161">次の規則を使用すると、アプリケーション モデルにプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="5669e-161">The following convention is used to add a property to the application model.</span></span> 

<span data-ttu-id="5669e-162">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-162">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]</span></span>

<span data-ttu-id="5669e-163">MVC が追加されたときに、オプションとしてアプリケーション モデルの規則が適用される`ConfigureServices`で`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-163">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

<span data-ttu-id="5669e-164">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="5669e-164">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]</span></span>

<span data-ttu-id="5669e-165">プロパティからアクセス可能な`ActionDescriptor`コント ローラー アクション内でプロパティのコレクション。</span><span class="sxs-lookup"><span data-stu-id="5669e-165">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

<span data-ttu-id="5669e-166">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]</span><span class="sxs-lookup"><span data-stu-id="5669e-166">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]</span></span>

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="5669e-167">サンプル: ControllerModel 説明を変更します。</span><span class="sxs-lookup"><span data-stu-id="5669e-167">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="5669e-168">前の例のようにに、カスタム プロパティを含める、コント ローラー モデルを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="5669e-168">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="5669e-169">これらは、アプリケーション モデルで指定された同じ名前の既存のプロパティを上書きします。</span><span class="sxs-lookup"><span data-stu-id="5669e-169">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="5669e-170">次の規則の属性は、コント ローラー レベルの説明を追加します。</span><span class="sxs-lookup"><span data-stu-id="5669e-170">The following convention attribute adds a description at the controller level:</span></span>

<span data-ttu-id="5669e-171">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-171">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]</span></span>

<span data-ttu-id="5669e-172">この規則は、コント ローラーに属性として適用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-172">This convention is applied as an attribute on a controller.</span></span>

<span data-ttu-id="5669e-173">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="5669e-173">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]</span></span>

<span data-ttu-id="5669e-174">同様に、前の例のように「説明」プロパティにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5669e-174">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="5669e-175">サンプル: ActionModel 説明を変更します。</span><span class="sxs-lookup"><span data-stu-id="5669e-175">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="5669e-176">別の属性を規則は、既にアプリケーションまたはコント ローラーのレベルで適用される動作をオーバーライドする個々 のアクションに適用できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-176">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

<span data-ttu-id="5669e-177">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-177">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]</span></span>

<span data-ttu-id="5669e-178">前の例のコント ローラー内のアクションにこれを適用するには、コント ローラー レベルの規則を上書きする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5669e-178">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

<span data-ttu-id="5669e-179">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="5669e-179">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]</span></span>

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="5669e-180">例: ParameterModel を変更します。</span><span class="sxs-lookup"><span data-stu-id="5669e-180">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="5669e-181">アクション パラメーターを変更するには、次の規則を適用できます、`BindingInfo`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-181">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="5669e-182">次の規則は、パラメーターは、ルート パラメーター; である必要があります。その他の潜在的なバインドなどのソース (クエリ文字列の値) は無視されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-182">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

<span data-ttu-id="5669e-183">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-183">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]</span></span>

<span data-ttu-id="5669e-184">属性は、任意のアクション パラメーターを適用できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-184">The attribute may be applied to any action parameter:</span></span>

<span data-ttu-id="5669e-185">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="5669e-185">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]</span></span>

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="5669e-186">サンプル: ActionModel 名の変更</span><span class="sxs-lookup"><span data-stu-id="5669e-186">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="5669e-187">次の規則を変更、`ActionModel`を更新する、*名前*が適用されるアクションのです。</span><span class="sxs-lookup"><span data-stu-id="5669e-187">The following convention modifies the `ActionModel` to update the *name* of the action to which it is applied.</span></span> <span data-ttu-id="5669e-188">新しい名前は、属性のパラメーターとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-188">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="5669e-189">この新しい名前は、このアクション メソッドに到達するのに使用されるルートに影響するため、ルーティング、によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-189">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

<span data-ttu-id="5669e-190">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-190">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]</span></span>

<span data-ttu-id="5669e-191">この属性は、アクション メソッドに適用、 `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="5669e-191">This attribute is applied to an action method in the `HomeController`:</span></span>

<span data-ttu-id="5669e-192">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="5669e-192">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]</span></span>

<span data-ttu-id="5669e-193">メソッドの名前になっても`SomeName`、属性がメソッド名を使用する MVC 規則を上書きおよびとアクション名を置換`MyCoolAction`です。</span><span class="sxs-lookup"><span data-stu-id="5669e-193">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="5669e-194">したがって、この操作に到達するのに使用されるルートは`/Home/MyCoolAction`します。</span><span class="sxs-lookup"><span data-stu-id="5669e-194">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="5669e-195">この例には基本的には、組み込みの使用と同じ[ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="5669e-195">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="5669e-196">サンプル: カスタム ルーティング規約</span><span class="sxs-lookup"><span data-stu-id="5669e-196">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="5669e-197">使用することができます、`IApplicationModelConvention`をどのようにルーティング動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="5669e-197">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="5669e-198">次の規則を置き換えて、そのルート コント ローラーの名前空間を組み込む例、`.`を持つ名前空間で`/`ルート上で。</span><span class="sxs-lookup"><span data-stu-id="5669e-198">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

<span data-ttu-id="5669e-199">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-199">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]</span></span>

<span data-ttu-id="5669e-200">スタートアップのオプションとして、規則が追加されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-200">The convention is added as an option in Startup.</span></span>

<span data-ttu-id="5669e-201">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="5669e-201">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]</span></span>

> [!TIP]
> <span data-ttu-id="5669e-202">規則を追加することができます、[ミドルウェア](https://docs.microsoft.com/aspnet/core/fundamentals/middleware)にアクセスして`MvcOptions`を使用します。`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="5669e-202">You can add conventions to your [middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="5669e-203">このサンプルでは、コント ローラーが、名前に"Namespace"を持つ属性のルーティングを使用していないルートにこの規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-203">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="5669e-204">次のコント ローラーは、この規則を示しています。</span><span class="sxs-lookup"><span data-stu-id="5669e-204">The following controller demonstrates this convention:</span></span>

<span data-ttu-id="5669e-205">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="5669e-205">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]</span></span>

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="5669e-206">WebApiCompatShim でアプリケーション モデルの使用法</span><span class="sxs-lookup"><span data-stu-id="5669e-206">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="5669e-207">ASP.NET Core MVC は、ASP.NET Web API 2 から異なる一連の規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="5669e-207">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="5669e-208">カスタム規則を使用して、Web API アプリのものと一致するように、ASP.NET Core MVC アプリの動作を変更できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-208">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="5669e-209">Microsoft が付属しています、 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)特にこの目的のためです。</span><span class="sxs-lookup"><span data-stu-id="5669e-209">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="5669e-210">詳細については[ASP.NET Web API からの移行](https://docs.microsoft.com/aspnet/core/migration/webapi))。</span><span class="sxs-lookup"><span data-stu-id="5669e-210">Learn more about [migrating from ASP.NET Web API](https://docs.microsoft.com/aspnet/core/migration/webapi)).</span></span>

<span data-ttu-id="5669e-211">Web API の互換性 Shim を使用するのには、パッケージをプロジェクトに追加して、MVC を呼び出すことによって、規則を追加する必要があります`AddWebApiConventions`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="5669e-211">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="5669e-212">Shim によって提供される規則は、それらに適用された特定の属性を持っているアプリの一部にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-212">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="5669e-213">次の 4 つの属性は、コント ローラーで、規則、shim の規則によって変更する必要がありますコントロールに使用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-213">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="5669e-214">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="5669e-214">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="5669e-215">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="5669e-215">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="5669e-216">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="5669e-216">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="5669e-217">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="5669e-217">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="5669e-218">アクションの表記規則</span><span class="sxs-lookup"><span data-stu-id="5669e-218">Action Conventions</span></span>

<span data-ttu-id="5669e-219">`UseWebApiActionConventionsAttribute`名に基づいてアクションに HTTP メソッドをマップするために使用 (たとえば、`Get`にマップ`HttpGet`)。</span><span class="sxs-lookup"><span data-stu-id="5669e-219">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="5669e-220">属性のルーティングを使用しないアクションにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="5669e-220">It only applies to actions that do not use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="5669e-221">オーバーロード</span><span class="sxs-lookup"><span data-stu-id="5669e-221">Overloading</span></span>

<span data-ttu-id="5669e-222">`UseWebApiOverloadingAttribute`を適用するために使用、`WebApiOverloadingApplicationModelConvention`規則。</span><span class="sxs-lookup"><span data-stu-id="5669e-222">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="5669e-223">この規則の追加、`OverloadActionConstraint`アクションの選択のプロセスに制限されることは候補アクション要求がすべて非-省略可能なパラメーターを満たすものにします。</span><span class="sxs-lookup"><span data-stu-id="5669e-223">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="5669e-224">パラメーターの表記規則</span><span class="sxs-lookup"><span data-stu-id="5669e-224">Parameter Conventions</span></span>

<span data-ttu-id="5669e-225">`UseWebApiParameterConventionsAttribute`を適用するために使用、`WebApiParameterConventionsApplicationModelConvention`アクション規則。</span><span class="sxs-lookup"><span data-stu-id="5669e-225">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="5669e-226">この規則では、アクションのパラメーターとして使用される単純な型がバインドされている URI から既定では、複合型は、要求本文からバインドを指定します。</span><span class="sxs-lookup"><span data-stu-id="5669e-226">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="5669e-227">ルート</span><span class="sxs-lookup"><span data-stu-id="5669e-227">Routes</span></span>

<span data-ttu-id="5669e-228">`UseWebApiRoutesAttribute`コントロールかどうか、`WebApiApplicationModelConvention`コント ローラーの規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="5669e-228">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="5669e-229">サポートを追加するこの規則が使用される有効な場合、[領域](https://docs.microsoft.com/aspnet/core/mvc/controllers/areas)ルートにします。</span><span class="sxs-lookup"><span data-stu-id="5669e-229">When enabled, this convention is used to add support for [areas](https://docs.microsoft.com/aspnet/core/mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="5669e-230">互換パッケージが含まれています、規則のセットだけでなく、`System.Web.Http.ApiController`基底クラスを Web API によって提供されるものを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5669e-230">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="5669e-231">これにより、Web API および継承用に記述された、コント ローラーからその`ApiController`ASP.NET Core MVC での実行中に、設計どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="5669e-231">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="5669e-232">この基本コント ローラー クラスはすべての装飾、`UseWebApi*`上記の属性です。</span><span class="sxs-lookup"><span data-stu-id="5669e-232">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="5669e-233">`ApiController`プロパティ、メソッド、および Web API で検出されると互換性のある結果の型を公開します。</span><span class="sxs-lookup"><span data-stu-id="5669e-233">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="5669e-234">ApiExplorer を使用してアプリを文書化するには</span><span class="sxs-lookup"><span data-stu-id="5669e-234">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="5669e-235">アプリケーション モデルは、公開、 [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)プロパティ レベルに応じたアプリの構造をスキャンするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="5669e-235">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="5669e-236">これに使用できる[Swagger などのツールを使用して、Web Api のヘルプ ページを生成する](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger)です。</span><span class="sxs-lookup"><span data-stu-id="5669e-236">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="5669e-237">`ApiExplorer`プロパティが公開する`IsVisible`アプリのモデルのどの部分を公開するかを指定する設定できるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5669e-237">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="5669e-238">規則を使用してこの設定を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="5669e-238">You can configure this setting using a convention:</span></span>

<span data-ttu-id="5669e-239">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]</span><span class="sxs-lookup"><span data-stu-id="5669e-239">[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]</span></span>

<span data-ttu-id="5669e-240">このアプローチ (およびその他の規則が必要な場合) を使用して、有効にするにまたは、アプリ内で任意のレベルで API の可視性を無効にします。</span><span class="sxs-lookup"><span data-stu-id="5669e-240">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
