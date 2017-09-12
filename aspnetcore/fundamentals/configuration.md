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
ms.openlocfilehash: a14bc7fbcdac9acddfdab4fcd6e40385ca48bcc4
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="ecf26-104">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="ecf26-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="ecf26-105">[Rick Anderson](https://twitter.com/RickAndMSFT)、[マーク Michaelis](http://intellitect.com/author/mark-michaelis/)、 [Steve Smith](https://ardalis.com/)、および[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ecf26-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ecf26-106">構成 API は、名前と値のペアの一覧で、アプリケーションを構成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="ecf26-107">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="ecf26-108">名前と値のペアは、複数レベルの階層化することができます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="ecf26-109">構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-109">There are configuration providers for:</span></span>

* <span data-ttu-id="ecf26-110">ファイル形式 (INI、JSON、および XML)</span><span class="sxs-lookup"><span data-stu-id="ecf26-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="ecf26-111">コマンドライン引数</span><span class="sxs-lookup"><span data-stu-id="ecf26-111">Command-line arguments</span></span>
* <span data-ttu-id="ecf26-112">環境変数</span><span class="sxs-lookup"><span data-stu-id="ecf26-112">Environment variables</span></span>
* <span data-ttu-id="ecf26-113">メモリ内の .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="ecf26-113">In-memory .NET objects</span></span>
* <span data-ttu-id="ecf26-114">暗号化されたユーザー ストア</span><span class="sxs-lookup"><span data-stu-id="ecf26-114">An encrypted user store</span></span>
* [<span data-ttu-id="ecf26-115">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ecf26-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="ecf26-116">カスタム プロバイダーをインストールまたは作成します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="ecf26-117">各構成値は、文字列のキーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="ecf26-118">カスタム設定を逆シリアル化する組み込みのバインディング サポートは[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)オブジェクト (シンプルな .NET クラスのプロパティを持つ)。</span><span class="sxs-lookup"><span data-stu-id="ecf26-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="ecf26-119">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="ecf26-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="ecf26-120">単純な構成</span><span class="sxs-lookup"><span data-stu-id="ecf26-120">Simple configuration</span></span>

<span data-ttu-id="ecf26-121">次のコンソール アプリケーションは、JSON の構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="ecf26-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="ecf26-123">アプリでは、読み取りし、次の構成設定を表示します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="ecf26-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="ecf26-125">構成は、ノードが、コロンで区切られた名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="ecf26-126">値を取得するには、アクセス、`Configuration`インデクサーに対して、対応する項目のキー。</span><span class="sxs-lookup"><span data-stu-id="ecf26-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="ecf26-127">JSON 形式構成ソース内の配列を使用するには、コロンで区切られた文字列の一部として、配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="ecf26-128">次の例は、上記の最初の項目の名前を取得`wizards`配列。</span><span class="sxs-lookup"><span data-stu-id="ecf26-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="ecf26-129">組み込みに書き込まれる名前と値のペアで`Configuration`プロバイダーは**いない**永続化して、ただし、プロバイダーを作成できます、カスタム値を保存します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="ecf26-130">参照してください[カスタム構成プロバイダー](xref:fundamentals/configuration#custom-config-providers)です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="ecf26-131">上記のサンプルでは、構成のインデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="ecf26-132">外部アクセス構成を`Startup`を使用して、[オプション パターン](xref:fundamentals/configuration#options-config-objects)です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="ecf26-133">*オプション パターン*この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="ecf26-134">通常をさまざまな環境、開発、テスト、および運用環境などの別の構成設定があります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-134">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="ecf26-135">`CreateDefaultBuilder` ASP.NET Core 2.x アプリケーションでも拡張メソッド (またはを使用して`AddJsonFile`と`AddEnvironmentVariables`ASP.NET Core 1.x アプリ内で直接) JSON ファイルおよびシステム構成ソースを読み取るための構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="ecf26-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="ecf26-136">*appsettings.json*</span></span>
* <span data-ttu-id="ecf26-137">* appsettings です。\<EnvironmentName > .json</span><span class="sxs-lookup"><span data-stu-id="ecf26-137">*appsettings.\<EnvironmentName>.json</span></span>
* <span data-ttu-id="ecf26-138">環境変数</span><span class="sxs-lookup"><span data-stu-id="ecf26-138">environment variables</span></span>

<span data-ttu-id="ecf26-139">参照してください[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)パラメーターの説明。</span><span class="sxs-lookup"><span data-stu-id="ecf26-139">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="ecf26-140">`reloadOnChange`ASP.NET Core 1.1 以降のみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="ecf26-141">構成ソースは、指定された順序で読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-141">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="ecf26-142">上記のコードでは、環境変数が前回読み取られます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="ecf26-143">環境を使用して設定、構成値には、2 つの以前のプロバイダーで設定されていると交換してください。</span><span class="sxs-lookup"><span data-stu-id="ecf26-143">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="ecf26-144">環境が通常のいずれかに設定されている`Development`、 `Staging`、または`Production`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-144">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="ecf26-145">参照してください[複数の環境で作業](environments.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-145">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="ecf26-146">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="ecf26-146">Configuration considerations:</span></span>

* <span data-ttu-id="ecf26-147">`IOptionsSnapshot`変更されたとき、構成データを再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="ecf26-148">使用して`IOptionsSnapshot`構成データを再読み込みする必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="ecf26-148">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="ecf26-149">参照してください[IOptionsSnapshot](#ioptionssnapshot)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-149">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="ecf26-150">構成キーは、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-150">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="ecf26-151">ベスト プラクティスは、ローカルの環境は、何も展開された構成ファイルで設定をオーバーライドできるようにする、最後に、環境変数を指定するです。</span><span class="sxs-lookup"><span data-stu-id="ecf26-151">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="ecf26-152">**決して**プロバイダーの構成コードまたは構成ファイルをプレーン テキストでパスワードまたは他の機密データを格納します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-152">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="ecf26-153">環境をテストしたり、開発に実稼働のシークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="ecf26-153">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="ecf26-154">代わりに、リポジトリに誤ってコミットするため、プロジェクト ツリーの外部のシークレットを指定します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-154">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="ecf26-155">詳細については[複数の環境で作業](environments.md)および管理する[アプリ シークレットは、開発中の安全な保管](../security/app-secrets.md)です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-155">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="ecf26-156">場合`:`することはできません、システムで環境変数の使用を置き換えます`:`で`__`(二重のアンダー スコア)。</span><span class="sxs-lookup"><span data-stu-id="ecf26-156">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="ecf26-157">オプションと構成オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-157">Using Options and configuration objects</span></span>

<span data-ttu-id="ecf26-158">オプションのパターンでは、カスタム オプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-158">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="ecf26-159">アプリ内で各機能の分離型のクラスを作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-159">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="ecf26-160">分離されたクラスに従ってください。</span><span class="sxs-lookup"><span data-stu-id="ecf26-160">Decoupled classes follow:</span></span>

* <span data-ttu-id="ecf26-161">[インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/) : クラスが使用する構成設定のみに依存します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-161">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="ecf26-162">[関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が依存するまたは相互に結合します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-162">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="ecf26-163">オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-163">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ecf26-164">例:</span><span class="sxs-lookup"><span data-stu-id="ecf26-164">For example:</span></span>

<span data-ttu-id="ecf26-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="ecf26-166">次のコードでは、JSON の構成プロバイダーは有効です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-166">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="ecf26-167">`MyOptions`クラスをサービス コンテナーに追加および構成にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-167">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="ecf26-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span></span>

<span data-ttu-id="ecf26-169">次[コント ローラー](../mvc/controllers/index.md)使用[コンス トラクター依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-169">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="ecf26-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="ecf26-171">次の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-171">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="ecf26-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="ecf26-173">`HomeController.Index`メソッドを返します。`option1 = value1_from_json, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-173">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="ecf26-174">一般的なアプリは、構成全体を 1 つのオプション ファイルにバインドされません。</span><span class="sxs-lookup"><span data-stu-id="ecf26-174">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="ecf26-175">後で使用する方法を示します`GetSection`セクションにバインドします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-175">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="ecf26-176">次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-176">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ecf26-177">使用してバインディングを構成するデリゲートを使用して、`MyOptions`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-177">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="ecf26-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="ecf26-179">複数の構成プロバイダーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-179">You can add multiple configuration providers.</span></span> <span data-ttu-id="ecf26-180">NuGet パッケージの構成プロバイダーを利用できます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-180">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="ecf26-181">登録されている順序で適用されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-181">They are applied in order they are registered.</span></span>

<span data-ttu-id="ecf26-182">各呼び出し`Configure<TOptions>`を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-182">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="ecf26-183">前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json* --の値が、`Option1`構成済みのデリゲートによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-183">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="ecf26-184">"Wins"最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、(構成値を設定します)。</span><span class="sxs-lookup"><span data-stu-id="ecf26-184">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="ecf26-185">上記のコードでは、`HomeController.Index`メソッドを返します。`option1 = value1_from_action, option2 = 2`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-185">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="ecf26-186">フォームの構成キー オプションの種類の各プロパティがバインドされているオプションを構成にバインドすると、`property[:sub-property:]`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-186">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ecf26-187">たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-187">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="ecf26-188">この記事で後述サブプロパティ サンプルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-188">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="ecf26-189">次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-189">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ecf26-190">バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-190">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="ecf26-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="ecf26-192">注: この拡張メソッドが必要です、 `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ecf26-192">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="ecf26-193">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-193">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="ecf26-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="ecf26-195">`MySubOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ecf26-195">The `MySubOptions` class:</span></span>

<span data-ttu-id="ecf26-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="ecf26-197">次の`Controller`:</span><span class="sxs-lookup"><span data-stu-id="ecf26-197">With the following `Controller`:</span></span>

<span data-ttu-id="ecf26-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="ecf26-199">`subOption1 = subvalue1_from_json, subOption2 = 200`返されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-199">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="ecf26-200">ビューのモデルでのオプションを指定したり挿入したりできます`IOptions<TOptions>`ビューに直接。</span><span class="sxs-lookup"><span data-stu-id="ecf26-200">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

<span data-ttu-id="ecf26-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="ecf26-202">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="ecf26-202">IOptionsSnapshot</span></span>

<span data-ttu-id="ecf26-203">*ASP.NET Core 1.1 以上が必要です。*</span><span class="sxs-lookup"><span data-stu-id="ecf26-203">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="ecf26-204">`IOptionsSnapshot`構成ファイルが変更されたときに、構成データを再読み込みをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-204">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="ecf26-205">最小限のオーバーヘッドがあります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-205">It has minimal overhead.</span></span> <span data-ttu-id="ecf26-206">使用して`IOptionsSnapshot`で`reloadOnChange: true`、オプションにバインドされた`IConfiguration`および変更されたときに再読み込みします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-206">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="ecf26-207">次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*config.json*変更します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-207">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="ecf26-208">同じサーバーに要求が返される時刻*config.json*が**いない**を変更します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-208">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="ecf26-209">後の最初の要求*config.json*変更新しい時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-209">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="ecf26-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="ecf26-211">次の図は、サーバーの出力を示しています。</span><span class="sxs-lookup"><span data-stu-id="ecf26-211">The following image shows the server output:</span></span>

![ブラウザーのイメージを示す"最終更新日: 2016 11 月 22 日 4時 43分 PM"](configuration/_static/first.png)

<span data-ttu-id="ecf26-213">ブラウザーを更新するメッセージの値または表示される時刻は変更されません (ときに*config.json*が変更されていない)。</span><span class="sxs-lookup"><span data-stu-id="ecf26-213">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="ecf26-214">変更し、保存、 *config.json*し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-214">Change and save the  *config.json* and then refresh the browser:</span></span>

![ブラウザーのイメージを示す"e: に最後に更新された 2016 11 月 22 日 4時 53分 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="ecf26-216">メモリ内のプロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="ecf26-216">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="ecf26-217">次の例では、メモリ内のプロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-217">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="ecf26-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span></span>

<span data-ttu-id="ecf26-219">構成値は、文字列として返されますが、バインド、オブジェクトの作成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-219">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="ecf26-220">バインディングでは、POCO オブジェクトやオブジェクトも全体のグラフを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-220">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="ecf26-221">次の例は、バインドする方法を示しています。`MyWindow`および ASP.NET Core MVC アプリのオプションのパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-221">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="ecf26-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="ecf26-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="ecf26-224">カスタム クラスをバインド`ConfigureServices`ホストを作成するときに。</span><span class="sxs-lookup"><span data-stu-id="ecf26-224">Bind the custom class in `ConfigureServices` when building the host:</span></span>

<span data-ttu-id="ecf26-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span></span>

<span data-ttu-id="ecf26-226">設定を表示、 `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="ecf26-226">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="ecf26-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="ecf26-228">GetValue</span><span class="sxs-lookup"><span data-stu-id="ecf26-228">GetValue</span></span>

<span data-ttu-id="ecf26-229">次の例は、 [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="ecf26-229">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="ecf26-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="ecf26-231">ConfigurationBinder の`GetValue<T>`メソッドでは、既定値 (このサンプルでは 80) を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-231">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="ecf26-232">`GetValue<T>`単純なシナリオでは、セクション全体にバインドされません。</span><span class="sxs-lookup"><span data-stu-id="ecf26-232">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="ecf26-233">`GetValue<T>`スカラー値を取得`GetSection(key).Value`特定の種類に変換します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-233">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="ecf26-234">オブジェクト グラフへのバインド</span><span class="sxs-lookup"><span data-stu-id="ecf26-234">Binding to an object graph</span></span>

<span data-ttu-id="ecf26-235">クラス内の各オブジェクトを再帰的にバインドをできます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-235">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="ecf26-236">次の検討`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ecf26-236">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="ecf26-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="ecf26-238">次の例にバインド、`AppOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="ecf26-238">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="ecf26-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="ecf26-240">**ASP.NET Core 1.1**以上使用できると`Get<T>`、セクション全体と連携します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-240">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="ecf26-241">`Get<T>`使用するよりも多くのも、`Bind`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-241">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="ecf26-242">次のコードは、使用する方法を示しています。`Get<T>`と上のサンプル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-242">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="ecf26-243">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-243">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="ecf26-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="ecf26-245">プログラムは、表示`Height 11`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-245">The program displays `Height 11`.</span></span>

<span data-ttu-id="ecf26-246">次のコードは、単位に使用できる構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-246">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="ecf26-247">Entity Framework のカスタム プロバイダーの基本的なサンプル</span><span class="sxs-lookup"><span data-stu-id="ecf26-247">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="ecf26-248">このセクションでは、EF を使用してデータベースから名前/値ペアを表示する基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-248">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="ecf26-249">定義、`ConfigurationValue`データベースの構成値を格納するためのエンティティ。</span><span class="sxs-lookup"><span data-stu-id="ecf26-249">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="ecf26-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="ecf26-251">追加、`ConfigurationContext`を保存し、構成された値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-251">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="ecf26-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="ecf26-253">実装するクラスを作成する[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="ecf26-253">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="ecf26-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="ecf26-255">継承することで、カスタム構成プロバイダーを作成する[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-255">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="ecf26-256">構成プロバイダーは、空であるときに、データベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-256">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="ecf26-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="ecf26-258">サンプルを実行する、強調表示されているデータベースからの値 ("value_from_ef_1"および"value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-258">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="ecf26-259">追加することができます、`EFConfigSource`構成ソースを追加するための拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="ecf26-259">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="ecf26-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="ecf26-261">次のコードは、ユーザー設定を使用する方法を示しています`EFConfigProvider`:。</span><span class="sxs-lookup"><span data-stu-id="ecf26-261">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="ecf26-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span></span>

<span data-ttu-id="ecf26-263">サンプルを追加、カスタム注`EFConfigProvider`JSON プロバイダーの後にそのため、データベースから設定設定を上書きするから、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-263">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="ecf26-264">以下を使用して*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecf26-264">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="ecf26-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="ecf26-266">次のものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-266">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="ecf26-267">コマンドライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ecf26-267">CommandLine configuration provider</span></span>

<span data-ttu-id="ecf26-268">次の例では、コマンドライン構成プロバイダーが最終有効にします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-268">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="ecf26-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="ecf26-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="ecf26-270">構成設定を渡すには、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-270">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="ecf26-271">これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-271">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="ecf26-272">`GetSwitchMappings`メソッドでは、使用できます。`-`なく`/`先頭のサブキーのプレフィックスを取り除きます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-272">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="ecf26-273">例:</span><span class="sxs-lookup"><span data-stu-id="ecf26-273">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="ecf26-274">表示します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-274">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="ecf26-275">コマンドライン引数には、値 (null を指定できます) を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-275">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="ecf26-276">例:</span><span class="sxs-lookup"><span data-stu-id="ecf26-276">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="ecf26-277">[Ok] が、</span><span class="sxs-lookup"><span data-stu-id="ecf26-277">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="ecf26-278">例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-278">results in an exception.</span></span> <span data-ttu-id="ecf26-279">コマンド ライン スイッチのプレフィックスの - または--がない対応するスイッチのマッピングを指定する場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-279">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="ecf26-280">Web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="ecf26-280">The web.config file</span></span>

<span data-ttu-id="ecf26-281">A *web.config* IIS または IIS Express でアプリをホストしている場合は、ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-281">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="ecf26-282">*web.config*アプリを起動するように IIS で AspNetCoreModule をオンにします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-282">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="ecf26-283">設定*web.config*アプリを起動し、その他の IIS の設定とモジュールを構成するために IIS の AspNetCoreModule を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ecf26-283">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="ecf26-284">Visual Studio を使用して削除すると*web.config*、Visual Studio は、新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-284">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="ecf26-285">補足メモ</span><span class="sxs-lookup"><span data-stu-id="ecf26-285">Additional notes</span></span>

* <span data-ttu-id="ecf26-286">依存関係の挿入 (DI) が設定されていないまで後`ConfigureServices`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-286">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="ecf26-287">構成システムでは DI に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ecf26-287">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="ecf26-288">`IConfiguration`2 つの特殊化があります。</span><span class="sxs-lookup"><span data-stu-id="ecf26-288">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="ecf26-289">`IConfigurationRoot`ルート ノードに使用します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-289">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="ecf26-290">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="ecf26-290">Can trigger a reload.</span></span>
  * <span data-ttu-id="ecf26-291">`IConfigurationSection`構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="ecf26-291">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="ecf26-292">`GetSection`と`GetChildren`を返し、`IConfigurationSection`です。</span><span class="sxs-lookup"><span data-stu-id="ecf26-292">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="ecf26-293">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ecf26-293">Additional resources</span></span>

* [<span data-ttu-id="ecf26-294">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="ecf26-294">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="ecf26-295">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="ecf26-295">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="ecf26-296">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="ecf26-296">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="ecf26-297">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ecf26-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
