---
title: ASP.NET Core の構成
author: guardrex
description: 構成 API を使用して、ASP.NET Core アプリを構成する方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: ad430f50b3d2616437b716b8be275937ef282bea
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882527"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="4aa18-103">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="4aa18-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="4aa18-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4aa18-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4aa18-105">ASP.NET Core でのアプリの構成は、"*構成プロバイダー*" によって設定するキーと値のペアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="4aa18-106">構成プロバイダーは、さまざまな構成のソースから構成データを読み取り、キーと値のペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="4aa18-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4aa18-107">Azure Key Vault</span></span>
* <span data-ttu-id="4aa18-108">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="4aa18-108">Command-line arguments</span></span>
* <span data-ttu-id="4aa18-109">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="4aa18-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4aa18-110">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="4aa18-110">Directory files</span></span>
* <span data-ttu-id="4aa18-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="4aa18-111">Environment variables</span></span>
* <span data-ttu-id="4aa18-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="4aa18-112">In-memory .NET objects</span></span>
* <span data-ttu-id="4aa18-113">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="4aa18-113">Settings files</span></span>

<span data-ttu-id="4aa18-114">"*オプション パターン*" は、このトピックで説明する構成の概念を拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="4aa18-115">オプションでは、クラスを使用して関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="4aa18-116">オプション パターンの使用について詳しくは、「<xref:fundamentals/configuration/options>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4aa18-117">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4aa18-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4aa18-118">これらの 3 つのパッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="4aa18-119">ホストとアプリの構成</span><span class="sxs-lookup"><span data-stu-id="4aa18-119">Host vs. app configuration</span></span>

<span data-ttu-id="4aa18-120">アプリを構成して起動する前に、"*ホスト*" を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="4aa18-121">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4aa18-122">アプリとホストは、両方ともこのトピックで説明する構成プロバイダーを使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="4aa18-123">ホストの構成のキーと値のペアは、アプリのグローバル構成の一部となります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="4aa18-124">ホストをビルドするときの構成プロバイダーの使用方法、およびホストの構成に対する構成ソースの影響について詳しくは、[ホスト](xref:fundamentals/index#host)に関する説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="4aa18-125">既定の構成</span><span class="sxs-lookup"><span data-stu-id="4aa18-125">Default configuration</span></span>

<span data-ttu-id="4aa18-126">ASP.NET Core の [dotnet new](/dotnet/core/tools/dotnet-new) テンプレートに基づく Web アプリは、ホストの構築時に <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="4aa18-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> により、次の順序でアプリの既定の構成が提供されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="4aa18-128">ホストの構成は、次から提供されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="4aa18-129">[環境変数構成プロバイダー](#environment-variables-configuration-provider)を使用する、プレフィックス `ASPNETCORE_` (`ASPNETCORE_ENVIRONMENT` など) が付いた環境変数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="4aa18-130">構成のキーと値のペアが読み込まれるときに、プレフィックス (`ASPNETCORE_`) は削除されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="4aa18-131">[コマンドライン構成プロバイダー](#command-line-configuration-provider)を使用するコマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="4aa18-132">アプリの構成は、次から提供されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="4aa18-133">[ファイル構成プロバイダー](#file-configuration-provider)を使用する *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="4aa18-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="4aa18-134">[ファイル構成プロバイダー](#file-configuration-provider)を使用する *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="4aa18-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="4aa18-135">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[シークレット マネージャー](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="4aa18-136">[環境変数構成プロバイダー](#environment-variables-configuration-provider)を使用する環境変数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="4aa18-137">カスタム プレフィックスが使用されている場合 (たとえば、`PREFIX_` と `.AddEnvironmentVariables(prefix: "PREFIX_")`)、構成のキーと値のペアが読み込まれるときにプレフィックスが削除されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="4aa18-138">[コマンドライン構成プロバイダー](#command-line-configuration-provider)を使用するコマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="4aa18-139">これらの構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="4aa18-140">ホストと <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> の詳細については、<xref:fundamentals/host/web-host#set-up-a-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="4aa18-141">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="4aa18-141">Security</span></span>

<span data-ttu-id="4aa18-142">次のベスト プラクティスを採用します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="4aa18-143">構成プロバイダーのコードやプレーンテキストの構成ファイルには、パスワードなどの機密データを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="4aa18-144">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="4aa18-145">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="4aa18-146">[複数の環境を使用する方法](xref:fundamentals/environments)や、[シークレット マネージャーでの開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)の管理 (機密データを格納するための環境変数の使用に関するアドバイスを含む) について、さらに学習することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="4aa18-147">シークレット マネージャーは、ファイル構成プロバイダーを使用して、ユーザーの機密情報をローカル システム上の JSON ファイルに格納します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="4aa18-148">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4aa18-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) は、アプリのシークレットを安全に格納するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="4aa18-150">詳細については、「<xref:security/key-vault-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="4aa18-151">階層的な構成データ</span><span class="sxs-lookup"><span data-stu-id="4aa18-151">Hierarchical configuration data</span></span>

<span data-ttu-id="4aa18-152">構成 API は、構成キー内の区切り記号を使用して階層データをフラット化することにより、階層的な構成データを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="4aa18-153">次の JSON ファイルでは、2 つのセクションの構造化階層に 4 つのキーが存在します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="4aa18-154">ファイルが構成に読み込まれると、構成ソースの元の階層データ構造を維持するために、一意なキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="4aa18-155">セクションとキーは、元の構造を維持するために、コロン (`:`) を使用してフラット化されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="4aa18-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-156">section0:key0</span></span>
* <span data-ttu-id="4aa18-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-157">section0:key1</span></span>
* <span data-ttu-id="4aa18-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-158">section1:key0</span></span>
* <span data-ttu-id="4aa18-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-159">section1:key1</span></span>

<span data-ttu-id="4aa18-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> メソッドと <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> メソッドを使用して、構成データ内のセクションとセクションの子を分離することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="4aa18-161">これらのメソッドについては、後ほど「[GetSection、GetChildren、Exists](#getsection-getchildren-and-exists)」で説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="4aa18-162">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="4aa18-163">規約</span><span class="sxs-lookup"><span data-stu-id="4aa18-163">Conventions</span></span>

