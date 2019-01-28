---
title: ASP.NET Core の構成
author: guardrex
description: 構成 API を使用して、ASP.NET Core アプリを構成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 2465570e469020ae2855508bd1bfc8528e188ebb
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073167"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="15b7a-103">ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="15b7a-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="15b7a-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15b7a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="15b7a-105">ASP.NET Core でのアプリの構成は、"*構成プロバイダー*" によって設定するキーと値のペアに基づいています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="15b7a-106">構成プロバイダーは、さまざまな構成のソースから構成データを読み取り、キーと値のペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="15b7a-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="15b7a-107">Azure Key Vault</span></span>
* <span data-ttu-id="15b7a-108">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="15b7a-108">Command-line arguments</span></span>
* <span data-ttu-id="15b7a-109">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="15b7a-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="15b7a-110">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-110">Directory files</span></span>
* <span data-ttu-id="15b7a-111">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-111">Environment variables</span></span>
* <span data-ttu-id="15b7a-112">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="15b7a-112">In-memory .NET objects</span></span>
* <span data-ttu-id="15b7a-113">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="15b7a-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="15b7a-114">Azure Key Vault</span></span>
* <span data-ttu-id="15b7a-115">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="15b7a-115">Command-line arguments</span></span>
* <span data-ttu-id="15b7a-116">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="15b7a-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="15b7a-117">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-117">Environment variables</span></span>
* <span data-ttu-id="15b7a-118">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="15b7a-118">In-memory .NET objects</span></span>
* <span data-ttu-id="15b7a-119">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="15b7a-120">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="15b7a-120">Command-line arguments</span></span>
* <span data-ttu-id="15b7a-121">カスタム プロバイダー (インストール済みまたは作成済み)</span><span class="sxs-lookup"><span data-stu-id="15b7a-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="15b7a-122">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-122">Environment variables</span></span>
* <span data-ttu-id="15b7a-123">メモリ内 .NET オブジェクト</span><span class="sxs-lookup"><span data-stu-id="15b7a-123">In-memory .NET objects</span></span>
* <span data-ttu-id="15b7a-124">設定ファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="15b7a-125">"*オプション パターン*" は、このトピックで説明する構成の概念を拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="15b7a-126">オプションでは、クラスを使用して関連する設定のグループを表します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="15b7a-127">オプション パターンの使用について詳しくは、「<xref:fundamentals/configuration/options>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="15b7a-128">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="15b7a-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-129">これらの 3 つのパッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-130">これらの 3 つのパッケージは、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="15b7a-131">ホストとアプリの構成</span><span class="sxs-lookup"><span data-stu-id="15b7a-131">Host vs. app configuration</span></span>

<span data-ttu-id="15b7a-132">アプリを構成して起動する前に、"*ホスト*" を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="15b7a-133">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="15b7a-134">アプリとホストは、両方ともこのトピックで説明する構成プロバイダーを使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="15b7a-135">ホストの構成のキーと値のペアは、アプリのグローバル構成の一部となります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="15b7a-136">ホストをビルドするときの構成プロバイダーの使用方法、およびホストの構成に対する構成ソースの影響について詳しくは、「<xref:fundamentals/host/index>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="15b7a-137">既定の構成</span><span class="sxs-lookup"><span data-stu-id="15b7a-137">Default configuration</span></span>

