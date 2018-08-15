---
title: ASP.NET Core のオプション パターン
author: guardrex
description: ASP.NET Core アプリの関連のある設定のグループを表すオプション パターンを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: ef6b0117b88c4c79771f0280267bd99993028ac8
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655421"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="6aac7-103">ASP.NET Core のオプション パターン</span><span class="sxs-lookup"><span data-stu-id="6aac7-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="6aac7-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6aac7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6aac7-105">オプション パターンではクラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="6aac7-106">構成設定が機能別に個々のクラスに分離されるとき、アプリは次の 2 つの重要なソフトウェア エンジニアリング原則に従います。</span><span class="sxs-lookup"><span data-stu-id="6aac7-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="6aac7-107">[ISP (Interface Segregation Principle/インターフェイス分離の原則)](http://deviq.com/interface-segregation-principle/): それが使用する構成設定にのみ依存する機能 (クラス)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="6aac7-108">[懸念事項の分離 (Separation of Concerns)](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が互いに非依存。</span><span class="sxs-lookup"><span data-stu-id="6aac7-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="6aac7-109">[サンプル コードをご覧ください。ダウンロードも可能です](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) この記事はサンプル アプリを用意すると進めやすくなります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="6aac7-110">基本的なオプション構成</span><span class="sxs-lookup"><span data-stu-id="6aac7-110">Basic options configuration</span></span>

<span data-ttu-id="6aac7-111">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;1 は基本的なオプション構成です。</span><span class="sxs-lookup"><span data-stu-id="6aac7-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-112">オプション クラスはパラメーターのないパブリック コンストラクターを持った非抽象でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="6aac7-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="6aac7-113">次のクラス `MyOptions` には `Option1` と `Option2` という 2 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="6aac7-114">既定値の設定は任意ですが、次の例のクラス コンストラクターは既定値 `Option1` を設定します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="6aac7-115">`Option2` には、プロパティを直接初期化することで既定値が設定されます (*Models/MyOptions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="6aac7-116">`MyOptions` クラスは [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) でサービス コンテナーに追加され、次の構成にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="6aac7-117">次のページ モデルは[コンストラクターの依存関係挿入](xref:mvc/controllers/dependency-injection)と [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) を利用し、設定にアクセスします (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-117">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="6aac7-118">サンプルの *appsettings.json* ファイルは `option1` と `option2` の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="6aac7-119">アプリの実行時、ページ モデルの `OnGet` メソッドは文字列を返し、オプション クラス値を表示します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="6aac7-120">カスタム [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) を使用して設定ファイルからオプションの構成を読み込むときには、基本パスが正しく設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="6aac7-121">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を使用して、設定ファイルからオプションの構成を読み込むときには、基本パスを明示的に設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6aac7-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="6aac7-122">デリゲートで単純なオプションを構成する</span><span class="sxs-lookup"><span data-stu-id="6aac7-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="6aac7-123">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;2 では、デリゲートで単純なオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-124">デリゲートを使用し、オプション値を設定します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-124">Use a delegate to set options values.</span></span> <span data-ttu-id="6aac7-125">サンプル アプリでは、`MyOptionsWithDelegateConfig` クラス (*Models/MyOptionsWithDelegateConfig.cs*) を使用しています。</span><span class="sxs-lookup"><span data-stu-id="6aac7-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="6aac7-126">次のコードでは、2 番目の `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="6aac7-127">デリゲートを利用し、`MyOptionsWithDelegateConfig` とのバインディングを構成します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="6aac7-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6aac7-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="6aac7-129">複数の構成プロバイダーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="6aac7-130">構成プロバイダーは NuGet パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="6aac7-131">登録順序で適用されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="6aac7-132">[Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) を呼び出すたびに `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="6aac7-133">先の例では、値 `Option1` と `Option2` が両方とも *appsettings.json* で指定されていますが、構成されているデリゲートにより値 `Option1` と `Option2` がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="6aac7-134">複数の構成サービスが有効になっているとき、最後に指定された構成ソースが*優先*され、それにより構成値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="6aac7-135">アプリの実行時、ページ モデルの `OnGet` メソッドは文字列を返し、オプション クラス値を表示します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="6aac7-136">サブオプション構成</span><span class="sxs-lookup"><span data-stu-id="6aac7-136">Suboptions configuration</span></span>