<span data-ttu-id="4aa18-164">アプリの起動時に、各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="4aa18-165">ファイル構成プロバイダーには、アプリの起動後に基になる設定ファイルが変更された場合に、構成を再読み込みする機能があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-165">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="4aa18-166">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-166">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4aa18-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> は、アプリの[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) コンテナーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4aa18-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> を Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> を挿入して、クラスの構成を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="4aa18-169">構成プロバイダーでは DI を使用できません。ホストによって構成プロバイダーが設定されている場合、DI を使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="4aa18-170">構成キーでは、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="4aa18-171">キーでは、大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-171">Keys are case-insensitive.</span></span> <span data-ttu-id="4aa18-172">たとえば、`ConnectionString` と `connectionstring` は同等のキーとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="4aa18-173">同じキーに対する値が、同じまたは別の構成プロバイダーによって設定された場合、最後にキーに設定された値が使用される値となります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="4aa18-174">階層キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-174">Hierarchical keys</span></span>
  * <span data-ttu-id="4aa18-175">構成 API 内では、すべてのプラットフォームでコロン (`:`) の区切りが機能します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="4aa18-176">環境変数内では、コロン区切りがすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="4aa18-177">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="4aa18-178">Azure Key Vault では、階層キーは区切り記号として `--` (2 つのダッシュ) を使用します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4aa18-179">コードを指定して、アプリの構成にシークレットが読み込まれるときにダッシュをコロンで置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="4aa18-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4aa18-181">配列のバインドについては、「[配列をクラスにバインドする](#bind-an-array-to-a-class)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="4aa18-182">構成値では、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="4aa18-183">値は文字列です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-183">Values are strings.</span></span>
* <span data-ttu-id="4aa18-184">Null 値を構成に格納したり、オブジェクトにバインドしたりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="4aa18-185">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-185">Providers</span></span>

