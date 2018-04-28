---
title: ASP.NET Core の構成
author: rick-anderson
description: 構成 API を使用して、複数の方法で ASP.NET Core アプリを構成します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 8325776597eab9c673a4847edf87ae0d9e0d3efb
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2018
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="25afb-103">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="25afb-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="25afb-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="25afb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="25afb-105">構成 API は、名前と値のペアのリストに基づく ASP.NET Core Web アプリの構成方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="25afb-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="25afb-106">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="25afb-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="25afb-107">名前と値の組は、複数レベルの階層にグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="25afb-108">以下の構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="25afb-108">There are configuration providers for:</span></span>

* <span data-ttu-id="25afb-109">ファイル形式 (INI、JSON、および XML)。</span><span class="sxs-lookup"><span data-stu-id="25afb-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="25afb-110">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="25afb-110">Command-line arguments.</span></span>
* <span data-ttu-id="25afb-111">環境変数。</span><span class="sxs-lookup"><span data-stu-id="25afb-111">Environment variables.</span></span>
* <span data-ttu-id="25afb-112">メモリ内 .NET オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="25afb-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="25afb-113">暗号化されていない[シークレット マネージャー](xref:security/app-secrets)の記憶域。</span><span class="sxs-lookup"><span data-stu-id="25afb-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="25afb-114">[Azure Key Vault](xref:security/key-vault-configuration) などの暗号化されたユーザー ストア。</span><span class="sxs-lookup"><span data-stu-id="25afb-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="25afb-115">カスタム プロバイダー (インストール済みまたは作成済み)。</span><span class="sxs-lookup"><span data-stu-id="25afb-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="25afb-116">各構成値は文字列キーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="25afb-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="25afb-117">設定をカスタム [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクト (プロパティを持つ単純な .NET クラス) に逆シリアル化するための組み込みのバインド サポートがあります。</span><span class="sxs-lookup"><span data-stu-id="25afb-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="25afb-118">オプション パターンではオプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="25afb-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="25afb-119">オプション パターンの使用の詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="25afb-120">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="25afb-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="25afb-121">JSON 構成</span><span class="sxs-lookup"><span data-stu-id="25afb-121">JSON configuration</span></span>

<span data-ttu-id="25afb-122">次のコンソール アプリでは JSON 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="25afb-123">アプリは以下の構成設定を読み取り、表示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="25afb-124">構成は、コロン (`:`) で区切られたノードの名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="25afb-125">値を取得するには、対応する項目のキーを使用して `Configuration` インデクサーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="25afb-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="25afb-126">JSON 形式の構成ソースの配列を操作する場合は、コロンで区切られた文字列の一部として配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="25afb-127">次の例では、上記の `wizards` 配列の最初の項目の名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="25afb-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="25afb-128">組み込みの[構成](/dotnet/api/microsoft.extensions.configuration)プロバイダーに書き込まれた名前と値のペアは永続化**されません**。</span><span class="sxs-lookup"><span data-stu-id="25afb-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="25afb-129">ただし、値を保存するカスタム プロバイダーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="25afb-130">[カスタム構成プロバイダー](xref:fundamentals/configuration/index#custom-config-providers)に関する記述を参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="25afb-131">上記のサンプルでは、構成インデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="25afb-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="25afb-132">`Startup` の外部の構成にアクセスする場合は、*オプション パターン*を使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="25afb-133">詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="25afb-134">XML 構成</span><span class="sxs-lookup"><span data-stu-id="25afb-134">XML configuration</span></span>

<span data-ttu-id="25afb-135">XML 形式の構成ソースの配列を操作するには、各要素に `name` インデックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="25afb-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="25afb-136">インデックスを使用して値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="25afb-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="25afb-137">環境別の構成</span><span class="sxs-lookup"><span data-stu-id="25afb-137">Configuration by environment</span></span>