<span data-ttu-id="6aac7-137">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;3 はサブオプション構成です。</span><span class="sxs-lookup"><span data-stu-id="6aac7-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-138">アプリでは、アプリの特定の機能グループ (クラス) に関連するオプション クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="6aac7-139">構成値を必要とするアプリの各パーツには、そのパーツが使用する構成値へのアクセスのみを与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="6aac7-140">オプションを構成にバインドするとき、オプション タイプの各プロパティはフォーム `property[:sub-property:]` の構成キーにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="6aac7-141">たとえば、`MyOptions.Option1` プロパティはキー `Option1` にバインドされます。このキーは *appsettings.json* の `option1` プロパティから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="6aac7-142">次のコードでは、3 番目の `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="6aac7-143">`MySubOptions` を *appsettings.json* ファイルのセクション `subsection` にバインドします。</span><span class="sxs-lookup"><span data-stu-id="6aac7-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="6aac7-144">拡張メソッド `GetSection` は、[Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet パッケージを必要とします。</span><span class="sxs-lookup"><span data-stu-id="6aac7-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="6aac7-145">アプリで [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) を使用している場合、このパッケージは自動的に含まれます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="6aac7-146">サンプルの *appsettings.json* ファイルは、`suboption1` と `suboption2` のキーで `subsection` メンバーを定義します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="6aac7-147">`MySubOptions` クラスは、`SubOption1` プロパティと `SubOption2` プロパティを定義し、オプションの値を保持します (*Models/MySubOptions.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="6aac7-148">ページ モデルの `OnGet` メソッドは、オプションの値を含む文字列を返します (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-148">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="6aac7-149">アプリの実行時、`OnGet` メソッドは文字列を返し、サブオプション クラス値を表示します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="6aac7-150">ビュー モデルまたは直接的なビュー挿入で与えられるオプション</span><span class="sxs-lookup"><span data-stu-id="6aac7-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="6aac7-151">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;4 は、ビュー モデルまたは直接的なビュー挿入で与えられるオプションです。</span><span class="sxs-lookup"><span data-stu-id="6aac7-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-152">オプションはビュー モデルで提供するか、ビューに `IOptions<TOptions>` を直接挿入することで提供できます (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="6aac7-153">直接挿入の場合、`@inject` ディレクティブで `IOptions<MyOptions>` を挿入します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="6aac7-154">アプリの実行時、レンダリングされたページにオプションの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-154">When the app is run, the options values are shown in the rendered page:</span></span>

![オプション値の Option1: value1_from_json と Option2: -1 はモデルから読み込まれるか、ビューへの挿入によって読み込まれます。](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="6aac7-156">IOptionsSnapshot で構成データを再読み込みする</span><span class="sxs-lookup"><span data-stu-id="6aac7-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="6aac7-157">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;5 では、`IOptionsSnapshot` で構成データを再読み込みする方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) では、最小の処理オーバーヘッドでオプションを再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6aac7-159">オプションは、要求の有効期間中にアクセスされ、キャッシュされたとき、要求につき 1 回計算されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-159">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6aac7-160">`IOptionsSnapshot` は [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) のスナップショットであり、データ ソースの変更に基づいてモニターが変更をトリガーするたびに自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-160">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="6aac7-161">次の例では、*appsettings.json* の変更後、新しい `IOptionsSnapshot` が作成されます (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-161">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="6aac7-162">サーバーに複数の要求が届くと、ファイルが変更され、構成が再読み込みされるまで、*appsettings.json* ファイルによって提供される定数値が返されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-162">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="6aac7-163">次のイメージでは、初期値の `option1` と `option2` が *appsettings.json* ファイルから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-163">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="6aac7-164">*appsettings.json* ファイルの値を `value1_from_json UPDATED` と `200` に変更します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-164">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="6aac7-165">*appsettings.json* ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-165">Save the *appsettings.json* file.</span></span> <span data-ttu-id="6aac7-166">ブラウザーを更新し、オプション値が更新されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-166">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="6aac7-167">IConfigureNamedOptions による名前付きオプションのサポート</span><span class="sxs-lookup"><span data-stu-id="6aac7-167">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="6aac7-168">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;6 は、[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) による名前付きオプションのサポートです。</span><span class="sxs-lookup"><span data-stu-id="6aac7-168">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6aac7-169">*名前付きオプション*をサポートすることで、アプリでは名前付きオプション構成が区別されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="6aac7-170">サンプル アプリでは、名前付きオプションは [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) で宣言されます。このオプションは、拡張メソッド [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="6aac7-171">サンプル アプリは [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) で名前付きオプションにアクセスします (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="6aac7-172">サンプル アプリを実行すると、名前付きオプションが返されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="6aac7-173">`named_options_1` 値が構成から与えられます。これは *appsettings.json* ファイルから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="6aac7-174">`named_options_2` 値は次により提供されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="6aac7-175">`Option1` の `ConfigureServices` の `named_options_2` デリゲート。</span><span class="sxs-lookup"><span data-stu-id="6aac7-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="6aac7-176">`MyOptions` クラスによって提供される `Option2` の既定値。</span><span class="sxs-lookup"><span data-stu-id="6aac7-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="6aac7-177">名前付きオプション インスタンスはすべて、[OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) メソッドで構成します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="6aac7-178">次のコードでは、すべての名前付き構成インスタンスの `Option1` が共通値で構成されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="6aac7-179">`Configure` メソッドに次のコードを手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="6aac7-180">コードを追加した後、サンプル アプリを実行すると、次の結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="6aac7-181">すべてのオプションが名前付きインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="6aac7-181">All options are named instances.</span></span> <span data-ttu-id="6aac7-182">既存の `IConfigureOption` インスタンスは、`string.Empty` である、`Options.DefaultName` インスタンスを対象とするものとして処理されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="6aac7-183">`IConfigureNamedOptions` はまた、`IConfigureOptions` を実装します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="6aac7-184">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([参照元](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) の既定の実装には、それぞれを適切に使用するためのロジックが与えられます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="6aac7-185">名前付きオプション `null` は、特定の名前付きオプションの代わりにすべての名前付きインスタンスを対象にするときに使用されます ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) と [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) ではこの規則が使用されます)。</span><span class="sxs-lookup"><span data-stu-id="6aac7-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="6aac7-186">オプションの検証</span><span class="sxs-lookup"><span data-stu-id="6aac7-186">Options validation</span></span>

<span data-ttu-id="6aac7-187">オプションが構成されている場合は、オプションの検証を使用してオプションを検証することができます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-187">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="6aac7-188">オプションが有効なら `true` を、無効なら `false` を返す検証メソッドと共に、`Validate` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-188">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="6aac7-189">前の例では、名前付きのオプションのインスタンスを `optionalOptionsName` に設定しました。</span><span class="sxs-lookup"><span data-stu-id="6aac7-189">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="6aac7-190">既定のオプションのインスタンスは `Options.DefaultName` です。</span><span class="sxs-lookup"><span data-stu-id="6aac7-190">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="6aac7-191">オプションのインスタンスが作成されると、検証が実行されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-191">Validation runs when the options instance is created.</span></span> <span data-ttu-id="6aac7-192">オプションのインスタンスが最初にアクセスされる際は、検証に合格することが保証されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-192">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6aac7-193">オプションの検証は、最初に構成されて検証された後のオプションの変更を防ぐことはできません。</span><span class="sxs-lookup"><span data-stu-id="6aac7-193">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="6aac7-194">`Validate` メソッドは、`Func<TOptions, bool>` を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-194">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="6aac7-195">検証を完全にカスタマイズするには、`IValidateOptions<TOptions>` を実装します。これにより、次が可能になります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-195">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="6aac7-196">複数のオプションの種類の検証: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="6aac7-196">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="6aac7-197">別のオプションの種類に基づく検証: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="6aac7-197">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="6aac7-198">`IValidateOptions` は次を検証します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-198">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="6aac7-199">特定の名前付きのオプションのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="6aac7-199">A specific named options instance.</span></span>
* <span data-ttu-id="6aac7-200">`name` が `null` の場合はすべてのオプション。</span><span class="sxs-lookup"><span data-stu-id="6aac7-200">All options when `name` is `null`.</span></span>

<span data-ttu-id="6aac7-201">インターフェイスの実装から `ValidateOptionsResult` を返します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-201">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="6aac7-202">今後のリリースでは、Eager 検証 (スタートアップ時のフェイル ファスト) およびデータ注釈ベースの検証が予定されています。</span><span class="sxs-lookup"><span data-stu-id="6aac7-202">Eager validation (fail fast at startup) and data annotation-based validation are scheduled for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="6aac7-203">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="6aac7-203">IPostConfigureOptions</span></span>

<span data-ttu-id="6aac7-204">[IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) でポスト構成を設定します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-204">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="6aac7-205">ポスト構成は、すべての [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 構成の後に実行されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-205">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="6aac7-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) は、名前付きオプションのポスト構成に利用できます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="6aac7-207">すべての名前付き構成インスタンスをポスト構成するには、[PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) を使用します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-207">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="6aac7-208">オプション ファクトリ、監視、キャッシュ</span><span class="sxs-lookup"><span data-stu-id="6aac7-208">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="6aac7-209">`TOptions` インスタンスが変わるとき、[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) が通知に使用されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-209">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="6aac7-210">`IOptionsMonitor` は、再読み込み可能なオプション、変更通知、`IPostConfigureOptions` に対応しています。</span><span class="sxs-lookup"><span data-stu-id="6aac7-210">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6aac7-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) は、新しいオプション インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="6aac7-212">[Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) メソッドが 1 つ含まれています。</span><span class="sxs-lookup"><span data-stu-id="6aac7-212">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="6aac7-213">既定の実装では、登録されている `IConfigureOptions` と `IPostConfigureOptions` がすべて受け取られ、先にすべての構成を実行し、その後、ポスト構成を実行します。</span><span class="sxs-lookup"><span data-stu-id="6aac7-213">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="6aac7-214">`IConfigureNamedOptions` と `IConfigureOptions` が区別され、適切なインターフェイスのみが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-214">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="6aac7-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) は `IOptionsMonitor` によって `TOptions` インスタンスのキャッシュに使用されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="6aac7-216">`IOptionsMonitorCache` は、値が再計算されるよう、モニターのオプション インスタンスを無効にします ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="6aac7-216">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="6aac7-217">[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd) を利用し、手動で値を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-217">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="6aac7-218">[Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) メソッドは、すべての名前付きインスタンスをオンデマンドで再作成するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-218">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="6aac7-219">スタートアップ時にオプションにアクセスする</span><span class="sxs-lookup"><span data-stu-id="6aac7-219">Accessing options during startup</span></span>

<span data-ttu-id="6aac7-220">サービスは `Configure` メソッドの実行前に構築されるため、`IOptions` は `Startup.Configure` で使用できます。</span><span class="sxs-lookup"><span data-stu-id="6aac7-220">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="6aac7-221">`IOptions` は `Startup.ConfigureServices` では使用できません。</span><span class="sxs-lookup"><span data-stu-id="6aac7-221">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6aac7-222">サービスの登録順序が原因で、オプションの状態が一貫しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="6aac7-222">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6aac7-223">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6aac7-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
