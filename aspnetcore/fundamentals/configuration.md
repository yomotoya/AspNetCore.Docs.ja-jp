---
title: "ASP.NET Core の構成"
author: rick-anderson
description: "構成 API を使用して、複数のソースからの ASP.NET Core アプリケーションを構成する方法を説明します。"
keywords: "ASP.NET Core、構成では、JSON の構成"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="ffa39-104">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="ffa39-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="ffa39-105">[Rick Anderson](https://twitter.com/RickAndMSFT)、[マーク Michaelis](http://intellitect.com/author/mark-michaelis/)、 [Steve Smith](https://ardalis.com/)、 [Daniel Roth](https://github.com/danroth27)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ffa39-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ffa39-106">構成 API は、名前と値のペアの一覧で、アプリケーションを構成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="ffa39-107">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="ffa39-108">名前と値のペアは、複数レベルの階層化することができます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="ffa39-109">構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-109">There are configuration providers for:</span></span>

* <span data-ttu-id="ffa39-110">ファイル形式 (INI、JSON、および XML)</span><span class="sxs-lookup"><span data-stu-id="ffa39-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="ffa39-111">コマンドライン引数</span><span class="sxs-lookup"><span data-stu-id="ffa39-111">Command-line arguments</span></span>
* <span data-ttu-id="ffa39-112">環境変数</span><span class="sxs-lookup"><span data-stu-id="ffa39-112">Environment variables</span></span>
* <span data-ttu-id="ffa39-113">メモリ内の .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="ffa39-113">In-memory .NET objects</span></span>
* <span data-ttu-id="ffa39-114">暗号化されたユーザー ストア</span><span class="sxs-lookup"><span data-stu-id="ffa39-114">An encrypted user store</span></span>
* [<span data-ttu-id="ffa39-115">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ffa39-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="ffa39-116">カスタム プロバイダーをインストールまたは作成します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="ffa39-117">各構成値は、文字列のキーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="ffa39-118">カスタム設定を逆シリアル化する組み込みのバインディング サポートは[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)オブジェクト (シンプルな .NET クラスのプロパティを持つ)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="ffa39-119">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ffa39-119">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="simple-configuration"></a><span data-ttu-id="ffa39-120">単純な構成</span><span class="sxs-lookup"><span data-stu-id="ffa39-120">Simple configuration</span></span>

<span data-ttu-id="ffa39-121">次のコンソール アプリケーションは、JSON の構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="ffa39-122">アプリでは、読み取りし、次の構成設定を表示します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="ffa39-123">構成は、ノードが、コロンで区切られた名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="ffa39-124">値を取得するには、アクセス、`Configuration`インデクサーに対して、対応する項目のキー。</span><span class="sxs-lookup"><span data-stu-id="ffa39-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="ffa39-125">JSON 形式構成ソース内の配列を使用するには、コロンで区切られた文字列の一部として、配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="ffa39-126">次の例は、上記の最初の項目の名前を取得`wizards`配列。</span><span class="sxs-lookup"><span data-stu-id="ffa39-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="ffa39-127">組み込みに書き込まれる名前と値のペアで`Configuration`プロバイダーは**いない**永続化して、ただし、プロバイダーを作成できます、カスタム値を保存します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="ffa39-128">参照してください[カスタム構成プロバイダー](xref:fundamentals/configuration#custom-config-providers)です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="ffa39-129">上記のサンプルでは、構成のインデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="ffa39-130">外部アクセス構成を`Startup`を使用して、[オプション パターン](xref:fundamentals/configuration#options-config-objects)です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="ffa39-131">*オプション パターン*この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="ffa39-132">通常をさまざまな環境、開発、テスト、および運用環境などの別の構成設定があります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="ffa39-133">`CreateDefaultBuilder` ASP.NET Core 2.x アプリケーションでも拡張メソッド (またはを使用して`AddJsonFile`と`AddEnvironmentVariables`ASP.NET Core 1.x アプリ内で直接) JSON ファイルおよびシステム構成ソースを読み取るための構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="ffa39-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="ffa39-134">*appsettings.json*</span></span>
* <span data-ttu-id="ffa39-135">*appsettings です。\<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="ffa39-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="ffa39-136">環境変数</span><span class="sxs-lookup"><span data-stu-id="ffa39-136">environment variables</span></span>

<span data-ttu-id="ffa39-137">参照してください[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)パラメーターの説明。</span><span class="sxs-lookup"><span data-stu-id="ffa39-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="ffa39-138">`reloadOnChange`ASP.NET Core 1.1 以降のみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="ffa39-139">構成ソースは、指定された順序で読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="ffa39-140">上記のコードでは、環境変数が前回読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="ffa39-141">環境を使用して設定、構成値には、2 つの以前のプロバイダーで設定されていると交換してください。</span><span class="sxs-lookup"><span data-stu-id="ffa39-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="ffa39-142">環境が通常のいずれかに設定されている`Development`、 `Staging`、または`Production`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="ffa39-143">参照してください[複数の環境で作業](environments.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="ffa39-144">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="ffa39-144">Configuration considerations:</span></span>

* <span data-ttu-id="ffa39-145">`IOptionsSnapshot`変更されたとき、構成データを再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="ffa39-146">使用して`IOptionsSnapshot`構成データを再読み込みする必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="ffa39-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="ffa39-147">参照してください[IOptionsSnapshot](#ioptionssnapshot)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="ffa39-148">構成キーは、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="ffa39-149">ベスト プラクティスは、ローカルの環境は、何も展開された構成ファイルで設定をオーバーライドできるようにする、最後に、環境変数を指定するです。</span><span class="sxs-lookup"><span data-stu-id="ffa39-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="ffa39-150">**決して**プロバイダーの構成コードまたは構成ファイルをプレーン テキストでパスワードまたは他の機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="ffa39-151">環境をテストしたり、開発に実稼働のシークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="ffa39-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="ffa39-152">代わりに、リポジトリに誤ってコミットするため、プロジェクト ツリーの外部のシークレットを指定します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="ffa39-153">詳細については[複数の環境で作業](environments.md)および管理する[アプリ シークレットは、開発中の安全な保管](../security/app-secrets.md)です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="ffa39-154">場合`:`することはできません、システムで環境変数の使用を置き換えます`:`で`__`(二重のアンダー スコア)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="ffa39-155">オプションと構成オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-155">Using Options and configuration objects</span></span>

<span data-ttu-id="ffa39-156">オプションのパターンでは、カスタム オプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="ffa39-157">アプリ内で各機能の分離型のクラスを作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="ffa39-158">分離されたクラスに従ってください。</span><span class="sxs-lookup"><span data-stu-id="ffa39-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="ffa39-159">[インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/) : クラスが使用する構成設定のみに依存します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="ffa39-160">[関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が依存するまたは相互に結合します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="ffa39-161">オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ffa39-162">例:</span><span class="sxs-lookup"><span data-stu-id="ffa39-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

<span data-ttu-id="ffa39-163">次のコードでは、JSON の構成プロバイダーは有効です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="ffa39-164">`MyOptions`クラスをサービス コンテナーに追加および構成にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="ffa39-165">次[コント ローラー](../mvc/controllers/index.md)使用[コンス トラクター依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="ffa39-166">次の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="ffa39-167">`HomeController.Index`メソッドを返します。`option1 = value1_from_json, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="ffa39-168">一般的なアプリは、構成全体を 1 つのオプション ファイルにバインドされません。</span><span class="sxs-lookup"><span data-stu-id="ffa39-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="ffa39-169">後で使用する方法を示します`GetSection`セクションにバインドします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="ffa39-170">次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ffa39-171">使用してバインディングを構成するデリゲートを使用して、`MyOptions`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="ffa39-172">複数の構成プロバイダーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="ffa39-173">NuGet パッケージの構成プロバイダーを利用できます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="ffa39-174">登録されている順序で適用されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="ffa39-175">各呼び出し`Configure<TOptions>`を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="ffa39-176">前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json* --の値が、`Option1`構成済みのデリゲートによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="ffa39-177">"Wins"最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、(構成値を設定します)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="ffa39-178">上記のコードでは、`HomeController.Index`メソッドを返します。`option1 = value1_from_action, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="ffa39-179">フォームの構成キー オプションの種類の各プロパティがバインドされているオプションを構成にバインドすると、`property[:sub-property:]`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ffa39-180">たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="ffa39-181">この記事で後述サブプロパティ サンプルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="ffa39-182">次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ffa39-183">バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="ffa39-184">注: この拡張メソッドが必要です、 `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ffa39-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="ffa39-185">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="ffa39-186">`MySubOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ffa39-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ffa39-187">次の`Controller`:</span><span class="sxs-lookup"><span data-stu-id="ffa39-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="ffa39-188">`subOption1 = subvalue1_from_json, subOption2 = 200`返されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="ffa39-189">ビューのモデルでのオプションを指定したり挿入したりできます`IOptions<TOptions>`ビューに直接。</span><span class="sxs-lookup"><span data-stu-id="ffa39-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="ffa39-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="ffa39-190">IOptionsSnapshot</span></span>

<span data-ttu-id="ffa39-191">*ASP.NET Core 1.1 以上が必要です。*</span><span class="sxs-lookup"><span data-stu-id="ffa39-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="ffa39-192">`IOptionsSnapshot`構成ファイルが変更されたときに、構成データを再読み込みをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="ffa39-193">最小限のオーバーヘッドがあります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-193">It has minimal overhead.</span></span> <span data-ttu-id="ffa39-194">使用して`IOptionsSnapshot`で`reloadOnChange: true`、オプションにバインドされた`IConfiguration`および変更されたときに再読み込みします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="ffa39-195">次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*config.json*変更します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="ffa39-196">同じサーバーに要求が返される時刻*config.json*が**いない**を変更します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="ffa39-197">後の最初の要求*config.json*変更新しい時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="ffa39-198">次の図は、サーバーの出力を示しています。</span><span class="sxs-lookup"><span data-stu-id="ffa39-198">The following image shows the server output:</span></span>

![ブラウザーのイメージを示す"最終更新日: 2016 11 月 22 日 4時 43分 PM"](configuration/_static/first.png)

<span data-ttu-id="ffa39-200">ブラウザーを更新するメッセージの値または表示される時刻は変更されません (ときに*config.json*が変更されていない)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="ffa39-201">変更し、保存、 *config.json*し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![ブラウザーのイメージを示す"e: に最後に更新された 2016 11 月 22 日 4時 53分 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="ffa39-203">メモリ内のプロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="ffa39-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="ffa39-204">次の例では、メモリ内のプロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="ffa39-205">構成値は、文字列として返されますが、バインド、オブジェクトの作成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="ffa39-206">バインディングでは、POCO オブジェクトやオブジェクトも全体のグラフを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="ffa39-207">次の例は、バインドする方法を示しています。`MyWindow`および ASP.NET Core MVC アプリのオプションのパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="ffa39-208">カスタム クラスをバインド`ConfigureServices`ホストを作成するときに。</span><span class="sxs-lookup"><span data-stu-id="ffa39-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="ffa39-209">設定を表示、 `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="ffa39-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="ffa39-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="ffa39-210">GetValue</span></span>

<span data-ttu-id="ffa39-211">次の例は、 [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="ffa39-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="ffa39-212">ConfigurationBinder の`GetValue<T>`メソッドでは、既定値 (このサンプルでは 80) を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="ffa39-213">`GetValue<T>`単純なシナリオでは、セクション全体にバインドされません。</span><span class="sxs-lookup"><span data-stu-id="ffa39-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="ffa39-214">`GetValue<T>`スカラー値を取得`GetSection(key).Value`特定の種類に変換します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="ffa39-215">オブジェクト グラフへのバインド</span><span class="sxs-lookup"><span data-stu-id="ffa39-215">Binding to an object graph</span></span>

<span data-ttu-id="ffa39-216">クラス内の各オブジェクトを再帰的にバインドをできます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="ffa39-217">次の検討`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ffa39-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="ffa39-218">次の例にバインド、`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ffa39-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="ffa39-219">**ASP.NET Core 1.1**以上使用できると`Get<T>`、セクション全体と連携します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="ffa39-220">`Get<T>`使用するよりも多くのも、`Bind`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="ffa39-221">次のコードは、使用する方法を示しています。`Get<T>`と上のサンプル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="ffa39-222">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="ffa39-223">プログラムは、表示`Height 11`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="ffa39-224">次のコードは、単位に使用できる構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-224">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="ffa39-225">Entity Framework のカスタム プロバイダーの基本的なサンプル</span><span class="sxs-lookup"><span data-stu-id="ffa39-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="ffa39-226">このセクションでは、EF を使用してデータベースから名前/値ペアを表示する基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="ffa39-227">定義、`ConfigurationValue`データベースの構成値を格納するためのエンティティ。</span><span class="sxs-lookup"><span data-stu-id="ffa39-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="ffa39-228">追加、`ConfigurationContext`を保存し、構成された値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="ffa39-229">実装するクラスを作成する[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="ffa39-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="ffa39-230">継承することで、カスタム構成プロバイダーを作成する[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="ffa39-231">構成プロバイダーは、空であるときに、データベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="ffa39-232">サンプルを実行する、強調表示されているデータベースからの値 ("value_from_ef_1"および"value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="ffa39-233">追加することができます、`EFConfigSource`構成ソースを追加するための拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="ffa39-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="ffa39-234">次のコードは、ユーザー設定を使用する方法を示しています`EFConfigProvider`:。</span><span class="sxs-lookup"><span data-stu-id="ffa39-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="ffa39-235">サンプルを追加、カスタム注`EFConfigProvider`JSON プロバイダーの後にそのため、データベースから設定設定を上書きするから、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="ffa39-236">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ffa39-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="ffa39-237">次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="ffa39-238">コマンドライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ffa39-238">CommandLine configuration provider</span></span>

<span data-ttu-id="ffa39-239">[コマンドライン構成プロバイダー](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)実行時に構成のキー/値ペアのコマンドライン引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="ffa39-240">表示またはダウンロードするコマンドラインのサンプルの構成</span><span class="sxs-lookup"><span data-stu-id="ffa39-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="ffa39-241">プロバイダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="ffa39-242">基本構成</span><span class="sxs-lookup"><span data-stu-id="ffa39-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="ffa39-243">コマンドライン構成をアクティブに呼び出して、`AddCommandLine`拡張メソッドのインスタンスを[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="ffa39-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="ffa39-244">コードを実行するには、次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="ffa39-245">値を変更するコマンド ラインでキーと値のペアの引数を渡す`Profile:MachineName`と`App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="ffa39-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="ffa39-246">コンソール ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="ffa39-247">コマンドライン構成とその他の構成プロバイダーによって提供される構成を上書きするには、呼び出す`AddCommandLine`の最後に`ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ffa39-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ffa39-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ffa39-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ffa39-249">ASP.NET Core 2.x アプリケーションの一般的な便利な静的メソッドを使用して`CreateDefaultBuilder`ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="ffa39-250">`CreateDefaultBuilder`オプションの構成を読み込む*される appsettings.json*、 *appsettings {。環境} .json*、[ユーザー シークレット](xref:security/app-secrets)(で、`Development`環境)、環境変数とコマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="ffa39-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="ffa39-251">コマンドライン構成プロバイダーは、最後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="ffa39-252">最後に、プロバイダーを呼び出すことにより、構成セットを他の構成プロバイダーをオーバーライドする実行時に渡されるコマンドライン引数が以前に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="ffa39-253">注意してください*appsettings*ファイル`reloadOnChange`を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="ffa39-254">構成値が一致する場合にコマンドライン引数がオーバーライドされる、 *appsettings*アプリが開始した後にファイルが変更されました。</span><span class="sxs-lookup"><span data-stu-id="ffa39-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="ffa39-255">使用する代わりに、`CreateDefaultBuilder`メソッドを使用してホストを作成する[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)と構成を手動で構築および[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ASP.NET Core ではサポートされて 2.x です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="ffa39-256">詳細については、ASP.NET Core 1.x タブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ffa39-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ffa39-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ffa39-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ffa39-258">作成、 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)を呼び出すと、`AddCommandLine`コマンドライン構成プロバイダーを使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="ffa39-259">最後に、プロバイダーを呼び出すことにより、構成セットを他の構成プロバイダーをオーバーライドする実行時に渡されるコマンドライン引数が以前に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="ffa39-260">構成の適用[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)で、`UseConfiguration`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ffa39-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="ffa39-261">引数</span><span class="sxs-lookup"><span data-stu-id="ffa39-261">Arguments</span></span>

<span data-ttu-id="ffa39-262">コマンドラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="ffa39-263">引数の形式</span><span class="sxs-lookup"><span data-stu-id="ffa39-263">Argument format</span></span>                                                     | <span data-ttu-id="ffa39-264">例</span><span class="sxs-lookup"><span data-stu-id="ffa39-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="ffa39-265">1 つの引数: キー値のペアは等号で区切られた (`=`)</span><span class="sxs-lookup"><span data-stu-id="ffa39-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="ffa39-266">2 つの引数の順序: キー値の組み合わせをスペースで区切って</span><span class="sxs-lookup"><span data-stu-id="ffa39-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="ffa39-267">**1 つの引数**</span><span class="sxs-lookup"><span data-stu-id="ffa39-267">**Single argument**</span></span>

<span data-ttu-id="ffa39-268">値は、等号を従う必要があります (`=`)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="ffa39-269">値を null にすることができます (たとえば、 `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="ffa39-270">キーは、プレフィックスがあります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-270">The key may have a prefix.</span></span>

| <span data-ttu-id="ffa39-271">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="ffa39-271">Key prefix</span></span>               | <span data-ttu-id="ffa39-272">例</span><span class="sxs-lookup"><span data-stu-id="ffa39-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="ffa39-273">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="ffa39-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="ffa39-274">単一のダッシュ (`-`) & #8224 です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="ffa39-275">2 個のダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="ffa39-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="ffa39-276">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="ffa39-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="ffa39-277">& #8224 です。単一 dash プレフィックスを持つキー (`-`) で指定する必要があります[マッピングを切り替える](#switch-mappings)、以下に説明します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="ffa39-278">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="ffa39-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="ffa39-279">メモ: 場合`-key1`に存在しない、[マッピングを切り替える](#switch-mappings)構成プロバイダーに指定された、`FormatException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="ffa39-280">**2 つの引数の順序**</span><span class="sxs-lookup"><span data-stu-id="ffa39-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="ffa39-281">値は null にすることはできません、スペースで区切られたキーに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="ffa39-282">キーには、プレフィックスが必要です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-282">The key must have a prefix.</span></span>

| <span data-ttu-id="ffa39-283">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="ffa39-283">Key prefix</span></span>               | <span data-ttu-id="ffa39-284">例</span><span class="sxs-lookup"><span data-stu-id="ffa39-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="ffa39-285">単一のダッシュ (`-`) & #8224 です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="ffa39-286">2 個のダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="ffa39-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="ffa39-287">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="ffa39-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="ffa39-288">& #8224 です。単一 dash プレフィックスを持つキー (`-`) で指定する必要があります[マッピングを切り替える](#switch-mappings)、以下に説明します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="ffa39-289">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="ffa39-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="ffa39-290">メモ: 場合`-key1`に存在しない、[マッピングを切り替える](#switch-mappings)構成プロバイダーに指定された、`FormatException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="ffa39-291">重複するキー</span><span class="sxs-lookup"><span data-stu-id="ffa39-291">Duplicate keys</span></span>

<span data-ttu-id="ffa39-292">重複するキーを指定しない場合は、最後のキー/値ペアが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="ffa39-293">スイッチのマッピング</span><span class="sxs-lookup"><span data-stu-id="ffa39-293">Switch mappings</span></span>

<span data-ttu-id="ffa39-294">構成を手動で構築するとき`ConfigurationBuilder`、必要に応じて、スイッチのマッピングのディクショナリを指定することができます、`AddCommandLine`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ffa39-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="ffa39-295">スイッチのマッピングを使用すると、キー名の交換ロジックを提供できます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="ffa39-296">スイッチのマッピングのディクショナリを使用すると、コマンドライン引数によって提供されるキーに一致するキーのディクショナリがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="ffa39-297">コマンド ライン キーがディクショナリ内で見つかった場合、ディクショナリの値 (キーの交換) は、構成設定に渡されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="ffa39-298">スイッチ マッピングは任意のコマンド ライン キー ダッシュを 1 つ付いて (`-`)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="ffa39-299">マッピングのディクショナリ キーの規則が切り替わります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="ffa39-300">スイッチがダッシュで開始する必要があります (`-`) または 2 つの破線 (`--`)。</span><span class="sxs-lookup"><span data-stu-id="ffa39-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="ffa39-301">スイッチのマッピングのディクショナリでは、重複するキーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="ffa39-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="ffa39-302">次の例で、`GetSwitchMappings`メソッドにより、1 つのダッシュを使用する、コマンドライン引数 (`-`) プレフィックスをキーし、サブキーの先頭のプレフィックスを回避します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="ffa39-303">コマンドライン引数を指定せず、辞書が渡された`AddInMemoryCollection`構成値を設定します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="ffa39-304">次のコマンドを使用して、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="ffa39-305">コンソール ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="ffa39-306">構成設定を渡すには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="ffa39-307">コンソール ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="ffa39-308">スイッチのマッピングのディクショナリを作成した後、次の表に示すようにデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ffa39-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="ffa39-309">キー</span><span class="sxs-lookup"><span data-stu-id="ffa39-309">Key</span></span>            | <span data-ttu-id="ffa39-310">値</span><span class="sxs-lookup"><span data-stu-id="ffa39-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="ffa39-311">ディクショナリを使用して、キーの切り替えを示すためには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="ffa39-312">コマンド ライン キーが交換されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-312">The command-line keys are swapped.</span></span> <span data-ttu-id="ffa39-313">コンソール ウィンドウに表示の構成値`Profile:MachineName`と`App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="ffa39-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="ffa39-314">Web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="ffa39-314">The web.config file</span></span>

<span data-ttu-id="ffa39-315">A *web.config* IIS または IIS Express でアプリをホストしている場合は、ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="ffa39-316">*web.config*アプリを起動するように IIS で AspNetCoreModule をオンにします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="ffa39-317">設定*web.config*アプリを起動し、その他の IIS の設定とモジュールを構成するために IIS の AspNetCoreModule を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ffa39-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="ffa39-318">Visual Studio を使用して削除すると*web.config*、Visual Studio は、新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="ffa39-319">補足メモ</span><span class="sxs-lookup"><span data-stu-id="ffa39-319">Additional notes</span></span>

* <span data-ttu-id="ffa39-320">依存関係の挿入 (DI) が設定されていないまで後`ConfigureServices`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="ffa39-321">構成システムでは DI に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ffa39-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="ffa39-322">`IConfiguration`2 つの特殊化があります。</span><span class="sxs-lookup"><span data-stu-id="ffa39-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="ffa39-323">`IConfigurationRoot`ルート ノードに使用します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="ffa39-324">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="ffa39-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="ffa39-325">`IConfigurationSection`構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="ffa39-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="ffa39-326">`GetSection`と`GetChildren`を返し、`IConfigurationSection`です。</span><span class="sxs-lookup"><span data-stu-id="ffa39-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ffa39-327">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ffa39-327">Additional resources</span></span>

* [<span data-ttu-id="ffa39-328">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="ffa39-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="ffa39-329">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="ffa39-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="ffa39-330">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="ffa39-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="ffa39-331">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="ffa39-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="ffa39-332">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ffa39-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
