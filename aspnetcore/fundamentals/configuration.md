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
ms.openlocfilehash: dae7ac6e377d2c17bc8f86e5b6da98107366cc73
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="82523-104">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="82523-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="82523-105">[Rick Anderson](https://twitter.com/RickAndMSFT)、[マーク Michaelis](http://intellitect.com/author/mark-michaelis/)、 [Steve Smith](http://ardalis.com)、および[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="82523-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="82523-106">構成 API は、名前と値のペアの一覧で、アプリケーションを構成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="82523-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="82523-107">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="82523-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="82523-108">名前と値のペアは、複数レベルの階層化することができます。</span><span class="sxs-lookup"><span data-stu-id="82523-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="82523-109">構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="82523-109">There are configuration providers for:</span></span>

* <span data-ttu-id="82523-110">ファイル形式 (INI、JSON、および XML)</span><span class="sxs-lookup"><span data-stu-id="82523-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="82523-111">コマンドライン引数</span><span class="sxs-lookup"><span data-stu-id="82523-111">Command-line arguments</span></span>
* <span data-ttu-id="82523-112">環境変数</span><span class="sxs-lookup"><span data-stu-id="82523-112">Environment variables</span></span>
* <span data-ttu-id="82523-113">メモリ内の .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="82523-113">In-memory .NET objects</span></span>
* <span data-ttu-id="82523-114">暗号化されたユーザー ストア</span><span class="sxs-lookup"><span data-stu-id="82523-114">An encrypted user store</span></span>
* [<span data-ttu-id="82523-115">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="82523-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="82523-116">カスタム プロバイダーをインストールまたは作成します。</span><span class="sxs-lookup"><span data-stu-id="82523-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="82523-117">各構成値は、文字列のキーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="82523-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="82523-118">カスタム設定を逆シリアル化する組み込みのバインディング サポートは[POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)オブジェクト (シンプルな .NET クラスのプロパティを持つ)。</span><span class="sxs-lookup"><span data-stu-id="82523-118">There's built-in binding support to deserialize settings into a custom [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="82523-119">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="82523-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="82523-120">単純な構成</span><span class="sxs-lookup"><span data-stu-id="82523-120">Simple configuration</span></span>

<span data-ttu-id="82523-121">次のコンソール アプリケーションは、JSON の構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="82523-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="82523-123">アプリでは、読み取りし、次の構成設定を表示します。</span><span class="sxs-lookup"><span data-stu-id="82523-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="82523-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="82523-125">構成は、ノードが、コロンで区切られた名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="82523-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="82523-126">値を取得するには、アクセス、`Configuration`インデクサーに対して、対応する項目のキー。</span><span class="sxs-lookup"><span data-stu-id="82523-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="82523-127">JSON 形式構成ソース内の配列を使用するには、コロンで区切られた文字列の一部として、配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="82523-128">次の例は、上記の最初の項目の名前を取得`wizards`配列。</span><span class="sxs-lookup"><span data-stu-id="82523-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="82523-129">組み込みに書き込まれる名前と値のペアで`Configuration`プロバイダーは**いない**永続化して、ただし、プロバイダーを作成できます、カスタム値を保存します。</span><span class="sxs-lookup"><span data-stu-id="82523-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="82523-130">参照してください[カスタム構成プロバイダー](xref:fundamentals/configuration#custom-config-providers)です。</span><span class="sxs-lookup"><span data-stu-id="82523-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="82523-131">上記のサンプルでは、構成のインデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="82523-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="82523-132">外部アクセス構成を`Startup`を使用して、[オプション パターン](xref:fundamentals/configuration#options-config-objects)です。</span><span class="sxs-lookup"><span data-stu-id="82523-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="82523-133">*オプション パターン*この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="82523-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="82523-134">通常をさまざまな環境、開発、テストおよび運用環境などの別の構成設定があります。</span><span class="sxs-lookup"><span data-stu-id="82523-134">It's typical to have different configuration settings for different environments, for example, development, test and production.</span></span> <span data-ttu-id="82523-135">次の強調表示されたコードでは、3 つのソースに 2 つの構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="82523-135">The following highlighted code adds two configuration providers to three sources:</span></span>

1. <span data-ttu-id="82523-136">JSON プロバイダー、読み取り*される appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="82523-136">JSON provider, reading *appsettings.json*</span></span>
2. <span data-ttu-id="82523-137">JSON プロバイダー、読み取り*appsettings\< 。EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="82523-137">JSON provider, reading *appsettings.\<EnvironmentName>.json*</span></span>
3. <span data-ttu-id="82523-138">環境変数プロバイダー</span><span class="sxs-lookup"><span data-stu-id="82523-138">Environment variables provider</span></span>

<span data-ttu-id="82523-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span><span class="sxs-lookup"><span data-stu-id="82523-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span></span>

<span data-ttu-id="82523-140">参照してください[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)パラメーターの説明。</span><span class="sxs-lookup"><span data-stu-id="82523-140">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="82523-141">`reloadOnChange`ASP.NET Core 1.1 以降のみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="82523-141">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="82523-142">構成ソースは、指定された順序で読み取られます。</span><span class="sxs-lookup"><span data-stu-id="82523-142">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="82523-143">上記のコードでは、環境変数が前回読み取られます。</span><span class="sxs-lookup"><span data-stu-id="82523-143">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="82523-144">環境を使用して設定、構成値には、2 つの以前のプロバイダーで設定されていると交換してください。</span><span class="sxs-lookup"><span data-stu-id="82523-144">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="82523-145">環境が通常のいずれかに設定されている`Development`、 `Staging`、または`Production`です。</span><span class="sxs-lookup"><span data-stu-id="82523-145">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="82523-146">参照してください[複数の環境で作業](environments.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="82523-146">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="82523-147">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="82523-147">Configuration considerations:</span></span>

* <span data-ttu-id="82523-148">`IOptionsSnapshot`変更されたとき、構成データを再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="82523-148">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="82523-149">使用して`IOptionsSnapshot`構成データを再読み込みする必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="82523-149">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="82523-150">参照してください[IOptionsSnapshot](#ioptionssnapshot)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="82523-150">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="82523-151">構成キーは、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="82523-151">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="82523-152">ベスト プラクティスは、ローカルの環境は、何も展開された構成ファイルで設定をオーバーライドできるようにする、最後に、環境変数を指定するです。</span><span class="sxs-lookup"><span data-stu-id="82523-152">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="82523-153">**決して**プロバイダーの構成コードまたは構成ファイルをプレーン テキストでパスワードまたは他の機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="82523-153">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="82523-154">環境をテストしたり、開発に実稼働のシークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="82523-154">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="82523-155">代わりに、リポジトリに誤ってコミットするため、プロジェクト ツリーの外部のシークレットを指定します。</span><span class="sxs-lookup"><span data-stu-id="82523-155">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="82523-156">詳細については[複数の環境で作業](environments.md)および管理する[アプリ シークレットは、開発中の安全な保管](../security/app-secrets.md)です。</span><span class="sxs-lookup"><span data-stu-id="82523-156">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="82523-157">場合`:`することはできません、システムで環境変数の使用を置き換えます`:`で`__`(二重のアンダー スコア)。</span><span class="sxs-lookup"><span data-stu-id="82523-157">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="82523-158">オプションと構成オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-158">Using Options and configuration objects</span></span>

<span data-ttu-id="82523-159">オプションのパターンでは、カスタム オプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="82523-159">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="82523-160">アプリ内で各機能の分離型のクラスを作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="82523-160">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="82523-161">分離されたクラスに従ってください。</span><span class="sxs-lookup"><span data-stu-id="82523-161">Decoupled classes follow:</span></span>

* <span data-ttu-id="82523-162">[インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/) : クラスが使用する構成設定のみに依存します。</span><span class="sxs-lookup"><span data-stu-id="82523-162">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="82523-163">[関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が依存するまたは相互に結合します。</span><span class="sxs-lookup"><span data-stu-id="82523-163">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="82523-164">オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-164">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="82523-165">例:</span><span class="sxs-lookup"><span data-stu-id="82523-165">For example:</span></span>

<span data-ttu-id="82523-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="82523-167">次のコードでは、JSON の構成プロバイダーは有効です。</span><span class="sxs-lookup"><span data-stu-id="82523-167">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="82523-168">`MyOptions`クラスをサービス コンテナーに追加および構成にバインドします。</span><span class="sxs-lookup"><span data-stu-id="82523-168">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="82523-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span><span class="sxs-lookup"><span data-stu-id="82523-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span></span>

<span data-ttu-id="82523-170">次[コント ローラー](../mvc/controllers/index.md)使用[コンス トラクター依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="82523-170">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="82523-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="82523-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="82523-172">次の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-172">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="82523-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="82523-174">`HomeController.Index`メソッドを返します。`option1 = value1_from_json, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="82523-174">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="82523-175">一般的なアプリは、構成全体を 1 つのオプション ファイルにバインドされません。</span><span class="sxs-lookup"><span data-stu-id="82523-175">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="82523-176">後で使用する方法を示します`GetSection`セクションにバインドします。</span><span class="sxs-lookup"><span data-stu-id="82523-176">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="82523-177">次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="82523-177">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="82523-178">使用してバインディングを構成するデリゲートを使用して、`MyOptions`です。</span><span class="sxs-lookup"><span data-stu-id="82523-178">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="82523-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="82523-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="82523-180">複数の構成プロバイダーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="82523-180">You can add multiple configuration providers.</span></span> <span data-ttu-id="82523-181">NuGet パッケージの構成プロバイダーを利用できます。</span><span class="sxs-lookup"><span data-stu-id="82523-181">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="82523-182">登録されている順序で適用されます。</span><span class="sxs-lookup"><span data-stu-id="82523-182">They are applied in order they are registered.</span></span>

<span data-ttu-id="82523-183">各呼び出し`Configure<TOptions>`を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="82523-183">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="82523-184">前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json* --の値が、`Option1`構成済みのデリゲートによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="82523-184">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="82523-185">"Wins"最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、(構成値を設定します)。</span><span class="sxs-lookup"><span data-stu-id="82523-185">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="82523-186">上記のコードでは、`HomeController.Index`メソッドを返します。`option1 = value1_from_action, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="82523-186">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="82523-187">フォームの構成キー オプションの種類の各プロパティがバインドされているオプションを構成にバインドすると、`property[:sub-property:]`です。</span><span class="sxs-lookup"><span data-stu-id="82523-187">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="82523-188">たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="82523-188">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="82523-189">この記事で後述サブプロパティ サンプルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82523-189">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="82523-190">次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="82523-190">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="82523-191">バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-191">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="82523-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="82523-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="82523-193">注: この拡張メソッドが必要です、 `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="82523-193">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="82523-194">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-194">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="82523-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="82523-196">`MySubOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="82523-196">The `MySubOptions` class:</span></span>

<span data-ttu-id="82523-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span></span>

<span data-ttu-id="82523-198">次の`Controller`:</span><span class="sxs-lookup"><span data-stu-id="82523-198">With the following `Controller`:</span></span>

<span data-ttu-id="82523-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="82523-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="82523-200">`subOption1 = subvalue1_from_json, subOption2 = 200`返されます。</span><span class="sxs-lookup"><span data-stu-id="82523-200">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="82523-201">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="82523-201">IOptionsSnapshot</span></span>

<span data-ttu-id="82523-202">*ASP.NET Core 1.1 以上が必要です。*</span><span class="sxs-lookup"><span data-stu-id="82523-202">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="82523-203">`IOptionsSnapshot`構成ファイルが変更されたときに、構成データを再読み込みをサポートします。</span><span class="sxs-lookup"><span data-stu-id="82523-203">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="82523-204">最小限のオーバーヘッドがあります。</span><span class="sxs-lookup"><span data-stu-id="82523-204">It has minimal overhead.</span></span> <span data-ttu-id="82523-205">使用して`IOptionsSnapshot`で`reloadOnChange: true`、オプションにバインドされた`IConfiguration`および変更されたときに再読み込みします。</span><span class="sxs-lookup"><span data-stu-id="82523-205">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="82523-206">次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*config.json*変更します。</span><span class="sxs-lookup"><span data-stu-id="82523-206">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="82523-207">同じサーバーに要求が返される時刻*config.json*が**いない**を変更します。</span><span class="sxs-lookup"><span data-stu-id="82523-207">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="82523-208">後の最初の要求*config.json*変更新しい時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="82523-208">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="82523-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="82523-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="82523-210">次の図は、サーバーの出力を示しています。</span><span class="sxs-lookup"><span data-stu-id="82523-210">The following image shows the server output:</span></span>

![ブラウザーのイメージを示す"最終更新日: 2016 11 月 22 日 4時 43分 PM"](configuration/_static/first.png)

<span data-ttu-id="82523-212">ブラウザーを更新するメッセージの値または表示される時刻は変更されません (ときに*config.json*が変更されていない)。</span><span class="sxs-lookup"><span data-stu-id="82523-212">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="82523-213">変更し、保存、 *config.json*し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="82523-213">Change and save the  *config.json* and then refresh the browser:</span></span>

![ブラウザーのイメージを示す"e: に最後に更新された 2016 11 月 22 日 4時 53分 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="82523-215">メモリ内のプロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="82523-215">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="82523-216">次の例では、メモリ内のプロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="82523-216">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="82523-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span></span>

<span data-ttu-id="82523-218">構成値は、文字列として返されますが、バインド、オブジェクトの作成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="82523-218">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="82523-219">バインディングでは、POCO オブジェクトやオブジェクトも全体のグラフを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="82523-219">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="82523-220">次の例は、バインドする方法を示しています。`MyWindow`および ASP.NET Core MVC アプリのオプションのパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-220">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="82523-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="82523-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="82523-223">カスタム クラスをバインド`ConfigureServices`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="82523-223">Bind the custom class in `ConfigureServices` in the `Startup` class:</span></span>

<span data-ttu-id="82523-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span><span class="sxs-lookup"><span data-stu-id="82523-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span></span>

<span data-ttu-id="82523-225">設定を表示、 `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="82523-225">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="82523-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="82523-227">GetValue</span><span class="sxs-lookup"><span data-stu-id="82523-227">GetValue</span></span>

<span data-ttu-id="82523-228">次の例は、 [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="82523-228">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="82523-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="82523-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="82523-230">ConfigurationBinder の`GetValue<T>`メソッドでは、既定値 (このサンプルでは 80) を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="82523-230">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="82523-231">`GetValue<T>`単純なシナリオでは、セクション全体にバインドされません。</span><span class="sxs-lookup"><span data-stu-id="82523-231">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="82523-232">`GetValue<T>`スカラー値を取得`GetSection(key).Value`特定の種類に変換します。</span><span class="sxs-lookup"><span data-stu-id="82523-232">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="82523-233">オブジェクト グラフへのバインド</span><span class="sxs-lookup"><span data-stu-id="82523-233">Binding to an object graph</span></span>

<span data-ttu-id="82523-234">クラス内の各オブジェクトを再帰的にバインドをできます。</span><span class="sxs-lookup"><span data-stu-id="82523-234">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="82523-235">次の検討`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="82523-235">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="82523-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="82523-237">次の例にバインド、`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="82523-237">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="82523-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="82523-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="82523-239">**ASP.NET Core 1.1**以上使用できると`Get<T>`、セクション全体と連携します。</span><span class="sxs-lookup"><span data-stu-id="82523-239">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="82523-240">`Get<T>`使用するよりも多くのも、`Bind`です。</span><span class="sxs-lookup"><span data-stu-id="82523-240">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="82523-241">次のコードは、使用する方法を示しています。`Get<T>`と上のサンプル。</span><span class="sxs-lookup"><span data-stu-id="82523-241">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="82523-242">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-242">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="82523-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="82523-244">プログラムは、表示`Height 11`です。</span><span class="sxs-lookup"><span data-stu-id="82523-244">The program displays `Height 11`.</span></span>

<span data-ttu-id="82523-245">次のコードは、単位に使用できる構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="82523-245">The following code can be used to unit test the configuration:</span></span>

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

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="82523-246">Entity Framework のカスタム プロバイダーの基本的なサンプル</span><span class="sxs-lookup"><span data-stu-id="82523-246">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="82523-247">このセクションでは、EF を使用してデータベースから名前/値ペアを表示する基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="82523-247">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="82523-248">定義、`ConfigurationValue`データベースの構成値を格納するためのエンティティ。</span><span class="sxs-lookup"><span data-stu-id="82523-248">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="82523-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="82523-250">追加、`ConfigurationContext`を保存し、構成された値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="82523-250">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="82523-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="82523-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="82523-252">実装するクラスを作成する[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="82523-252">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="82523-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="82523-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="82523-254">継承することで、カスタム構成プロバイダーを作成する[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)です。</span><span class="sxs-lookup"><span data-stu-id="82523-254">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="82523-255">構成プロバイダーは、空であるときに、データベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="82523-255">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="82523-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="82523-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="82523-257">サンプルを実行する、強調表示されているデータベースからの値 ("value_from_ef_1"および"value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="82523-257">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="82523-258">追加することができます、`EFConfigSource`構成ソースを追加するための拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="82523-258">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="82523-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="82523-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="82523-260">次のコードは、ユーザー設定を使用する方法を示しています`EFConfigProvider`:。</span><span class="sxs-lookup"><span data-stu-id="82523-260">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="82523-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span><span class="sxs-lookup"><span data-stu-id="82523-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span></span>

<span data-ttu-id="82523-262">サンプルを追加、カスタム注`EFConfigProvider`JSON プロバイダーの後にそのため、データベースから設定設定を上書きするから、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-262">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="82523-263">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="82523-263">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="82523-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="82523-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="82523-265">次のものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82523-265">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="82523-266">コマンドライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="82523-266">CommandLine configuration provider</span></span>

<span data-ttu-id="82523-267">次の例では、コマンドライン構成プロバイダーが最終有効にします。</span><span class="sxs-lookup"><span data-stu-id="82523-267">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="82523-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="82523-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="82523-269">構成設定を渡すには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-269">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="82523-270">これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="82523-270">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="82523-271">`GetSwitchMappings`メソッドでは、使用できます。`-`なく`/`先頭のサブキーのプレフィックスを取り除きます。</span><span class="sxs-lookup"><span data-stu-id="82523-271">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="82523-272">例:</span><span class="sxs-lookup"><span data-stu-id="82523-272">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="82523-273">表示します。</span><span class="sxs-lookup"><span data-stu-id="82523-273">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="82523-274">コマンドライン引数には、値 (null を指定できます) を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="82523-274">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="82523-275">例:</span><span class="sxs-lookup"><span data-stu-id="82523-275">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="82523-276">[Ok] が、</span><span class="sxs-lookup"><span data-stu-id="82523-276">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="82523-277">例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="82523-277">results in an exception.</span></span> <span data-ttu-id="82523-278">コマンド ライン スイッチのプレフィックスの - または--がない対応するスイッチのマッピングを指定する場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="82523-278">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="82523-279">Web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="82523-279">The web.config file</span></span>

<span data-ttu-id="82523-280">A *web.config* IIS または IIS Express でアプリをホストしている場合は、ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="82523-280">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="82523-281">*web.config*アプリを起動するように IIS で AspNetCoreModule をオンにします。</span><span class="sxs-lookup"><span data-stu-id="82523-281">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="82523-282">設定*web.config*アプリを起動し、その他の IIS の設定とモジュールを構成するために IIS の AspNetCoreModule を有効にします。</span><span class="sxs-lookup"><span data-stu-id="82523-282">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="82523-283">Visual Studio を使用して削除すると*web.config*、Visual Studio は、新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="82523-283">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="82523-284">補足メモ</span><span class="sxs-lookup"><span data-stu-id="82523-284">Additional notes</span></span>

* <span data-ttu-id="82523-285">依存関係の挿入 (DI) が設定されていないまで後`ConfigureServices`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="82523-285">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="82523-286">構成システムでは DI に注意してください。</span><span class="sxs-lookup"><span data-stu-id="82523-286">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="82523-287">`IConfiguration`2 つの特殊化があります。</span><span class="sxs-lookup"><span data-stu-id="82523-287">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="82523-288">`IConfigurationRoot`ルート ノードに使用します。</span><span class="sxs-lookup"><span data-stu-id="82523-288">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="82523-289">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="82523-289">Can trigger a reload.</span></span>
  * <span data-ttu-id="82523-290">`IConfigurationSection`構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="82523-290">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="82523-291">`GetSection`と`GetChildren`を返し、`IConfigurationSection`です。</span><span class="sxs-lookup"><span data-stu-id="82523-291">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="82523-292">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="82523-292">Additional resources</span></span>

* [<span data-ttu-id="82523-293">複数の環境での作業</span><span class="sxs-lookup"><span data-stu-id="82523-293">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="82523-294">アプリ シークレットは、開発中の安全な格納場所</span><span class="sxs-lookup"><span data-stu-id="82523-294">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="82523-295">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="82523-295">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="82523-296">Azure Key Vault の構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="82523-296">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