<span data-ttu-id="15b7a-138">ASP.NET Core の [dotnet new](/dotnet/core/tools/dotnet-new) テンプレートに基づく Web アプリは、ホストの構築時に <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="15b7a-139">`CreateDefaultBuilder` はアプリの既定の構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="15b7a-140">ホストの構成は、次から提供されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="15b7a-141">[環境変数構成プロバイダー](#environment-variables-configuration-provider)を使用する、プレフィックス `ASPNETCORE_` (`ASPNETCORE_ENVIRONMENT` など) が付いた環境変数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="15b7a-142">[コマンドライン構成プロバイダー](#command-line-configuration-provider)を使用するコマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="15b7a-143">アプリの構成は、次の順序で提供されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="15b7a-144">[ファイル構成プロバイダー](#file-configuration-provider)を使用する *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="15b7a-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="15b7a-145">[ファイル構成プロバイダー](#file-configuration-provider)を使用する *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="15b7a-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="15b7a-146">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[シークレット マネージャー](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="15b7a-147">[環境変数構成プロバイダー](#environment-variables-configuration-provider)を使用する環境変数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="15b7a-148">[コマンドライン構成プロバイダー](#command-line-configuration-provider)を使用するコマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="15b7a-149">これらの構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="15b7a-150">ホストと `CreateDefaultBuilder` の詳細については、<xref:fundamentals/host/web-host#set-up-a-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="15b7a-151">セキュリティ</span><span class="sxs-lookup"><span data-stu-id="15b7a-151">Security</span></span>

<span data-ttu-id="15b7a-152">次のベスト プラクティスを採用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="15b7a-153">構成プロバイダーのコードやプレーンテキストの構成ファイルには、パスワードなどの機密データを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="15b7a-154">開発環境やテスト環境では運用シークレットを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="15b7a-155">プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="15b7a-156">[複数の環境を使用する方法](xref:fundamentals/environments)や、[シークレット マネージャーでの開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)の管理 (機密データを格納するための環境変数の使用に関するアドバイスを含む) について、さらに学習することができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="15b7a-157">シークレット マネージャーは、ファイル構成プロバイダーを使用して、ユーザーの機密情報をローカル システム上の JSON ファイルに格納します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="15b7a-158">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="15b7a-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) は、アプリのシークレットを安全に格納するための 1 つのオプションです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="15b7a-160">詳細については、「<xref:security/key-vault-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="15b7a-161">階層的な構成データ</span><span class="sxs-lookup"><span data-stu-id="15b7a-161">Hierarchical configuration data</span></span>

<span data-ttu-id="15b7a-162">構成 API は、構成キー内の区切り記号を使用して階層データをフラット化することにより、階層的な構成データを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="15b7a-163">次の JSON ファイルでは、2 つのセクションの構造化階層に 4 つのキーが存在します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="15b7a-164">ファイルが構成に読み込まれると、構成ソースの元の階層データ構造を維持するために、一意なキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="15b7a-165">セクションとキーは、元の構造を維持するために、コロン (`:`) を使用してフラット化されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="15b7a-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-166">section0:key0</span></span>
* <span data-ttu-id="15b7a-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-167">section0:key1</span></span>
* <span data-ttu-id="15b7a-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-168">section1:key0</span></span>
* <span data-ttu-id="15b7a-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-169">section1:key1</span></span>

<span data-ttu-id="15b7a-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> メソッドと <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> メソッドを使用して、構成データ内のセクションとセクションの子を分離することができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="15b7a-171">これらのメソッドについては、後ほど「[GetSection、GetChildren、Exists](#getsection-getchildren-and-exists)」で説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="15b7a-172">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="15b7a-173">規約</span><span class="sxs-lookup"><span data-stu-id="15b7a-173">Conventions</span></span>

<span data-ttu-id="15b7a-174">アプリの起動時に、各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="15b7a-175">ファイル構成プロバイダーには、アプリの起動後に基になる設定ファイルが変更された場合に、構成を再読み込みする機能があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="15b7a-176">ファイル構成プロバイダーについては、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="15b7a-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> は、アプリの[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) コンテナーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="15b7a-178">構成プロバイダーでは DI を使用できません。ホストによって構成プロバイダーが設定されている場合、DI を使用できないためです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="15b7a-179">構成キーでは、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="15b7a-180">キーでは、大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-180">Keys are case-insensitive.</span></span> <span data-ttu-id="15b7a-181">たとえば、`ConnectionString` と `connectionstring` は同等のキーとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="15b7a-182">同じキーに対する値が、同じまたは別の構成プロバイダーによって設定された場合、最後にキーに設定された値が使用される値となります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="15b7a-183">階層キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-183">Hierarchical keys</span></span>
  * <span data-ttu-id="15b7a-184">構成 API 内では、すべてのプラットフォームでコロン (`:`) の区切りが機能します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="15b7a-185">環境変数内では、コロン区切りがすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="15b7a-186">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに変換されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="15b7a-187">Azure Key Vault では、階層キーは区切り記号として `--` (2 つのダッシュ) を使用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="15b7a-188">コードを指定して、アプリの構成にシークレットが読み込まれるときにダッシュをコロンで置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="15b7a-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="15b7a-190">配列のバインドについては、「[配列をクラスにバインドする](#bind-an-array-to-a-class)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="15b7a-191">構成値では、次の規則が採用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="15b7a-192">値は文字列です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-192">Values are strings.</span></span>
* <span data-ttu-id="15b7a-193">Null 値を構成に格納したり、オブジェクトにバインドしたりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="15b7a-194">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-194">Providers</span></span>

<span data-ttu-id="15b7a-195">ASP.NET Core アプリで使用できる構成プロバイダーを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="15b7a-196">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-196">Provider</span></span> | <span data-ttu-id="15b7a-197">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="15b7a-198">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="15b7a-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="15b7a-199">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="15b7a-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="15b7a-200">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="15b7a-201">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="15b7a-201">Command-line parameters</span></span> |
| [<span data-ttu-id="15b7a-202">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="15b7a-203">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="15b7a-203">Custom source</span></span> |
| [<span data-ttu-id="15b7a-204">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="15b7a-205">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-205">Environment variables</span></span> |
| [<span data-ttu-id="15b7a-206">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="15b7a-207">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="15b7a-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="15b7a-208">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="15b7a-209">ディレクトリ ファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-209">Directory files</span></span> |
| [<span data-ttu-id="15b7a-210">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="15b7a-211">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="15b7a-211">In-memory collections</span></span> |
| <span data-ttu-id="15b7a-212">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="15b7a-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="15b7a-213">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="15b7a-214">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-214">Provider</span></span> | <span data-ttu-id="15b7a-215">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="15b7a-216">[Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="15b7a-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="15b7a-217">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="15b7a-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="15b7a-218">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="15b7a-219">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="15b7a-219">Command-line parameters</span></span> |
| [<span data-ttu-id="15b7a-220">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="15b7a-221">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="15b7a-221">Custom source</span></span> |
| [<span data-ttu-id="15b7a-222">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="15b7a-223">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-223">Environment variables</span></span> |
| [<span data-ttu-id="15b7a-224">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="15b7a-225">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="15b7a-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="15b7a-226">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="15b7a-227">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="15b7a-227">In-memory collections</span></span> |
| <span data-ttu-id="15b7a-228">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="15b7a-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="15b7a-229">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="15b7a-230">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-230">Provider</span></span> | <span data-ttu-id="15b7a-231">&hellip; から構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="15b7a-232">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="15b7a-233">コマンド ライン パラメーター</span><span class="sxs-lookup"><span data-stu-id="15b7a-233">Command-line parameters</span></span> |
| [<span data-ttu-id="15b7a-234">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="15b7a-235">カスタム ソース</span><span class="sxs-lookup"><span data-stu-id="15b7a-235">Custom source</span></span> |
| [<span data-ttu-id="15b7a-236">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="15b7a-237">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-237">Environment variables</span></span> |
| [<span data-ttu-id="15b7a-238">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="15b7a-239">ファイル (INI、JSON、XML)</span><span class="sxs-lookup"><span data-stu-id="15b7a-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="15b7a-240">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="15b7a-241">メモリ内コレクション</span><span class="sxs-lookup"><span data-stu-id="15b7a-241">In-memory collections</span></span> |
| <span data-ttu-id="15b7a-242">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) ("*セキュリティ*" トピック)</span><span class="sxs-lookup"><span data-stu-id="15b7a-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="15b7a-243">ユーザー プロファイル ディレクトリ内のファイル</span><span class="sxs-lookup"><span data-stu-id="15b7a-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="15b7a-244">アプリの起動時に各構成プロバイダーが指定されている順序で構成ソースが読み取られます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="15b7a-245">このトピックで説明する構成プロバイダーは、それらをコードで配置する順ではなく、アルファベット順で説明します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="15b7a-246">基になる構成ソースの優先順位に合わせるために、コード内で構成プロバイダーを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="15b7a-247">一般的な一連の構成プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="15b7a-248">ファイル (*appsettings.json*、*appsettings.{Environment}.json*。`{Environment}` はアプリの現在のホスト環境です)</span><span class="sxs-lookup"><span data-stu-id="15b7a-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="15b7a-249">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="15b7a-249">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="15b7a-250">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合のみ)</span><span class="sxs-lookup"><span data-stu-id="15b7a-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="15b7a-251">環境変数</span><span class="sxs-lookup"><span data-stu-id="15b7a-251">Environment variables</span></span>
1. <span data-ttu-id="15b7a-252">コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="15b7a-252">Command-line arguments</span></span>

<span data-ttu-id="15b7a-253">コマンド ライン引数が他のプロバイダーによって設定された構成をオーバーライドできるようにするために、コマンド ラインの構成プロバイダーを一連のプロバイダーの最後に配置するのは、一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-254">この一連のプロバイダーは、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を使用して新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化するときに配置されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="15b7a-255">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-256"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> および `Startup` 内のその <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> メソッドの呼び出しを使用して、(ホストではなく) アプリ用に一連のプロバイダーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="15b7a-257">前の例では、<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> によって環境名 (`env.EnvironmentName`) とアプリのアセンブリ名 (`env.ApplicationName`) が提供されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="15b7a-258">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="15b7a-259">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-260">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="15b7a-261">.</span><span class="sxs-lookup"><span data-stu-id="15b7a-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="15b7a-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="15b7a-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="15b7a-263">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出し、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> によって自動的に追加されるものに加え、アプリの構成プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="15b7a-264">コマンド ライン構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="15b7a-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> では、実行時にコマンドライン引数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="15b7a-266">コマンド ライン構成をアクティブにするために、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドが <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-267"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddCommandLine` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="15b7a-268">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="15b7a-269">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="15b7a-270">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="15b7a-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="15b7a-271">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="15b7a-272">環境変数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-272">Environment variables.</span></span>

<span data-ttu-id="15b7a-273">`CreateDefaultBuilder` はコマンド ライン構成プロバイダーを最後に追加します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="15b7a-274">実行時に渡されるコマンド ライン引数によって、他のプロバイダーによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="15b7a-275">`CreateDefaultBuilder` は、ホストが作成されるときに機能します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="15b7a-276">そのため、`CreateDefaultBuilder` によってアクティブ化されるコマンド ライン構成によって、ホストの構成方法に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-277">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="15b7a-278">`AddCommandLine` は既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="15b7a-279">アプリの構成を指定して、引き続きコマンドラインの引数でその構成をオーバーライドできるようにする必要がある場合、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> でアプリの追加のプロバイダーを呼び出し、最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="15b7a-280"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-281"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="15b7a-282">`UseConfiguration` が呼び出される場合、`AddCommandLine` は既に `CreateDefaultBuilder` によって呼び出されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="15b7a-283">アプリの構成を指定して、引き続きコマンドラインの引数でその構成をオーバーライドできるようにする必要がある場合、`ConfigurationBuilder` でアプリの追加のプロバイダーを呼び出し、最後に `AddCommandLine` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="15b7a-284"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-285">コマンド ライン構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-286">最後にプロバイダーを呼び出すことにより、実行時に渡されるコマンドライン引数で、他の構成プロバイダーによって設定された構成をオーバーライドできるようにします。</span><span class="sxs-lookup"><span data-stu-id="15b7a-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="15b7a-287"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="15b7a-288">**例**</span><span class="sxs-lookup"><span data-stu-id="15b7a-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-289">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-290">1.x のサンプル アプリでは、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> の <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="15b7a-291">プロジェクトのディレクトリでコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="15b7a-292">`dotnet run` コマンドにコマンドライン引数を指定します (`dotnet run CommandLineKey=CommandLineValue`)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="15b7a-293">アプリを実行したら、アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="15b7a-294">出力に、`dotnet run` に提供される構成のコマンド ライン引数のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="15b7a-295">引数</span><span class="sxs-lookup"><span data-stu-id="15b7a-295">Arguments</span></span>

<span data-ttu-id="15b7a-296">値は等号 (`=`) の後に続ける必要があります。または、値をスペースの後に続ける場合は、キーにプレフィックス (`--`または`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="15b7a-297">等号を使用する場合は、値に null を指定できます (例: `CommandLineKey=`)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="15b7a-298">キーのプレフィックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-298">Key prefix</span></span>               | <span data-ttu-id="15b7a-299">例</span><span class="sxs-lookup"><span data-stu-id="15b7a-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="15b7a-300">プレフィックスなし</span><span class="sxs-lookup"><span data-stu-id="15b7a-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="15b7a-301">2 つのダッシュ (`--`)</span><span class="sxs-lookup"><span data-stu-id="15b7a-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="15b7a-302">`--CommandLineKey2=value2`、 `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="15b7a-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="15b7a-303">スラッシュ (`/`)</span><span class="sxs-lookup"><span data-stu-id="15b7a-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="15b7a-304">`/CommandLineKey3=value3`、 `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="15b7a-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="15b7a-305">同じコマンド内のコマンド ライン引数で、等号を使用するキーと値のペアと、スペースを使用するキーと値のペアを混在させないでください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="15b7a-306">コマンドの例:</span><span class="sxs-lookup"><span data-stu-id="15b7a-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="15b7a-307">スイッチ マッピング</span><span class="sxs-lookup"><span data-stu-id="15b7a-307">Switch mappings</span></span>

<span data-ttu-id="15b7a-308">スイッチ マッピングでは、キー名の交換ロジックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="15b7a-309"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> で構成を手動でビルドするときに、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> メソッドにスイッチ置換のディクショナリを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="15b7a-310">スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="15b7a-311">コマンド ライン キーがディクショナリで見つかった場合は、アプリの構成にキーと値のペアを設定するためにディクショナリの値 (キー交換) が返されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="15b7a-312">スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="15b7a-313">スイッチ マッピング ディクショナリ キーの規則:</span><span class="sxs-lookup"><span data-stu-id="15b7a-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="15b7a-314">スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="15b7a-315">スイッチ マッピング ディクショナリに重複キーを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-316">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="15b7a-317">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="15b7a-318">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="15b7a-319">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="15b7a-320">ソリューションでは `CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="15b7a-321">前の例で示したように、スイッチ マッピングを使用する場合は `CreateDefaultBuilder` への呼び出しが引数を渡すことはできません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="15b7a-322">`CreateDefaultBuilder` メソッドの `AddCommandLine` の呼び出しにはマップされたスイッチが含まれないため、スイッチ マッピング ディクショナリを `CreateDefaultBuilder` に渡す方法はありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="15b7a-323">引数にマップされたスイッチが含まれており、それが `CreateDefaultBuilder` に渡される場合は、その `AddCommandLine` プロバイダーは <xref:System.FormatException> で初期化に失敗します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="15b7a-324">ソリューションでは `CreateDefaultBuilder` に引数を渡す代わりに、`ConfigurationBuilder` メソッドの `AddCommandLine` メソッドに、引数とスイッチ マッピング ディクショナリの両方を処理させることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="15b7a-325">スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="15b7a-326">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-326">Key</span></span>       | <span data-ttu-id="15b7a-327">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="15b7a-328">アプリの起動時にスイッチ マッピングされたキーを使用する場合、構成は、ディクショナリによって指定されたキーでの構成値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="15b7a-329">上記のコマンドを実行すると、次の表に示す値が構成に含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="15b7a-330">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-330">Key</span></span>               | <span data-ttu-id="15b7a-331">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="15b7a-332">環境変数構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="15b7a-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> では、実行時に環境変数のキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="15b7a-334">環境変数の構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-335">環境変数内で階層キーを操作する場合、コロン区切り (`:`) がすべてのプラットフォームでは機能しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="15b7a-336">二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされ、コロンに置換されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="15b7a-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) を使用すると、環境変数構成プロバイダーを使用してアプリの構成をオーバーライドすることができる環境変数を、Azure Portal で設定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="15b7a-338">詳細については、「[Azure アプリ: Azure Portal を使用してアプリの構成をオーバーライドする](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-339">新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、`ASPNETCORE_` というプレフィックスが付いた環境変数に対して自動的に `AddEnvironmentVariables` が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="15b7a-340">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="15b7a-341">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="15b7a-342">プレフィックスなしの `AddEnvironmentVariables` 呼び出しによる、プレフィックスの付いていない環境変数からのアプリの構成。</span><span class="sxs-lookup"><span data-stu-id="15b7a-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="15b7a-343">*appsettings.json* および *appsettings.{Environment}.json* からのオプションの構成。</span><span class="sxs-lookup"><span data-stu-id="15b7a-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="15b7a-344">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="15b7a-345">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-345">Command-line arguments.</span></span>

<span data-ttu-id="15b7a-346">ユーザー シークレットと *appsettings* ファイルから構成が設定された後に、環境変数構成プロバイダーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="15b7a-347">この位置でプロバイダーを呼び出すことにより、実行時に読み込まれた環境変数が、ユーザー シークレットと *appsettings* ファイルによって設定された構成をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-348">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="15b7a-349">追加の環境変数からアプリの構成を指定する必要がある場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリの追加プロバイダーを呼び出し、そのプレフィックスを含む `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="15b7a-350"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-351"><xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddEnvironmentVariables` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="15b7a-352"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="15b7a-353">追加の環境変数からアプリの構成を指定する必要がある場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> のアプリの追加プロバイダーを呼び出し、そのプレフィックスを含む `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="15b7a-354"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-355"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="15b7a-356">**例**</span><span class="sxs-lookup"><span data-stu-id="15b7a-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-357">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには `AddEnvironmentVariables` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-358">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddEnvironmentVariables` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="15b7a-359">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-359">Run the sample app.</span></span> <span data-ttu-id="15b7a-360">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="15b7a-361">出力に、環境変数 `ENVIRONMENT` のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="15b7a-362">値には、アプリを実行している環境が反映されます (ローカルで実行している場合は通常 `Development`)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="15b7a-363">アプリによって表示される環境変数のリストを短くするために、アプリでは次で始まる環境変数がフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="15b7a-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="15b7a-364">ASPNETCORE_</span></span>
* <span data-ttu-id="15b7a-365">urls</span><span class="sxs-lookup"><span data-stu-id="15b7a-365">urls</span></span>
* <span data-ttu-id="15b7a-366">ログの記録</span><span class="sxs-lookup"><span data-stu-id="15b7a-366">Logging</span></span>
* <span data-ttu-id="15b7a-367">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="15b7a-367">ENVIRONMENT</span></span>
* <span data-ttu-id="15b7a-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="15b7a-368">contentRoot</span></span>
* <span data-ttu-id="15b7a-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="15b7a-369">AllowedHosts</span></span>
* <span data-ttu-id="15b7a-370">applicationName</span><span class="sxs-lookup"><span data-stu-id="15b7a-370">applicationName</span></span>
* <span data-ttu-id="15b7a-371">CommandLine</span><span class="sxs-lookup"><span data-stu-id="15b7a-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-372">アプリで使用できるすべての環境変数を公開する場合は、*Pages/Index.cshtml.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-373">アプリで使用できるすべての環境変数を公開する場合は、*Controllers/HomeController.cs* の `FilteredConfiguration` を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="15b7a-374">プレフィックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-374">Prefixes</span></span>

<span data-ttu-id="15b7a-375">アプリの構成に読み込まれる環境変数は、`AddEnvironmentVariables` メソッドにプレフィックスを指定すると、フィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="15b7a-376">たとえば、プレフィックス `CUSTOM_` で環境変数をフィルター処理するには、構成プロバイダーにプレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="15b7a-377">構成のキーと値のペアが作成されるときに、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-378">静的な簡易メソッド `CreateDefaultBuilder` によって <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> が作成され、アプリのホストが確立されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="15b7a-379">`WebHostBuilder` は、作成されるときに、`ASPNETCORE_` というプレフィックスが付いた環境変数からホストの構成を見つけます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="15b7a-380">**接続文字列のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="15b7a-380">**Connection string prefixes**</span></span>

<span data-ttu-id="15b7a-381">構成 API には、アプリの環境に向けた Azure の接続文字列の構成に関係する、4 つの接続文字列環境変数のための特別な処理規則があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="15b7a-382">表に示されるプレフィックスを含む環境変数は、`AddEnvironmentVariables` にプレフィックスが指定されていない場合、アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="15b7a-383">接続文字列のプレフィックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-383">Connection string prefix</span></span> | <span data-ttu-id="15b7a-384">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="15b7a-385">カスタム プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="15b7a-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="15b7a-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="15b7a-387">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="15b7a-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="15b7a-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="15b7a-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="15b7a-389">表に示す 4 つのプレフィックスのいずれかを使用して、環境変数が検出され構成に読み込まれた場合:</span><span class="sxs-lookup"><span data-stu-id="15b7a-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="15b7a-390">環境変数のプレフィックスを削除し、構成キーのセクション (`ConnectionStrings`) を追加することによって、構成キーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="15b7a-391">データベースの接続プロバイダーを表す新しい構成のキーと値のペアが作成されます (示されたプロバイダーを含まない `CUSTOMCONNSTR_` を除く)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="15b7a-392">環境変数キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-392">Environment variable key</span></span> | <span data-ttu-id="15b7a-393">変換された構成キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-393">Converted configuration key</span></span> | <span data-ttu-id="15b7a-394">プロバイダーの構成エントリ</span><span class="sxs-lookup"><span data-stu-id="15b7a-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="15b7a-395">構成エントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="15b7a-396">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="15b7a-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="15b7a-397">値: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="15b7a-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="15b7a-398">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="15b7a-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="15b7a-399">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="15b7a-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="15b7a-400">キー: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="15b7a-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="15b7a-401">値: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="15b7a-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="15b7a-402">ファイル構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-402">File Configuration Provider</span></span>

<span data-ttu-id="15b7a-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> は、ファイル システムから構成を読み込むための基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="15b7a-404">次の構成プロバイダーは、特定のファイルの種類専用です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="15b7a-405">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="15b7a-406">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="15b7a-407">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="15b7a-408">INI 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-408">INI Configuration Provider</span></span>

<span data-ttu-id="15b7a-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> では、実行時に INI ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="15b7a-410">INI ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-411">INI ファイルの構成では、セクションの区切り記号としてコロンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="15b7a-412">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="15b7a-413">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-413">Whether the file is optional.</span></span>
* <span data-ttu-id="15b7a-414">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="15b7a-415">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-416">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="15b7a-417">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-418">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-419"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-420">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="15b7a-421">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-422">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-423"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-424"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="15b7a-425">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-426">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-427">INI 構成ファイルの汎用的な例:</span><span class="sxs-lookup"><span data-stu-id="15b7a-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="15b7a-428">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="15b7a-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-429">section0:key0</span></span>
* <span data-ttu-id="15b7a-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-430">section0:key1</span></span>
* <span data-ttu-id="15b7a-431">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="15b7a-431">section1:subsection:key</span></span>
* <span data-ttu-id="15b7a-432">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="15b7a-432">section2:subsection0:key</span></span>
* <span data-ttu-id="15b7a-433">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="15b7a-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="15b7a-434">JSON 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-434">JSON Configuration Provider</span></span>

<span data-ttu-id="15b7a-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> では、実行時に JSON ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="15b7a-436">JSON ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-437">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="15b7a-438">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-438">Whether the file is optional.</span></span>
* <span data-ttu-id="15b7a-439">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="15b7a-440">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-441"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> で新しい <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を初期化すると、自動的に `AddJsonFile` が 2 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="15b7a-442">このメソッドは、次から構成を読み込むために呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="15b7a-443">*appsettings.json* &ndash; このファイルが最初に読み取られます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="15b7a-444">ファイルの環境バージョンは、*appsettings.json* ファイルによって指定される値をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="15b7a-445">*appsettings.{Environment}.json* &ndash; ファイルの環境バージョンは、[IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) に基づいて読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="15b7a-446">詳細については、「[Web ホスト: ホストを設定する](xref:fundamentals/host/web-host#set-up-a-host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="15b7a-447">`CreateDefaultBuilder` では次のものも読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="15b7a-448">環境変数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-448">Environment variables.</span></span>
* <span data-ttu-id="15b7a-449">[ユーザー シークレット (Secret Manager)](xref:security/app-secrets) (開発環境の場合)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="15b7a-450">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="15b7a-450">Command-line arguments.</span></span>

<span data-ttu-id="15b7a-451">JSON 構成プロバイダーが最初に確立されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="15b7a-452">このため、ユーザー シークレット、環境変数、およびコマンド ライン引数によって、*appsettings* ファイルによって設定された構成がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-453">ホストのビルド時に <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、*appsettings.json* と *appsettings.{Environment}.json* 以外のファイルにアプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="15b7a-454">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-455">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-456"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-457">また、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの `AddJsonFile` 拡張メソッドを直接呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-458">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="15b7a-459">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-460">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-461"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-462"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="15b7a-463">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-464">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-465">**例**</span><span class="sxs-lookup"><span data-stu-id="15b7a-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-466">2.x のサンプル アプリでは、静的な簡易メソッド `CreateDefaultBuilder` を利用してホストをビルドします。これには 2 回の `AddJsonFile` の呼び出しが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="15b7a-467">構成は、*appsettings.json* および *appsettings.{Environment}.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-468">1.x のサンプル アプリでは、`ConfigurationBuilder` の `AddJsonFile` を 2 回呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="15b7a-469">構成は、*appsettings.json* および *appsettings.{Environment}.json* から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="15b7a-470">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-470">Run the sample app.</span></span> <span data-ttu-id="15b7a-471">アプリに対して `http://localhost:5000` でブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="15b7a-472">出力に、環境に応じて、表に示す構成のキーと値のペアが含まれていることを観察します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="15b7a-473">ログの構成キーでは、階層の区切り文字としてコロン (`:`) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="15b7a-474">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-474">Key</span></span>                        | <span data-ttu-id="15b7a-475">開発時の値</span><span class="sxs-lookup"><span data-stu-id="15b7a-475">Development Value</span></span> | <span data-ttu-id="15b7a-476">運用時の値</span><span class="sxs-lookup"><span data-stu-id="15b7a-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="15b7a-477">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="15b7a-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="15b7a-478">情報</span><span class="sxs-lookup"><span data-stu-id="15b7a-478">Information</span></span>       | <span data-ttu-id="15b7a-479">情報</span><span class="sxs-lookup"><span data-stu-id="15b7a-479">Information</span></span>      |
| <span data-ttu-id="15b7a-480">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="15b7a-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="15b7a-481">情報</span><span class="sxs-lookup"><span data-stu-id="15b7a-481">Information</span></span>       | <span data-ttu-id="15b7a-482">情報</span><span class="sxs-lookup"><span data-stu-id="15b7a-482">Information</span></span>      |
| <span data-ttu-id="15b7a-483">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="15b7a-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="15b7a-484">デバッグ</span><span class="sxs-lookup"><span data-stu-id="15b7a-484">Debug</span></span>             | <span data-ttu-id="15b7a-485">Error</span><span class="sxs-lookup"><span data-stu-id="15b7a-485">Error</span></span>            |
| <span data-ttu-id="15b7a-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="15b7a-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="15b7a-487">XML 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-487">XML Configuration Provider</span></span>

<span data-ttu-id="15b7a-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> では、実行時に XML ファイルのキーと値のペアから構成が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="15b7a-489">XML ファイルの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-490">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="15b7a-491">ファイルを省略可能かどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-491">Whether the file is optional.</span></span>
* <span data-ttu-id="15b7a-492">ファイルが変更された場合に構成を再度読み込むかどうか。</span><span class="sxs-lookup"><span data-stu-id="15b7a-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="15b7a-493">ファイルにアクセスするために <xref:Microsoft.Extensions.FileProviders.IFileProvider> が使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="15b7a-494">構成ファイルのルート ノードは、構成のキーと値のペアを作成するときには無視されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="15b7a-495">ファイル内でドキュメント型定義 (DTD) または名前空間を指定しないでください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-496">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="15b7a-497">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-498">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-499"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-500">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="15b7a-501">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-502">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-503"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-504"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="15b7a-505">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-506">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-507">XML 構成ファイルでは、繰り返しのセクション用に個別の要素名を使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="15b7a-508">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="15b7a-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-509">section0:key0</span></span>
* <span data-ttu-id="15b7a-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-510">section0:key1</span></span>
* <span data-ttu-id="15b7a-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-511">section1:key0</span></span>
* <span data-ttu-id="15b7a-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-512">section1:key1</span></span>

<span data-ttu-id="15b7a-513">同じ要素名を使用する要素の繰り返しは、要素を区別するために `name` 属性を使用する場合に機能します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="15b7a-514">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="15b7a-515">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-515">section:section0:key:key0</span></span>
* <span data-ttu-id="15b7a-516">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-516">section:section0:key:key1</span></span>
* <span data-ttu-id="15b7a-517">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-517">section:section1:key:key0</span></span>
* <span data-ttu-id="15b7a-518">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-518">section:section1:key:key1</span></span>

<span data-ttu-id="15b7a-519">値を指定するために属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="15b7a-520">前の構成ファイルでは、`value` を使用して次のキーが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="15b7a-521">key:attribute</span><span class="sxs-lookup"><span data-stu-id="15b7a-521">key:attribute</span></span>
* <span data-ttu-id="15b7a-522">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="15b7a-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="15b7a-523">ファイルごとのキーの構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="15b7a-524"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> では、構成のキーと値のペアとしてディレクトリのファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="15b7a-525">キーはファイル名です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-525">The key is the file name.</span></span> <span data-ttu-id="15b7a-526">値にはファイルのコンテンツが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-526">The value contains the file's contents.</span></span> <span data-ttu-id="15b7a-527">ファイルごとのキーの構成プロバイダーは、Docker ホスティングのシナリオで使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="15b7a-528">ファイルごとのキーの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="15b7a-529">ファイルに対する `directoryPath` は、絶対パスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="15b7a-530">オーバーロードによって次のものを指定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="15b7a-531">ソースを構成する `Action<KeyPerFileConfigurationSource>` デリゲート。</span><span class="sxs-lookup"><span data-stu-id="15b7a-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="15b7a-532">ディレクトリを省略可能かどうか、またディレクトリへのパス。</span><span class="sxs-lookup"><span data-stu-id="15b7a-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="15b7a-533">アンダースコア 2 つ (`__`) は、ファイル名で構成キーの区切り記号として使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="15b7a-534">たとえば、ファイル名 `Logging__LogLevel__System` では、構成キー `Logging:LogLevel:System` が生成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="15b7a-535">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="15b7a-536">基本パスは <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> に設定されています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="15b7a-537">`SetBasePath` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-538"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="15b7a-539">メモリ構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-539">Memory Configuration Provider</span></span>

<span data-ttu-id="15b7a-540"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> では、構成のキーと値のペアとして、メモリ内コレクションが使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="15b7a-541">メモリ内コレクションの構成をアクティブにするには、<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> のインスタンスの <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="15b7a-542">構成プロバイダーは、`IEnumerable<KeyValuePair<String,String>>` を使用して初期化できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15b7a-543">ホストをビルドするときに <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出して、アプリの構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="15b7a-544"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15b7a-545">`CreateDefaultBuilder` を呼び出す場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="15b7a-546"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を直接作成する場合、次の構成で <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-547"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に構成を適用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="15b7a-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="15b7a-548">GetValue</span></span>

<span data-ttu-id="15b7a-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) によって、指定したキーで構成から値が抽出され、それが指定した型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="15b7a-550">オーバーロードを使用すると、キーが見つからない場合に既定値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="15b7a-551">次の例では、キー `NumberKey` を使用して構成から文字列値を抽出し、その値を `int` に型付けして、変数 `intValue` に格納します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="15b7a-552">構成キーで `NumberKey` が見つからない場合、`intValue` は 規定値 `99` を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="15b7a-553">GetSection、GetChildren、Exists</span><span class="sxs-lookup"><span data-stu-id="15b7a-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="15b7a-554">以下の例では、次の JSON ファイルについて考えます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="15b7a-555">4 つのキーが 2 つのセクションに含まれています。そのうちの 1 つには、一組のサブセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="15b7a-556">ファイルが構成に読み取られると、次の一意の階層キーが作成され、構成値が保持されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="15b7a-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-557">section0:key0</span></span>
* <span data-ttu-id="15b7a-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-558">section0:key1</span></span>
* <span data-ttu-id="15b7a-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-559">section1:key0</span></span>
* <span data-ttu-id="15b7a-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-560">section1:key1</span></span>
* <span data-ttu-id="15b7a-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="15b7a-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="15b7a-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="15b7a-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="15b7a-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="15b7a-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="15b7a-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="15b7a-565">GetSection</span></span>

<span data-ttu-id="15b7a-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) では、指定したサブセクションのキーを持つ構成のサブセクションが抽出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="15b7a-567">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-568">`section1` のキーと値のペアのみを含む <xref:Microsoft.Extensions.Configuration.IConfigurationSection> を返すには、`GetSection` を呼び出してセクション名を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="15b7a-569">`configSection` には値はなく、キーとパスのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="15b7a-570">同様に、`section2:subsection0` のキーの値を取得するには、`GetSection` を呼び出してセクションのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="15b7a-571">`GetSection` が `null` を返すことはありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="15b7a-572">一致するセクションが見つからない場合は、空の `IConfigurationSection` が返されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="15b7a-573">`GetSection` で一致するセクションが返されると、<xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> は設定されません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="15b7a-574"><xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> と <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> はセクションが存在する場合に返されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="15b7a-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="15b7a-575">GetChildren</span></span>

<span data-ttu-id="15b7a-576">`section2` での [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) の呼び出しでは、次を含む `IEnumerable<IConfigurationSection>` が取得されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="15b7a-577">既存</span><span class="sxs-lookup"><span data-stu-id="15b7a-577">Exists</span></span>

<span data-ttu-id="15b7a-578">[ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) を使用して、構成セクションが存在するかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="15b7a-579">例のデータの場合、構成データに `section2:subsection2` セクションが存在しないため、`sectionExists` は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="15b7a-580">クラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="15b7a-580">Bind to a class</span></span>

<span data-ttu-id="15b7a-581">"*オプション パターン*" を使用して、関連する設定のグループを表すクラスに構成をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="15b7a-582">詳細については、「<xref:fundamentals/configuration/options>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="15b7a-583">構成値は文字列として返されますが、<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> を呼び出すことで [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクトを構築できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="15b7a-584">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-585">サンプル アプリには `Starship` モデル (*Models/Starship.cs*) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-586">サンプル アプリが JSON 構成プロバイダーを使用して構成を読み込むときに、*starship.json* ファイルの `starship` セクションで構成が作成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="15b7a-587">次の構成のキーと値のペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="15b7a-588">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-588">Key</span></span>                   | <span data-ttu-id="15b7a-589">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="15b7a-590">starship:name</span><span class="sxs-lookup"><span data-stu-id="15b7a-590">starship:name</span></span>         | <span data-ttu-id="15b7a-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="15b7a-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="15b7a-592">starship:registry</span><span class="sxs-lookup"><span data-stu-id="15b7a-592">starship:registry</span></span>     | <span data-ttu-id="15b7a-593">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="15b7a-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="15b7a-594">starship:class</span><span class="sxs-lookup"><span data-stu-id="15b7a-594">starship:class</span></span>        | <span data-ttu-id="15b7a-595">Constitution</span><span class="sxs-lookup"><span data-stu-id="15b7a-595">Constitution</span></span>                                      |
| <span data-ttu-id="15b7a-596">starship:length</span><span class="sxs-lookup"><span data-stu-id="15b7a-596">starship:length</span></span>       | <span data-ttu-id="15b7a-597">304.8</span><span class="sxs-lookup"><span data-stu-id="15b7a-597">304.8</span></span>                                             |
| <span data-ttu-id="15b7a-598">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="15b7a-598">starship:commissioned</span></span> | <span data-ttu-id="15b7a-599">False</span><span class="sxs-lookup"><span data-stu-id="15b7a-599">False</span></span>                                             |
| <span data-ttu-id="15b7a-600">trademark</span><span class="sxs-lookup"><span data-stu-id="15b7a-600">trademark</span></span>             | <span data-ttu-id="15b7a-601">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="15b7a-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="15b7a-602">サンプル アプリは `starship` キーを使用して `GetSection` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="15b7a-603">`starship` のキーと値のペアは分離されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="15b7a-604">`Starship` クラスのインスタンスを渡すサブセクションで、`Bind` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="15b7a-605">インスタンスの値をバインドした後、インスタンスがレンダリング用のプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="15b7a-606">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="15b7a-607">オブジェクト グラフにバインドする</span><span class="sxs-lookup"><span data-stu-id="15b7a-607">Bind to an object graph</span></span>

<span data-ttu-id="15b7a-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> では、POCO オブジェクト グラフ全体をバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="15b7a-609">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15b7a-610">サンプルには、オブジェクト グラフに `Metadata` クラスと `Actors` クラスが含まれる `TvShow` モデルが含まれます (*Models/TvShow.cs*)。</span><span class="sxs-lookup"><span data-stu-id="15b7a-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-611">サンプル アプリには、構成データを含む *tvshow.xml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="15b7a-612">構成は、`Bind` メソッドを使用して、`TvShow` オブジェクト グラフ全体にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="15b7a-613">バインドされたインスタンスは、表示のためにプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-613">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="15b7a-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) によってバインドされ、指定した型が返されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="15b7a-615">`Get<T>` は `Bind` を使用するよりも便利です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="15b7a-616">次のコードは、前の例と共に `Get<T>` を使用する方法を示しています。これにより、バインドされたインスタンスを、表示用に使用するプロパティに直接割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="15b7a-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="15b7a-618">`Get<T>` は、ASP.NET Core 1.1 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="15b7a-619">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="15b7a-620">配列をクラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="15b7a-620">Bind an array to a class</span></span>

<span data-ttu-id="15b7a-621">*サンプル アプリは、このセクションで説明する概念を示しています。*</span><span class="sxs-lookup"><span data-stu-id="15b7a-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="15b7a-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> は、構成キーで配列インデックスを使用して、オブジェクトに対する配列のバインドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="15b7a-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="15b7a-623">数値のキー セグメント (`:0:`、`:1:`、&hellip; `:{n}:`) を公開する配列形式は、すべて POCO クラスの配列にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="15b7a-624">`Bind` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="15b7a-625">バインドは慣例に従って指定されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-625">Binding is provided by convention.</span></span> <span data-ttu-id="15b7a-626">カスタム構成プロバイダーが配列のバインドを実装する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="15b7a-627">**メモリ内配列の処理**</span><span class="sxs-lookup"><span data-stu-id="15b7a-627">**In-memory array processing**</span></span>

<span data-ttu-id="15b7a-628">次の表に示す構成のキーと値について考えます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="15b7a-629">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-629">Key</span></span>             | <span data-ttu-id="15b7a-630">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="15b7a-631">配列:エントリ:0</span><span class="sxs-lookup"><span data-stu-id="15b7a-631">array:entries:0</span></span> | <span data-ttu-id="15b7a-632">value0</span><span class="sxs-lookup"><span data-stu-id="15b7a-632">value0</span></span> |
| <span data-ttu-id="15b7a-633">配列:エントリ:1</span><span class="sxs-lookup"><span data-stu-id="15b7a-633">array:entries:1</span></span> | <span data-ttu-id="15b7a-634">value1</span><span class="sxs-lookup"><span data-stu-id="15b7a-634">value1</span></span> |
| <span data-ttu-id="15b7a-635">配列:エントリ:2</span><span class="sxs-lookup"><span data-stu-id="15b7a-635">array:entries:2</span></span> | <span data-ttu-id="15b7a-636">value2</span><span class="sxs-lookup"><span data-stu-id="15b7a-636">value2</span></span> |
| <span data-ttu-id="15b7a-637">配列:エントリ:4</span><span class="sxs-lookup"><span data-stu-id="15b7a-637">array:entries:4</span></span> | <span data-ttu-id="15b7a-638">value4</span><span class="sxs-lookup"><span data-stu-id="15b7a-638">value4</span></span> |
| <span data-ttu-id="15b7a-639">配列:エントリ:5</span><span class="sxs-lookup"><span data-stu-id="15b7a-639">array:entries:5</span></span> | <span data-ttu-id="15b7a-640">value5</span><span class="sxs-lookup"><span data-stu-id="15b7a-640">value5</span></span> |

<span data-ttu-id="15b7a-641">これらのキーと値は、メモリ構成プロバイダーを使用してサンプル アプリに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="15b7a-642">配列は、インデックス &num;3 の値をスキップします。</span><span class="sxs-lookup"><span data-stu-id="15b7a-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="15b7a-643">構成バインダーは、null 値をバインドしたり、バインドされたオブジェクトに null エントリを作成したりすることはできません。このことは、この配列をオブジェクトにバインドした結果によって明らかになります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="15b7a-644">サンプル アプリでは、バインドされた構成データを保持するために POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-645">構成データはオブジェクトにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="15b7a-646">`GetSection` は [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内の [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="15b7a-647">また、[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 構文を使用して、コードをよりコンパクトにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="15b7a-648">バインドされたオブジェクト (`ArrayExample` のインスタンス) は、構成から配列データを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="15b7a-649">`ArrayExample.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="15b7a-650">`ArrayExample.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="15b7a-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="15b7a-651">0</span><span class="sxs-lookup"><span data-stu-id="15b7a-651">0</span></span>                            | <span data-ttu-id="15b7a-652">value0</span><span class="sxs-lookup"><span data-stu-id="15b7a-652">value0</span></span>                       |
| <span data-ttu-id="15b7a-653">1</span><span class="sxs-lookup"><span data-stu-id="15b7a-653">1</span></span>                            | <span data-ttu-id="15b7a-654">value1</span><span class="sxs-lookup"><span data-stu-id="15b7a-654">value1</span></span>                       |
| <span data-ttu-id="15b7a-655">2</span><span class="sxs-lookup"><span data-stu-id="15b7a-655">2</span></span>                            | <span data-ttu-id="15b7a-656">value2</span><span class="sxs-lookup"><span data-stu-id="15b7a-656">value2</span></span>                       |
| <span data-ttu-id="15b7a-657">3</span><span class="sxs-lookup"><span data-stu-id="15b7a-657">3</span></span>                            | <span data-ttu-id="15b7a-658">value4</span><span class="sxs-lookup"><span data-stu-id="15b7a-658">value4</span></span>                       |
| <span data-ttu-id="15b7a-659">4</span><span class="sxs-lookup"><span data-stu-id="15b7a-659">4</span></span>                            | <span data-ttu-id="15b7a-660">value5</span><span class="sxs-lookup"><span data-stu-id="15b7a-660">value5</span></span>                       |

<span data-ttu-id="15b7a-661">バインドされたオブジェクトのインデックス &num;3 によって、`array:4` 構成キーの構成データと、その値 `value4` が保持されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="15b7a-662">配列を含む構成データがバインドされると、構成キーの配列のインデックスは、オブジェクトを作成するときに構成データを反復処理するためだけに使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="15b7a-663">構成データに null 値を保持することはできません。また、構成キーの配列が 1 つまたは複数のインデックスをスキップしても、バインドされたオブジェクトに null 値のエントリは作成されません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="15b7a-664">インデックス &num;3 の不足している構成項目は、`ArrayExample` インスタンスにバインドする前に、構成で適切なキーと値のペアを生成する構成プロバイダーによって指定できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="15b7a-665">不足しているキーと値のペアを含む JSON 構成プロバイダーがサンプルに含まれる場合、`ArrayExample.Entries` は完全な構成の配列と一致します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="15b7a-666">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15b7a-667"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>の場合:</span><span class="sxs-lookup"><span data-stu-id="15b7a-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15b7a-668">`Startup` コンストラクターの場合:</span><span class="sxs-lookup"><span data-stu-id="15b7a-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="15b7a-669">表に示すキーと値のペアが構成に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="15b7a-670">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-670">Key</span></span>             | <span data-ttu-id="15b7a-671">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="15b7a-672">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="15b7a-672">array:entries:3</span></span> | <span data-ttu-id="15b7a-673">value3</span><span class="sxs-lookup"><span data-stu-id="15b7a-673">value3</span></span> |

<span data-ttu-id="15b7a-674">JSON 構成プロバイダーにインデックス &num;3 のエントリが含まれた後に `ArrayExample` クラスのインスタンスがバインドされる場合、`ArrayExample.Entries` 配列に値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="15b7a-675">`ArrayExample.Entries` インデックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="15b7a-676">`ArrayExample.Entries` 値</span><span class="sxs-lookup"><span data-stu-id="15b7a-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="15b7a-677">0</span><span class="sxs-lookup"><span data-stu-id="15b7a-677">0</span></span>                            | <span data-ttu-id="15b7a-678">value0</span><span class="sxs-lookup"><span data-stu-id="15b7a-678">value0</span></span>                       |
| <span data-ttu-id="15b7a-679">1</span><span class="sxs-lookup"><span data-stu-id="15b7a-679">1</span></span>                            | <span data-ttu-id="15b7a-680">value1</span><span class="sxs-lookup"><span data-stu-id="15b7a-680">value1</span></span>                       |
| <span data-ttu-id="15b7a-681">2</span><span class="sxs-lookup"><span data-stu-id="15b7a-681">2</span></span>                            | <span data-ttu-id="15b7a-682">value2</span><span class="sxs-lookup"><span data-stu-id="15b7a-682">value2</span></span>                       |
| <span data-ttu-id="15b7a-683">3</span><span class="sxs-lookup"><span data-stu-id="15b7a-683">3</span></span>                            | <span data-ttu-id="15b7a-684">value3</span><span class="sxs-lookup"><span data-stu-id="15b7a-684">value3</span></span>                       |
| <span data-ttu-id="15b7a-685">4</span><span class="sxs-lookup"><span data-stu-id="15b7a-685">4</span></span>                            | <span data-ttu-id="15b7a-686">value4</span><span class="sxs-lookup"><span data-stu-id="15b7a-686">value4</span></span>                       |
| <span data-ttu-id="15b7a-687">5</span><span class="sxs-lookup"><span data-stu-id="15b7a-687">5</span></span>                            | <span data-ttu-id="15b7a-688">value5</span><span class="sxs-lookup"><span data-stu-id="15b7a-688">value5</span></span>                       |

<span data-ttu-id="15b7a-689">**JSON 配列の処理**</span><span class="sxs-lookup"><span data-stu-id="15b7a-689">**JSON array processing**</span></span>

<span data-ttu-id="15b7a-690">JSON ファイルに配列が含まれる場合、配列要素の構成キーは、0 から始まるセクションのインデックスで作成されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="15b7a-691">次の構成ファイルにおいて、`subsection` は配列です。</span><span class="sxs-lookup"><span data-stu-id="15b7a-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="15b7a-692">JSON 構成プロバイダーは、次のキーと値のペアに構成データを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="15b7a-693">キー</span><span class="sxs-lookup"><span data-stu-id="15b7a-693">Key</span></span>                     | <span data-ttu-id="15b7a-694">[値]</span><span class="sxs-lookup"><span data-stu-id="15b7a-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="15b7a-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="15b7a-695">json_array:key</span></span>          | <span data-ttu-id="15b7a-696">valueA</span><span class="sxs-lookup"><span data-stu-id="15b7a-696">valueA</span></span> |
| <span data-ttu-id="15b7a-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="15b7a-697">json_array:subsection:0</span></span> | <span data-ttu-id="15b7a-698">valueB</span><span class="sxs-lookup"><span data-stu-id="15b7a-698">valueB</span></span> |
| <span data-ttu-id="15b7a-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="15b7a-699">json_array:subsection:1</span></span> | <span data-ttu-id="15b7a-700">valueC</span><span class="sxs-lookup"><span data-stu-id="15b7a-700">valueC</span></span> |
| <span data-ttu-id="15b7a-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="15b7a-701">json_array:subsection:2</span></span> | <span data-ttu-id="15b7a-702">valueD</span><span class="sxs-lookup"><span data-stu-id="15b7a-702">valueD</span></span> |

<span data-ttu-id="15b7a-703">サンプル アプリでは、構成のキーと値のペアをバインドするために、次の POCO クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-704">バインド後、`JsonArrayExample.Key` は値 `valueA` を保持します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="15b7a-705">サブセクションの値は、POCO 配列のプロパティ `Subsection` に格納されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="15b7a-706">`JsonArrayExample.Subsection` インデックス</span><span class="sxs-lookup"><span data-stu-id="15b7a-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="15b7a-707">`JsonArrayExample.Subsection` 値</span><span class="sxs-lookup"><span data-stu-id="15b7a-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="15b7a-708">0</span><span class="sxs-lookup"><span data-stu-id="15b7a-708">0</span></span>                                   | <span data-ttu-id="15b7a-709">valueB</span><span class="sxs-lookup"><span data-stu-id="15b7a-709">valueB</span></span>                              |
| <span data-ttu-id="15b7a-710">1</span><span class="sxs-lookup"><span data-stu-id="15b7a-710">1</span></span>                                   | <span data-ttu-id="15b7a-711">valueC</span><span class="sxs-lookup"><span data-stu-id="15b7a-711">valueC</span></span>                              |
| <span data-ttu-id="15b7a-712">2</span><span class="sxs-lookup"><span data-stu-id="15b7a-712">2</span></span>                                   | <span data-ttu-id="15b7a-713">valueD</span><span class="sxs-lookup"><span data-stu-id="15b7a-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="15b7a-714">カスタム構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="15b7a-714">Custom configuration provider</span></span>

<span data-ttu-id="15b7a-715">サンプル アプリでは、[Entity Framework (EF)](/ef/core/) を使用してデータベースから構成のキーと値のペアを読み取る、基本的な構成プロバイダーを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="15b7a-716">プロバイダーの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="15b7a-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="15b7a-717">EF のメモリ内データベースは、デモンストレーションのために使用されます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="15b7a-718">接続文字列を必要とするデータベースを使用するには、第 2 の `ConfigurationBuilder` を実装して、別の構成プロバイダーからの接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="15b7a-719">プロバイダーは、起動時に、構成にデータベース テーブルを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="15b7a-720">プロバイダーは、キー単位でデータベースを照会しません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="15b7a-721">変更時に再度読み込む機能は実装されていません。このため、アプリの起動後にデータベースを更新しても、アプリの構成には影響がありません。</span><span class="sxs-lookup"><span data-stu-id="15b7a-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="15b7a-722">データベースに構成値を格納するための `EFConfigurationValue` エンティティを定義します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="15b7a-723">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-724">構成した値を格納し、その値にアクセスするための `EFConfigurationContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="15b7a-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-726"><xref:Microsoft.Extensions.Configuration.IConfigurationSource> を実装するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="15b7a-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-728"><xref:Microsoft.Extensions.Configuration.ConfigurationProvider> から継承して、カスタム構成プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="15b7a-729">データベースが空だった場合、構成プロバイダーはこれを初期化します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="15b7a-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-731">`AddEFConfiguration` 拡張メソッドを使用すると、`ConfigurationBuilder` に構成ソースを追加できます。</span><span class="sxs-lookup"><span data-stu-id="15b7a-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="15b7a-732">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="15b7a-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="15b7a-733">次のコードでは、*Program.cs* でカスタムの `EFConfigurationProvider` を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="15b7a-734">起動中に構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="15b7a-734">Access configuration during startup</span></span>

<span data-ttu-id="15b7a-735">`Startup` コンストラクターに `IConfiguration` を挿入して、`Startup.ConfigureServices` で構成値にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="15b7a-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="15b7a-736">`Startup.Configure` で構成にアクセスするには、メソッドに直接 `IConfiguration` を挿入するか、コンストラクターからのインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="15b7a-737">起動時の簡易メソッドを使用して構成にアクセスする例については、[アプリ起動時の簡易メソッド](xref:fundamentals/startup#convenience-methods)に関連する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="15b7a-738">Razor Pages ページまたは MVC ビューで構成にアクセスする</span><span class="sxs-lookup"><span data-stu-id="15b7a-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="15b7a-739">Razor Pages ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](xref:Microsoft.Extensions.Configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、<xref:Microsoft.Extensions.Configuration.IConfiguration> をページまたはビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="15b7a-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="15b7a-740">Razor ページ: </span><span class="sxs-lookup"><span data-stu-id="15b7a-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="15b7a-741">MVC ビュー: </span><span class="sxs-lookup"><span data-stu-id="15b7a-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="15b7a-742">外部アセンブリから構成を追加する</span><span class="sxs-lookup"><span data-stu-id="15b7a-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="15b7a-743"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="15b7a-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="15b7a-744">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="15b7a-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15b7a-745">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="15b7a-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* [<span data-ttu-id="15b7a-746">Microsoft の構成について詳しく調べる</span><span class="sxs-lookup"><span data-stu-id="15b7a-746">Deep Dive into Microsoft Configuration</span></span>](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
