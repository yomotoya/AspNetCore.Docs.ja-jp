---
title: "ASP.NET Core のオプションのパターン"
author: guardrex
description: "ASP.NET Core アプリケーションに関連する設定のグループを表し、オプションのパターンを使用する方法を検出します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="35f70-103">ASP.NET Core のオプションのパターン</span><span class="sxs-lookup"><span data-stu-id="35f70-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="35f70-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35f70-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="35f70-105">オプションのパターンでは、オプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="35f70-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="35f70-106">構成設定については、個別のオプション クラスに分離機能によって、アプリは 2 つの重要なソフトウェア エンジニア リング原則に従って。</span><span class="sxs-lookup"><span data-stu-id="35f70-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="35f70-107">[インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/): 構成の設定に依存する機能 (クラス) が使用する構成設定のみに依存します。</span><span class="sxs-lookup"><span data-stu-id="35f70-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="35f70-108">[関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定は、依存または相互に結合がありません。</span><span class="sxs-lookup"><span data-stu-id="35f70-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="35f70-109">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) この記事では、次のサンプル アプリを使用しやすくします。</span><span class="sxs-lookup"><span data-stu-id="35f70-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="35f70-110">基本的なオプションの構成</span><span class="sxs-lookup"><span data-stu-id="35f70-110">Basic options configuration</span></span>

<span data-ttu-id="35f70-111">基本的なオプション構成については、例として説明&num;では 1、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-112">オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="35f70-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="35f70-113">次のクラスでは、 `MyOptions`、2 つのプロパティを持つ`Option1`と`Option2`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="35f70-114">既定値の設定オプションですが、次の例では、クラス コンス トラクターの既定値を設定する`Option1`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="35f70-115">`Option2`既定値が直接にプロパティを初期化することによって設定されて (*Models/MyOptions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="35f70-116">`MyOptions`クラスをサービス コンテナーに追加[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)構成にバインドされているとします。</span><span class="sxs-lookup"><span data-stu-id="35f70-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="35f70-117">次は、モデルの使用をページ[コンス トラクターの依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1)設定にアクセスする (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="35f70-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="35f70-118">サンプルの*される appsettings.json*ファイルの値を指定する`option1`と`option2`:</span><span class="sxs-lookup"><span data-stu-id="35f70-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="35f70-119">実行時に、アプリは、ページのモデルの`OnGet`メソッド オプション クラス値を示す文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="35f70-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="35f70-120">デリゲートで単純なオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="35f70-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="35f70-121">例として、デリゲートを使用して簡単なオプションの構成が示されている&num;2 に、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-122">デリゲートを使用すると、オプションの値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="35f70-122">Use a delegate to set options values.</span></span> <span data-ttu-id="35f70-123">サンプル アプリは、`MyOptionsWithDelegateConfig`クラス (*Models/MyOptionsWithDelegateConfig.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="35f70-124">次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="35f70-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="35f70-125">使用してバインディングを構成するデリゲートを使用して、 `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="35f70-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="35f70-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="35f70-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="35f70-127">複数の構成プロバイダーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="35f70-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="35f70-128">NuGet パッケージの構成プロバイダーを利用できます。</span><span class="sxs-lookup"><span data-stu-id="35f70-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="35f70-129">これらは、登録されていることを適用しています。</span><span class="sxs-lookup"><span data-stu-id="35f70-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="35f70-130">各呼び出し[構成&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure)を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="35f70-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="35f70-131">前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json*の値が、`Option1`と`Option2`構成済みのデリゲートによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="35f70-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="35f70-132">最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、 *wins*と構成値を設定します。</span><span class="sxs-lookup"><span data-stu-id="35f70-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="35f70-133">実行時に、アプリは、ページのモデルの`OnGet`メソッド オプション クラス値を示す文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="35f70-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="35f70-134">サブオプション構成</span><span class="sxs-lookup"><span data-stu-id="35f70-134">Suboptions configuration</span></span>

<span data-ttu-id="35f70-135">サブオプション構成が例として示されている&num;3 に、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-136">アプリでは、アプリ内の特定の機能グループ (クラス) に関連するオプション クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="35f70-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="35f70-137">構成値を必要とするアプリの一部のみを使用する構成値へのアクセスが必要です。</span><span class="sxs-lookup"><span data-stu-id="35f70-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="35f70-138">フォームの構成キー オプションの種類の各プロパティがバインドされているバインドするときのオプションの構成に、`property[:sub-property:]`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="35f70-139">たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="35f70-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="35f70-140">次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="35f70-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="35f70-141">バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="35f70-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="35f70-142">`GetSection`拡張メソッドが必要です、 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="35f70-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="35f70-143">アプリで使用する場合、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、パッケージが自動的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="35f70-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="35f70-144">サンプルの*される appsettings.json*ファイルを定義、`subsection`のキーを持つメンバー`suboption1`と`suboption2`:</span><span class="sxs-lookup"><span data-stu-id="35f70-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="35f70-145">`MySubOptions`クラス、プロパティを定義`SubOption1`と`SubOption2`サブ オプションの値を保持するために、(*Models/MySubOptions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="35f70-146">ページのモデルの`OnGet`メソッド サブ オプション値を使用して文字列を返します (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="35f70-147">アプリを実行すると、`OnGet`メソッド クラス値を表示するには、サブ オプション文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="35f70-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="35f70-148">直接ビュー インジェクションまたはビューのモデルによって提供されるオプション</span><span class="sxs-lookup"><span data-stu-id="35f70-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="35f70-149">直接ビュー インジェクションまたはビューのモデルによって提供されるオプションは、例として説明&num;4 で、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-150">ビュー モデルまたは挿入することで、オプションを指定することができます`IOptions<TOptions>`ビューに直接 (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="35f70-151">直接挿入には、挿入`IOptions<MyOptions>`で、`@inject`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="35f70-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="35f70-152">アプリを実行すると、レンダリングされるページのオプションの値のとおりです。</span><span class="sxs-lookup"><span data-stu-id="35f70-152">When the app is run, the option values are shown in the rendered page:</span></span>

![オプション値オプション 1: value1_from_json と・ オプション 2:-1 は、ビューに挿入して、モデルから読み込まれます。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="35f70-154">IOptionsSnapshot で構成データを再読み込み</span><span class="sxs-lookup"><span data-stu-id="35f70-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="35f70-155">構成データを再読み込みする`IOptionsSnapshot`の例に示す&num;に 5、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-156">*ASP.NET Core 1.1 以降が必要です。*</span><span class="sxs-lookup"><span data-stu-id="35f70-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="35f70-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1)最小限処理のオーバーヘッドを使用してオプションを再読み込みをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="35f70-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="35f70-158">ASP.NET Core 1.1 で`IOptionsSnapshot`のスナップショットは、 [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)され、更新プログラムに自動的にするたびに、モニターがトリガー データ ソースの変更に基づいて変更します。</span><span class="sxs-lookup"><span data-stu-id="35f70-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="35f70-159">ASP.NET Core 2.0 以降では、1 回の要求アクセスされ、要求の有効期間中にキャッシュされる場合にオプションが 1 回計算されます。</span><span class="sxs-lookup"><span data-stu-id="35f70-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="35f70-160">次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*される appsettings.json*変更 (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="35f70-161">サーバーに複数の要求がによって提供される定数の値を返す、*される appsettings.json*ファイルのファイルが変更され、構成を再読み込みされるまでです。</span><span class="sxs-lookup"><span data-stu-id="35f70-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="35f70-162">次の図は、初期`option1`と`option2`から読み込まれた値、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="35f70-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="35f70-163">値を変更、*される appsettings.json*ファイルの名前を`value1_from_json UPDATED`と`200`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="35f70-164">保存、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="35f70-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="35f70-165">ブラウザーを更新するオプションの値が更新されるを参照してください。</span><span class="sxs-lookup"><span data-stu-id="35f70-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="35f70-166">名前付き IConfigureNamedOptions とオプションのサポート</span><span class="sxs-lookup"><span data-stu-id="35f70-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="35f70-167">名前付きのオプション サポート[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1)例として説明されて&num;6 インチ、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="35f70-168">*ASP.NET Core 2.0 以降が必要です。*</span><span class="sxs-lookup"><span data-stu-id="35f70-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="35f70-169">*オプションを名前付き*サポートが名前付きのオプションの構成の間で区別するためにアプリを許可します。</span><span class="sxs-lookup"><span data-stu-id="35f70-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="35f70-170">サンプル アプリでは名前付きのオプションで宣言されて、 [ConfigureNamedOptions&lt;TOptions&gt;です。構成](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure)メソッド。</span><span class="sxs-lookup"><span data-stu-id="35f70-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="35f70-171">サンプル アプリを使用する名前付きのオプションにアクセスする[IOptionsSnapshot&lt;TOptions&gt;です。取得](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get)(*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="35f70-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="35f70-172">サンプル アプリを実行するには、名前付きのオプションが返されます。</span><span class="sxs-lookup"><span data-stu-id="35f70-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="35f70-173">`named_options_1`値がから提供される構成から読み込まれますが、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="35f70-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="35f70-174">`named_options_2`値は、によってを提供されます。</span><span class="sxs-lookup"><span data-stu-id="35f70-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="35f70-175">`named_options_2`で委任`ConfigureServices`の`Option1`します。</span><span class="sxs-lookup"><span data-stu-id="35f70-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="35f70-176">既定値`Option2`によって提供される、`MyOptions`クラスです。</span><span class="sxs-lookup"><span data-stu-id="35f70-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="35f70-177">名前付きのオプションのすべてのインスタンスを構成する、 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="35f70-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="35f70-178">次のコードは構成`Option1`すべてに共通の値を持つ構成インスタンスという名前です。</span><span class="sxs-lookup"><span data-stu-id="35f70-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="35f70-179">次のコードに手動で追加、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="35f70-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="35f70-180">コードを追加した後、サンプル アプリを実行するには、次の結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="35f70-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="35f70-181">ASP.NET Core 2.0 以降では、すべてのオプションは、名前付きインスタンス。</span><span class="sxs-lookup"><span data-stu-id="35f70-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="35f70-182">既存の`IConfigureOption`インスタンスはターゲットとして扱われます、 `Options.DefaultName` 、インスタンス`string.Empty`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="35f70-183">`IConfigureNamedOptions`実装も`IConfigureOptions`します。</span><span class="sxs-lookup"><span data-stu-id="35f70-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="35f70-184">既定の実装、 [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([参照ソース](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) 各を適切に使用するロジックを備えています。</span><span class="sxs-lookup"><span data-stu-id="35f70-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="35f70-185">`null`名前付きのオプションを使用して、特定の名前付きインスタンスではなく名前付きインスタンスのすべての対象 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)と[PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)この規則を使用)。</span><span class="sxs-lookup"><span data-stu-id="35f70-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="35f70-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="35f70-186">IPostConfigureOptions</span></span>

<span data-ttu-id="35f70-187">*ASP.NET Core 2.0 以降が必要です。*</span><span class="sxs-lookup"><span data-stu-id="35f70-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="35f70-188">設定と postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="35f70-189">Postconfiguration が実行される結局[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)構成が行われます。</span><span class="sxs-lookup"><span data-stu-id="35f70-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="35f70-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure)は名前付きのオプションの構成後に使用できます。</span><span class="sxs-lookup"><span data-stu-id="35f70-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="35f70-191">使用して[PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)名前付きインスタンスの構成の後にすべてを構成します。</span><span class="sxs-lookup"><span data-stu-id="35f70-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="35f70-192">オプションのファクトリ、監視、およびキャッシュ</span><span class="sxs-lookup"><span data-stu-id="35f70-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="35f70-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)通知を使用するときに`TOptions`インスタンスを変更します。</span><span class="sxs-lookup"><span data-stu-id="35f70-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="35f70-194">`IOptionsMonitor`再読み込みオプションをサポートする、変更通知、および`IPostConfigureOptions`です。</span><span class="sxs-lookup"><span data-stu-id="35f70-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="35f70-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1)は新規に作成するオプションのインスタンスに (ASP.NET Core 2.0 またはそれ以降) があります。</span><span class="sxs-lookup"><span data-stu-id="35f70-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="35f70-196">1 つが[作成](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="35f70-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="35f70-197">既定の実装は、すべての登録済み`IConfigureOptions`と`IPostConfigureOptions`すべて実行し、最初に構成、その後で、後の構成します。</span><span class="sxs-lookup"><span data-stu-id="35f70-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="35f70-198">間で区別するため`IConfigureNamedOptions`と`IConfigureOptions`のみ適切なインターフェイスを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="35f70-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="35f70-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 またはそれ以降) を使って`IOptionsMonitor`キャッシュに`TOptions`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="35f70-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="35f70-200">`IOptionsMonitorCache`値が再計算されるようにモニターでオプションのインスタンスを無効になります ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="35f70-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="35f70-201">値が手動でに導入されたも[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)です。</span><span class="sxs-lookup"><span data-stu-id="35f70-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="35f70-202">[クリア](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)要求時に、すべての名前付きインスタンスを再作成する必要がありますとメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="35f70-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="35f70-203">関連項目</span><span class="sxs-lookup"><span data-stu-id="35f70-203">See also</span></span>

* [<span data-ttu-id="35f70-204">構成</span><span class="sxs-lookup"><span data-stu-id="35f70-204">Configuration</span></span>](xref:fundamentals/configuration/index)
