---
title: ASP.NET Core の構成
author: guardrex
description: 構成 API を使用して、ASP.NET Core アプリを構成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 35f283becd156da22a4d9d2034055ee79b75ffda
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326174"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="14804-103">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="14804-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="14804-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="14804-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="14804-105">ASP.NET Core でのアプリの構成は、"*構成プロバイダー*" によって設定するキーと値のペアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="14804-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="14804-106">構成プロバイダーは、さまざまな構成のソースから構成データを読み取り、キーと値のペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="14804-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="14804-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="14804-107">Azure Key Vault</span></span>
* <span data-ttu-id="14804-108">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="14804-108">Command-line arguments</span></span>
* <span data-ttu-id="14804-109">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="14804-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="14804-110">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="14804-110">Directory files</span></span>
* <span data-ttu-id="14804-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-111">Environment variables</span></span>
* <span data-ttu-id="14804-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="14804-112">In-memory .NET objects</span></span>
* <span data-ttu-id="14804-113">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="14804-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="14804-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="14804-114">Azure Key Vault</span></span>
* <span data-ttu-id="14804-115">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="14804-115">Command-line arguments</span></span>
* <span data-ttu-id="14804-116">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="14804-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="14804-117">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-117">Environment variables</span></span>
* <span data-ttu-id="14804-118">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="14804-118">In-memory .NET objects</span></span>
* <span data-ttu-id="14804-119">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="14804-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="14804-120">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="14804-120">Command-line arguments</span></span>
* <span data-ttu-id="14804-121">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="14804-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="14804-122">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-122">Environment variables</span></span>
* <span data-ttu-id="14804-123">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="14804-123">In-memory .NET objects</span></span>
* <span data-ttu-id="14804-124">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="14804-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="14804-125">"*オプション パターン*" は、このトピックで説明する構成の概念を拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="14804-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="14804-126">オプションでは、クラスを使用して関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="14804-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="14804-127">オプション パターンの使用について詳しくは、「<xref:fundamentals/configuration/options>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="14804-128">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="14804-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="14804-129">このトピックで提供される例は、次のものに依存しています。</span><span class="sxs-lookup"><span data-stu-id="14804-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="14804-130"><xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> によるアプリの基本パスの設定。</span><span class="sxs-lookup"><span data-stu-id="14804-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="14804-131">[Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージを参照することで、アプリが `SetBasePath` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="14804-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="14804-132"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> による構成ファイルのセクションの解決。</span><span class="sxs-lookup"><span data-stu-id="14804-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="14804-133">[Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージを参照することで、アプリが `GetSection` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="14804-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="14804-134"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> および [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) による構成の .NET クラスへのバインド。</span><span class="sxs-lookup"><span data-stu-id="14804-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="14804-135">[Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージを参照することで、アプリが `Bind` と `Get<T>` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="14804-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="14804-136">`Get<T>` は、ASP.NET Core 1.1 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-137">これらの 3 つのパッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-138">これらの 3 つのパッケージは、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="14804-139">ホストとアプリの構成</span><span class="sxs-lookup"><span data-stu-id="14804-139">Host vs. app configuration</span></span>

<span data-ttu-id="14804-140">アプリを構成して起動する前に、"*ホスト*" を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="14804-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="14804-141">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="14804-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="14804-142">アプリとホストは、両方ともこのトピックで説明する構成プロバイダーを使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="14804-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="14804-143">ホストの構成のキーと値のペアは、アプリのグローバル構成の一部となります。</span><span class="sxs-lookup"><span data-stu-id="14804-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="14804-144">ホストをビルドするときの構成プロバイダーの使用方法、およびホストの構成に対する構成ソースの影響について詳しくは、「<xref:fundamentals/host/index>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="14804-145">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="14804-145">Security</span></span>

<span data-ttu-id="14804-146">次のベスト プラクティスを採用します。</span><span class="sxs-lookup"><span data-stu-id="14804-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="14804-147">構成プロバイダーのコードやプレーンテキストの構成ファイルには、パスワードなどの機密データを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="14804-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="14804-148">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="14804-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="14804-149">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="14804-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="14804-150">[複数の環境を使用する方法](xref:fundamentals/environments)や、[シークレット マネージャーでの開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)の管理 (機密データを格納するための環境変数の使用に関するアドバイスを含む) について、さらに学習することができます。</span><span class="sxs-lookup"><span data-stu-id="14804-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="14804-151">シークレット マネージャーは、ファイル構成プロバイダーを使用して、ユーザーの機密情報をローカル システム上の JSON ファイルに格納します。</span><span class="sxs-lookup"><span data-stu-id="14804-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="14804-152">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="14804-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="14804-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) は、アプリのシークレットを安全に格納するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="14804-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="14804-154">詳細については、「<xref:security/key-vault-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="14804-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="14804-155">階層的な構成データ</span><span class="sxs-lookup"><span data-stu-id="14804-155">Hierarchical configuration data</span></span>

<span data-ttu-id="14804-156">構成 API は、構成キー内の区切り記号を使用して階層データをフラット化することにより、階層的な構成データを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="14804-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="14804-157">次の JSON ファイルでは、2 つのセクションの構造化階層に 4 つのキーが存在します。</span><span class="sxs-lookup"><span data-stu-id="14804-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="14804-158">ファイルが構成に読み込まれると、構成ソースの元の階層データ構造を維持するために、一意なキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="14804-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="14804-159">セクションとキーは、元の構造を維持するために、コロン (`:`) を使用してフラット化されます。</span><span class="sxs-lookup"><span data-stu-id="14804-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="14804-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="14804-160">section0:key0</span></span>
* <span data-ttu-id="14804-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="14804-161">section0:key1</span></span>
* <span data-ttu-id="14804-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="14804-162">section1:key0</span></span>
* <span data-ttu-id="14804-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="14804-163">section1:key1</span></span>

<span data-ttu-id="14804-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> メソッドと <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> メソッドを使用して、構成データ内のセクションとセクションの子を分離することができます。</span><span class="sxs-lookup"><span data-stu-id="14804-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="14804-165">これらのメソッドについては、後ほど「[GetSection、GetChildren、Exists](#getsection-getchildren-and-exists)」で説明します。</span><span class="sxs-lookup"><span data-stu-id="14804-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="14804-166">規約</span><span class="sxs-lookup"><span data-stu-id="14804-166">Conventions</span></span>

<span data-ttu-id="14804-167">アプリの起動時に、各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="14804-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="14804-168">ファイル構成プロバイダーには、アプリの起動後に基になる設定ファイルが変更された場合に、構成を再読み込みする機能があります。</span><span class="sxs-lookup"><span data-stu-id="14804-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="14804-169">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="14804-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="14804-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> は、アプリの[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) コンテナーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="14804-171">構成プロバイダーでは DI を使用できません。ホストによって構成プロバイダーが設定されている場合、DI を使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="14804-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="14804-172">構成キーでは、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="14804-173">キーでは、大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="14804-173">Keys are case-insensitive.</span></span> <span data-ttu-id="14804-174">たとえば、`ConnectionString` と `connectionstring` は同等のキーとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="14804-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="14804-175">同じキーに対する値が、同じまたは別の構成プロバイダーによって設定された場合、最後にキーに設定された値が使用される値となります。</span><span class="sxs-lookup"><span data-stu-id="14804-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="14804-176">階層キー</span><span class="sxs-lookup"><span data-stu-id="14804-176">Hierarchical keys</span></span>
  * <span data-ttu-id="14804-177">構成 API 内では、すべてのプラットフォームでコロン (`:`) の区切りが機能します。</span><span class="sxs-lookup"><span data-stu-id="14804-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="14804-178">環境変数内では、コロン区切りがすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="14804-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="14804-179">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="14804-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="14804-180">Azure Key Vault では、階層キーは区切り記号として `--` (2 つのダッシュ) を使用します。</span><span class="sxs-lookup"><span data-stu-id="14804-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="14804-181">コードを指定して、アプリの構成にシークレットが読み込まれるときにダッシュをコロンで置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="14804-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="14804-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="14804-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="14804-183">配列のバインドについては、「[配列をクラスにバインドする](#bind-an-array-to-a-class)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="14804-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="14804-184">構成値では、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="14804-185">値は文字列です。</span><span class="sxs-lookup"><span data-stu-id="14804-185">Values are strings.</span></span>
* <span data-ttu-id="14804-186">Null 値を構成に格納したり、オブジェクトにバインドしたりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="14804-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="14804-187">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-187">Providers</span></span>

<span data-ttu-id="14804-188">ASP.NET Core アプリで使用できる構成プロバイダーを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="14804-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="14804-189">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-189">Provider</span></span> | <span data-ttu-id="14804-190">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="14804-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="14804-191">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="14804-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="14804-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="14804-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="14804-193">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="14804-194">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="14804-194">Command-line parameters</span></span> |
| [<span data-ttu-id="14804-195">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="14804-196">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="14804-196">Custom source</span></span> |
| [<span data-ttu-id="14804-197">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="14804-198">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-198">Environment variables</span></span> |
| [<span data-ttu-id="14804-199">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="14804-200">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="14804-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="14804-201">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="14804-202">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="14804-202">Directory files</span></span> |
| [<span data-ttu-id="14804-203">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="14804-204">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="14804-204">In-memory collections</span></span> |
| <span data-ttu-id="14804-205">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="14804-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="14804-206">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="14804-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="14804-207">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-207">Provider</span></span> | <span data-ttu-id="14804-208">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="14804-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="14804-209">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="14804-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="14804-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="14804-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="14804-211">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="14804-212">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="14804-212">Command-line parameters</span></span> |
| [<span data-ttu-id="14804-213">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="14804-214">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="14804-214">Custom source</span></span> |
| [<span data-ttu-id="14804-215">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="14804-216">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-216">Environment variables</span></span> |
| [<span data-ttu-id="14804-217">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="14804-218">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="14804-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="14804-219">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="14804-220">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="14804-220">In-memory collections</span></span> |
| <span data-ttu-id="14804-221">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="14804-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="14804-222">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="14804-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="14804-223">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-223">Provider</span></span> | <span data-ttu-id="14804-224">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="14804-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="14804-225">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="14804-226">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="14804-226">Command-line parameters</span></span> |
| [<span data-ttu-id="14804-227">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="14804-228">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="14804-228">Custom source</span></span> |
| [<span data-ttu-id="14804-229">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="14804-230">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-230">Environment variables</span></span> |
| [<span data-ttu-id="14804-231">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="14804-232">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="14804-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="14804-233">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="14804-234">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="14804-234">In-memory collections</span></span> |
| <span data-ttu-id="14804-235">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="14804-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="14804-236">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="14804-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="14804-237">アプリの起動時に各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="14804-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="14804-238">このトピックで説明する構成プロバイダーは、それらをコードで配置する順ではなく、アルファベット順で説明します。</span><span class="sxs-lookup"><span data-stu-id="14804-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="14804-239">基になる構成ソースの優先順位に合わせるために、コード内で構成プロバイダーを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="14804-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="14804-240">一般的な一連の構成プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="14804-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="14804-241">ファイル (*appsettings.json*、*appsettings.{Environment}.json*。`{Environment}` はアプリの現在のホスト環境です)</span><span class="sxs-lookup"><span data-stu-id="14804-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="14804-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="14804-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="14804-243">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合のみ)</span><span class="sxs-lookup"><span data-stu-id="14804-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="14804-244">環境変数</span><span class="sxs-lookup"><span data-stu-id="14804-244">Environment variables</span></span>
1. <span data-ttu-id="14804-245">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="14804-245">Command-line arguments</span></span>

<span data-ttu-id="14804-246">コマンド ライン引数が他のプロバイダーによって設定された構成をオーバーライドできるようにするために、コマンド ラインの構成プロバイダーを一連のプロバイダーの最後に配置するのは、一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="14804-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-247">この一連のプロバイダーは、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を使用して新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化するときに配置されます。</span><span class="sxs-lookup"><span data-stu-id="14804-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="14804-248">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-249"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> および `Startup` 内のその <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> メソッドの呼び出しを使用して、(ホストではなく) アプリ用に一連のプロバイダーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="14804-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="14804-250">前の例では、<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> によって環境名 (`env.EnvironmentName`) とアプリのアセンブリ名 (`env.ApplicationName`) が提供されます。</span><span class="sxs-lookup"><span data-stu-id="14804-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="14804-251">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="14804-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="14804-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="14804-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="14804-253">Web ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出し、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> によって自動的に追加されるものに加え、アプリの構成プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="14804-254">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="14804-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> では、実行時にコマンドライン引数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="14804-256">コマンド ライン構成をアクティブにするために、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドが <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-257"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddCommandLine` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="14804-258">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="14804-259">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="14804-260">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="14804-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="14804-261">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="14804-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="14804-262">環境変数。</span><span class="sxs-lookup"><span data-stu-id="14804-262">Environment variables.</span></span>

<span data-ttu-id="14804-263">`CreateDefaultBuilder` はコマンド ライン構成プロバイダーを最後に追加します。</span><span class="sxs-lookup"><span data-stu-id="14804-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="14804-264">実行時に渡されるコマンド ライン引数によって、他のプロバイダーによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="14804-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="14804-265">`CreateDefaultBuilder` は、ホストが作成されるときに機能します。</span><span class="sxs-lookup"><span data-stu-id="14804-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="14804-266">そのため、`CreateDefaultBuilder` によってアクティブ化されるコマンド ライン構成によって、ホストの構成方法に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-267">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="14804-268">`AddCommandLine` は既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="14804-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14804-269">アプリの構成を指定して、引き続きコマンドラインの引数でその構成をオーバーライドできるようにする必要がある場合、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> でアプリの追加のプロバイダーを呼び出し、最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-270"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-271"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="14804-272">`UseConfiguration` が呼び出される場合、`AddCommandLine` は既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="14804-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="14804-273">アプリの構成を指定して、引き続きコマンドラインの引数でその構成をオーバーライドできるようにする必要がある場合、`ConfigurationBuilder` でアプリの追加のプロバイダーを呼び出し、最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-274"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-275">コマンド ライン構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-276">最後にプロバイダーを呼び出すことにより、実行時に渡されるコマンドライン引数で、他の構成プロバイダーによって設定された構成をオーバーライドできるようにします。</span><span class="sxs-lookup"><span data-stu-id="14804-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="14804-277"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="14804-278">**例**</span><span class="sxs-lookup"><span data-stu-id="14804-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-279">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-280">1.x のサンプル アプリでは、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> の <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="14804-281">プロジェクトのディレクトリでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="14804-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="14804-282">`dotnet run` コマンドにコマンドライン引数を指定します (`dotnet run CommandLineKey=CommandLineValue`)。</span><span class="sxs-lookup"><span data-stu-id="14804-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="14804-283">アプリを実行したら、アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="14804-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="14804-284">出力に、`dotnet run` に提供される構成のコマンド ライン引数のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="14804-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="14804-285">引数</span><span class="sxs-lookup"><span data-stu-id="14804-285">Arguments</span></span>

<span data-ttu-id="14804-286">値は等号 (`=`) の後に続ける必要があります。または、値をスペースの後に続ける場合は、キーにプレフィックス (`--`または`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="14804-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="14804-287">等号を使用する場合は、値に null を指定できます (例: `CommandLineKey=`)。</span><span class="sxs-lookup"><span data-stu-id="14804-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="14804-288">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="14804-288">Key prefix</span></span>               | <span data-ttu-id="14804-289">例</span><span class="sxs-lookup"><span data-stu-id="14804-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="14804-290">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="14804-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="14804-291">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="14804-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="14804-292">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="14804-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="14804-293">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="14804-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="14804-294">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="14804-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="14804-295">同じコマンド内のコマンド ライン引数で、等号を使用するキーと値のペアと、スペースを使用するキーと値のペアを混在させないでください。</span><span class="sxs-lookup"><span data-stu-id="14804-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="14804-296">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="14804-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="14804-297">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="14804-297">Switch mappings</span></span>

<span data-ttu-id="14804-298">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="14804-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="14804-299"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> で構成を手動でビルドするときに、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> メソッドにスイッチ置換のディクショナリを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="14804-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="14804-300">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="14804-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="14804-301">コマンド ライン キーがディクショナリで見つかった場合は、アプリの構成にキーと値のペアを設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="14804-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="14804-302">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="14804-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="14804-303">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="14804-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="14804-304">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="14804-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="14804-305">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="14804-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-306">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-307">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="14804-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="14804-308">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="14804-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14804-309">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="14804-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="14804-310">ソリューションでは `CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-311">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="14804-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="14804-312">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="14804-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14804-313">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="14804-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="14804-314">ソリューションでは `CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="14804-315">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="14804-316">キー</span><span class="sxs-lookup"><span data-stu-id="14804-316">Key</span></span>       | <span data-ttu-id="14804-317">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="14804-318">アプリの起動時にスイッチ マッピングされたキーを使用する場合、構成は、ディクショナリによって指定されたキーでの構成値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="14804-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="14804-319">上記のコマンドを実行すると、次の表に示す値が構成に含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="14804-320">キー</span><span class="sxs-lookup"><span data-stu-id="14804-320">Key</span></span>               | <span data-ttu-id="14804-321">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="14804-322">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="14804-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> では、実行時に環境変数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="14804-324">環境変数の構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-325">環境変数内で階層キーを操作する場合、コロン区切り (`:`) がすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="14804-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="14804-326">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに置換されます。</span><span class="sxs-lookup"><span data-stu-id="14804-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="14804-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) を使用すると、環境変数構成プロバイダーを使用してアプリの構成をオーバーライドすることができる環境変数を、Azure Portal で設定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="14804-328">詳細については、[Azure アプリの、Azure Portal を使用したアプリの構成のオーバーライド](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-329"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddEnvironmentVariables` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="14804-330">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="14804-331">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="14804-332">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="14804-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="14804-333">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="14804-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="14804-334">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="14804-334">Command-line arguments.</span></span>

<span data-ttu-id="14804-335">ユーザー シークレットと *appsettings* ファイルから構成が設定された後に、環境変数構成プロバイダーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-335">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="14804-336">この位置でプロバイダーを呼び出すことにより、実行時に読み込まれた環境変数が、ユーザー シークレットと *appsettings* ファイルによって設定された構成をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-337">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="14804-338">`ASPNETCORE_` というプレフィックスが付いた環境変数に対する `AddEnvironmentVariables` は、既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="14804-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14804-339">追加の環境変数からアプリの構成を指定する必要がある場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリの追加プロバイダーを呼び出し、そのプレフィックスを含む `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_")
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-340"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-341"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddEnvironmentVariables` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="14804-342"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="14804-343">`ASPNETCORE_` というプレフィックスが付いた環境変数に対する `AddEnvironmentVariables` は、既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="14804-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14804-344">追加の環境変数からアプリの構成を指定する必要がある場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリの追加プロバイダーを呼び出し、そのプレフィックスを含む `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-345"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-346"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="14804-347">**例**</span><span class="sxs-lookup"><span data-stu-id="14804-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-348">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには `AddEnvironmentVariables` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-349">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="14804-350">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="14804-350">Run the sample app.</span></span> <span data-ttu-id="14804-351">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="14804-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="14804-352">出力に、環境変数 `ENVIRONMENT` のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="14804-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="14804-353">値には、アプリを実行している環境が反映されます (ローカルで実行している場合は通常 `Development`)。</span><span class="sxs-lookup"><span data-stu-id="14804-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="14804-354">アプリによって表示される環境変数のリストを短くするために、アプリでは次で始まる環境変数がフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="14804-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="14804-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="14804-355">ASPNETCORE_</span></span>
* <span data-ttu-id="14804-356">urls</span><span class="sxs-lookup"><span data-stu-id="14804-356">urls</span></span>
* <span data-ttu-id="14804-357">ログの記録</span><span class="sxs-lookup"><span data-stu-id="14804-357">Logging</span></span>
* <span data-ttu-id="14804-358">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="14804-358">ENVIRONMENT</span></span>
* <span data-ttu-id="14804-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="14804-359">contentRoot</span></span>
* <span data-ttu-id="14804-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="14804-360">AllowedHosts</span></span>
* <span data-ttu-id="14804-361">applicationName</span><span class="sxs-lookup"><span data-stu-id="14804-361">applicationName</span></span>
* <span data-ttu-id="14804-362">CommandLine</span><span class="sxs-lookup"><span data-stu-id="14804-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-363">アプリで使用できるすべての環境変数を公開する場合は、*Pages/Index.cshtml.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="14804-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-364">アプリで使用できるすべての環境変数を公開する場合は、*Controllers/HomeController.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="14804-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="14804-365">プレフィックス</span><span class="sxs-lookup"><span data-stu-id="14804-365">Prefixes</span></span>

<span data-ttu-id="14804-366">アプリの構成に読み込まれる環境変数は、`AddEnvironmentVariables` メソッドにプレフィックスを指定すると、フィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="14804-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="14804-367">たとえば、プレフィックス `CUSTOM_` で環境変数をフィルター処理するには、構成プロバイダーにプレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="14804-368">構成のキーと値のペアが作成されるときに、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="14804-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-369">静的な簡易メソッド `CreateDefaultBuilder` によって <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> が作成され、アプリのホストが確立されます。</span><span class="sxs-lookup"><span data-stu-id="14804-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="14804-370">`WebHostBuilder` は、作成されるときに、`ASPNETCORE_` というプレフィックスが付いた環境変数からホストの構成を見つけます。</span><span class="sxs-lookup"><span data-stu-id="14804-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="14804-371">**接続文字列のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="14804-371">**Connection string prefixes**</span></span>

<span data-ttu-id="14804-372">構成 API には、アプリの環境に向けた Azure の接続文字列の構成に関係する、4 つの接続文字列環境変数のための特別な処理規則があります。</span><span class="sxs-lookup"><span data-stu-id="14804-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="14804-373">表に示されるプレフィックスを含む環境変数は、`AddEnvironmentVariables` にプレフィックスが指定されていない場合、アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="14804-374">接続文字列のプレフィックス</span><span class="sxs-lookup"><span data-stu-id="14804-374">Connection string prefix</span></span> | <span data-ttu-id="14804-375">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="14804-376">カスタム プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="14804-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="14804-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="14804-378">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="14804-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="14804-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="14804-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="14804-380">表に示す 4 つのプレフィックスのいずれかを使用して、環境変数が検出され構成に読み込まれた場合:</span><span class="sxs-lookup"><span data-stu-id="14804-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="14804-381">環境変数のプレフィックスを削除し、構成キーのセクション (`ConnectionStrings`) を追加することによって、構成キーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="14804-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="14804-382">データベースの接続プロバイダーを表す新しい構成のキーと値のペアが作成されます (示されたプロバイダーを含まない `CUSTOMCONNSTR_` を除く)。</span><span class="sxs-lookup"><span data-stu-id="14804-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="14804-383">環境変数キー</span><span class="sxs-lookup"><span data-stu-id="14804-383">Environment variable key</span></span> | <span data-ttu-id="14804-384">変換された構成キー</span><span class="sxs-lookup"><span data-stu-id="14804-384">Converted configuration key</span></span> | <span data-ttu-id="14804-385">プロバイダーの構成エントリ</span><span class="sxs-lookup"><span data-stu-id="14804-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="14804-386">構成エントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="14804-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="14804-387">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="14804-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="14804-388">値: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="14804-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="14804-389">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="14804-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="14804-390">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="14804-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="14804-391">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="14804-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="14804-392">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="14804-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="14804-393">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-393">File Configuration Provider</span></span>

<span data-ttu-id="14804-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> は、ファイル システムから構成を読み込むための基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="14804-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="14804-395">次の構成プロバイダーは、特定のファイルの種類専用です。</span><span class="sxs-lookup"><span data-stu-id="14804-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="14804-396">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="14804-397">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="14804-398">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="14804-399">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-399">INI Configuration Provider</span></span>

<span data-ttu-id="14804-400"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> では、実行時に INI ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="14804-401">INI ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-402">INI ファイルの構成では、セクションの区切り記号としてコロンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="14804-403">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="14804-404">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-404">Whether the file is optional.</span></span>
* <span data-ttu-id="14804-405">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="14804-406">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-407">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-408"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-409">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-410"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-411"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="14804-412">INI 構成ファイルの汎用的な例:</span><span class="sxs-lookup"><span data-stu-id="14804-412">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="14804-413">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="14804-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="14804-414">section0:key0</span></span>
* <span data-ttu-id="14804-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="14804-415">section0:key1</span></span>
* <span data-ttu-id="14804-416">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="14804-416">section1:subsection:key</span></span>
* <span data-ttu-id="14804-417">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="14804-417">section2:subsection0:key</span></span>
* <span data-ttu-id="14804-418">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="14804-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="14804-419">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-419">JSON Configuration Provider</span></span>

<span data-ttu-id="14804-420"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> では、実行時に JSON ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="14804-421">JSON ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-422">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="14804-423">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-423">Whether the file is optional.</span></span>
* <span data-ttu-id="14804-424">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="14804-425">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-426"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddJsonFile` が 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="14804-427">このメソッドは、次から構成を読み込むために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="14804-428">*appsettings.json* &ndash; このファイルが最初に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="14804-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="14804-429">ファイルの環境バージョンは、*appsettings.json* ファイルによって指定される値をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="14804-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="14804-430">*appsettings.{Environment}.json* &ndash; ファイルの環境バージョンは、[IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) に基づいて読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="14804-431">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="14804-432">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="14804-433">環境変数。</span><span class="sxs-lookup"><span data-stu-id="14804-433">Environment variables.</span></span>
* <span data-ttu-id="14804-434">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="14804-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="14804-435">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="14804-435">Command-line arguments.</span></span>

<span data-ttu-id="14804-436">JSON 構成プロバイダーが最初に確立されます。</span><span class="sxs-lookup"><span data-stu-id="14804-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="14804-437">このため、ユーザー シークレット、環境変数、およびコマンド ライン引数によって、*appsettings* ファイルによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="14804-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-438">ホストのビルド時に <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、*appsettings.json* と *appsettings.{Environment}.json* 以外のファイルにアプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-439"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-440">また、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddJsonFile` 拡張メソッドを直接呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="14804-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-441">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-442"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-443"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="14804-444">**例**</span><span class="sxs-lookup"><span data-stu-id="14804-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-445">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには 2 回の `AddJsonFile` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="14804-446">構成は、*appsettings.json* および *appsettings.{Environment}.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-447">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddJsonFile` を 2 回呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="14804-448">構成は、*appsettings.json* および *appsettings.{Environment}.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="14804-449">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="14804-449">Run the sample app.</span></span> <span data-ttu-id="14804-450">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="14804-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="14804-451">出力に、環境に応じて、表に示す構成のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="14804-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="14804-452">ログの構成キーでは、階層の区切り文字としてコロン (`:`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="14804-453">キー</span><span class="sxs-lookup"><span data-stu-id="14804-453">Key</span></span>                        | <span data-ttu-id="14804-454">開発時の値</span><span class="sxs-lookup"><span data-stu-id="14804-454">Development Value</span></span> | <span data-ttu-id="14804-455">運用時の値</span><span class="sxs-lookup"><span data-stu-id="14804-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="14804-456">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="14804-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="14804-457">情報</span><span class="sxs-lookup"><span data-stu-id="14804-457">Information</span></span>       | <span data-ttu-id="14804-458">情報</span><span class="sxs-lookup"><span data-stu-id="14804-458">Information</span></span>      |
| <span data-ttu-id="14804-459">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="14804-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="14804-460">情報</span><span class="sxs-lookup"><span data-stu-id="14804-460">Information</span></span>       | <span data-ttu-id="14804-461">情報</span><span class="sxs-lookup"><span data-stu-id="14804-461">Information</span></span>      |
| <span data-ttu-id="14804-462">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="14804-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="14804-463">デバッグ</span><span class="sxs-lookup"><span data-stu-id="14804-463">Debug</span></span>             | <span data-ttu-id="14804-464">Error</span><span class="sxs-lookup"><span data-stu-id="14804-464">Error</span></span>            |
| <span data-ttu-id="14804-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="14804-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="14804-466">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-466">XML Configuration Provider</span></span>

<span data-ttu-id="14804-467"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> では、実行時に XML ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="14804-468">XML ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-469">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="14804-470">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-470">Whether the file is optional.</span></span>
* <span data-ttu-id="14804-471">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="14804-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="14804-472">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="14804-473">構成ファイルのルート ノードは、構成のキーと値のペアを作成するときには無視されます。</span><span class="sxs-lookup"><span data-stu-id="14804-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="14804-474">ファイル内でドキュメント型定義 (DTD) または名前空間を指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="14804-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-475">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-476"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-477">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-478"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-479"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="14804-480">XML 構成ファイルでは、繰り返しのセクション用に個別の要素名を使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-480">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="14804-481">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="14804-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="14804-482">section0:key0</span></span>
* <span data-ttu-id="14804-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="14804-483">section0:key1</span></span>
* <span data-ttu-id="14804-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="14804-484">section1:key0</span></span>
* <span data-ttu-id="14804-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="14804-485">section1:key1</span></span>

<span data-ttu-id="14804-486">同じ要素名を使用する要素の繰り返しは、要素を区別するために `name` 属性を使用する場合に機能します。</span><span class="sxs-lookup"><span data-stu-id="14804-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="14804-487">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="14804-488">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="14804-488">section:section0:key:key0</span></span>
* <span data-ttu-id="14804-489">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="14804-489">section:section0:key:key1</span></span>
* <span data-ttu-id="14804-490">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="14804-490">section:section1:key:key0</span></span>
* <span data-ttu-id="14804-491">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="14804-491">section:section1:key:key1</span></span>

<span data-ttu-id="14804-492">値を指定するために属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="14804-493">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="14804-494">key:attribute</span><span class="sxs-lookup"><span data-stu-id="14804-494">key:attribute</span></span>
* <span data-ttu-id="14804-495">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="14804-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="14804-496">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="14804-497"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> では、構成のキーと値のペアとしてディレクトリのファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="14804-498">キーはファイル名です。</span><span class="sxs-lookup"><span data-stu-id="14804-498">The key is the file name.</span></span> <span data-ttu-id="14804-499">値にはファイルのコンテンツが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-499">The value contains the file's contents.</span></span> <span data-ttu-id="14804-500">ファイルごとのキーの構成プロバイダーは、Docker ホスティングのシナリオで使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="14804-501">ファイルごとのキーの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="14804-502">ファイルに対する `directoryPath` は、絶対パスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="14804-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="14804-503">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="14804-504">ソースを構成する `Action<KeyPerFileConfigurationSource>` デリゲート。</span><span class="sxs-lookup"><span data-stu-id="14804-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="14804-505">ディレクトリを省略可能かどうか、またディレクトリへのパス。</span><span class="sxs-lookup"><span data-stu-id="14804-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="14804-506">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddKeyPerFile(directoryPath: path, optional: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-507"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="14804-508">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-508">Memory Configuration Provider</span></span>

<span data-ttu-id="14804-509"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> では、構成のキーと値のペアとして、メモリ内コレクションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="14804-510">メモリ内コレクションの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="14804-511">構成プロバイダーは、`IEnumerable<KeyValuePair<String,String>>` を使用して初期化できます。</span><span class="sxs-lookup"><span data-stu-id="14804-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="14804-512">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="14804-513"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="14804-514">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="14804-515"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-516"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="14804-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="14804-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="14804-517">GetValue</span></span>

<span data-ttu-id="14804-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) によって、指定したキーで構成から値が抽出され、それが指定した型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="14804-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="14804-519">オーバーロードを使用すると、キーが見つからない場合に既定値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="14804-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="14804-520">次の例では、キー `NumberKey` を使用して構成から文字列値を抽出し、その値を `int` に型付けして、変数 `intValue` に格納します。</span><span class="sxs-lookup"><span data-stu-id="14804-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="14804-521">構成キーで `NumberKey` が見つからない場合、`intValue` は 規定値 `99` を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="14804-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="14804-522">GetSection、GetChildren、Exists</span><span class="sxs-lookup"><span data-stu-id="14804-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="14804-523">以下の例では、次の JSON ファイルについて考えます。</span><span class="sxs-lookup"><span data-stu-id="14804-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="14804-524">4 つのキーが 2 つのセクションに含まれています。そのうちの 1 つには、一組のサブセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="14804-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="14804-525">ファイルが構成に読み取られると、次の一意の階層キーが作成され、構成値が保持されます。</span><span class="sxs-lookup"><span data-stu-id="14804-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="14804-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="14804-526">section0:key0</span></span>
* <span data-ttu-id="14804-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="14804-527">section0:key1</span></span>
* <span data-ttu-id="14804-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="14804-528">section1:key0</span></span>
* <span data-ttu-id="14804-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="14804-529">section1:key1</span></span>
* <span data-ttu-id="14804-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="14804-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="14804-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="14804-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="14804-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="14804-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="14804-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="14804-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="14804-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="14804-534">GetSection</span></span>

<span data-ttu-id="14804-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) では、指定したサブセクションのキーを持つ構成のサブセクションが抽出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="14804-536">`section1` のキーと値のペアのみを含む <xref:Microsoft.Extensions.Configuration.IConfigurationSection> を返すには、`GetSection` を呼び出してセクション名を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="14804-537">同様に、`section2:subsection0` のキーの値を取得するには、`GetSection` を呼び出してセクションのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="14804-538">`GetSection` が `null` を返すことはありません。</span><span class="sxs-lookup"><span data-stu-id="14804-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="14804-539">一致するセクションが見つからない場合は、空の `IConfigurationSection` が返されます。</span><span class="sxs-lookup"><span data-stu-id="14804-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="14804-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="14804-540">GetChildren</span></span>

<span data-ttu-id="14804-541">`section2` での [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) の呼び出しでは、次を含む `IEnumerable<IConfigurationSection>` が取得されます。</span><span class="sxs-lookup"><span data-stu-id="14804-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="14804-542">既存</span><span class="sxs-lookup"><span data-stu-id="14804-542">Exists</span></span>

<span data-ttu-id="14804-543">[ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) を使用して、構成セクションが存在するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="14804-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="14804-544">例のデータの場合、構成データに `section2:subsection2` セクションが存在しないため、`sectionExists` は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="14804-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="14804-545">クラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="14804-545">Bind to a class</span></span>

<span data-ttu-id="14804-546">"*オプション パターン*" を使用して、関連する設定のグループを表すクラスに構成をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="14804-547">詳細については、「<xref:fundamentals/configuration/options>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="14804-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="14804-548">構成値は文字列として返されますが、<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> を呼び出すことで [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="14804-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="14804-549">サンプル アプリには `Starship` モデル (*Models/Starship.cs*) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-550">サンプル アプリが JSON 構成プロバイダーを使用して構成を読み込むときに、*starship.json* ファイルの `starship` セクションで構成が作成されます。</span><span class="sxs-lookup"><span data-stu-id="14804-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="14804-551">次の構成のキーと値のペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="14804-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="14804-552">キー</span><span class="sxs-lookup"><span data-stu-id="14804-552">Key</span></span>                   | <span data-ttu-id="14804-553">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="14804-554">starship:name</span><span class="sxs-lookup"><span data-stu-id="14804-554">starship:name</span></span>         | <span data-ttu-id="14804-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="14804-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="14804-556">starship:registry</span><span class="sxs-lookup"><span data-stu-id="14804-556">starship:registry</span></span>     | <span data-ttu-id="14804-557">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="14804-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="14804-558">starship:class</span><span class="sxs-lookup"><span data-stu-id="14804-558">starship:class</span></span>        | <span data-ttu-id="14804-559">Constitution</span><span class="sxs-lookup"><span data-stu-id="14804-559">Constitution</span></span>                                      |
| <span data-ttu-id="14804-560">starship:length</span><span class="sxs-lookup"><span data-stu-id="14804-560">starship:length</span></span>       | <span data-ttu-id="14804-561">304.8</span><span class="sxs-lookup"><span data-stu-id="14804-561">304.8</span></span>                                             |
| <span data-ttu-id="14804-562">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="14804-562">starship:commissioned</span></span> | <span data-ttu-id="14804-563">False</span><span class="sxs-lookup"><span data-stu-id="14804-563">False</span></span>                                             |
| <span data-ttu-id="14804-564">trademark</span><span class="sxs-lookup"><span data-stu-id="14804-564">trademark</span></span>             | <span data-ttu-id="14804-565">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="14804-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="14804-566">サンプル アプリは `starship` キーを使用して `GetSection` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="14804-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="14804-567">`starship` のキーと値のペアは分離されます。</span><span class="sxs-lookup"><span data-stu-id="14804-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="14804-568">`Starship` クラスのインスタンスを渡すサブセクションで、`Bind` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="14804-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="14804-569">インスタンスの値をバインドした後、インスタンスがレンダリング用のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="14804-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="14804-570">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="14804-570">Bind to an object graph</span></span>

<span data-ttu-id="14804-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> では、POCO オブジェクト グラフ全体をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="14804-572">サンプルには、オブジェクト グラフに `Metadata` クラスと `Actors` クラスが含まれる `TvShow` モデルが含まれます (*Models/TvShow.cs*)。</span><span class="sxs-lookup"><span data-stu-id="14804-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-573">サンプル アプリには、構成データを含む *tvshow.xml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="14804-574">構成は、`Bind` メソッドを使用して、`TvShow` オブジェクト グラフ全体にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="14804-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="14804-575">バインドされたインスタンスは、表示のためにプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="14804-575">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="14804-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) によってバインドされ、指定した型が返されます。</span><span class="sxs-lookup"><span data-stu-id="14804-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="14804-577">`Get<T>` は `Bind` を使用するよりも便利です。</span><span class="sxs-lookup"><span data-stu-id="14804-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="14804-578">次のコードは、前の例と共に `Get<T>` を使用する方法を示しています。これにより、バインドされたインスタンスを、表示用に使用するプロパティに直接割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="14804-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="14804-579">配列をクラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="14804-579">Bind an array to a class</span></span>

<span data-ttu-id="14804-580">*サンプル アプリは、このセクションで説明する概念を示しています。*</span><span class="sxs-lookup"><span data-stu-id="14804-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="14804-581"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="14804-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="14804-582">数値のキー セグメント (`:0:`、`:1:`、&hellip; `:{n}:`) を公開する配列形式は、すべて POCO クラスの配列にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="14804-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="14804-583">バインドは慣例に従って指定されます。</span><span class="sxs-lookup"><span data-stu-id="14804-583">Binding is provided by convention.</span></span> <span data-ttu-id="14804-584">カスタム構成プロバイダーが配列のバインドを実装する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="14804-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="14804-585">**メモリ内配列の処理**</span><span class="sxs-lookup"><span data-stu-id="14804-585">**In-memory array processing**</span></span>

<span data-ttu-id="14804-586">次の表に示す構成のキーと値について考えます。</span><span class="sxs-lookup"><span data-stu-id="14804-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="14804-587">キー</span><span class="sxs-lookup"><span data-stu-id="14804-587">Key</span></span>     | <span data-ttu-id="14804-588">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-588">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="14804-589">array:0</span><span class="sxs-lookup"><span data-stu-id="14804-589">array:0</span></span> | <span data-ttu-id="14804-590">value0</span><span class="sxs-lookup"><span data-stu-id="14804-590">value0</span></span> |
| <span data-ttu-id="14804-591">array:1</span><span class="sxs-lookup"><span data-stu-id="14804-591">array:1</span></span> | <span data-ttu-id="14804-592">value1</span><span class="sxs-lookup"><span data-stu-id="14804-592">value1</span></span> |
| <span data-ttu-id="14804-593">array:2</span><span class="sxs-lookup"><span data-stu-id="14804-593">array:2</span></span> | <span data-ttu-id="14804-594">value2</span><span class="sxs-lookup"><span data-stu-id="14804-594">value2</span></span> |
| <span data-ttu-id="14804-595">array:4</span><span class="sxs-lookup"><span data-stu-id="14804-595">array:4</span></span> | <span data-ttu-id="14804-596">value4</span><span class="sxs-lookup"><span data-stu-id="14804-596">value4</span></span> |
| <span data-ttu-id="14804-597">array:5</span><span class="sxs-lookup"><span data-stu-id="14804-597">array:5</span></span> | <span data-ttu-id="14804-598">value5</span><span class="sxs-lookup"><span data-stu-id="14804-598">value5</span></span> |

<span data-ttu-id="14804-599">これらのキーと値は、メモリ構成プロバイダーを使用してサンプル アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="14804-600">配列は、インデックス &num;3 の値をスキップします。</span><span class="sxs-lookup"><span data-stu-id="14804-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="14804-601">構成バインダーは、null 値をバインドしたり、バインドされたオブジェクトに null エントリを作成したりすることはできません。このことは、この配列をオブジェクトにバインドした結果によって明らかになります。</span><span class="sxs-lookup"><span data-stu-id="14804-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="14804-602">サンプル アプリでは、バインドされた構成データを保持するために POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-603">構成データはオブジェクトにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="14804-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="14804-604">また、[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 構文を使用して、コードをよりコンパクトにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="14804-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="14804-605">バインドされたオブジェクト (`ArrayExample` のインスタンス) は、構成から配列データを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="14804-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="14804-606">`ArrayExamples.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="14804-606">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="14804-607">`ArrayExamples.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="14804-607">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="14804-608">0</span><span class="sxs-lookup"><span data-stu-id="14804-608">0</span></span>                             | <span data-ttu-id="14804-609">value0</span><span class="sxs-lookup"><span data-stu-id="14804-609">value0</span></span>                        |
| <span data-ttu-id="14804-610">1</span><span class="sxs-lookup"><span data-stu-id="14804-610">1</span></span>                             | <span data-ttu-id="14804-611">value1</span><span class="sxs-lookup"><span data-stu-id="14804-611">value1</span></span>                        |
| <span data-ttu-id="14804-612">2</span><span class="sxs-lookup"><span data-stu-id="14804-612">2</span></span>                             | <span data-ttu-id="14804-613">value2</span><span class="sxs-lookup"><span data-stu-id="14804-613">value2</span></span>                        |
| <span data-ttu-id="14804-614">3</span><span class="sxs-lookup"><span data-stu-id="14804-614">3</span></span>                             | <span data-ttu-id="14804-615">value4</span><span class="sxs-lookup"><span data-stu-id="14804-615">value4</span></span>                        |
| <span data-ttu-id="14804-616">4</span><span class="sxs-lookup"><span data-stu-id="14804-616">4</span></span>                             | <span data-ttu-id="14804-617">value5</span><span class="sxs-lookup"><span data-stu-id="14804-617">value5</span></span>                        |

<span data-ttu-id="14804-618">バインドされたオブジェクトのインデックス &num;3 によって、`array:4` 構成キーの構成データと、その値 `value4` が保持されます。</span><span class="sxs-lookup"><span data-stu-id="14804-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="14804-619">配列を含む構成データがバインドされると、構成キーの配列のインデックスは、オブジェクトを作成するときに構成データを反復処理するためだけに使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="14804-620">構成データに null 値を保持することはできません。また、構成キーの配列が 1 つまたは複数のインデックスをスキップしても、バインドされたオブジェクトに null 値のエントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="14804-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="14804-621">インデックス &num;3 の不足している構成項目は、`ArrayExamples` インスタンスにバインドする前に、構成で適切なキーと値のペアを生成する構成プロバイダーによって指定できます。</span><span class="sxs-lookup"><span data-stu-id="14804-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="14804-622">不足しているキーと値のペアを含む JSON 構成プロバイダーがサンプルに含まれる場合、`ArrayExamples.Entries` は完全な構成の配列と一致します。</span><span class="sxs-lookup"><span data-stu-id="14804-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="14804-623">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="14804-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="14804-624"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>の場合:</span><span class="sxs-lookup"><span data-stu-id="14804-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="14804-625">`Startup` コンストラクターの場合:</span><span class="sxs-lookup"><span data-stu-id="14804-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="14804-626">表に示すキーと値のペアが構成に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="14804-627">キー</span><span class="sxs-lookup"><span data-stu-id="14804-627">Key</span></span>             | <span data-ttu-id="14804-628">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="14804-629">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="14804-629">array:entries:3</span></span> | <span data-ttu-id="14804-630">value3</span><span class="sxs-lookup"><span data-stu-id="14804-630">value3</span></span> |

<span data-ttu-id="14804-631">JSON 構成プロバイダーにインデックス &num;3 のエントリが含まれた後に `ArrayExamples` クラスのインスタンスがバインドされる場合、`ArrayExamples.Entries` 配列に値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="14804-631">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="14804-632">`ArrayExamples.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="14804-632">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="14804-633">`ArrayExamples.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="14804-633">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="14804-634">0</span><span class="sxs-lookup"><span data-stu-id="14804-634">0</span></span>                             | <span data-ttu-id="14804-635">value0</span><span class="sxs-lookup"><span data-stu-id="14804-635">value0</span></span>                        |
| <span data-ttu-id="14804-636">1</span><span class="sxs-lookup"><span data-stu-id="14804-636">1</span></span>                             | <span data-ttu-id="14804-637">value1</span><span class="sxs-lookup"><span data-stu-id="14804-637">value1</span></span>                        |
| <span data-ttu-id="14804-638">2</span><span class="sxs-lookup"><span data-stu-id="14804-638">2</span></span>                             | <span data-ttu-id="14804-639">value2</span><span class="sxs-lookup"><span data-stu-id="14804-639">value2</span></span>                        |
| <span data-ttu-id="14804-640">3</span><span class="sxs-lookup"><span data-stu-id="14804-640">3</span></span>                             | <span data-ttu-id="14804-641">value3</span><span class="sxs-lookup"><span data-stu-id="14804-641">value3</span></span>                        |
| <span data-ttu-id="14804-642">4</span><span class="sxs-lookup"><span data-stu-id="14804-642">4</span></span>                             | <span data-ttu-id="14804-643">value4</span><span class="sxs-lookup"><span data-stu-id="14804-643">value4</span></span>                        |
| <span data-ttu-id="14804-644">5</span><span class="sxs-lookup"><span data-stu-id="14804-644">5</span></span>                             | <span data-ttu-id="14804-645">value5</span><span class="sxs-lookup"><span data-stu-id="14804-645">value5</span></span>                        |

<span data-ttu-id="14804-646">**JSON 配列の処理**</span><span class="sxs-lookup"><span data-stu-id="14804-646">**JSON array processing**</span></span>

<span data-ttu-id="14804-647">JSON ファイルに配列が含まれる場合、配列要素の構成キーは、0 から始まるセクションのインデックスで作成されます。</span><span class="sxs-lookup"><span data-stu-id="14804-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="14804-648">次の構成ファイルにおいて、`subsection` は配列です。</span><span class="sxs-lookup"><span data-stu-id="14804-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="14804-649">JSON 構成プロバイダーは、次のキーと値のペアに構成データを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="14804-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="14804-650">キー</span><span class="sxs-lookup"><span data-stu-id="14804-650">Key</span></span>                     | <span data-ttu-id="14804-651">[値]</span><span class="sxs-lookup"><span data-stu-id="14804-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="14804-652">json_array:key</span><span class="sxs-lookup"><span data-stu-id="14804-652">json_array:key</span></span>          | <span data-ttu-id="14804-653">valueA</span><span class="sxs-lookup"><span data-stu-id="14804-653">valueA</span></span> |
| <span data-ttu-id="14804-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="14804-654">json_array:subsection:0</span></span> | <span data-ttu-id="14804-655">valueB</span><span class="sxs-lookup"><span data-stu-id="14804-655">valueB</span></span> |
| <span data-ttu-id="14804-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="14804-656">json_array:subsection:1</span></span> | <span data-ttu-id="14804-657">valueC</span><span class="sxs-lookup"><span data-stu-id="14804-657">valueC</span></span> |
| <span data-ttu-id="14804-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="14804-658">json_array:subsection:2</span></span> | <span data-ttu-id="14804-659">valueD</span><span class="sxs-lookup"><span data-stu-id="14804-659">valueD</span></span> |

<span data-ttu-id="14804-660">サンプル アプリでは、構成のキーと値のペアをバインドするために、次の POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="14804-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-661">バインド後、`JsonArrayExample.Key` は値 `valueA` を保持します。</span><span class="sxs-lookup"><span data-stu-id="14804-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="14804-662">サブセクションの値は、POCO 配列のプロパティ `Subsection` に格納されます。</span><span class="sxs-lookup"><span data-stu-id="14804-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="14804-663">`JsonArrayExample.Subsection` インデックス</span><span class="sxs-lookup"><span data-stu-id="14804-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="14804-664">`JsonArrayExample.Subsection` 値</span><span class="sxs-lookup"><span data-stu-id="14804-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="14804-665">0</span><span class="sxs-lookup"><span data-stu-id="14804-665">0</span></span>                                   | <span data-ttu-id="14804-666">valueB</span><span class="sxs-lookup"><span data-stu-id="14804-666">valueB</span></span>                              |
| <span data-ttu-id="14804-667">1</span><span class="sxs-lookup"><span data-stu-id="14804-667">1</span></span>                                   | <span data-ttu-id="14804-668">valueC</span><span class="sxs-lookup"><span data-stu-id="14804-668">valueC</span></span>                              |
| <span data-ttu-id="14804-669">2</span><span class="sxs-lookup"><span data-stu-id="14804-669">2</span></span>                                   | <span data-ttu-id="14804-670">valueD</span><span class="sxs-lookup"><span data-stu-id="14804-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="14804-671">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="14804-671">Custom configuration provider</span></span>

<span data-ttu-id="14804-672">サンプル アプリでは、[Entity Framework (EF)](/ef/core/) を使用してデータベースから構成のキーと値のペアを読み取る、基本的な構成プロバイダーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="14804-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="14804-673">プロバイダーの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="14804-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="14804-674">EF のメモリ内データベースは、デモンストレーションのために使用されます。</span><span class="sxs-lookup"><span data-stu-id="14804-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="14804-675">接続文字列を必要とするデータベースを使用するには、第 2 の `ConfigurationBuilder` を実装して、別の構成プロバイダーからの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="14804-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="14804-676">プロバイダーは、起動時に、構成にデータベース テーブルを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="14804-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="14804-677">プロバイダーは、キー単位でデータベースを照会しません。</span><span class="sxs-lookup"><span data-stu-id="14804-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="14804-678">変更時に再度読み込む機能は実装されていません。このため、アプリの起動後にデータベースを更新しても、アプリの構成には影響がありません。</span><span class="sxs-lookup"><span data-stu-id="14804-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="14804-679">データベースに構成値を格納するための `EFConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="14804-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="14804-680">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="14804-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-681">構成した値を格納し、その値にアクセスするための `EFConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="14804-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="14804-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="14804-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-683"><xref:Microsoft.Extensions.Configuration.IConfigurationSource> を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="14804-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="14804-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="14804-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-685"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider> から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="14804-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="14804-686">データベースが空だった場合、構成プロバイダーはこれを初期化します。</span><span class="sxs-lookup"><span data-stu-id="14804-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="14804-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="14804-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-688">`AddEFConfiguration` 拡張メソッドを使用すると、`ConfigurationBuilder` に構成ソースを追加できます。</span><span class="sxs-lookup"><span data-stu-id="14804-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="14804-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="14804-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="14804-690">次のコードでは、*Program.cs* でカスタムの `EFConfigurationProvider` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="14804-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="14804-691">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="14804-691">Access configuration during startup</span></span>

<span data-ttu-id="14804-692">`Startup` コンストラクターに `IConfiguration` を挿入して、`Startup.ConfigureServices` で構成値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="14804-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="14804-693">`Startup.Configure` で構成にアクセスするには、メソッドに直接 `IConfiguration` を挿入するか、コンストラクターからのインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="14804-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="14804-694">起動時の簡易メソッドを使用して構成にアクセスする例については、[アプリ起動時の簡易メソッド](xref:fundamentals/startup#convenience-methods)に関連する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="14804-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="14804-695">Razor Pages ページまたは MVC ビューで構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="14804-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="14804-696">Razor Pages ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](xref:Microsoft.Extensions.Configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、<xref:Microsoft.Extensions.Configuration.IConfiguration> をページまたはビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="14804-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="14804-697">Razor ページ: </span><span class="sxs-lookup"><span data-stu-id="14804-697">In a Razor Pages page:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="14804-698">MVC ビュー: </span><span class="sxs-lookup"><span data-stu-id="14804-698">In an MVC view:</span></span>

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="14804-699">外部アセンブリから構成を追加する</span><span class="sxs-lookup"><span data-stu-id="14804-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="14804-700"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="14804-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="14804-701">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="14804-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14804-702">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="14804-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="14804-703">Microsoft の構成について詳しく調べる</span><span class="sxs-lookup"><span data-stu-id="14804-703">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
