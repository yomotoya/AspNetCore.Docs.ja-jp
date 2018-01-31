---
title: "ASP.NET Core の構成"
author: rick-anderson
description: "構成 API を使用して、複数の方法で ASP.NET Core アプリを構成します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 6b9dcfcc2fa380b601eee56095f2e6a6dbe07732
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="7f995-103">ASP.NET Core アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="7f995-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="7f995-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7f995-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7f995-105">構成 API は、名前と値のペアのリストに基づく ASP.NET Core Web アプリの構成方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="7f995-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="7f995-106">構成は実行時に複数のソースから読み取られます。</span><span class="sxs-lookup"><span data-stu-id="7f995-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="7f995-107">名前と値の組は、複数レベルの階層にグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="7f995-108">以下の構成プロバイダーがあります。</span><span class="sxs-lookup"><span data-stu-id="7f995-108">There are configuration providers for:</span></span>

* <span data-ttu-id="7f995-109">ファイル形式 (INI、JSON、および XML)</span><span class="sxs-lookup"><span data-stu-id="7f995-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="7f995-110">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="7f995-110">Command-line arguments</span></span>
* <span data-ttu-id="7f995-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="7f995-111">Environment variables</span></span>
* <span data-ttu-id="7f995-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="7f995-112">In-memory .NET objects</span></span>
* <span data-ttu-id="7f995-113">暗号化されたユーザー ストア</span><span class="sxs-lookup"><span data-stu-id="7f995-113">An encrypted user store</span></span>
* [<span data-ttu-id="7f995-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7f995-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="7f995-115">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="7f995-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="7f995-116">各構成値は文字列キーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="7f995-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="7f995-117">設定をカスタム [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクト (プロパティを持つ単純な .NET クラス) に逆シリアル化するための組み込みのバインド サポートがあります。</span><span class="sxs-lookup"><span data-stu-id="7f995-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="7f995-118">オプション パターンではオプション クラスを使用して、関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="7f995-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="7f995-119">オプション パターンの使用の詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="7f995-120">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7f995-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="7f995-121">JSON 構成</span><span class="sxs-lookup"><span data-stu-id="7f995-121">JSON configuration</span></span>

<span data-ttu-id="7f995-122">次のコンソール アプリでは JSON 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="7f995-123">アプリは以下の構成設定を読み取り、表示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="7f995-124">構成は、コロンで区切られたノードの名前と値のペアの階層リストで構成されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="7f995-125">値を取得するには、対応する項目のキーを使用して `Configuration` インデクサーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="7f995-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="7f995-126">JSON 形式の構成ソースの配列を操作する場合は、コロンで区切られた文字列の一部として配列インデックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="7f995-127">次の例では、上記の `wizards` 配列の最初の項目の名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="7f995-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="7f995-128">組み込みの[構成](/dotnet/api/microsoft.extensions.configuration)プロバイダーに書き込まれた名前と値のペアは永続化**されません**。</span><span class="sxs-lookup"><span data-stu-id="7f995-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="7f995-129">ただし、値を保存するカスタム プロバイダーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="7f995-130">[カスタム構成プロバイダー](xref:fundamentals/configuration/index#custom-config-providers)に関する記述を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="7f995-131">上記のサンプルでは、構成インデクサーを使用して値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="7f995-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="7f995-132">`Startup` の外部の構成にアクセスする場合は、*オプション パターン*を使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="7f995-133">詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="7f995-134">環境別の構成</span><span class="sxs-lookup"><span data-stu-id="7f995-134">Configuration by environment</span></span>

<span data-ttu-id="7f995-135">開発、テスト、運用などの異なる環境に応じて異なる構成を設定するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="7f995-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="7f995-136">ASP.NET Core 2.x アプリの `CreateDefaultBuilder` 拡張メソッド (ASP.NET Core 1.x アプリの場合は、`AddJsonFile` および `AddEnvironmentVariables` を直接使用) では、JSON ファイルとシステム構成ソースを読み取るために構成プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="7f995-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="7f995-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="7f995-137">*appsettings.json*</span></span>
* <span data-ttu-id="7f995-138">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="7f995-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="7f995-139">環境変数</span><span class="sxs-lookup"><span data-stu-id="7f995-139">Environment variables</span></span>

<span data-ttu-id="7f995-140">ASP.NET Core 1.x アプリは、`AddJsonFile` および [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="7f995-141">パラメーターの説明については、「[AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="7f995-142">`reloadOnChange` は、ASP.NET Core 1.1 以降でのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="7f995-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="7f995-143">構成ソースは指定された順に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="7f995-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="7f995-144">先のコードでは、環境変数が最後に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="7f995-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="7f995-145">環境を使用して設定された構成値で、2 つの前述のプロバイダーで設定された値が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="7f995-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="7f995-146">次の *appsettings.Staging.json* ファイルをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7f995-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="7f995-147">環境が `Staging` に設定されているとき、次の `Configure` メソッドは `MyConfig` の値を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="7f995-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="7f995-148">通常、環境は `Development`、`Staging`、または `Production` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="7f995-149">詳細については、[「Working with multiple environments」](xref:fundamentals/environments) (複数の環境での使用) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7f995-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7f995-150">構成に関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="7f995-150">Configuration considerations:</span></span>

* <span data-ttu-id="7f995-151">`IOptionsSnapshot` では、構成データを変更時に再読み込みできます。</span><span class="sxs-lookup"><span data-stu-id="7f995-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="7f995-152">詳細については、「[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="7f995-153">構成キーでは大文字と小文字が区別**されません**。</span><span class="sxs-lookup"><span data-stu-id="7f995-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="7f995-154">構成プロバイダー コードやプレーンテキストの構成ファイルには、パスワードなどの機密データは格納**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="7f995-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="7f995-155">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="7f995-155">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="7f995-156">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="7f995-156">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="7f995-157">[複数の環境での作業](xref:fundamentals/environments)および[開発中のアプリ シークレットの安全な格納場所](xref:security/app-secrets)の管理の詳細を確認してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="7f995-158">ご利用のシステムの環境変数でコロン (`:`) を使用できない場合は、コロン (`:`) をアンダースコア 2 つ (`__`) に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="7f995-158">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="7f995-159">メモリ内プロバイダーと POCO クラスへのバインディング</span><span class="sxs-lookup"><span data-stu-id="7f995-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="7f995-160">次の例では、メモリ内プロバイダーを使用して、クラスにバインドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="7f995-161">構成値は文字列として返されますが、バインディングすることでプロジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="7f995-162">バインディングすることで POCO オブジェクトや、オブジェクト グラフ全体を取得できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-162">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="7f995-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="7f995-163">GetValue</span></span>

<span data-ttu-id="7f995-164">次の例では、[GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 拡張メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="7f995-165">ConfigurationBinder の `GetValue<T>` メソッドでは、既定値 (サンプルでは 80) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-165">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="7f995-166">`GetValue<T>` は単純なシナリオ用であり、セクション全体にはバインドされません。</span><span class="sxs-lookup"><span data-stu-id="7f995-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="7f995-167">`GetValue<T>` は、特定の型に変換された `GetSection(key).Value` からスカラー値を取得します。</span><span class="sxs-lookup"><span data-stu-id="7f995-167">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="7f995-168">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="7f995-168">Bind to an object graph</span></span>

<span data-ttu-id="7f995-169">クラス内の各オブジェクトに再帰的にバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-169">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="7f995-170">次の `AppSettings` クラスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="7f995-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="7f995-171">次のサンプルでは `AppSettings` クラスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="7f995-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="7f995-172">**ASP.NET Core 1.1** 以上では、セクション全体を操作する `Get<T>` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="7f995-173">`Get<T>` は `Bind` を使用するよりも便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="7f995-174">次のコードでは、上記のサンプルで `Get<T>` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="7f995-175">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="7f995-176">プログラムは `Height 11` を表示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="7f995-177">次のコードを使用して、構成の単体テストを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-177">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="7f995-178">Entity Framework のカスタム プロバイダーを作成する</span><span class="sxs-lookup"><span data-stu-id="7f995-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="7f995-179">このセクションでは、EF を使用してデータベースから名前と値のペアを読み取る基本的な構成プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="7f995-180">データベースに構成値を格納するための `ConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="7f995-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="7f995-181">構成した値を格納し、その値にアクセスするための `ConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="7f995-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="7f995-182">[IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f995-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="7f995-183">[ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f995-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="7f995-184">構成プロバイダーは、空の場合にデータベースを初期化します。</span><span class="sxs-lookup"><span data-stu-id="7f995-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="7f995-185">サンプルを実行すると、データベースからの強調表示された値 ("value_from_ef_1" および "value_from_ef_2") が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="7f995-186">構成ソースを追加するための `EFConfigSource` 拡張メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-186">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="7f995-187">次のコードに、カスタム `EFConfigProvider` の使用方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7f995-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="7f995-188">サンプルでは JSON プロバイダーの後にカスタム `EFConfigProvider` を追加するため、データベースからのすべての設定で *appsettings.json* ファイルからの設定がオーバーライドされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="7f995-189">以下の *appsettings.json* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="7f995-190">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="7f995-191">CommandLine 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="7f995-191">CommandLine configuration provider</span></span>

<span data-ttu-id="7f995-192">[CommandLine 構成プロバイダー](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)は、実行時に構成するコマンドライン引数のキーと値のペアを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="7f995-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="7f995-193">CommandLine 構成サンプルを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="7f995-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="7f995-194">CommandLine 構成プロバイダーをセットアップして使用する</span><span class="sxs-lookup"><span data-stu-id="7f995-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="7f995-195">基本構成</span><span class="sxs-lookup"><span data-stu-id="7f995-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="7f995-196">コマンド ライン構成をアクティブにするには、[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) のインスタンスで `AddCommandLine` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7f995-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="7f995-197">コードを実行すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="7f995-198">コマンド ラインで引数のキーと値のペアを渡すと、`Profile:MachineName` と `App:MainWindow:Left` の値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="7f995-199">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="7f995-200">他の構成プロバイダーによって提供された構成をコマンドライン構成でオーバーライドするには、`ConfigurationBuilder` で最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7f995-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7f995-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7f995-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7f995-202">一般的な ASP.NET Core 2.x アプリでは、便利な静的メソッド `CreateDefaultBuilder` を使用してホストを構築します。</span><span class="sxs-lookup"><span data-stu-id="7f995-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="7f995-203">`CreateDefaultBuilder` では、省略可能構成を *appsettings.json*、*appsettings.{Environment}.json*、[user secrets](xref:security/app-secrets) (`Development` 環境の場合)、環境変数、およびコマンドライン引数から読み込みます。</span><span class="sxs-lookup"><span data-stu-id="7f995-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="7f995-204">CommandLine 構成プロバイダーは最後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="7f995-205">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="7f995-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="7f995-206">次のような *appsettings* ファイルの場合:</span><span class="sxs-lookup"><span data-stu-id="7f995-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="7f995-207">`reloadOnChange` が有効です。</span><span class="sxs-lookup"><span data-stu-id="7f995-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="7f995-208">コマンドライン引数と *appsettings* ファイルに同じ設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7f995-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="7f995-209">一致するコマンドライン引数を含む *appsettings* ファイルは、アプリの起動後に変更されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="7f995-210">上記の条件がすべて真になると、コマンドライン引数がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="7f995-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="7f995-211">ASP.NET Core 2.x アプリでは、`CreateDefaultBuilder` ではなく [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-211">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="7f995-212">`WebHostBuilder` を使用する場合は、[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) によって構成を手動で設定します。</span><span class="sxs-lookup"><span data-stu-id="7f995-212">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="7f995-213">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-213">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7f995-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7f995-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7f995-215">[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) を作成し、`AddCommandLine` メソッドを呼び出して CommandLine 構成プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-215">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="7f995-216">最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="7f995-216">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="7f995-217">`UseConfiguration` メソッドを使用して、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-217">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="7f995-218">引数</span><span class="sxs-lookup"><span data-stu-id="7f995-218">Arguments</span></span>

<span data-ttu-id="7f995-219">コマンド ラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに準拠する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-219">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="7f995-220">引数の形式</span><span class="sxs-lookup"><span data-stu-id="7f995-220">Argument format</span></span>                                                     | <span data-ttu-id="7f995-221">例</span><span class="sxs-lookup"><span data-stu-id="7f995-221">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="7f995-222">単一引数: 等号 (`=`) で区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="7f995-222">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="7f995-223">2 つの引数のシーケンス: スペースで区切られたキーと値のペア</span><span class="sxs-lookup"><span data-stu-id="7f995-223">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="7f995-224">**単一引数**</span><span class="sxs-lookup"><span data-stu-id="7f995-224">**Single argument**</span></span>

<span data-ttu-id="7f995-225">値は等号 (`=`) の後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-225">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="7f995-226">値を null にすることができます (例: `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="7f995-226">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="7f995-227">キーにはプレフィックスが付く場合があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-227">The key may have a prefix.</span></span>

| <span data-ttu-id="7f995-228">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="7f995-228">Key prefix</span></span>               | <span data-ttu-id="7f995-229">例</span><span class="sxs-lookup"><span data-stu-id="7f995-229">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="7f995-230">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="7f995-230">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="7f995-231">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="7f995-231">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="7f995-232">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="7f995-232">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="7f995-233">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="7f995-233">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="7f995-234">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="7f995-234">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="7f995-235">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="7f995-235">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="7f995-236">注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="7f995-236">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="7f995-237">**2 つの引数のシーケンス**</span><span class="sxs-lookup"><span data-stu-id="7f995-237">**Sequence of two arguments**</span></span>

<span data-ttu-id="7f995-238">値を null にすることはできません。値はスペースで区切られたキーの後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-238">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="7f995-239">キーにはプレフィックスを付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-239">The key must have a prefix.</span></span>

| <span data-ttu-id="7f995-240">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="7f995-240">Key prefix</span></span>               | <span data-ttu-id="7f995-241">例</span><span class="sxs-lookup"><span data-stu-id="7f995-241">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="7f995-242">単一のダッシュ (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="7f995-242">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="7f995-243">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="7f995-243">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="7f995-244">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="7f995-244">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="7f995-245">&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。</span><span class="sxs-lookup"><span data-stu-id="7f995-245">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="7f995-246">コマンド例:</span><span class="sxs-lookup"><span data-stu-id="7f995-246">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="7f995-247">注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="7f995-247">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="7f995-248">重複キー</span><span class="sxs-lookup"><span data-stu-id="7f995-248">Duplicate keys</span></span>

<span data-ttu-id="7f995-249">重複キーが指定されている場合は、最後のキーと値のペアが使用されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-249">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="7f995-250">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="7f995-250">Switch mappings</span></span>

<span data-ttu-id="7f995-251">`ConfigurationBuilder` を使用して構成を手動でビルドする場合、スイッチ マッピング ディクショナリを `AddCommandLine` メソッドに追加できます。</span><span class="sxs-lookup"><span data-stu-id="7f995-251">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="7f995-252">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-252">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="7f995-253">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-253">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="7f995-254">ディクショナリでコマンドライン キーが見つかった場合は、構成を設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-254">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="7f995-255">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="7f995-255">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="7f995-256">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="7f995-256">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="7f995-257">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-257">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="7f995-258">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="7f995-258">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="7f995-259">以下の例では、`GetSwitchMappings` メソッドを使用します。これにより、コマンドライン引数で単一ダッシュ (`-`) キー プレフィックスを使用でき、先頭にサブキーのプレフィックスが付かないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-259">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="7f995-260">コマンドライン引数を指定しないと、`AddInMemoryCollection` に指定されたディクショナリで構成値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-260">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="7f995-261">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="7f995-261">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="7f995-262">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-262">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="7f995-263">構成設定を渡す場合は、以下のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-263">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="7f995-264">コンソール ウィンドウには次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-264">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="7f995-265">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7f995-265">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="7f995-266">キー</span><span class="sxs-lookup"><span data-stu-id="7f995-266">Key</span></span>            | <span data-ttu-id="7f995-267">[値]</span><span class="sxs-lookup"><span data-stu-id="7f995-267">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="7f995-268">ディクショナリを使用してキーの切り替えを示すために、以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7f995-268">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="7f995-269">コマンドライン キーが交換されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-269">The command-line keys are swapped.</span></span> <span data-ttu-id="7f995-270">コンソール ウィンドウに、`Profile:MachineName` と `App:MainWindow:Left` の構成値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-270">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="7f995-271">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="7f995-271">The web.config file</span></span>

<span data-ttu-id="7f995-272">IIS または IIS-Express でアプリをホストする場合は、*web.config* ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="7f995-272">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="7f995-273">*web.config* を設定することで、[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) を有効にし、アプリを起動して他の IIS 設定とモジュールを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="7f995-273">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="7f995-274">*web.config* ファイルが存在せず、プロジェクト ファイルに `<Project Sdk="Microsoft.NET.Sdk.Web">` が含まれている場合、プロジェクトを発行すると、発行先 (*publish* フォルダー) に *web.config* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-274">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="7f995-275">詳細については、「[IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index#webconfig)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-275">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="7f995-276">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="7f995-276">Accessing configuration during startup</span></span>

<span data-ttu-id="7f995-277">起動中に `ConfigureServices` または `Configure` 内の構成にアクセスするには、[アプリケーションの起動](xref:fundamentals/startup)に関するトピックに示されている例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f995-277">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="7f995-278">補足メモ</span><span class="sxs-lookup"><span data-stu-id="7f995-278">Additional notes</span></span>

* <span data-ttu-id="7f995-279">依存関係挿入 (DI) は、`ConfigureServices` が呼び出されるまでセットアップされません。</span><span class="sxs-lookup"><span data-stu-id="7f995-279">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="7f995-280">構成システムは DI に対応していません。</span><span class="sxs-lookup"><span data-stu-id="7f995-280">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="7f995-281">`IConfiguration` には次の 2 つ特性があります。</span><span class="sxs-lookup"><span data-stu-id="7f995-281">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="7f995-282">`IConfigurationRoot` ルート ノードに使用されます。</span><span class="sxs-lookup"><span data-stu-id="7f995-282">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="7f995-283">再読み込みをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="7f995-283">Can trigger a reload.</span></span>
  * <span data-ttu-id="7f995-284">`IConfigurationSection` 構成値のセクションを表します。</span><span class="sxs-lookup"><span data-stu-id="7f995-284">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="7f995-285">`GetSection` および `GetChildren` メソッドは `IConfigurationSection` を返します。</span><span class="sxs-lookup"><span data-stu-id="7f995-285">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="7f995-286">構成を再度読み込むとき、あるいは各プロバイダーにアクセスする場合、[IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) を使用します。</span><span class="sxs-lookup"><span data-stu-id="7f995-286">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="7f995-287">いずれの状況も一般的ではありません。</span><span class="sxs-lookup"><span data-stu-id="7f995-287">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f995-288">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7f995-288">Additional resources</span></span>

* [<span data-ttu-id="7f995-289">オプション</span><span class="sxs-lookup"><span data-stu-id="7f995-289">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="7f995-290">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="7f995-290">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="7f995-291">開発中のアプリ シークレットの安全な保存</span><span class="sxs-lookup"><span data-stu-id="7f995-291">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="7f995-292">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="7f995-292">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="7f995-293">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="7f995-293">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="7f995-294">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="7f995-294">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