<span data-ttu-id="4aa18-186">ASP.NET Core アプリで使用できる構成プロバイダーを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="4aa18-187">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-187">Provider</span></span> | <span data-ttu-id="4aa18-188">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4aa18-189">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="4aa18-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4aa18-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4aa18-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="4aa18-191">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4aa18-192">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="4aa18-192">Command-line parameters</span></span> |
| [<span data-ttu-id="4aa18-193">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4aa18-194">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="4aa18-194">Custom source</span></span> |
| [<span data-ttu-id="4aa18-195">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4aa18-196">環境変数</span><span class="sxs-lookup"><span data-stu-id="4aa18-196">Environment variables</span></span> |
| [<span data-ttu-id="4aa18-197">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4aa18-198">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="4aa18-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4aa18-199">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="4aa18-200">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="4aa18-200">Directory files</span></span> |
| [<span data-ttu-id="4aa18-201">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4aa18-202">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="4aa18-202">In-memory collections</span></span> |
| <span data-ttu-id="4aa18-203">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="4aa18-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4aa18-204">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="4aa18-204">File in the user profile directory</span></span> |

<span data-ttu-id="4aa18-205">アプリの起動時に各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="4aa18-206">このトピックで説明する構成プロバイダーは、それらをコードで配置する順ではなく、アルファベット順で説明します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="4aa18-207">基になる構成ソースの優先順位に合わせるために、コード内で構成プロバイダーを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="4aa18-208">一般的な一連の構成プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="4aa18-209">ファイル (*appsettings.json*、*appsettings.{Environment}.json*。`{Environment}` はアプリの現在のホスト環境です)</span><span class="sxs-lookup"><span data-stu-id="4aa18-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="4aa18-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4aa18-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="4aa18-211">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合のみ)</span><span class="sxs-lookup"><span data-stu-id="4aa18-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="4aa18-212">環境変数</span><span class="sxs-lookup"><span data-stu-id="4aa18-212">Environment variables</span></span>
1. <span data-ttu-id="4aa18-213">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="4aa18-213">Command-line arguments</span></span>

<span data-ttu-id="4aa18-214">コマンド ライン引数が他のプロバイダーによって設定された構成をオーバーライドできるようにするために、コマンド ラインの構成プロバイダーを一連のプロバイダーの最後に配置するのは、一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="4aa18-215">この一連のプロバイダーは、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を使用して新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化するときに配置されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4aa18-216">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="4aa18-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="4aa18-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="4aa18-218">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出し、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> によって自動的に追加されるものに加え、アプリの構成プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="4aa18-219"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリに指定した構成は、`Startup.ConfigureServices` などのアプリの起動中に使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4aa18-220">詳細については、「[起動中に構成にアクセスする](#access-configuration-during-startup)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="4aa18-221">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="4aa18-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> では、実行時にコマンドライン引数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="4aa18-223">コマンド ライン構成をアクティブにするために、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドが <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4aa18-224"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddCommandLine` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4aa18-225">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4aa18-226">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4aa18-227">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="4aa18-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4aa18-228">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4aa18-229">環境変数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-229">Environment variables.</span></span>

<span data-ttu-id="4aa18-230">`CreateDefaultBuilder` はコマンド ライン構成プロバイダーを最後に追加します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="4aa18-231">実行時に渡されるコマンド ライン引数によって、他のプロバイダーによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="4aa18-232">`CreateDefaultBuilder` は、ホストが作成されるときに機能します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="4aa18-233">そのため、`CreateDefaultBuilder` によってアクティブ化されるコマンド ライン構成によって、ホストの構成方法に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="4aa18-234">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4aa18-235">`AddCommandLine` は既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4aa18-236">アプリの構成を指定して、引き続きコマンドラインの引数でその構成をオーバーライドできるようにする必要がある場合、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> でアプリの追加のプロバイダーを呼び出し、最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-237"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4aa18-238">**例**</span><span class="sxs-lookup"><span data-stu-id="4aa18-238">**Example**</span></span>

<span data-ttu-id="4aa18-239">サンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="4aa18-240">プロジェクトのディレクトリでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="4aa18-241">`dotnet run` コマンドにコマンドライン引数を指定します (`dotnet run CommandLineKey=CommandLineValue`)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="4aa18-242">アプリを実行したら、アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4aa18-243">出力に、`dotnet run` に提供される構成のコマンド ライン引数のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="4aa18-244">引数</span><span class="sxs-lookup"><span data-stu-id="4aa18-244">Arguments</span></span>

<span data-ttu-id="4aa18-245">値は等号 (`=`) の後に続ける必要があります。または、値をスペースの後に続ける場合は、キーにプレフィックス (`--`または`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="4aa18-246">等号を使用する場合は、値に null を指定できます (例: `CommandLineKey=`)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="4aa18-247">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-247">Key prefix</span></span>               | <span data-ttu-id="4aa18-248">例</span><span class="sxs-lookup"><span data-stu-id="4aa18-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="4aa18-249">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="4aa18-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="4aa18-250">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="4aa18-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="4aa18-251">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="4aa18-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="4aa18-252">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="4aa18-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="4aa18-253">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="4aa18-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="4aa18-254">同じコマンド内のコマンド ライン引数で、等号を使用するキーと値のペアと、スペースを使用するキーと値のペアを混在させないでください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="4aa18-255">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="4aa18-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="4aa18-256">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="4aa18-256">Switch mappings</span></span>

<span data-ttu-id="4aa18-257">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="4aa18-258"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> で構成を手動でビルドするときに、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> メソッドにスイッチ置換のディクショナリを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="4aa18-259">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="4aa18-260">コマンド ライン キーがディクショナリで見つかった場合は、アプリの構成にキーと値のペアを設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="4aa18-261">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="4aa18-262">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="4aa18-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="4aa18-263">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="4aa18-264">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="4aa18-265">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-266">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4aa18-267">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4aa18-268">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4aa18-269">ソリューションでは `CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="4aa18-270">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="4aa18-271">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-271">Key</span></span>       | <span data-ttu-id="4aa18-272">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="4aa18-273">アプリの起動時にスイッチ マッピングされたキーを使用する場合、構成は、ディクショナリによって指定されたキーでの構成値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="4aa18-274">上記のコマンドを実行すると、次の表に示す値が構成に含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="4aa18-275">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-275">Key</span></span>               | <span data-ttu-id="4aa18-276">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="4aa18-277">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="4aa18-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> では、実行時に環境変数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="4aa18-279">環境変数の構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="4aa18-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) を使用すると、環境変数構成プロバイダーを使用してアプリの構成をオーバーライドすることができる環境変数を、Azure Portal で設定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="4aa18-281">詳細については、「[Azure アプリ: Azure Portal を使用してアプリの構成をオーバーライドする](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="4aa18-282">新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、`ASPNETCORE_` というプレフィックスが付いた環境変数に対して自動的に `AddEnvironmentVariables` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="4aa18-283">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4aa18-284">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4aa18-285">プレフィックスなしの `AddEnvironmentVariables` 呼び出しによる、プレフィックスの付いていない環境変数からのアプリの構成。</span><span class="sxs-lookup"><span data-stu-id="4aa18-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="4aa18-286">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="4aa18-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4aa18-287">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4aa18-288">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-288">Command-line arguments.</span></span>

<span data-ttu-id="4aa18-289">ユーザー シークレットと *appsettings* ファイルから構成が設定された後に、環境変数構成プロバイダーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="4aa18-290">この位置でプロバイダーを呼び出すことにより、実行時に読み込まれた環境変数が、ユーザー シークレットと *appsettings* ファイルによって設定された構成をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="4aa18-291">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4aa18-292">追加の環境変数からアプリの構成を指定する必要がある場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリの追加プロバイダーを呼び出し、そのプレフィックスを含む `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-293"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4aa18-294">**例**</span><span class="sxs-lookup"><span data-stu-id="4aa18-294">**Example**</span></span>

<span data-ttu-id="4aa18-295">サンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには `AddEnvironmentVariables` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="4aa18-296">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-296">Run the sample app.</span></span> <span data-ttu-id="4aa18-297">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4aa18-298">出力に、環境変数 `ENVIRONMENT` のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="4aa18-299">値には、アプリを実行している環境が反映されます (ローカルで実行している場合は通常 `Development`)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="4aa18-300">アプリによって表示される環境変数のリストを短くするために、アプリでは次で始まる環境変数がフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="4aa18-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="4aa18-301">ASPNETCORE_</span></span>
* <span data-ttu-id="4aa18-302">urls</span><span class="sxs-lookup"><span data-stu-id="4aa18-302">urls</span></span>
* <span data-ttu-id="4aa18-303">ログの記録</span><span class="sxs-lookup"><span data-stu-id="4aa18-303">Logging</span></span>
* <span data-ttu-id="4aa18-304">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="4aa18-304">ENVIRONMENT</span></span>
* <span data-ttu-id="4aa18-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="4aa18-305">contentRoot</span></span>
* <span data-ttu-id="4aa18-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4aa18-306">AllowedHosts</span></span>
* <span data-ttu-id="4aa18-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="4aa18-307">applicationName</span></span>
* <span data-ttu-id="4aa18-308">CommandLine</span><span class="sxs-lookup"><span data-stu-id="4aa18-308">CommandLine</span></span>

<span data-ttu-id="4aa18-309">アプリで使用できるすべての環境変数を公開する場合は、*Pages/Index.cshtml.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="4aa18-310">プレフィックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-310">Prefixes</span></span>

<span data-ttu-id="4aa18-311">アプリの構成に読み込まれる環境変数は、`AddEnvironmentVariables` メソッドにプレフィックスを指定すると、フィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="4aa18-312">たとえば、プレフィックス `CUSTOM_` で環境変数をフィルター処理するには、構成プロバイダーにプレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="4aa18-313">構成のキーと値のペアが作成されるときに、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="4aa18-314">静的な簡易メソッド `CreateDefaultBuilder` によって <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> が作成され、アプリのホストが確立されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="4aa18-315">`WebHostBuilder` は、作成されるときに、`ASPNETCORE_` というプレフィックスが付いた環境変数からホストの構成を見つけます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="4aa18-316">**接続文字列のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="4aa18-316">**Connection string prefixes**</span></span>

<span data-ttu-id="4aa18-317">構成 API には、アプリの環境に向けた Azure の接続文字列の構成に関係する、4 つの接続文字列環境変数のための特別な処理規則があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="4aa18-318">表に示されるプレフィックスを含む環境変数は、`AddEnvironmentVariables` にプレフィックスが指定されていない場合、アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="4aa18-319">接続文字列のプレフィックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-319">Connection string prefix</span></span> | <span data-ttu-id="4aa18-320">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="4aa18-321">カスタム プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="4aa18-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="4aa18-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="4aa18-323">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="4aa18-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="4aa18-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4aa18-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="4aa18-325">表に示す 4 つのプレフィックスのいずれかを使用して、環境変数が検出され構成に読み込まれた場合:</span><span class="sxs-lookup"><span data-stu-id="4aa18-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="4aa18-326">環境変数のプレフィックスを削除し、構成キーのセクション (`ConnectionStrings`) を追加することによって、構成キーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="4aa18-327">データベースの接続プロバイダーを表す新しい構成のキーと値のペアが作成されます (示されたプロバイダーを含まない `CUSTOMCONNSTR_` を除く)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="4aa18-328">環境変数キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-328">Environment variable key</span></span> | <span data-ttu-id="4aa18-329">変換された構成キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-329">Converted configuration key</span></span> | <span data-ttu-id="4aa18-330">プロバイダーの構成エントリ</span><span class="sxs-lookup"><span data-stu-id="4aa18-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4aa18-331">構成エントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4aa18-332">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4aa18-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4aa18-333">値: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="4aa18-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4aa18-334">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4aa18-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4aa18-335">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4aa18-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4aa18-336">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4aa18-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4aa18-337">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4aa18-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="4aa18-338">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-338">File Configuration Provider</span></span>

<span data-ttu-id="4aa18-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> は、ファイル システムから構成を読み込むための基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="4aa18-340">次の構成プロバイダーは、特定のファイルの種類専用です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="4aa18-341">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="4aa18-342">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="4aa18-343">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="4aa18-344">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-344">INI Configuration Provider</span></span>

<span data-ttu-id="4aa18-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> では、実行時に INI ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="4aa18-346">INI ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4aa18-347">INI ファイルの構成では、セクションの区切り記号としてコロンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="4aa18-348">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="4aa18-349">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-349">Whether the file is optional.</span></span>
* <span data-ttu-id="4aa18-350">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4aa18-351">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4aa18-352">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-353">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-354">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-355"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4aa18-356">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-357">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-358">INI 構成ファイルの汎用的な例:</span><span class="sxs-lookup"><span data-stu-id="4aa18-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="4aa18-359">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4aa18-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-360">section0:key0</span></span>
* <span data-ttu-id="4aa18-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-361">section0:key1</span></span>
* <span data-ttu-id="4aa18-362">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="4aa18-362">section1:subsection:key</span></span>
* <span data-ttu-id="4aa18-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="4aa18-363">section2:subsection0:key</span></span>
* <span data-ttu-id="4aa18-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="4aa18-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="4aa18-365">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-365">JSON Configuration Provider</span></span>

<span data-ttu-id="4aa18-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> では、実行時に JSON ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="4aa18-367">JSON ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4aa18-368">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="4aa18-369">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-369">Whether the file is optional.</span></span>
* <span data-ttu-id="4aa18-370">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4aa18-371">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4aa18-372"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddJsonFile` が 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4aa18-373">このメソッドは、次から構成を読み込むために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="4aa18-374">*appsettings.json* &ndash; このファイルが最初に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="4aa18-375">ファイルの環境バージョンは、*appsettings.json* ファイルによって指定される値をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="4aa18-376">*appsettings.{Environment}.json* &ndash; ファイルの環境バージョンは、[IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) に基づいて読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="4aa18-377">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4aa18-378">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4aa18-379">環境変数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-379">Environment variables.</span></span>
* <span data-ttu-id="4aa18-380">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4aa18-381">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="4aa18-381">Command-line arguments.</span></span>

<span data-ttu-id="4aa18-382">JSON 構成プロバイダーが最初に確立されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="4aa18-383">このため、ユーザー シークレット、環境変数、およびコマンド ライン引数によって、*appsettings* ファイルによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="4aa18-384">ホストのビルド時に <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、*appsettings.json* と *appsettings.{Environment}.json* 以外のファイルにアプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-385">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-386">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-387"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4aa18-388">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-389">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-390">**例**</span><span class="sxs-lookup"><span data-stu-id="4aa18-390">**Example**</span></span>

<span data-ttu-id="4aa18-391">サンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには `AddJsonFile` の 2 回の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="4aa18-392">構成は、*appsettings.json* および *appsettings.{Environment}.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="4aa18-393">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-393">Run the sample app.</span></span> <span data-ttu-id="4aa18-394">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4aa18-395">出力に、環境に応じて、表に示す構成のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="4aa18-396">ログの構成キーでは、階層の区切り文字としてコロン (`:`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="4aa18-397">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-397">Key</span></span>                        | <span data-ttu-id="4aa18-398">開発時の値</span><span class="sxs-lookup"><span data-stu-id="4aa18-398">Development Value</span></span> | <span data-ttu-id="4aa18-399">運用時の値</span><span class="sxs-lookup"><span data-stu-id="4aa18-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="4aa18-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="4aa18-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="4aa18-401">情報</span><span class="sxs-lookup"><span data-stu-id="4aa18-401">Information</span></span>       | <span data-ttu-id="4aa18-402">情報</span><span class="sxs-lookup"><span data-stu-id="4aa18-402">Information</span></span>      |
| <span data-ttu-id="4aa18-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="4aa18-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="4aa18-404">情報</span><span class="sxs-lookup"><span data-stu-id="4aa18-404">Information</span></span>       | <span data-ttu-id="4aa18-405">情報</span><span class="sxs-lookup"><span data-stu-id="4aa18-405">Information</span></span>      |
| <span data-ttu-id="4aa18-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="4aa18-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="4aa18-407">デバッグ</span><span class="sxs-lookup"><span data-stu-id="4aa18-407">Debug</span></span>             | <span data-ttu-id="4aa18-408">Error</span><span class="sxs-lookup"><span data-stu-id="4aa18-408">Error</span></span>            |
| <span data-ttu-id="4aa18-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4aa18-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="4aa18-410">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-410">XML Configuration Provider</span></span>

<span data-ttu-id="4aa18-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> では、実行時に XML ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="4aa18-412">XML ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4aa18-413">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="4aa18-414">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-414">Whether the file is optional.</span></span>
* <span data-ttu-id="4aa18-415">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="4aa18-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4aa18-416">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4aa18-417">構成ファイルのルート ノードは、構成のキーと値のペアを作成するときには無視されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="4aa18-418">ファイル内でドキュメント型定義 (DTD) または名前空間を指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="4aa18-419">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-420">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-421">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-422"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4aa18-423">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-424">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-425">XML 構成ファイルでは、繰り返しのセクション用に個別の要素名を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="4aa18-426">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4aa18-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-427">section0:key0</span></span>
* <span data-ttu-id="4aa18-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-428">section0:key1</span></span>
* <span data-ttu-id="4aa18-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-429">section1:key0</span></span>
* <span data-ttu-id="4aa18-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-430">section1:key1</span></span>

<span data-ttu-id="4aa18-431">同じ要素名を使用する要素の繰り返しは、要素を区別するために `name` 属性を使用する場合に機能します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="4aa18-432">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4aa18-433">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-433">section:section0:key:key0</span></span>
* <span data-ttu-id="4aa18-434">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-434">section:section0:key:key1</span></span>
* <span data-ttu-id="4aa18-435">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-435">section:section1:key:key0</span></span>
* <span data-ttu-id="4aa18-436">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-436">section:section1:key:key1</span></span>

<span data-ttu-id="4aa18-437">値を指定するために属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="4aa18-438">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4aa18-439">key:attribute</span><span class="sxs-lookup"><span data-stu-id="4aa18-439">key:attribute</span></span>
* <span data-ttu-id="4aa18-440">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="4aa18-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="4aa18-441">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="4aa18-442"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> では、構成のキーと値のペアとしてディレクトリのファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="4aa18-443">キーはファイル名です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-443">The key is the file name.</span></span> <span data-ttu-id="4aa18-444">値にはファイルのコンテンツが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-444">The value contains the file's contents.</span></span> <span data-ttu-id="4aa18-445">ファイルごとのキーの構成プロバイダーは、Docker ホスティングのシナリオで使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="4aa18-446">ファイルごとのキーの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4aa18-447">ファイルに対する `directoryPath` は、絶対パスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="4aa18-448">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="4aa18-449">ソースを構成する `Action<KeyPerFileConfigurationSource>` デリゲート。</span><span class="sxs-lookup"><span data-stu-id="4aa18-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="4aa18-450">ディレクトリを省略可能かどうか、またディレクトリへのパス。</span><span class="sxs-lookup"><span data-stu-id="4aa18-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="4aa18-451">アンダースコア 2 つ (`__`) は、ファイル名で構成キーの区切り記号として使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="4aa18-452">たとえば、ファイル名 `Logging__LogLevel__System` では、構成キー `Logging:LogLevel:System` が生成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="4aa18-453">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-454">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4aa18-455">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-456"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="4aa18-457">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-457">Memory Configuration Provider</span></span>

<span data-ttu-id="4aa18-458"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> では、構成のキーと値のペアとして、メモリ内コレクションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="4aa18-459">メモリ内コレクションの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4aa18-460">構成プロバイダーは、`IEnumerable<KeyValuePair<String,String>>` を使用して初期化できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="4aa18-461">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4aa18-462"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="4aa18-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="4aa18-463">GetValue</span></span>

<span data-ttu-id="4aa18-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) によって、指定したキーで構成から値が抽出され、それが指定した型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="4aa18-465">オーバーロードを使用すると、キーが見つからない場合に既定値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="4aa18-466">次のような例です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-466">The following example:</span></span>

* <span data-ttu-id="4aa18-467">キー `NumberKey` の文字列値を構成から抽出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="4aa18-468">`NumberKey` が構成キーに見つからない場合、既定値の `99` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="4aa18-469">値の型は `int` です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="4aa18-470">`NumberConfig` プロパティの値をページで使用するために格納します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="4aa18-471">GetSection、GetChildren、Exists</span><span class="sxs-lookup"><span data-stu-id="4aa18-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="4aa18-472">以下の例では、次の JSON ファイルについて考えます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="4aa18-473">4 つのキーが 2 つのセクションに含まれています。そのうちの 1 つには、一組のサブセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="4aa18-474">ファイルが構成に読み取られると、次の一意の階層キーが作成され、構成値が保持されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="4aa18-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-475">section0:key0</span></span>
* <span data-ttu-id="4aa18-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-476">section0:key1</span></span>
* <span data-ttu-id="4aa18-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-477">section1:key0</span></span>
* <span data-ttu-id="4aa18-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-478">section1:key1</span></span>
* <span data-ttu-id="4aa18-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="4aa18-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="4aa18-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="4aa18-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="4aa18-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="4aa18-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="4aa18-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="4aa18-483">GetSection</span></span>

<span data-ttu-id="4aa18-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) では、指定したサブセクションのキーを持つ構成のサブセクションが抽出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="4aa18-485">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-486">`section1` のキーと値のペアのみを含む <xref:Microsoft.Extensions.Configuration.IConfigurationSection> を返すには、`GetSection` を呼び出してセクション名を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="4aa18-487">`configSection` には値はなく、キーとパスのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="4aa18-488">同様に、`section2:subsection0` のキーの値を取得するには、`GetSection` を呼び出してセクションのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="4aa18-489">`GetSection` が `null` を返すことはありません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="4aa18-490">一致するセクションが見つからない場合は、空の `IConfigurationSection` が返されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="4aa18-491">`GetSection` で一致するセクションが返されると、<xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> は設定されません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="4aa18-492"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> と <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> はセクションが存在する場合に返されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="4aa18-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="4aa18-493">GetChildren</span></span>

<span data-ttu-id="4aa18-494">`section2` での [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) の呼び出しでは、次を含む `IEnumerable<IConfigurationSection>` が取得されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="4aa18-495">既存</span><span class="sxs-lookup"><span data-stu-id="4aa18-495">Exists</span></span>

<span data-ttu-id="4aa18-496">[ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) を使用して、構成セクションが存在するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="4aa18-497">例のデータの場合、構成データに `section2:subsection2` セクションが存在しないため、`sectionExists` は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="4aa18-498">クラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="4aa18-498">Bind to a class</span></span>

<span data-ttu-id="4aa18-499">"*オプション パターン*" を使用して、関連する設定のグループを表すクラスに構成をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="4aa18-500">詳細については、「<xref:fundamentals/configuration/options>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4aa18-501">構成値は文字列として返されますが、<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> を呼び出すことで [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="4aa18-502">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-503">サンプル アプリには `Starship` モデル (*Models/Starship.cs*) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="4aa18-504">サンプル アプリが JSON 構成プロバイダーを使用して構成を読み込むときに、*starship.json* ファイルの `starship` セクションで構成が作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="4aa18-505">次の構成のキーと値のペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="4aa18-506">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-506">Key</span></span>                   | <span data-ttu-id="4aa18-507">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="4aa18-508">starship:name</span><span class="sxs-lookup"><span data-stu-id="4aa18-508">starship:name</span></span>         | <span data-ttu-id="4aa18-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="4aa18-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="4aa18-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="4aa18-510">starship:registry</span></span>     | <span data-ttu-id="4aa18-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="4aa18-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="4aa18-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="4aa18-512">starship:class</span></span>        | <span data-ttu-id="4aa18-513">Constitution</span><span class="sxs-lookup"><span data-stu-id="4aa18-513">Constitution</span></span>                                      |
| <span data-ttu-id="4aa18-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="4aa18-514">starship:length</span></span>       | <span data-ttu-id="4aa18-515">304.8</span><span class="sxs-lookup"><span data-stu-id="4aa18-515">304.8</span></span>                                             |
| <span data-ttu-id="4aa18-516">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="4aa18-516">starship:commissioned</span></span> | <span data-ttu-id="4aa18-517">False</span><span class="sxs-lookup"><span data-stu-id="4aa18-517">False</span></span>                                             |
| <span data-ttu-id="4aa18-518">trademark</span><span class="sxs-lookup"><span data-stu-id="4aa18-518">trademark</span></span>             | <span data-ttu-id="4aa18-519">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="4aa18-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="4aa18-520">サンプル アプリは `starship` キーを使用して `GetSection` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="4aa18-521">`starship` のキーと値のペアは分離されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="4aa18-522">`Starship` クラスのインスタンスを渡すサブセクションで、`Bind` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="4aa18-523">インスタンスの値をバインドした後、インスタンスがレンダリング用のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="4aa18-524">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="4aa18-525">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="4aa18-525">Bind to an object graph</span></span>

<span data-ttu-id="4aa18-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> では、POCO オブジェクト グラフ全体をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="4aa18-527">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-528">サンプルには、オブジェクト グラフに `Metadata` クラスと `Actors` クラスが含まれる `TvShow` モデルが含まれます (*Models/TvShow.cs*)。</span><span class="sxs-lookup"><span data-stu-id="4aa18-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="4aa18-529">サンプル アプリには、構成データを含む *tvshow.xml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="4aa18-530">構成は、`Bind` メソッドを使用して、`TvShow` オブジェクト グラフ全体にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="4aa18-531">バインドされたインスタンスは、表示のためにプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="4aa18-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) によってバインドされ、指定した型が返されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="4aa18-533">`Get<T>` は `Bind` を使用するよりも便利です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="4aa18-534">次のコードは、前の例と共に `Get<T>` を使用する方法を示しています。これにより、バインドされたインスタンスを、表示用に使用するプロパティに直接割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="4aa18-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4aa18-536">`Get<T>` は、ASP.NET Core 1.1 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="4aa18-537">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4aa18-538">配列をクラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="4aa18-538">Bind an array to a class</span></span>

<span data-ttu-id="4aa18-539">*サンプル アプリは、このセクションで説明する概念を示しています。*</span><span class="sxs-lookup"><span data-stu-id="4aa18-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="4aa18-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="4aa18-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4aa18-541">数値のキー セグメント (`:0:`、`:1:`、&hellip; `:{n}:`) を公開する配列形式は、すべて POCO クラスの配列にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="4aa18-542">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="4aa18-543">バインドは慣例に従って指定されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-543">Binding is provided by convention.</span></span> <span data-ttu-id="4aa18-544">カスタム構成プロバイダーが配列のバインドを実装する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="4aa18-545">**メモリ内配列の処理**</span><span class="sxs-lookup"><span data-stu-id="4aa18-545">**In-memory array processing**</span></span>

<span data-ttu-id="4aa18-546">次の表に示す構成のキーと値について考えます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="4aa18-547">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-547">Key</span></span>             | <span data-ttu-id="4aa18-548">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4aa18-549">配列:エントリ:0</span><span class="sxs-lookup"><span data-stu-id="4aa18-549">array:entries:0</span></span> | <span data-ttu-id="4aa18-550">value0</span><span class="sxs-lookup"><span data-stu-id="4aa18-550">value0</span></span> |
| <span data-ttu-id="4aa18-551">配列:エントリ:1</span><span class="sxs-lookup"><span data-stu-id="4aa18-551">array:entries:1</span></span> | <span data-ttu-id="4aa18-552">value1</span><span class="sxs-lookup"><span data-stu-id="4aa18-552">value1</span></span> |
| <span data-ttu-id="4aa18-553">配列:エントリ:2</span><span class="sxs-lookup"><span data-stu-id="4aa18-553">array:entries:2</span></span> | <span data-ttu-id="4aa18-554">value2</span><span class="sxs-lookup"><span data-stu-id="4aa18-554">value2</span></span> |
| <span data-ttu-id="4aa18-555">配列:エントリ:4</span><span class="sxs-lookup"><span data-stu-id="4aa18-555">array:entries:4</span></span> | <span data-ttu-id="4aa18-556">value4</span><span class="sxs-lookup"><span data-stu-id="4aa18-556">value4</span></span> |
| <span data-ttu-id="4aa18-557">配列:エントリ:5</span><span class="sxs-lookup"><span data-stu-id="4aa18-557">array:entries:5</span></span> | <span data-ttu-id="4aa18-558">value5</span><span class="sxs-lookup"><span data-stu-id="4aa18-558">value5</span></span> |

<span data-ttu-id="4aa18-559">これらのキーと値は、メモリ構成プロバイダーを使用してサンプル アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="4aa18-560">配列は、インデックス &num;3 の値をスキップします。</span><span class="sxs-lookup"><span data-stu-id="4aa18-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="4aa18-561">構成バインダーは、null 値をバインドしたり、バインドされたオブジェクトに null エントリを作成したりすることはできません。このことは、この配列をオブジェクトにバインドした結果によって明らかになります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="4aa18-562">サンプル アプリでは、バインドされた構成データを保持するために POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="4aa18-563">構成データはオブジェクトにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="4aa18-564">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4aa18-565">また、[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 構文を使用して、コードをよりコンパクトにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="4aa18-566">バインドされたオブジェクト (`ArrayExample` のインスタンス) は、構成から配列データを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="4aa18-567">`ArrayExample.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="4aa18-568">`ArrayExample.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="4aa18-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="4aa18-569">0</span><span class="sxs-lookup"><span data-stu-id="4aa18-569">0</span></span>                            | <span data-ttu-id="4aa18-570">value0</span><span class="sxs-lookup"><span data-stu-id="4aa18-570">value0</span></span>                       |
| <span data-ttu-id="4aa18-571">1</span><span class="sxs-lookup"><span data-stu-id="4aa18-571">1</span></span>                            | <span data-ttu-id="4aa18-572">value1</span><span class="sxs-lookup"><span data-stu-id="4aa18-572">value1</span></span>                       |
| <span data-ttu-id="4aa18-573">2</span><span class="sxs-lookup"><span data-stu-id="4aa18-573">2</span></span>                            | <span data-ttu-id="4aa18-574">value2</span><span class="sxs-lookup"><span data-stu-id="4aa18-574">value2</span></span>                       |
| <span data-ttu-id="4aa18-575">3</span><span class="sxs-lookup"><span data-stu-id="4aa18-575">3</span></span>                            | <span data-ttu-id="4aa18-576">value4</span><span class="sxs-lookup"><span data-stu-id="4aa18-576">value4</span></span>                       |
| <span data-ttu-id="4aa18-577">4</span><span class="sxs-lookup"><span data-stu-id="4aa18-577">4</span></span>                            | <span data-ttu-id="4aa18-578">value5</span><span class="sxs-lookup"><span data-stu-id="4aa18-578">value5</span></span>                       |

<span data-ttu-id="4aa18-579">バインドされたオブジェクトのインデックス &num;3 によって、`array:4` 構成キーの構成データと、その値 `value4` が保持されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="4aa18-580">配列を含む構成データがバインドされると、構成キーの配列のインデックスは、オブジェクトを作成するときに構成データを反復処理するためだけに使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="4aa18-581">構成データに null 値を保持することはできません。また、構成キーの配列が 1 つまたは複数のインデックスをスキップしても、バインドされたオブジェクトに null 値のエントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="4aa18-582">インデックス &num;3 の不足している構成項目は、`ArrayExample` インスタンスにバインドする前に、構成で適切なキーと値のペアを生成する構成プロバイダーによって指定できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="4aa18-583">不足しているキーと値のペアを含む JSON 構成プロバイダーがサンプルに含まれる場合、`ArrayExample.Entries` は完全な構成の配列と一致します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="4aa18-584">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="4aa18-585"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>の場合:</span><span class="sxs-lookup"><span data-stu-id="4aa18-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="4aa18-586">表に示すキーと値のペアが構成に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="4aa18-587">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-587">Key</span></span>             | <span data-ttu-id="4aa18-588">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4aa18-589">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="4aa18-589">array:entries:3</span></span> | <span data-ttu-id="4aa18-590">value3</span><span class="sxs-lookup"><span data-stu-id="4aa18-590">value3</span></span> |

<span data-ttu-id="4aa18-591">JSON 構成プロバイダーにインデックス &num;3 のエントリが含まれた後に `ArrayExample` クラスのインスタンスがバインドされる場合、`ArrayExample.Entries` 配列に値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="4aa18-592">`ArrayExample.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="4aa18-593">`ArrayExample.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="4aa18-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="4aa18-594">0</span><span class="sxs-lookup"><span data-stu-id="4aa18-594">0</span></span>                            | <span data-ttu-id="4aa18-595">value0</span><span class="sxs-lookup"><span data-stu-id="4aa18-595">value0</span></span>                       |
| <span data-ttu-id="4aa18-596">1</span><span class="sxs-lookup"><span data-stu-id="4aa18-596">1</span></span>                            | <span data-ttu-id="4aa18-597">value1</span><span class="sxs-lookup"><span data-stu-id="4aa18-597">value1</span></span>                       |
| <span data-ttu-id="4aa18-598">2</span><span class="sxs-lookup"><span data-stu-id="4aa18-598">2</span></span>                            | <span data-ttu-id="4aa18-599">value2</span><span class="sxs-lookup"><span data-stu-id="4aa18-599">value2</span></span>                       |
| <span data-ttu-id="4aa18-600">3</span><span class="sxs-lookup"><span data-stu-id="4aa18-600">3</span></span>                            | <span data-ttu-id="4aa18-601">value3</span><span class="sxs-lookup"><span data-stu-id="4aa18-601">value3</span></span>                       |
| <span data-ttu-id="4aa18-602">4</span><span class="sxs-lookup"><span data-stu-id="4aa18-602">4</span></span>                            | <span data-ttu-id="4aa18-603">value4</span><span class="sxs-lookup"><span data-stu-id="4aa18-603">value4</span></span>                       |
| <span data-ttu-id="4aa18-604">5</span><span class="sxs-lookup"><span data-stu-id="4aa18-604">5</span></span>                            | <span data-ttu-id="4aa18-605">value5</span><span class="sxs-lookup"><span data-stu-id="4aa18-605">value5</span></span>                       |

<span data-ttu-id="4aa18-606">**JSON 配列の処理**</span><span class="sxs-lookup"><span data-stu-id="4aa18-606">**JSON array processing**</span></span>

<span data-ttu-id="4aa18-607">JSON ファイルに配列が含まれる場合、配列要素の構成キーは、0 から始まるセクションのインデックスで作成されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="4aa18-608">次の構成ファイルにおいて、`subsection` は配列です。</span><span class="sxs-lookup"><span data-stu-id="4aa18-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="4aa18-609">JSON 構成プロバイダーは、次のキーと値のペアに構成データを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="4aa18-610">キー</span><span class="sxs-lookup"><span data-stu-id="4aa18-610">Key</span></span>                     | <span data-ttu-id="4aa18-611">[値]</span><span class="sxs-lookup"><span data-stu-id="4aa18-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="4aa18-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="4aa18-612">json_array:key</span></span>          | <span data-ttu-id="4aa18-613">valueA</span><span class="sxs-lookup"><span data-stu-id="4aa18-613">valueA</span></span> |
| <span data-ttu-id="4aa18-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="4aa18-614">json_array:subsection:0</span></span> | <span data-ttu-id="4aa18-615">valueB</span><span class="sxs-lookup"><span data-stu-id="4aa18-615">valueB</span></span> |
| <span data-ttu-id="4aa18-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="4aa18-616">json_array:subsection:1</span></span> | <span data-ttu-id="4aa18-617">valueC</span><span class="sxs-lookup"><span data-stu-id="4aa18-617">valueC</span></span> |
| <span data-ttu-id="4aa18-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="4aa18-618">json_array:subsection:2</span></span> | <span data-ttu-id="4aa18-619">valueD</span><span class="sxs-lookup"><span data-stu-id="4aa18-619">valueD</span></span> |

<span data-ttu-id="4aa18-620">サンプル アプリでは、構成のキーと値のペアをバインドするために、次の POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="4aa18-621">バインド後、`JsonArrayExample.Key` は値 `valueA` を保持します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="4aa18-622">サブセクションの値は、POCO 配列のプロパティ `Subsection` に格納されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="4aa18-623">`JsonArrayExample.Subsection` インデックス</span><span class="sxs-lookup"><span data-stu-id="4aa18-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="4aa18-624">`JsonArrayExample.Subsection` 値</span><span class="sxs-lookup"><span data-stu-id="4aa18-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="4aa18-625">0</span><span class="sxs-lookup"><span data-stu-id="4aa18-625">0</span></span>                                   | <span data-ttu-id="4aa18-626">valueB</span><span class="sxs-lookup"><span data-stu-id="4aa18-626">valueB</span></span>                              |
| <span data-ttu-id="4aa18-627">1</span><span class="sxs-lookup"><span data-stu-id="4aa18-627">1</span></span>                                   | <span data-ttu-id="4aa18-628">valueC</span><span class="sxs-lookup"><span data-stu-id="4aa18-628">valueC</span></span>                              |
| <span data-ttu-id="4aa18-629">2</span><span class="sxs-lookup"><span data-stu-id="4aa18-629">2</span></span>                                   | <span data-ttu-id="4aa18-630">valueD</span><span class="sxs-lookup"><span data-stu-id="4aa18-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="4aa18-631">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4aa18-631">Custom configuration provider</span></span>

<span data-ttu-id="4aa18-632">サンプル アプリでは、[Entity Framework (EF)](/ef/core/) を使用してデータベースから構成のキーと値のペアを読み取る、基本的な構成プロバイダーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="4aa18-633">プロバイダーの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4aa18-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="4aa18-634">EF のメモリ内データベースは、デモンストレーションのために使用されます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="4aa18-635">接続文字列を必要とするデータベースを使用するには、第 2 の `ConfigurationBuilder` を実装して、別の構成プロバイダーからの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="4aa18-636">プロバイダーは、起動時に、構成にデータベース テーブルを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="4aa18-637">プロバイダーは、キー単位でデータベースを照会しません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="4aa18-638">変更時に再度読み込む機能は実装されていません。このため、アプリの起動後にデータベースを更新しても、アプリの構成には影響がありません。</span><span class="sxs-lookup"><span data-stu-id="4aa18-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="4aa18-639">データベースに構成値を格納するための `EFConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="4aa18-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="4aa18-641">構成した値を格納し、その値にアクセスするための `EFConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="4aa18-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="4aa18-643"><xref:Microsoft.Extensions.Configuration.IConfigurationSource> を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="4aa18-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="4aa18-645"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider> から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="4aa18-646">データベースが空だった場合、構成プロバイダーはこれを初期化します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="4aa18-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="4aa18-648">`AddEFConfiguration` 拡張メソッドを使用すると、`ConfigurationBuilder` に構成ソースを追加できます。</span><span class="sxs-lookup"><span data-stu-id="4aa18-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="4aa18-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aa18-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="4aa18-650">次のコードでは、*Program.cs* でカスタムの `EFConfigurationProvider` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="4aa18-651">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="4aa18-651">Access configuration during startup</span></span>

<span data-ttu-id="4aa18-652">`Startup` コンストラクターに `IConfiguration` を挿入して、`Startup.ConfigureServices` で構成値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="4aa18-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4aa18-653">`Startup.Configure` で構成にアクセスするには、メソッドに直接 `IConfiguration` を挿入するか、コンストラクターからのインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="4aa18-654">起動時の簡易メソッドを使用して構成にアクセスする例については、[アプリ起動時の簡易メソッド](xref:fundamentals/startup#convenience-methods)に関連する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="4aa18-655">Razor Pages ページまたは MVC ビューで構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="4aa18-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="4aa18-656">Razor Pages ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](xref:Microsoft.Extensions.Configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、<xref:Microsoft.Extensions.Configuration.IConfiguration> をページまたはビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="4aa18-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="4aa18-657">Razor ページ: </span><span class="sxs-lookup"><span data-stu-id="4aa18-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="4aa18-658">MVC ビュー: </span><span class="sxs-lookup"><span data-stu-id="4aa18-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="4aa18-659">外部アセンブリから構成を追加する</span><span class="sxs-lookup"><span data-stu-id="4aa18-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="4aa18-660"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="4aa18-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="4aa18-661">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4aa18-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4aa18-662">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4aa18-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="4aa18-663">Microsoft の構成について詳しく調べる</span><span class="sxs-lookup"><span data-stu-id="4aa18-663">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
