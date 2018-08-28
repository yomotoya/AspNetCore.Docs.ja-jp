---
title: ASP.NET Core の構成
author: guardrex
description: 構成 API を使用して、ASP.NET Core アプリを構成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927879"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e4eeb-103">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="e4eeb-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e4eeb-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e4eeb-105">ASP.NET Core でのアプリの構成は、"*構成プロバイダー*" によって設定するキーと値のペアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="e4eeb-106">構成プロバイダーは、さまざまな構成のソースから構成データを読み取り、キーと値のペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="e4eeb-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e4eeb-107">Azure Key Vault</span></span>
* <span data-ttu-id="e4eeb-108">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-108">Command-line arguments</span></span>
* <span data-ttu-id="e4eeb-109">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e4eeb-110">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-110">Directory files</span></span>
* <span data-ttu-id="e4eeb-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-111">Environment variables</span></span>
* <span data-ttu-id="e4eeb-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="e4eeb-112">In-memory .NET objects</span></span>
* <span data-ttu-id="e4eeb-113">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="e4eeb-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e4eeb-114">Azure Key Vault</span></span>
* <span data-ttu-id="e4eeb-115">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-115">Command-line arguments</span></span>
* <span data-ttu-id="e4eeb-116">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e4eeb-117">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-117">Environment variables</span></span>
* <span data-ttu-id="e4eeb-118">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="e4eeb-118">In-memory .NET objects</span></span>
* <span data-ttu-id="e4eeb-119">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="e4eeb-120">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-120">Command-line arguments</span></span>
* <span data-ttu-id="e4eeb-121">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="e4eeb-122">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-122">Environment variables</span></span>
* <span data-ttu-id="e4eeb-123">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="e4eeb-123">In-memory .NET objects</span></span>
* <span data-ttu-id="e4eeb-124">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="e4eeb-125">"*オプション パターン*" は、このトピックで説明する構成の概念を拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="e4eeb-126">オプションでは、クラスを使用して関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="e4eeb-127">オプション パターンの使用について詳しくは、「<xref:fundamentals/configuration/options>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e4eeb-128">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e4eeb-129">このトピックで提供される例は、次のものに依存しています。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="e4eeb-130"><xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> によるアプリの基本パスの設定。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="e4eeb-131">[Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージを参照することで、アプリが `SetBasePath` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="e4eeb-132"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> による構成ファイルのセクションの解決。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="e4eeb-133">[Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージを参照することで、アプリが `GetSection` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="e4eeb-134"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> および [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) による構成の .NET クラスへのバインド。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="e4eeb-135">[Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージを参照することで、アプリが `Bind` と `Get<T>` を使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="e4eeb-136">`Get<T>` は、ASP.NET Core 1.1 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e4eeb-137">これらの 3 つのパッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e4eeb-138">これらの 3 つのパッケージは、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="e4eeb-139">ホストとアプリの構成</span><span class="sxs-lookup"><span data-stu-id="e4eeb-139">Host vs. app configuration</span></span>

<span data-ttu-id="e4eeb-140">アプリを構成して起動する前に、"*ホスト*" を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="e4eeb-141">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e4eeb-142">アプリとホストは、両方ともこのトピックで説明する構成プロバイダーを使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="e4eeb-143">ホストの構成のキーと値のペアは、アプリのグローバル構成の一部となります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="e4eeb-144">ホストをビルドするときの構成プロバイダーの使用方法、およびホストの構成に対する構成ソースの影響について詳しくは、「<xref:fundamentals/host/index>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="e4eeb-145">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="e4eeb-145">Security</span></span>

<span data-ttu-id="e4eeb-146">次のベスト プラクティスを採用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="e4eeb-147">構成プロバイダーのコードやプレーンテキストの構成ファイルには、パスワードなどの機密データを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="e4eeb-148">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="e4eeb-149">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="e4eeb-150">[複数の環境を使用する方法](xref:fundamentals/environments)や、[シークレット マネージャーでの開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)の管理 (機密データを格納するための環境変数の使用に関するアドバイスを含む) について、さらに学習することができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="e4eeb-151">シークレット マネージャーは、ファイル構成プロバイダーを使用して、ユーザーの機密情報をローカル システム上の JSON ファイルに格納します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="e4eeb-152">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e4eeb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) は、アプリのシークレットを安全に格納するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="e4eeb-154">詳細については、「<xref:security/key-vault-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="e4eeb-155">階層的な構成データ</span><span class="sxs-lookup"><span data-stu-id="e4eeb-155">Hierarchical configuration data</span></span>

<span data-ttu-id="e4eeb-156">構成 API は、構成キー内の区切り記号を使用して階層データをフラット化することにより、階層的な構成データを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="e4eeb-157">次の JSON ファイルでは、2 つのセクションの構造化階層に 4 つのキーが存在します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="e4eeb-158">ファイルが構成に読み込まれると、構成ソースの元の階層データ構造を維持するために、一意なキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="e4eeb-159">セクションとキーは、元の構造を維持するために、コロン (`:`) を使用してフラット化されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="e4eeb-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-160">section0:key0</span></span>
* <span data-ttu-id="e4eeb-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-161">section0:key1</span></span>
* <span data-ttu-id="e4eeb-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-162">section1:key0</span></span>
* <span data-ttu-id="e4eeb-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-163">section1:key1</span></span>

<span data-ttu-id="e4eeb-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> メソッドと <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> メソッドを使用して、構成データ内のセクションとセクションの子を分離することができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="e4eeb-165">これらのメソッドについては、後ほど「[GetSection、GetChildren、Exists](#getsection-getchildren-and-exists)」で説明します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="e4eeb-166">規約</span><span class="sxs-lookup"><span data-stu-id="e4eeb-166">Conventions</span></span>

<span data-ttu-id="e4eeb-167">アプリの起動時に、各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="e4eeb-168">ファイル構成プロバイダーには、アプリの起動後に基になる設定ファイルが変更された場合に、構成を再読み込みする機能があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="e4eeb-169">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="e4eeb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> は、アプリの[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) コンテナーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e4eeb-171">構成プロバイダーでは DI を使用できません。ホストによって構成プロバイダーが設定されている場合、DI を使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="e4eeb-172">構成キーでは、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="e4eeb-173">キーでは、大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-173">Keys are case-insensitive.</span></span> <span data-ttu-id="e4eeb-174">たとえば、`ConnectionString` と `connectionstring` は同等のキーとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="e4eeb-175">同じキーに対する値が、同じまたは別の構成プロバイダーによって設定された場合、最後にキーに設定された値が使用される値となります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="e4eeb-176">階層キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-176">Hierarchical keys</span></span>
  * <span data-ttu-id="e4eeb-177">構成 API 内では、すべてのプラットフォームでコロン (`:`) の区切りが機能します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="e4eeb-178">環境変数内では、コロン区切りがすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="e4eeb-179">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="e4eeb-180">Azure Key Vault では、階層キーは区切り記号として `--` (2 つのダッシュ) を使用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e4eeb-181">コードを指定して、アプリの構成にシークレットが読み込まれるときにダッシュをコロンで置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="e4eeb-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e4eeb-183">配列のバインドについては、「[配列をクラスにバインドする](#bind-an-array-to-a-class)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="e4eeb-184">構成値では、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="e4eeb-185">値は文字列です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-185">Values are strings.</span></span>
* <span data-ttu-id="e4eeb-186">Null 値を構成に格納したり、オブジェクトにバインドしたりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="e4eeb-187">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-187">Providers</span></span>

<span data-ttu-id="e4eeb-188">ASP.NET Core アプリで使用できる構成プロバイダーを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="e4eeb-189">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-189">Provider</span></span> | <span data-ttu-id="e4eeb-190">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e4eeb-191">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e4eeb-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e4eeb-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="e4eeb-193">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e4eeb-194">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="e4eeb-194">Command-line parameters</span></span> |
| [<span data-ttu-id="e4eeb-195">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e4eeb-196">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="e4eeb-196">Custom source</span></span> |
| [<span data-ttu-id="e4eeb-197">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e4eeb-198">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-198">Environment variables</span></span> |
| [<span data-ttu-id="e4eeb-199">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e4eeb-200">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e4eeb-201">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="e4eeb-202">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-202">Directory files</span></span> |
| [<span data-ttu-id="e4eeb-203">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e4eeb-204">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="e4eeb-204">In-memory collections</span></span> |
| <span data-ttu-id="e4eeb-205">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e4eeb-206">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="e4eeb-207">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-207">Provider</span></span> | <span data-ttu-id="e4eeb-208">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="e4eeb-209">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="e4eeb-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e4eeb-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="e4eeb-211">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e4eeb-212">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="e4eeb-212">Command-line parameters</span></span> |
| [<span data-ttu-id="e4eeb-213">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e4eeb-214">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="e4eeb-214">Custom source</span></span> |
| [<span data-ttu-id="e4eeb-215">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e4eeb-216">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-216">Environment variables</span></span> |
| [<span data-ttu-id="e4eeb-217">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e4eeb-218">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e4eeb-219">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e4eeb-220">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="e4eeb-220">In-memory collections</span></span> |
| <span data-ttu-id="e4eeb-221">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e4eeb-222">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="e4eeb-223">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-223">Provider</span></span> | <span data-ttu-id="e4eeb-224">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="e4eeb-225">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="e4eeb-226">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="e4eeb-226">Command-line parameters</span></span> |
| [<span data-ttu-id="e4eeb-227">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="e4eeb-228">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="e4eeb-228">Custom source</span></span> |
| [<span data-ttu-id="e4eeb-229">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="e4eeb-230">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-230">Environment variables</span></span> |
| [<span data-ttu-id="e4eeb-231">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="e4eeb-232">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="e4eeb-233">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="e4eeb-234">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="e4eeb-234">In-memory collections</span></span> |
| <span data-ttu-id="e4eeb-235">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="e4eeb-236">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="e4eeb-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="e4eeb-237">アプリの起動時に各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="e4eeb-238">このトピックで説明する構成プロバイダーは、それらをコードで配置する順ではなく、アルファベット順で説明します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="e4eeb-239">基になる構成ソースの優先順位に合わせるために、コード内で構成プロバイダーを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="e4eeb-240">一般的な一連の構成プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="e4eeb-241">ファイル (*appsettings.json*、 *appsettings.&lt;Environment&gt;.json*。`<Environment>` はアプリの現在のホスト環境です)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="e4eeb-242">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合のみ)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="e4eeb-243">環境変数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-243">Environment variables</span></span>
1. <span data-ttu-id="e4eeb-244">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-244">Command-line arguments</span></span>

<span data-ttu-id="e4eeb-245">コマンド ライン引数が他のプロバイダーによって設定された構成をオーバーライドできるようにするために、コマンド ラインの構成プロバイダーを一連のプロバイダーの最後に配置するのは、一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-246">この一連のプロバイダーは、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を使用して新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化するときに配置されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e4eeb-247">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e4eeb-248">Web ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="e4eeb-249">`ConfigureAppConfiguration` *は、ASP.NET Core 2.1 以降で使用できます。*</span><span class="sxs-lookup"><span data-stu-id="e4eeb-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-250"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> および `Startup` 内のその <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> メソッドの呼び出しを使用して、(ホストではなく) アプリ用に一連のプロバイダーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="e4eeb-251">前の例では、<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> によって環境名 (`env.EnvironmentName`) とアプリのアセンブリ名 (`env.ApplicationName`) が提供されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="e4eeb-252">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="e4eeb-253">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="e4eeb-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> では、実行時にコマンドライン引数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-255">コマンド ライン構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-256"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddCommandLine` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e4eeb-257">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e4eeb-258">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e4eeb-259">*appsettings.json* および *appsettings.&lt;Environment&gt;.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="e4eeb-260">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e4eeb-261">環境変数。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-261">Environment variables.</span></span>

<span data-ttu-id="e4eeb-262">`CreateDefaultBuilder` はコマンド ライン構成プロバイダーを最後に追加します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="e4eeb-263">実行時に渡されるコマンド ライン引数によって、他のプロバイダーによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="e4eeb-264">`CreateDefaultBuilder` は、ホストが作成されるときに機能します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="e4eeb-265">そのため、`CreateDefaultBuilder` によってアクティブ化されるコマンド ライン構成によって、ホストの構成方法に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="e4eeb-266">`CreateDefaultBuilder` を呼び出さず手動でホストをビルドする場合は、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> インスタンスの `AddCommandLine` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-267">コマンド ライン構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-268">最後にプロバイダーを呼び出すことにより、実行時に渡されるコマンドライン引数で、他の構成プロバイダーによって設定された構成をオーバーライドできるようにします。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="e4eeb-269"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="e4eeb-270">**例**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-271">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-272">1.x のサンプル アプリでは、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> の <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="e4eeb-273">プロジェクトのディレクトリでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="e4eeb-274">`dotnet run` コマンドにコマンドライン引数を指定します (`dotnet run CommandLineKey=CommandLineValue`)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="e4eeb-275">アプリを実行したら、アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e4eeb-276">出力に、`dotnet run` に提供される構成のコマンド ライン引数のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="e4eeb-277">引数</span><span class="sxs-lookup"><span data-stu-id="e4eeb-277">Arguments</span></span>

<span data-ttu-id="e4eeb-278">値は等号 (`=`) の後に続ける必要があります。または、値をスペースの後に続ける場合は、キーにプレフィックス (`--`または`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="e4eeb-279">等号を使用する場合は、値に null を指定できます (例: `CommandLineKey=`)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="e4eeb-280">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-280">Key prefix</span></span>               | <span data-ttu-id="e4eeb-281">例</span><span class="sxs-lookup"><span data-stu-id="e4eeb-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="e4eeb-282">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="e4eeb-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="e4eeb-283">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="e4eeb-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="e4eeb-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="e4eeb-285">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="e4eeb-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="e4eeb-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="e4eeb-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="e4eeb-287">同じコマンド内のコマンド ライン引数で、等号を使用するキーと値のペアと、スペースを使用するキーと値のペアを混在させないでください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="e4eeb-288">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="e4eeb-289">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="e4eeb-289">Switch mappings</span></span>

<span data-ttu-id="e4eeb-290">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="e4eeb-291"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> で構成を手動でビルドするときに、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> メソッドにスイッチ置換のディクショナリを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="e4eeb-292">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e4eeb-293">コマンド ライン キーがディクショナリで見つかった場合は、アプリの構成にキーと値のペアを設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="e4eeb-294">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e4eeb-295">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e4eeb-296">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e4eeb-297">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="e4eeb-298">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="e4eeb-299">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e4eeb-300">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="e4eeb-301">解決策は、`CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="e4eeb-302">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e4eeb-303">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-303">Key</span></span>       | <span data-ttu-id="e4eeb-304">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="e4eeb-305">アプリの起動時にスイッチ マッピングされたキーを使用する場合、構成は、ディクショナリによって指定されたキーでの構成値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="e4eeb-306">上記のコマンドを実行すると、次の表に示す値が構成に含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="e4eeb-307">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-307">Key</span></span>               | <span data-ttu-id="e4eeb-308">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="e4eeb-309">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="e4eeb-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> では、実行時に環境変数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="e4eeb-311">環境変数の構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-312">環境変数内で階層キーを操作する場合、コロン区切り (`:`) がすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="e4eeb-313">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに置換されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="e4eeb-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) を使用すると、環境変数構成プロバイダーを使用してアプリの構成をオーバーライドすることができる環境変数を、Azure Portal で設定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="e4eeb-315">詳細については、[Azure アプリの、Azure Portal を使用したアプリの構成のオーバーライド](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-316"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddEnvironmentVariables` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e4eeb-317">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e4eeb-318">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e4eeb-319">*appsettings.json* および *appsettings.&lt;Environment&gt;.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="e4eeb-320">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e4eeb-321">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-321">Command-line arguments.</span></span>

<span data-ttu-id="e4eeb-322">ユーザー シークレットと *appsettings* ファイルから構成が設定された後に、環境変数構成プロバイダーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="e4eeb-323">この位置でプロバイダーを呼び出すことにより、実行時に読み込まれた環境変数が、ユーザー シークレットと *appsettings* ファイルによって設定された構成をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="e4eeb-324">また、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddEnvironmentVariables` 拡張メソッドを直接呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-325">`UseConfiguration` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e4eeb-326">**例**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-327">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには `AddEnvironmentVariables` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-328">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="e4eeb-329">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-329">Run the sample app.</span></span> <span data-ttu-id="e4eeb-330">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e4eeb-331">出力に、環境変数 `ENVIRONMENT` のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="e4eeb-332">値には、アプリを実行している環境が反映されます (ローカルで実行している場合は通常 `Development`)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="e4eeb-333">アプリによって表示される環境変数のリストを短くするために、アプリでは次で始まる環境変数がフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="e4eeb-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="e4eeb-334">ASPNETCORE_</span></span>
* <span data-ttu-id="e4eeb-335">urls</span><span class="sxs-lookup"><span data-stu-id="e4eeb-335">urls</span></span>
* <span data-ttu-id="e4eeb-336">ログの記録</span><span class="sxs-lookup"><span data-stu-id="e4eeb-336">Logging</span></span>
* <span data-ttu-id="e4eeb-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="e4eeb-337">ENVIRONMENT</span></span>
* <span data-ttu-id="e4eeb-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="e4eeb-338">contentRoot</span></span>
* <span data-ttu-id="e4eeb-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e4eeb-339">AllowedHosts</span></span>
* <span data-ttu-id="e4eeb-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="e4eeb-340">applicationName</span></span>
* <span data-ttu-id="e4eeb-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="e4eeb-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-342">アプリで使用できるすべての環境変数を公開する場合は、*Pages/Index.cshtml.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-343">アプリで使用できるすべての環境変数を公開する場合は、*Controllers/HomeController.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="e4eeb-344">プレフィックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-344">Prefixes</span></span>

<span data-ttu-id="e4eeb-345">アプリの構成に読み込まれる環境変数は、`AddEnvironmentVariables` メソッドにプレフィックスを指定すると、フィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="e4eeb-346">たとえば、プレフィックス `CUSTOM_` で環境変数をフィルター処理するには、構成プロバイダーにプレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="e4eeb-347">構成のキーと値のペアが作成されるときに、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-348">静的な簡易メソッド `CreateDefaultBuilder` によって <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> が作成され、アプリのホストが確立されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="e4eeb-349">`WebHostBuilder` は、作成されるときに、`ASPNETCORE_` というプレフィックスが付いた環境変数からホストの構成を見つけます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="e4eeb-350">**接続文字列のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-350">**Connection string prefixes**</span></span>

<span data-ttu-id="e4eeb-351">構成 API には、アプリの環境に向けた Azure の接続文字列の構成に関係する、4 つの接続文字列環境変数のための特別な処理規則があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="e4eeb-352">表に示されるプレフィックスを含む環境変数は、`AddEnvironmentVariables` にプレフィックスが指定されていない場合、アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="e4eeb-353">接続文字列のプレフィックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-353">Connection string prefix</span></span> | <span data-ttu-id="e4eeb-354">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="e4eeb-355">カスタム プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="e4eeb-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="e4eeb-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="e4eeb-357">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e4eeb-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="e4eeb-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e4eeb-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="e4eeb-359">表に示す 4 つのプレフィックスのいずれかを使用して、環境変数が検出され構成に読み込まれた場合:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="e4eeb-360">環境変数のプレフィックスを削除し、構成キーのセクション (`ConnectionStrings`) を追加することによって、構成キーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="e4eeb-361">データベースの接続プロバイダーを表す新しい構成のキーと値のペアが作成されます (示されたプロバイダーを含まない `CUSTOMCONNSTR_` を除く)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="e4eeb-362">環境変数キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-362">Environment variable key</span></span> | <span data-ttu-id="e4eeb-363">変換された構成キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-363">Converted configuration key</span></span> | <span data-ttu-id="e4eeb-364">プロバイダーの構成エントリ</span><span class="sxs-lookup"><span data-stu-id="e4eeb-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e4eeb-365">構成エントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e4eeb-366">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e4eeb-367">値: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="e4eeb-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e4eeb-368">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e4eeb-369">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e4eeb-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="e4eeb-370">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="e4eeb-371">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="e4eeb-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="e4eeb-372">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-372">File Configuration Provider</span></span>

<span data-ttu-id="e4eeb-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> は、ファイル システムから構成を読み込むための基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="e4eeb-374">次の構成プロバイダーは、特定のファイルの種類専用です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="e4eeb-375">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="e4eeb-376">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="e4eeb-377">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="e4eeb-378">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-378">INI Configuration Provider</span></span>

<span data-ttu-id="e4eeb-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> では、実行時に INI ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="e4eeb-380">INI ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-381">INI ファイルの構成では、セクションの区切り記号としてコロンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="e4eeb-382">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="e4eeb-383">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-383">Whether the file is optional.</span></span>
* <span data-ttu-id="e4eeb-384">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e4eeb-385">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-386">`CreateDefaultBuilder` を呼び出す場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e4eeb-387"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-388">`UseConfiguration` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e4eeb-389">INI 構成ファイルの汎用的な例:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="e4eeb-390">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e4eeb-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-391">section0:key0</span></span>
* <span data-ttu-id="e4eeb-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-392">section0:key1</span></span>
* <span data-ttu-id="e4eeb-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="e4eeb-393">section1:subsection:key</span></span>
* <span data-ttu-id="e4eeb-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="e4eeb-394">section2:subsection0:key</span></span>
* <span data-ttu-id="e4eeb-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="e4eeb-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="e4eeb-396">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-396">JSON Configuration Provider</span></span>

<span data-ttu-id="e4eeb-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> では、実行時に JSON ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="e4eeb-398">JSON ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-399">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="e4eeb-400">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-400">Whether the file is optional.</span></span>
* <span data-ttu-id="e4eeb-401">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e4eeb-402">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-403"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddJsonFile` が 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="e4eeb-404">このメソッドは、次から構成を読み込むために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="e4eeb-405">*appsettings.json* &ndash; このファイルが最初に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="e4eeb-406">ファイルの環境バージョンは、*appsettings.json* ファイルによって指定される値をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="e4eeb-407">*appsettings.&lt;Environment&gt;.json* &ndash; ファイルの環境バージョンは、[IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) に基づいて読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="e4eeb-408">詳細については、[Web ホストのホストの設定](xref:fundamentals/host/web-host#set-up-a-host)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="e4eeb-409">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="e4eeb-410">環境変数。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-410">Environment variables.</span></span>
* <span data-ttu-id="e4eeb-411">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="e4eeb-412">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-412">Command-line arguments.</span></span>

<span data-ttu-id="e4eeb-413">JSON 構成プロバイダーが最初に確立されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="e4eeb-414">このため、ユーザー シークレット、環境変数、およびコマンド ライン引数によって、*appsettings* ファイルによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="e4eeb-415">また、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddJsonFile` 拡張メソッドを直接呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-416">`CreateDefaultBuilder` を呼び出す場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e4eeb-417"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-418">`UseConfiguration` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e4eeb-419">**例**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-420">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには 2 回の `AddJsonFile` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="e4eeb-421">構成は、*appsettings.json* および *appsettings.&lt;Environment&gt;.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-422">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddJsonFile` を 2 回呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="e4eeb-423">構成は、*appsettings.json* および *appsettings.&lt;Environment&gt;.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="e4eeb-424">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-424">Run the sample app.</span></span> <span data-ttu-id="e4eeb-425">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="e4eeb-426">出力に、環境に応じて、表に示す構成のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="e4eeb-427">ログの構成キーでは、階層の区切り文字としてコロン (`:`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="e4eeb-428">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-428">Key</span></span>                        | <span data-ttu-id="e4eeb-429">開発時の値</span><span class="sxs-lookup"><span data-stu-id="e4eeb-429">Development Value</span></span> | <span data-ttu-id="e4eeb-430">運用時の値</span><span class="sxs-lookup"><span data-stu-id="e4eeb-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="e4eeb-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="e4eeb-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="e4eeb-432">情報</span><span class="sxs-lookup"><span data-stu-id="e4eeb-432">Information</span></span>       | <span data-ttu-id="e4eeb-433">情報</span><span class="sxs-lookup"><span data-stu-id="e4eeb-433">Information</span></span>      |
| <span data-ttu-id="e4eeb-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="e4eeb-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="e4eeb-435">情報</span><span class="sxs-lookup"><span data-stu-id="e4eeb-435">Information</span></span>       | <span data-ttu-id="e4eeb-436">情報</span><span class="sxs-lookup"><span data-stu-id="e4eeb-436">Information</span></span>      |
| <span data-ttu-id="e4eeb-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="e4eeb-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="e4eeb-438">デバッグ</span><span class="sxs-lookup"><span data-stu-id="e4eeb-438">Debug</span></span>             | <span data-ttu-id="e4eeb-439">Error</span><span class="sxs-lookup"><span data-stu-id="e4eeb-439">Error</span></span>            |
| <span data-ttu-id="e4eeb-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e4eeb-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="e4eeb-441">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-441">XML Configuration Provider</span></span>

<span data-ttu-id="e4eeb-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> では、実行時に XML ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="e4eeb-443">XML ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-444">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="e4eeb-445">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-445">Whether the file is optional.</span></span>
* <span data-ttu-id="e4eeb-446">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="e4eeb-447">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="e4eeb-448">構成ファイルのルート ノードは、構成のキーと値のペアを作成するときには無視されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="e4eeb-449">ファイル内でドキュメント型定義 (DTD) または名前空間を指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-450">`CreateDefaultBuilder` を呼び出す場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e4eeb-451"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-452">`UseConfiguration` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="e4eeb-453">XML 構成ファイルでは、繰り返しのセクション用に個別の要素名を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="e4eeb-454">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e4eeb-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-455">section0:key0</span></span>
* <span data-ttu-id="e4eeb-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-456">section0:key1</span></span>
* <span data-ttu-id="e4eeb-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-457">section1:key0</span></span>
* <span data-ttu-id="e4eeb-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-458">section1:key1</span></span>

<span data-ttu-id="e4eeb-459">同じ要素名を使用する要素の繰り返しは、要素を区別するために `name` 属性を使用する場合に機能します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="e4eeb-460">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e4eeb-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-461">section:section0:key:key0</span></span>
* <span data-ttu-id="e4eeb-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-462">section:section0:key:key1</span></span>
* <span data-ttu-id="e4eeb-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-463">section:section1:key:key0</span></span>
* <span data-ttu-id="e4eeb-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-464">section:section1:key:key1</span></span>

<span data-ttu-id="e4eeb-465">値を指定するために属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="e4eeb-466">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="e4eeb-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="e4eeb-467">key:attribute</span></span>
* <span data-ttu-id="e4eeb-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="e4eeb-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="e4eeb-469">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="e4eeb-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> では、構成のキーと値のペアとしてディレクトリのファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="e4eeb-471">キーはファイル名です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-471">The key is the file name.</span></span> <span data-ttu-id="e4eeb-472">値にはファイルのコンテンツが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-472">The value contains the file's contents.</span></span> <span data-ttu-id="e4eeb-473">ファイルごとのキーの構成プロバイダーは、Docker ホスティングのシナリオで使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="e4eeb-474">ファイルごとのキーの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="e4eeb-475">ファイルに対する `directoryPath` は、絶対パスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="e4eeb-476">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="e4eeb-477">ソースを構成する `Action<KeyPerFileConfigurationSource>` デリゲート。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="e4eeb-478">ディレクトリを省略可能かどうか、またディレクトリへのパス。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="e4eeb-479">`CreateDefaultBuilder` を呼び出す場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="e4eeb-480"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="e4eeb-481">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-481">Memory Configuration Provider</span></span>

<span data-ttu-id="e4eeb-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> では、構成のキーと値のペアとして、メモリ内コレクションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="e4eeb-483">メモリ内コレクションの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="e4eeb-484">構成プロバイダーは、`IEnumerable<KeyValuePair<String,String>>` を使用して初期化できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-485">`CreateDefaultBuilder` を呼び出す場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="e4eeb-486"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で `UseConfiguration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-487">`UseConfiguration` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="e4eeb-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="e4eeb-488">GetValue</span></span>

<span data-ttu-id="e4eeb-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) によって、指定したキーで構成から値が抽出され、それが指定した型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="e4eeb-490">オーバーロードを使用すると、キーが見つからない場合に既定値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="e4eeb-491">次の例では、キー `NumberKey` を使用して構成から文字列値を抽出し、その値を `int` に型付けして、変数 `intValue` に格納します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="e4eeb-492">構成キーで `NumberKey` が見つからない場合、`intValue` は 規定値 `99` を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="e4eeb-493">GetSection、GetChildren、Exists</span><span class="sxs-lookup"><span data-stu-id="e4eeb-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="e4eeb-494">以下の例では、次の JSON ファイルについて考えます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="e4eeb-495">4 つのキーが 2 つのセクションに含まれています。そのうちの 1 つには、一組のサブセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="e4eeb-496">ファイルが構成に読み取られると、次の一意の階層キーが作成され、構成値が保持されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="e4eeb-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-497">section0:key0</span></span>
* <span data-ttu-id="e4eeb-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-498">section0:key1</span></span>
* <span data-ttu-id="e4eeb-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-499">section1:key0</span></span>
* <span data-ttu-id="e4eeb-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-500">section1:key1</span></span>
* <span data-ttu-id="e4eeb-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="e4eeb-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="e4eeb-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="e4eeb-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="e4eeb-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="e4eeb-505">GetSection</span></span>

<span data-ttu-id="e4eeb-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) では、指定したサブセクションのキーを持つ構成のサブセクションが抽出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="e4eeb-507">`section1` のキーと値のペアのみを含む <xref:Microsoft.Extensions.Configuration.IConfigurationSection> を返すには、`GetSection` を呼び出してセクション名を指定します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="e4eeb-508">同様に、`section2:subsection0` のキーの値を取得するには、`GetSection` を呼び出してセクションのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="e4eeb-509">`GetSection` が `null` を返すことはありません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="e4eeb-510">一致するセクションが見つからない場合は、空の `IConfigurationSection` が返されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="e4eeb-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="e4eeb-511">GetChildren</span></span>

<span data-ttu-id="e4eeb-512">`section2` での [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) の呼び出しでは、次を含む `IEnumerable<IConfigurationSection>` が取得されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="e4eeb-513">既存</span><span class="sxs-lookup"><span data-stu-id="e4eeb-513">Exists</span></span>

<span data-ttu-id="e4eeb-514">[ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) を使用して、構成セクションが存在するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="e4eeb-515">例のデータの場合、構成データに `section2:subsection2` セクションが存在しないため、`sectionExists` は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="e4eeb-516">クラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="e4eeb-516">Bind to a class</span></span>

<span data-ttu-id="e4eeb-517">"*オプション パターン*" を使用して、関連する設定のグループを表すクラスに構成をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="e4eeb-518">詳細については、「<xref:fundamentals/configuration/options>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="e4eeb-519">構成値は文字列として返されますが、<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> を呼び出すことで [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="e4eeb-520">サンプル アプリには `Starship` モデル (*Models/Starship.cs*) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-521">サンプル アプリが JSON 構成プロバイダーを使用して構成を読み込むときに、*starship.json* ファイルの `starship` セクションで構成が作成されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="e4eeb-522">次の構成のキーと値のペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="e4eeb-523">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-523">Key</span></span>                   | <span data-ttu-id="e4eeb-524">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="e4eeb-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="e4eeb-525">starship:name</span></span>         | <span data-ttu-id="e4eeb-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="e4eeb-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="e4eeb-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="e4eeb-527">starship:registry</span></span>     | <span data-ttu-id="e4eeb-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="e4eeb-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="e4eeb-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="e4eeb-529">starship:class</span></span>        | <span data-ttu-id="e4eeb-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="e4eeb-530">Constitution</span></span>                                      |
| <span data-ttu-id="e4eeb-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="e4eeb-531">starship:length</span></span>       | <span data-ttu-id="e4eeb-532">304.8</span><span class="sxs-lookup"><span data-stu-id="e4eeb-532">304.8</span></span>                                             |
| <span data-ttu-id="e4eeb-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="e4eeb-533">starship:commissioned</span></span> | <span data-ttu-id="e4eeb-534">False</span><span class="sxs-lookup"><span data-stu-id="e4eeb-534">False</span></span>                                             |
| <span data-ttu-id="e4eeb-535">trademark</span><span class="sxs-lookup"><span data-stu-id="e4eeb-535">trademark</span></span>             | <span data-ttu-id="e4eeb-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="e4eeb-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="e4eeb-537">サンプル アプリは `starship` キーを使用して `GetSection` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="e4eeb-538">`starship` のキーと値のペアは分離されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="e4eeb-539">`Starship` クラスのインスタンスを渡すサブセクションで、`Bind` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="e4eeb-540">インスタンスの値をバインドした後、インスタンスがレンダリング用のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="e4eeb-541">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="e4eeb-541">Bind to an object graph</span></span>

<span data-ttu-id="e4eeb-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> では、POCO オブジェクト グラフ全体をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="e4eeb-543">サンプルには、オブジェクト グラフに `Metadata` クラスと `Actors` クラスが含まれる `TvShow` モデルが含まれます (*Models/TvShow.cs*)。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-544">サンプル アプリには、構成データを含む *tvshow.xml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="e4eeb-545">構成は、`Bind` メソッドを使用して、`TvShow` オブジェクト グラフ全体にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="e4eeb-546">バインドされたインスタンスは、表示のためにプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="e4eeb-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) によってバインドされ、指定した型が返されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="e4eeb-548">`Get<T>` は `Bind` を使用するよりも便利です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="e4eeb-549">次のコードは、前の例と共に `Get<T>` を使用する方法を示しています。これにより、バインドされたインスタンスを、表示用に使用するプロパティに直接割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e4eeb-550">配列をクラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="e4eeb-550">Bind an array to a class</span></span>

<span data-ttu-id="e4eeb-551">*サンプル アプリは、このセクションで説明する概念を示しています。*</span><span class="sxs-lookup"><span data-stu-id="e4eeb-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="e4eeb-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="e4eeb-553">数値のキー セグメント (`:0:`、`:1:`、&hellip; `:{n}:`) を公開する配列形式は、すべて POCO クラスの配列にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="e4eeb-554">バインドは慣例に従って指定されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-554">Binding is provided by convention.</span></span> <span data-ttu-id="e4eeb-555">カスタム構成プロバイダーが配列のバインドを実装する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="e4eeb-556">**メモリ内配列の処理**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-556">**In-memory array processing**</span></span>

<span data-ttu-id="e4eeb-557">次の表に示す構成のキーと値について考えます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="e4eeb-558">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-558">Key</span></span>     | <span data-ttu-id="e4eeb-559">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="e4eeb-560">array:0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-560">array:0</span></span> | <span data-ttu-id="e4eeb-561">value0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-561">value0</span></span> |
| <span data-ttu-id="e4eeb-562">array:1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-562">array:1</span></span> | <span data-ttu-id="e4eeb-563">value1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-563">value1</span></span> |
| <span data-ttu-id="e4eeb-564">array:2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-564">array:2</span></span> | <span data-ttu-id="e4eeb-565">value2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-565">value2</span></span> |
| <span data-ttu-id="e4eeb-566">array:4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-566">array:4</span></span> | <span data-ttu-id="e4eeb-567">value4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-567">value4</span></span> |
| <span data-ttu-id="e4eeb-568">array:5</span><span class="sxs-lookup"><span data-stu-id="e4eeb-568">array:5</span></span> | <span data-ttu-id="e4eeb-569">value5</span><span class="sxs-lookup"><span data-stu-id="e4eeb-569">value5</span></span> |

<span data-ttu-id="e4eeb-570">これらのキーと値は、メモリ構成プロバイダーを使用してサンプル アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="e4eeb-571">配列は、インデックス &num;3 の値をスキップします。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="e4eeb-572">構成バインダーは、null 値をバインドしたり、バインドされたオブジェクトに null エントリを作成したりすることはできません。このことは、この配列をオブジェクトにバインドした結果によって明らかになります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="e4eeb-573">サンプル アプリでは、バインドされた構成データを保持するために POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-574">構成データはオブジェクトにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="e4eeb-575">また、[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 構文を使用して、コードをよりコンパクトにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="e4eeb-576">バインドされたオブジェクト (`ArrayExample` のインスタンス) は、構成から配列データを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="e4eeb-577">`ArrayExamples.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="e4eeb-578">`ArrayExamples.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="e4eeb-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="e4eeb-579">0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-579">0</span></span>                             | <span data-ttu-id="e4eeb-580">value0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-580">value0</span></span>                        |
| <span data-ttu-id="e4eeb-581">1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-581">1</span></span>                             | <span data-ttu-id="e4eeb-582">value1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-582">value1</span></span>                        |
| <span data-ttu-id="e4eeb-583">2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-583">2</span></span>                             | <span data-ttu-id="e4eeb-584">value2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-584">value2</span></span>                        |
| <span data-ttu-id="e4eeb-585">3</span><span class="sxs-lookup"><span data-stu-id="e4eeb-585">3</span></span>                             | <span data-ttu-id="e4eeb-586">value4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-586">value4</span></span>                        |
| <span data-ttu-id="e4eeb-587">4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-587">4</span></span>                             | <span data-ttu-id="e4eeb-588">value5</span><span class="sxs-lookup"><span data-stu-id="e4eeb-588">value5</span></span>                        |

<span data-ttu-id="e4eeb-589">バインドされたオブジェクトのインデックス &num;3 によって、`array:4` 構成キーの構成データと、その値 `value4` が保持されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="e4eeb-590">配列を含む構成データがバインドされると、構成キーの配列のインデックスは、オブジェクトを作成するときに構成データを反復処理するためだけに使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="e4eeb-591">構成データに null 値を保持することはできません。また、構成キーの配列が 1 つまたは複数のインデックスをスキップしても、バインドされたオブジェクトに null 値のエントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="e4eeb-592">インデックス &num;3 の不足している構成項目は、`ArrayExamples` インスタンスにバインドする前に、構成で適切なキーと値のペアを生成する構成プロバイダーによって指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="e4eeb-593">不足しているキーと値のペアを含む JSON 構成プロバイダーがサンプルに含まれる場合、`ArrayExamples.Entries` は完全な構成の配列と一致します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="e4eeb-594">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e4eeb-595">`ConfigureAppConfiguration` の場合:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e4eeb-596">`Startup` コンストラクターの場合:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="e4eeb-597">表に示すキーと値のペアが構成に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="e4eeb-598">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-598">Key</span></span>             | <span data-ttu-id="e4eeb-599">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="e4eeb-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="e4eeb-600">array:entries:3</span></span> | <span data-ttu-id="e4eeb-601">value3</span><span class="sxs-lookup"><span data-stu-id="e4eeb-601">value3</span></span> |

<span data-ttu-id="e4eeb-602">JSON 構成プロバイダーにインデックス &num;3 のエントリが含まれた後に `ArrayExamples` クラスのインスタンスがバインドされる場合、`ArrayExamples.Entries` 配列に値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="e4eeb-603">`ArrayExamples.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="e4eeb-604">`ArrayExamples.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="e4eeb-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="e4eeb-605">0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-605">0</span></span>                             | <span data-ttu-id="e4eeb-606">value0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-606">value0</span></span>                        |
| <span data-ttu-id="e4eeb-607">1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-607">1</span></span>                             | <span data-ttu-id="e4eeb-608">value1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-608">value1</span></span>                        |
| <span data-ttu-id="e4eeb-609">2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-609">2</span></span>                             | <span data-ttu-id="e4eeb-610">value2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-610">value2</span></span>                        |
| <span data-ttu-id="e4eeb-611">3</span><span class="sxs-lookup"><span data-stu-id="e4eeb-611">3</span></span>                             | <span data-ttu-id="e4eeb-612">value3</span><span class="sxs-lookup"><span data-stu-id="e4eeb-612">value3</span></span>                        |
| <span data-ttu-id="e4eeb-613">4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-613">4</span></span>                             | <span data-ttu-id="e4eeb-614">value4</span><span class="sxs-lookup"><span data-stu-id="e4eeb-614">value4</span></span>                        |
| <span data-ttu-id="e4eeb-615">5</span><span class="sxs-lookup"><span data-stu-id="e4eeb-615">5</span></span>                             | <span data-ttu-id="e4eeb-616">value5</span><span class="sxs-lookup"><span data-stu-id="e4eeb-616">value5</span></span>                        |

<span data-ttu-id="e4eeb-617">**JSON 配列の処理**</span><span class="sxs-lookup"><span data-stu-id="e4eeb-617">**JSON array processing**</span></span>

<span data-ttu-id="e4eeb-618">JSON ファイルに配列が含まれる場合、配列要素の構成キーは、0 から始まるセクションのインデックスで作成されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="e4eeb-619">次の構成ファイルにおいて、`subsection` は配列です。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="e4eeb-620">JSON 構成プロバイダーは、次のキーと値のペアに構成データを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="e4eeb-621">キー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-621">Key</span></span>                     | <span data-ttu-id="e4eeb-622">[値]</span><span class="sxs-lookup"><span data-stu-id="e4eeb-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="e4eeb-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="e4eeb-623">json_array:key</span></span>          | <span data-ttu-id="e4eeb-624">valueA</span><span class="sxs-lookup"><span data-stu-id="e4eeb-624">valueA</span></span> |
| <span data-ttu-id="e4eeb-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-625">json_array:subsection:0</span></span> | <span data-ttu-id="e4eeb-626">valueB</span><span class="sxs-lookup"><span data-stu-id="e4eeb-626">valueB</span></span> |
| <span data-ttu-id="e4eeb-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-627">json_array:subsection:1</span></span> | <span data-ttu-id="e4eeb-628">valueC</span><span class="sxs-lookup"><span data-stu-id="e4eeb-628">valueC</span></span> |
| <span data-ttu-id="e4eeb-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-629">json_array:subsection:2</span></span> | <span data-ttu-id="e4eeb-630">valueD</span><span class="sxs-lookup"><span data-stu-id="e4eeb-630">valueD</span></span> |

<span data-ttu-id="e4eeb-631">サンプル アプリでは、構成のキーと値のペアをバインドするために、次の POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-632">バインド後、`JsonArrayExample.Key` は値 `valueA` を保持します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="e4eeb-633">サブセクションの値は、POCO 配列のプロパティ `Subsection` に格納されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="e4eeb-634">`JsonArrayExample.Subsection` インデックス</span><span class="sxs-lookup"><span data-stu-id="e4eeb-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="e4eeb-635">`JsonArrayExample.Subsection` 値</span><span class="sxs-lookup"><span data-stu-id="e4eeb-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="e4eeb-636">0</span><span class="sxs-lookup"><span data-stu-id="e4eeb-636">0</span></span>                                   | <span data-ttu-id="e4eeb-637">valueB</span><span class="sxs-lookup"><span data-stu-id="e4eeb-637">valueB</span></span>                              |
| <span data-ttu-id="e4eeb-638">1</span><span class="sxs-lookup"><span data-stu-id="e4eeb-638">1</span></span>                                   | <span data-ttu-id="e4eeb-639">valueC</span><span class="sxs-lookup"><span data-stu-id="e4eeb-639">valueC</span></span>                              |
| <span data-ttu-id="e4eeb-640">2</span><span class="sxs-lookup"><span data-stu-id="e4eeb-640">2</span></span>                                   | <span data-ttu-id="e4eeb-641">valueD</span><span class="sxs-lookup"><span data-stu-id="e4eeb-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="e4eeb-642">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4eeb-642">Custom configuration provider</span></span>

<span data-ttu-id="e4eeb-643">サンプル アプリでは、[Entity Framework (EF)](/ef/core/) を使用してデータベースから構成のキーと値のペアを読み取る、基本的な構成プロバイダーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="e4eeb-644">プロバイダーの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="e4eeb-645">EF のメモリ内データベースは、デモンストレーションのために使用されます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="e4eeb-646">接続文字列を必要とするデータベースを使用するには、第 2 の `ConfigurationBuilder` を実装して、別の構成プロバイダーからの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="e4eeb-647">プロバイダーは、起動時に、構成にデータベース テーブルを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="e4eeb-648">プロバイダーは、キー単位でデータベースを照会しません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="e4eeb-649">変更時に再度読み込む機能は実装されていません。このため、アプリの起動後にデータベースを更新しても、アプリの構成には影響がありません。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="e4eeb-650">データベースに構成値を格納するための `EFConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="e4eeb-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-652">構成した値を格納し、その値にアクセスするための `EFConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="e4eeb-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-654"><xref:Microsoft.Extensions.Configuration.IConfigurationSource> を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="e4eeb-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-656"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider> から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="e4eeb-657">データベースが空だった場合、構成プロバイダーはこれを初期化します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="e4eeb-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-659">`AddEFConfiguration` 拡張メソッドを使用すると、`ConfigurationBuilder` に構成ソースを追加できます。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="e4eeb-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4eeb-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="e4eeb-661">次のコードでは、*Program.cs* でカスタムの `EFConfigurationProvider` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="e4eeb-662">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="e4eeb-662">Access configuration during startup</span></span>

<span data-ttu-id="e4eeb-663">`Startup` コンストラクターに `IConfiguration` を挿入して、`Startup.ConfigureServices` で構成値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e4eeb-664">`Startup.Configure` で構成にアクセスするには、メソッドに直接 `IConfiguration` を挿入するか、コンストラクターからのインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="e4eeb-665">起動時の簡易メソッドを使用して構成にアクセスする例については、[アプリ起動時の簡易メソッド](xref:fundamentals/startup#convenience-methods)に関連する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="e4eeb-666">Razor Pages ページまたは MVC ビューで構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="e4eeb-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="e4eeb-667">Razor Pages ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](xref:Microsoft.Extensions.Configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、<xref:Microsoft.Extensions.Configuration.IConfiguration> をページまたはビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="e4eeb-668">Razor ページ: </span><span class="sxs-lookup"><span data-stu-id="e4eeb-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="e4eeb-669">MVC ビュー: </span><span class="sxs-lookup"><span data-stu-id="e4eeb-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="e4eeb-670">外部アセンブリから構成を追加する</span><span class="sxs-lookup"><span data-stu-id="e4eeb-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="e4eeb-671"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="e4eeb-672">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4eeb-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4eeb-673">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e4eeb-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="e4eeb-674">Microsoft の構成について詳しく調べる</span><span class="sxs-lookup"><span data-stu-id="e4eeb-674">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