<span data-ttu-id="25afb-138">開発、テスト、運用などの異なる環境に応じて異なる構成を設定するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="25afb-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="25afb-139">ASP.NET Core 2.x アプリの `CreateDefaultBuilder` 拡張メソッド (ASP.NET Core 1.x アプリの場合は、`AddJsonFile` および `AddEnvironmentVariables` を直接使用) では、JSON ファイルとシステム構成ソースを読み取るために構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="25afb-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="25afb-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="25afb-140">*appsettings.json*</span></span>
* <span data-ttu-id="25afb-141">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="25afb-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="25afb-142">環境変数</span><span class="sxs-lookup"><span data-stu-id="25afb-142">Environment variables</span></span>

<span data-ttu-id="25afb-143">ASP.NET Core 1.x アプリは、`AddJsonFile` および [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="25afb-144">パラメーターの説明については、「[AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="25afb-145">`reloadOnChange` は、ASP.NET Core 1.1 以降でのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="25afb-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="25afb-146">構成ソースは指定された順に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="25afb-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="25afb-147">先のコードでは、環境変数が最後に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="25afb-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="25afb-148">環境を使用して設定された構成値で、2 つの前述のプロバイダーで設定された値が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="25afb-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="25afb-149">次の *appsettings.Staging.json* ファイルをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="25afb-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="25afb-150">環境が `Staging` に設定されているとき、次の `Configure` メソッドは `MyConfig` の値を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="25afb-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="25afb-151">通常、環境は `Development`、`Staging`、または `Production` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="25afb-152">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-152">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="25afb-153">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="25afb-153">Configuration considerations:</span></span>

* <span data-ttu-id="25afb-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) では、構成データを変更時に再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="25afb-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="25afb-155">構成キーでは大文字と小文字が区別**されません**。</span><span class="sxs-lookup"><span data-stu-id="25afb-155">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="25afb-156">構成プロバイダー コードやプレーンテキストの構成ファイルには、パスワードなどの機密データは格納**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="25afb-156">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="25afb-157">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="25afb-157">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="25afb-158">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="25afb-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="25afb-159">[複数の環境での作業方法](xref:fundamentals/environments)および[開発中のアプリ シークレットの安全な格納場所](xref:security/app-secrets)の管理の詳細を確認してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-159">Learn more about [how to work with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="25afb-160">環境変数で指定された階層の構成値では、コロン (`:`) が一部のプラットフォームで動作しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-160">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="25afb-161">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="25afb-161">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="25afb-162">構成 API とやり取りするときは、コロン (`:`) がすべてのプラットフォームで動作します。</span><span class="sxs-lookup"><span data-stu-id="25afb-162">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="25afb-163">メモリ内プロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="25afb-163">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="25afb-164">次の例では、メモリ内プロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-164">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="25afb-165">構成値は文字列として返されますが、バインディングすることでプロジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-165">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="25afb-166">バインディングすることで POCO オブジェクトや、オブジェクト グラフ全体を取得できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-166">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="25afb-167">GetValue</span><span class="sxs-lookup"><span data-stu-id="25afb-167">GetValue</span></span>

<span data-ttu-id="25afb-168">次の例では、[GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 拡張メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-168">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="25afb-169">ConfigurationBinder の `GetValue<T>` メソッドでは、既定値 (サンプルでは 80) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-169">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="25afb-170">`GetValue<T>` は単純なシナリオ用であり、セクション全体にはバインドされません。</span><span class="sxs-lookup"><span data-stu-id="25afb-170">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="25afb-171">`GetValue<T>` は、特定の型に変換された `GetSection(key).Value` からスカラー値を取得します。</span><span class="sxs-lookup"><span data-stu-id="25afb-171">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="25afb-172">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="25afb-172">Bind to an object graph</span></span>

<span data-ttu-id="25afb-173">クラス内の各オブジェクトに再帰的にバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-173">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="25afb-174">次の `AppSettings` クラスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="25afb-174">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="25afb-175">次のサンプルでは `AppSettings` クラスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="25afb-175">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="25afb-176">**ASP.NET Core 1.1** 以上では、セクション全体を操作する `Get<T>` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-176">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="25afb-177">`Get<T>` は `Bind` を使用するよりも便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-177">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="25afb-178">次のコードでは、上記のサンプルで `Get<T>` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-178">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="25afb-179">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-179">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="25afb-180">プログラムは `Height 11` を表示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-180">The program displays `Height 11`.</span></span>

<span data-ttu-id="25afb-181">次のコードを使用して、構成の単体テストを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-181">The following code can be used to unit test the configuration:</span></span>

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="25afb-182">Entity Framework のカスタム プロバイダーを作成する</span><span class="sxs-lookup"><span data-stu-id="25afb-182">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="25afb-183">このセクションでは、EF を使用してデータベースから名前と値のペアを読み取る基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-183">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="25afb-184">データベースに構成値を格納するための `ConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="25afb-184">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="25afb-185">構成した値を格納し、その値にアクセスするための `ConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="25afb-185">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="25afb-186">[IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="25afb-186">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="25afb-187">[ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="25afb-187">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="25afb-188">構成プロバイダーは、空の場合にデータベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="25afb-188">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="25afb-189">サンプルを実行すると、データベースからの強調表示された値 ("value_from_ef_1" および "value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-189">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="25afb-190">構成ソースを追加するための `EFConfigSource` 拡張メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-190">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="25afb-191">次のコードに、カスタム `EFConfigProvider` の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="25afb-191">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="25afb-192">サンプルでは JSON プロバイダーの後にカスタム `EFConfigProvider` を追加するため、データベースからのすべての設定で *appsettings.json* ファイルからの設定がオーバーライドされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-192">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="25afb-193">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-193">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="25afb-194">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-194">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="25afb-195">CommandLine 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="25afb-195">CommandLine configuration provider</span></span>

<span data-ttu-id="25afb-196">[CommandLine 構成プロバイダー](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)は、実行時に構成するコマンドライン引数のキーと値のペアを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="25afb-196">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="25afb-197">CommandLine 構成サンプルを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="25afb-197">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="25afb-198">CommandLine 構成プロバイダーをセットアップして使用する</span><span class="sxs-lookup"><span data-stu-id="25afb-198">Setup and use the CommandLine configuration provider</span></span>

#### <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="25afb-199">基本構成</span><span class="sxs-lookup"><span data-stu-id="25afb-199">Basic Configuration</span></span>](#tab/basicconfiguration/)
<span data-ttu-id="25afb-200">コマンド ライン構成をアクティブにするには、[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) のインスタンスで `AddCommandLine` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="25afb-200">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="25afb-201">コードを実行すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-201">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="25afb-202">コマンド ラインで引数のキーと値のペアを渡すと、`Profile:MachineName` と `App:MainWindow:Left` の値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-202">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="25afb-203">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-203">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="25afb-204">他の構成プロバイダーによって提供された構成をコマンドライン構成でオーバーライドするには、`ConfigurationBuilder` で最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="25afb-204">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="25afb-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="25afb-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="25afb-206">一般的な ASP.NET Core 2.x アプリでは、便利な静的メソッド `CreateDefaultBuilder` を使用してホストを構築します。</span><span class="sxs-lookup"><span data-stu-id="25afb-206">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="25afb-207">`CreateDefaultBuilder` では、省略可能構成を *appsettings.json*、*appsettings.{Environment}.json*、[user secrets](xref:security/app-secrets) (`Development` 環境の場合)、環境変数、およびコマンドライン引数から読み込みます。</span><span class="sxs-lookup"><span data-stu-id="25afb-207">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="25afb-208">CommandLine 構成プロバイダーは最後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-208">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="25afb-209">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="25afb-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="25afb-210">次のような *appsettings* ファイルの場合:</span><span class="sxs-lookup"><span data-stu-id="25afb-210">For *appsettings* files where:</span></span>

* <span data-ttu-id="25afb-211">`reloadOnChange` が有効です。</span><span class="sxs-lookup"><span data-stu-id="25afb-211">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="25afb-212">コマンドライン引数と *appsettings* ファイルに同じ設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="25afb-212">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="25afb-213">一致するコマンドライン引数を含む *appsettings* ファイルは、アプリの起動後に変更されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-213">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="25afb-214">上記の条件がすべて真になると、コマンドライン引数がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="25afb-214">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="25afb-215">ASP.NET Core 2.x アプリでは、`CreateDefaultBuilder` ではなく [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-215">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="25afb-216">`WebHostBuilder` を使用する場合は、[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) によって構成を手動で設定します。</span><span class="sxs-lookup"><span data-stu-id="25afb-216">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="25afb-217">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-217">See the ASP.NET Core 1.x tab for more information.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="25afb-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="25afb-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="25afb-219">[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) を作成し、`AddCommandLine` メソッドを呼び出して CommandLine 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-219">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="25afb-220">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="25afb-220">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="25afb-221">`UseConfiguration` メソッドを使用して、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-221">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

* * *
### <a name="arguments"></a><span data-ttu-id="25afb-222">引数</span><span class="sxs-lookup"><span data-stu-id="25afb-222">Arguments</span></span>

<span data-ttu-id="25afb-223">コマンド ラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに準拠する必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-223">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="25afb-224">引数の形式</span><span class="sxs-lookup"><span data-stu-id="25afb-224">Argument format</span></span>                                                     | <span data-ttu-id="25afb-225">例</span><span class="sxs-lookup"><span data-stu-id="25afb-225">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="25afb-226">単一引数: 等号 (`=`) で区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="25afb-226">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="25afb-227">2 つの引数のシーケンス: スペースで区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="25afb-227">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="25afb-228">**単一引数**</span><span class="sxs-lookup"><span data-stu-id="25afb-228">**Single argument**</span></span>

<span data-ttu-id="25afb-229">値は等号 (`=`) の後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-229">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="25afb-230">値を null にすることができます (例: `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="25afb-230">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="25afb-231">キーにはプレフィックスが付く場合があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-231">The key may have a prefix.</span></span>

| <span data-ttu-id="25afb-232">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="25afb-232">Key prefix</span></span>               | <span data-ttu-id="25afb-233">例</span><span class="sxs-lookup"><span data-stu-id="25afb-233">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="25afb-234">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="25afb-234">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="25afb-235">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="25afb-235">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="25afb-236">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="25afb-236">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="25afb-237">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="25afb-237">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="25afb-238">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="25afb-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="25afb-239">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="25afb-239">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="25afb-240">注: `-key2` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="25afb-240">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="25afb-241">**2 つの引数のシーケンス**</span><span class="sxs-lookup"><span data-stu-id="25afb-241">**Sequence of two arguments**</span></span>

<span data-ttu-id="25afb-242">値を null にすることはできません。値はスペースで区切られたキーの後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-242">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="25afb-243">キーにはプレフィックスを付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-243">The key must have a prefix.</span></span>

| <span data-ttu-id="25afb-244">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="25afb-244">Key prefix</span></span>               | <span data-ttu-id="25afb-245">例</span><span class="sxs-lookup"><span data-stu-id="25afb-245">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="25afb-246">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="25afb-246">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="25afb-247">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="25afb-247">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="25afb-248">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="25afb-248">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="25afb-249">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="25afb-249">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="25afb-250">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="25afb-250">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="25afb-251">注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="25afb-251">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="25afb-252">重複キー</span><span class="sxs-lookup"><span data-stu-id="25afb-252">Duplicate keys</span></span>

<span data-ttu-id="25afb-253">重複キーが指定されている場合は、最後のキーと値のペアが使用されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-253">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="25afb-254">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="25afb-254">Switch mappings</span></span>

<span data-ttu-id="25afb-255">`ConfigurationBuilder` を使用して構成を手動でビルドする場合、スイッチ マッピング ディクショナリを `AddCommandLine` メソッドに追加できます。</span><span class="sxs-lookup"><span data-stu-id="25afb-255">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="25afb-256">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-256">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="25afb-257">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-257">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="25afb-258">ディクショナリでコマンドライン キーが見つかった場合は、構成を設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-258">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="25afb-259">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="25afb-259">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="25afb-260">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="25afb-260">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="25afb-261">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-261">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="25afb-262">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="25afb-262">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="25afb-263">以下の例では、`GetSwitchMappings` メソッドを使用します。これにより、コマンドライン引数で単一ダッシュ (`-`) キー プレフィックスを使用でき、先頭にサブキーのプレフィックスが付かないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-263">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="25afb-264">コマンドライン引数を指定しないと、`AddInMemoryCollection` に指定されたディクショナリで構成値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-264">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="25afb-265">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="25afb-265">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="25afb-266">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-266">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="25afb-267">構成設定を渡す場合は、以下のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-267">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="25afb-268">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-268">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="25afb-269">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="25afb-269">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="25afb-270">キー</span><span class="sxs-lookup"><span data-stu-id="25afb-270">Key</span></span>            | <span data-ttu-id="25afb-271">[値]</span><span class="sxs-lookup"><span data-stu-id="25afb-271">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="25afb-272">ディクショナリを使用してキーの切り替えを示すために、以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="25afb-272">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="25afb-273">コマンドライン キーが交換されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-273">The command-line keys are swapped.</span></span> <span data-ttu-id="25afb-274">コンソール ウィンドウに、`Profile:MachineName` と `App:MainWindow:Left` の構成値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-274">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="25afb-275">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="25afb-275">web.config file</span></span>

<span data-ttu-id="25afb-276">IIS または IIS-Express でアプリをホストする場合は、*web.config* ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="25afb-276">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="25afb-277">*web.config* を設定することで、[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) を有効にし、アプリを起動して他の IIS 設定とモジュールを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="25afb-277">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="25afb-278">*web.config* ファイルが存在せず、プロジェクト ファイルに `<Project Sdk="Microsoft.NET.Sdk.Web">` が含まれている場合、プロジェクトを発行すると、発行先 (*publish* フォルダー) に *web.config* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-278">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="25afb-279">詳細については、「[IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index#webconfig-file)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-279">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="25afb-280">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="25afb-280">Access configuration during startup</span></span>

<span data-ttu-id="25afb-281">起動中に `ConfigureServices` または `Configure` 内の構成にアクセスするには、[アプリケーションの起動](xref:fundamentals/startup)に関するトピックに示されている例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="25afb-281">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="25afb-282">Razor ページまたは MVC ビューで構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="25afb-282">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="25afb-283">Razor ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](/dotnet/api/microsoft.extensions.configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) をページまたはビューに注入します。</span><span class="sxs-lookup"><span data-stu-id="25afb-283">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="25afb-284">Razor ページ: </span><span class="sxs-lookup"><span data-stu-id="25afb-284">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="25afb-285">MVC ビュー: </span><span class="sxs-lookup"><span data-stu-id="25afb-285">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="25afb-286">補足メモ</span><span class="sxs-lookup"><span data-stu-id="25afb-286">Additional notes</span></span>

* <span data-ttu-id="25afb-287">依存関係挿入 (DI) は、`ConfigureServices` が呼び出されるまでセットアップされません。</span><span class="sxs-lookup"><span data-stu-id="25afb-287">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="25afb-288">構成システムは DI に対応していません。</span><span class="sxs-lookup"><span data-stu-id="25afb-288">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="25afb-289">`IConfiguration` には次の 2 つ特性があります。</span><span class="sxs-lookup"><span data-stu-id="25afb-289">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="25afb-290">`IConfigurationRoot` ルート ノードに使用されます。</span><span class="sxs-lookup"><span data-stu-id="25afb-290">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="25afb-291">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="25afb-291">Can trigger a reload.</span></span>
  * <span data-ttu-id="25afb-292">`IConfigurationSection` 構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="25afb-292">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="25afb-293">`GetSection` および `GetChildren` メソッドは `IConfigurationSection` を返します。</span><span class="sxs-lookup"><span data-stu-id="25afb-293">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="25afb-294">構成を再度読み込むとき、あるいは各プロバイダーにアクセスする場合、[IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) を使用します。</span><span class="sxs-lookup"><span data-stu-id="25afb-294">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="25afb-295">いずれの状況も一般的ではありません。</span><span class="sxs-lookup"><span data-stu-id="25afb-295">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25afb-296">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="25afb-296">Additional resources</span></span>

* [<span data-ttu-id="25afb-297">オプション</span><span class="sxs-lookup"><span data-stu-id="25afb-297">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="25afb-298">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="25afb-298">Work with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="25afb-299">開発中のアプリ シークレットの安全な格納</span><span class="sxs-lookup"><span data-stu-id="25afb-299">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="25afb-300">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="25afb-300">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="25afb-301">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="25afb-301">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="25afb-302">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="25afb-302">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
