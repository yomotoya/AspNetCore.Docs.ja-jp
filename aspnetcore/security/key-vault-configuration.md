---
title: ASP.NET Core での azure Key Vault 構成プロバイダー
author: guardrex
description: Azure Key Vault 構成プロバイダーを使用して、実行時に読み込まれる名前と値のペアを使用してアプリを構成する方法について説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 06445eb2ecec4cf101b23a4bfe131b2c56a18f62
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090307"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="04d8f-103">ASP.NET Core での azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="04d8f-103">Azure Key Vault configuration provider in ASP.NET Core</span></span>

<span data-ttu-id="04d8f-104">作成者 [Luke Latham](https://github.com/guardrex)および [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="04d8f-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="04d8f-105">このドキュメントは、使用する方法を説明します、 [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)構成プロバイダーを Azure Key Vault シークレットからアプリの構成値を読み込めません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="04d8f-106">Azure Key Vault とは、暗号化キーとアプリとサービスで使用されるシークレットを保護するのに役立つクラウド ベース サービスです。</span><span class="sxs-lookup"><span data-stu-id="04d8f-106">Azure Key Vault is a cloud-based service that helps you safeguard cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="04d8f-107">一般的なシナリオは、機密性の高い構成データにアクセスを制御して、FIPS 140-2 の要件を満たすレベル 2 検証済みハードウェア セキュリティ モジュール (HSM) の構成データを格納する場合。</span><span class="sxs-lookup"><span data-stu-id="04d8f-107">Common scenarios include controlling access to sensitive configuration data and meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span> <span data-ttu-id="04d8f-108">この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。</span><span class="sxs-lookup"><span data-stu-id="04d8f-108">This feature is available for apps that target ASP.NET Core 1.1 or higher.</span></span>

<span data-ttu-id="04d8f-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="04d8f-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="04d8f-110">Package</span><span class="sxs-lookup"><span data-stu-id="04d8f-110">Package</span></span>

<span data-ttu-id="04d8f-111">プロバイダーを使用するへの参照を追加、 [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="04d8f-111">To use the provider, add a reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="app-configuration"></a><span data-ttu-id="04d8f-112">アプリの構成</span><span class="sxs-lookup"><span data-stu-id="04d8f-112">App configuration</span></span>

<span data-ttu-id="04d8f-113">使用してプロバイダーを調べることができます、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-113">You can explore the provider with the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples).</span></span> <span data-ttu-id="04d8f-114">Key vault を確立し、資格情報コンテナーにシークレットを作成した後サンプル アプリは安全にシークレットの値をその構成に読み込むし、それらを web ページに表示します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-114">Once you establish a key vault and create secrets in the vault, the sample apps securely load the secret values into their configurations and display them in webpages.</span></span>

<span data-ttu-id="04d8f-115">アプリの構成に、プロバイダーを追加、`AddAzureKeyVault`拡張機能。</span><span class="sxs-lookup"><span data-stu-id="04d8f-115">The provider is added to the app's configuration with the `AddAzureKeyVault` extension.</span></span> <span data-ttu-id="04d8f-116">サンプル アプリで、拡張機能はから読み込まれた 3 つの構成値を使用して、 *appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="04d8f-116">In the sample apps, the extension uses three configuration values loaded from the *appsettings.json* file.</span></span>

| <span data-ttu-id="04d8f-117">アプリ設定</span><span class="sxs-lookup"><span data-stu-id="04d8f-117">App Setting</span></span>    | <span data-ttu-id="04d8f-118">説明</span><span class="sxs-lookup"><span data-stu-id="04d8f-118">Description</span></span>                    | <span data-ttu-id="04d8f-119">例</span><span class="sxs-lookup"><span data-stu-id="04d8f-119">Example</span></span>                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | <span data-ttu-id="04d8f-120">Azure Key Vault 名</span><span class="sxs-lookup"><span data-stu-id="04d8f-120">Azure Key Vault name</span></span>           | <span data-ttu-id="04d8f-121">contosovault</span><span class="sxs-lookup"><span data-stu-id="04d8f-121">contosovault</span></span>                                 |
| `ClientId`     | <span data-ttu-id="04d8f-122">Azure Active Directory アプリ Id</span><span class="sxs-lookup"><span data-stu-id="04d8f-122">Azure Active Directory App Id</span></span>  | <span data-ttu-id="04d8f-123">627e911e-43cc-61d4-992e-12db9c81b413</span><span class="sxs-lookup"><span data-stu-id="04d8f-123">627e911e-43cc-61d4-992e-12db9c81b413</span></span>         |
| `ClientSecret` | <span data-ttu-id="04d8f-124">Azure Active Directory アプリ キー</span><span class="sxs-lookup"><span data-stu-id="04d8f-124">Azure Active Directory App Key</span></span> | <span data-ttu-id="04d8f-125">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span><span class="sxs-lookup"><span data-stu-id="04d8f-125">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span></span> |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a><span data-ttu-id="04d8f-126">Key vault のシークレットを作成し、構成値 (basic サンプル) を読み込む</span><span class="sxs-lookup"><span data-stu-id="04d8f-126">Create key vault secrets and load configuration values (basic-sample)</span></span>

1. <span data-ttu-id="04d8f-127">Key vault を作成しにあるガイダンスに従って、アプリの Azure Active Directory (Azure AD) を設定[Azure Key Vault の概要](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-127">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="04d8f-128">使用して、キー コンテナーにシークレットを追加、[キー コンテナーの AzureRM PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure Portal](https://portal.azure.com/)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-128">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="04d8f-129">シークレットは、いずれかとして作成*手動*または*証明書*シークレット。</span><span class="sxs-lookup"><span data-stu-id="04d8f-129">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="04d8f-130">*証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-130">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="04d8f-131">使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアのシークレットを作成するオプション。</span><span class="sxs-lookup"><span data-stu-id="04d8f-131">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="04d8f-132">単純なシークレットは、名前と値のペアとして作成されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-132">Simple secrets are created as name-value pairs.</span></span> <span data-ttu-id="04d8f-133">Azure Key Vault シークレットの名前は、英数字とダッシュに制限されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-133">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span>
     * <span data-ttu-id="04d8f-134">階層型の値 (構成セクション) を使用して、 `--` (ダッシュを 2 つ)、サンプルでは、区切り記号として。</span><span class="sxs-lookup"><span data-stu-id="04d8f-134">Hierarchical values (configuration sections) use `--` (two dashes) as a separator in the sample.</span></span> <span data-ttu-id="04d8f-135">内のサブキーのセクションを区切るために通常使用される、コロン[ASP.NET Core 構成](xref:fundamentals/configuration/index)、シークレットの名前では許可されません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-135">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in secret names.</span></span> <span data-ttu-id="04d8f-136">そのため、2 つのダッシュが使用され、シークレットは、アプリの構成に読み込まれるときに、コロンのスワップします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-136">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>
     * <span data-ttu-id="04d8f-137">2 つ作成*手動*のシークレットを次の名前と値のペアでします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-137">Create two *Manual* secrets with the following name-value pairs.</span></span> <span data-ttu-id="04d8f-138">最初のシークレットは、単純な名前と値、し、2 つ目のシークレットは、セクションとシークレットの名前のサブキーを使用してシークレット値を作成します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-138">The first secret is a simple name and value, and the second secret creates a secret value with a section and subkey in the secret name:</span></span>
       * <span data-ttu-id="04d8f-139">`SecretName`: `secret_value_1`</span><span class="sxs-lookup"><span data-stu-id="04d8f-139">`SecretName`: `secret_value_1`</span></span>
       * <span data-ttu-id="04d8f-140">`Section--SecretName`: `secret_value_2`</span><span class="sxs-lookup"><span data-stu-id="04d8f-140">`Section--SecretName`: `secret_value_2`</span></span>
   * <span data-ttu-id="04d8f-141">Azure Active Directory でサンプル アプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-141">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="04d8f-142">キー コンテナーにアクセスするアプリを承認します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-142">Authorize the app to access the key vault.</span></span> <span data-ttu-id="04d8f-143">使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認する PowerShell コマンドレットを提供`List`と`Get`でシークレットへのアクセス`-PermissionsToSecrets list,get`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-143">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="04d8f-144">アプリの更新*appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-144">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="04d8f-145">その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`シークレットの名前と同じ名前でします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-145">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the secret name.</span></span>
   * <span data-ttu-id="04d8f-146">非階層型の値: の値は、`SecretName`では、行わ`config["SecretName"]`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-146">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
   * <span data-ttu-id="04d8f-147">階層型の値 (セクション): 使用`:`(コロン) 表記または`GetSection`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="04d8f-147">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="04d8f-148">構成値を取得するのにには、これらの方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-148">Use either of these approaches to obtain the configuration value:</span></span>
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="04d8f-149">アプリを実行すると、web ページには、読み込まれた、シークレットの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-149">When you run the app, a webpage shows the loaded secret values:</span></span>

![Azure Key Vault 構成プロバイダー経由で読み込まれるシークレットの値を表示するブラウザー ウィンドウ](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="04d8f-151">配列をクラスにバインドする</span><span class="sxs-lookup"><span data-stu-id="04d8f-151">Bind an array to a class</span></span>

<span data-ttu-id="04d8f-152">プロバイダーは、配列、POCO 配列にバインドするために構成値を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-152">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="04d8f-153">キーをコロンを含めることができる構成ソースから読み取るときに (`:`) 配列を構成するキーを区別するために、数値キー セグメントの区切り記号が使用される (`:0:`、 `:1:`,…</span><span class="sxs-lookup"><span data-stu-id="04d8f-153">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="04d8f-154">`:{n}:`)</span><span class="sxs-lookup"><span data-stu-id="04d8f-154">`:{n}:`).</span></span> <span data-ttu-id="04d8f-155">詳細については、次を参照してください。[構成: 配列をクラスにバインド](xref:fundamentals/configuration/index#bind-an-array-to-a-class)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-155">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="04d8f-156">Azure Key Vault のキーは、区切り記号としてコロンを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-156">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="04d8f-157">このトピックで説明されているアプローチは、二重ハイフンを使用 (`--`) (セクション) の階層値の区切り記号として。</span><span class="sxs-lookup"><span data-stu-id="04d8f-157">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="04d8f-158">ダッシュと数字キー セグメント倍精度浮動小数点で配列のキーが Azure Key Vault に格納されている (`--0--`、 `--1--`,…</span><span class="sxs-lookup"><span data-stu-id="04d8f-158">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="04d8f-159">`--{n}--`)</span><span class="sxs-lookup"><span data-stu-id="04d8f-159">`--{n}--`).</span></span>

<span data-ttu-id="04d8f-160">次の確認[Serilog](https://serilog.net/)ログ プロバイダーの構成 JSON ファイルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-160">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="04d8f-161">2 つのオブジェクトで定義されているリテラルは、`WriteTo`配列 2 つの Serilog を反映する*シンク*、ログ出力の変換先を記述します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-161">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="04d8f-162">上記の JSON ファイルに示すように構成が二重ダッシュを使用して Azure Key Vault に格納されている (`--`) 表記と数値のセグメント。</span><span class="sxs-lookup"><span data-stu-id="04d8f-162">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="04d8f-163">キー</span><span class="sxs-lookup"><span data-stu-id="04d8f-163">Key</span></span> | <span data-ttu-id="04d8f-164">[値]</span><span class="sxs-lookup"><span data-stu-id="04d8f-164">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a><span data-ttu-id="04d8f-165">プレフィックス付きの key vault のシークレットを作成し、構成値 (キーの名前のプレフィックス-サンプル) を読み込む</span><span class="sxs-lookup"><span data-stu-id="04d8f-165">Create prefixed key vault secrets and load configuration values (key-name-prefix-sample)</span></span>

<span data-ttu-id="04d8f-166">`AddAzureKeyVault` 実装を受け取るオーバー ロードを提供も`IKeyVaultSecretManager`、構成キーに変換されます主要 vault のシークレットを制御できます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-166">`AddAzureKeyVault` also provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="04d8f-167">たとえば、アプリの起動時に指定したプレフィックス値に基づくシークレットの値を読み込むインターフェイスを実装できます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-167">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="04d8f-168">これにより、たとえば、アプリのバージョンに基づくシークレットを読み込めませんできます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-168">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="04d8f-169">同じ key vault にシークレットを複数のアプリを配置する環境のシークレットを配置するか、key vault のシークレットでプレフィックスを使用しないでください (たとえば、*開発*と*運用*シークレット) を同じ資格情報コンテナー。</span><span class="sxs-lookup"><span data-stu-id="04d8f-169">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="04d8f-170">別のアプリと開発/運用環境で最高レベルのセキュリティのアプリの環境を分離する個別の key vault を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-170">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="04d8f-171">Key vault にシークレットを作成する 2 つ目のサンプル アプリを使用して、 `5000-AppSecret` (key vault のシークレット名で許可されていない期間)、アプリのバージョン 5.0.0.0 用アプリのシークレットを表します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-171">Using the second sample app, you create a secret in the key vault for `5000-AppSecret` (periods aren't allowed in key vault secret names) representing an app secret for version 5.0.0.0 of your app.</span></span> <span data-ttu-id="04d8f-172">シークレットを作成する別のバージョン、5.1.0.0、`5100-AppSecret`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-172">For another version, 5.1.0.0, you create a secret for `5100-AppSecret`.</span></span> <span data-ttu-id="04d8f-173">各アプリのバージョンでは、としては、その構成に独自のシークレット値を読み込みます`AppSecret`シークレットが読み込まれるバージョンを削除します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-173">Each app version loads its own secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span> <span data-ttu-id="04d8f-174">サンプルの実装は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-174">The sample's implementation is shown below:</span></span>

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="04d8f-175">`Load`メソッドが付いているバージョンのプレフィックスを検索する資格情報コンテナーのシークレットを反復処理するプロバイダーのアルゴリズムによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-175">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="04d8f-176">バージョンのプレフィックスがで検出されたときに`Load`、アルゴリズムを使用して、`GetKey`シークレット名の構成名を返すメソッド。</span><span class="sxs-lookup"><span data-stu-id="04d8f-176">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="04d8f-177">シークレットの名前からのバージョンのプレフィックスを削除し、アプリの構成に名前/値ペアの読み込みのシークレット名の残りの部分を返します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-177">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="04d8f-178">このアプローチを実装する場合。</span><span class="sxs-lookup"><span data-stu-id="04d8f-178">When you implement this approach:</span></span>

1. <span data-ttu-id="04d8f-179">Key vault のシークレットが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-179">The key vault secrets are loaded.</span></span>
2. <span data-ttu-id="04d8f-180">文字列のシークレットを`5000-AppSecret`が一致します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-180">The string secret for `5000-AppSecret` is matched.</span></span>
3. <span data-ttu-id="04d8f-181">バージョン、 `5000` (dash)、使用が終了、キー名から取り除かれます`AppSecret`シークレットの値を持つアプリの構成を読み込めません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-181">The version, `5000` (with the dash), is stripped off of the key name leaving `AppSecret` to load with the secret value into the app's configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="04d8f-182">独自に提供することも`KeyVaultClient`実装`AddAzureKeyVault`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-182">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="04d8f-183">カスタムのクライアントを指定してクライアントの構成プロバイダーとアプリの他の部分の 1 つのインスタンスを共有することができます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-183">Supplying a custom client allows you to share a single instance of the client between the configuration provider and other parts of your app.</span></span>

1. <span data-ttu-id="04d8f-184">Key vault を作成しにあるガイダンスに従って、アプリの Azure Active Directory (Azure AD) を設定[Azure Key Vault の概要](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-184">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="04d8f-185">使用して、キー コンテナーにシークレットを追加、[キー コンテナーの AzureRM PowerShell モジュール](/powershell/module/azurerm.keyvault)から使用可能な[PowerShell ギャラリー](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure Key Vault REST API](/rest/api/keyvault/)、または、 [Azure Portal](https://portal.azure.com/)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-185">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="04d8f-186">シークレットは、いずれかとして作成*手動*または*証明書*シークレット。</span><span class="sxs-lookup"><span data-stu-id="04d8f-186">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="04d8f-187">*証明書*シークレットはアプリやサービスで使用する証明書が、構成プロバイダーによってはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-187">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="04d8f-188">使用する必要があります、*手動*構成プロバイダーを使用するための名前と値のペアのシークレットを作成するオプション。</span><span class="sxs-lookup"><span data-stu-id="04d8f-188">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="04d8f-189">階層型の値 (構成セクション) を使用して、 `--` (ダッシュを 2 つ) を区切り記号として。</span><span class="sxs-lookup"><span data-stu-id="04d8f-189">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span>
     * <span data-ttu-id="04d8f-190">2 つ作成*手動*のシークレットを次の名前と値のペアで。</span><span class="sxs-lookup"><span data-stu-id="04d8f-190">Create two *Manual* secrets with the following name-value pairs:</span></span>
       * <span data-ttu-id="04d8f-191">`5000-AppSecret`: `5.0.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="04d8f-191">`5000-AppSecret`: `5.0.0.0_secret_value`</span></span>
       * <span data-ttu-id="04d8f-192">`5100-AppSecret`: `5.1.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="04d8f-192">`5100-AppSecret`: `5.1.0.0_secret_value`</span></span>
   * <span data-ttu-id="04d8f-193">Azure Active Directory でサンプル アプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-193">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="04d8f-194">キー コンテナーにアクセスするアプリを承認します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-194">Authorize the app to access the key vault.</span></span> <span data-ttu-id="04d8f-195">使用すると、 `Set-AzureRmKeyVaultAccessPolicy` 、key vault にアクセスするアプリを承認する PowerShell コマンドレットを提供`List`と`Get`でシークレットへのアクセス`-PermissionsToSecrets list,get`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-195">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="04d8f-196">アプリの更新*appsettings.json*の値を持つファイル`Vault`、 `ClientId`、および`ClientSecret`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-196">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="04d8f-197">その構成値を取得するサンプル アプリを実行する`IConfigurationRoot`プレフィックス付きのシークレットの名前と同じ名前でします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-197">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the prefixed secret name.</span></span> <span data-ttu-id="04d8f-198">このサンプルでは、プレフィックスは、アプリのバージョンは、指定した、 `PrefixKeyVaultSecretManager` Azure Key Vault 構成プロバイダーが追加されたとき。</span><span class="sxs-lookup"><span data-stu-id="04d8f-198">In this sample, the prefix is the app's version, which you provided to the `PrefixKeyVaultSecretManager` when you added the Azure Key Vault configuration provider.</span></span> <span data-ttu-id="04d8f-199">値は、`AppSecret`では、行わ`config["AppSecret"]`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-199">The value for `AppSecret` is obtained with `config["AppSecret"]`.</span></span> <span data-ttu-id="04d8f-200">アプリによって生成される web ページには、読み込まれた値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-200">The webpage generated by the app shows the loaded value:</span></span>

   ![アプリのバージョンは 5.0.0.0 場合に、Azure Key Vault 構成プロバイダーを使用してシークレット値を表示するブラウザー ウィンドウが読み込まれます](key-vault-configuration/_static/sample2-1.png)

4. <span data-ttu-id="04d8f-202">プロジェクト ファイルからのアプリのアセンブリのバージョンを変更`5.0.0.0`に`5.1.0.0`し、再度アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-202">Change the version of the app assembly in the project file from `5.0.0.0` to `5.1.0.0` and run the app again.</span></span> <span data-ttu-id="04d8f-203">今回は、返されるシークレットの値は`5.1.0.0_secret_value`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-203">This time, the secret value returned is `5.1.0.0_secret_value`.</span></span> <span data-ttu-id="04d8f-204">アプリによって生成される web ページには、読み込まれた値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-204">The webpage generated by the app shows the loaded value:</span></span>

   ![アプリのバージョンは 5.1.0.0 場合に、Azure Key Vault 構成プロバイダーを使用してシークレット値を表示するブラウザー ウィンドウが読み込まれます](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a><span data-ttu-id="04d8f-206">ClientSecret へのアクセス制御</span><span class="sxs-lookup"><span data-stu-id="04d8f-206">Control access to the ClientSecret</span></span>

<span data-ttu-id="04d8f-207">使用して、 [Secret Manager ツール](xref:security/app-secrets)維持するために、`ClientSecret`プロジェクト ソース ツリーの外部でします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-207">Use the [Secret Manager tool](xref:security/app-secrets) to maintain the `ClientSecret` outside of your project source tree.</span></span> <span data-ttu-id="04d8f-208">シークレット マネージャーを使用したアプリ シークレットを特定のプロジェクトに関連付けるし、複数のプロジェクトで共有できます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-208">With Secret Manager, you associate app secrets with a specific project and share them across multiple projects.</span></span>

<span data-ttu-id="04d8f-209">証明書をサポートする環境での .NET Framework アプリを開発する場合は、X.509 証明書で、Azure Key Vault に認証できます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-209">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="04d8f-210">X.509 証明書の秘密キーは、OS によって管理されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-210">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="04d8f-211">詳細については、次を参照してください。[クライアント シークレットの代わりに証明書による認証](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-211">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="04d8f-212">使用して、`AddAzureKeyVault`を受け入れるオーバー ロードを`X509Certificate2`(`_env`次の例。</span><span class="sxs-lookup"><span data-stu-id="04d8f-212">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a><span data-ttu-id="04d8f-213">シークレットを再読み込み</span><span class="sxs-lookup"><span data-stu-id="04d8f-213">Reload secrets</span></span>

<span data-ttu-id="04d8f-214">シークレットはまでキャッシュ`IConfigurationRoot.Reload()`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="04d8f-214">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="04d8f-215">期限切れ、無効にし、まで、アプリで、key vault にシークレットが更新されたが守られていない`Reload`を実行します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-215">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="04d8f-216">無効になっており、期限切れのシークレット</span><span class="sxs-lookup"><span data-stu-id="04d8f-216">Disabled and expired secrets</span></span>

<span data-ttu-id="04d8f-217">無効になっており、期限切れのシークレットのスロー、`KeyVaultClientException`します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-217">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="04d8f-218">アプリがスローされることを防ぐために、アプリの代わりにまたは、無効/有効期限切れのシークレットを更新します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-218">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="04d8f-219">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="04d8f-219">Troubleshoot</span></span>

<span data-ttu-id="04d8f-220">アプリは、プロバイダーを使用して構成の読み込みに失敗した場合、エラー メッセージが書き込む、 [ASP.NET Core のログ記録インフラストラクチャ](xref:fundamentals/logging/index)します。</span><span class="sxs-lookup"><span data-stu-id="04d8f-220">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="04d8f-221">次の条件は、読み込みを構成できないようにします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-221">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="04d8f-222">アプリは、Azure Active Directory で正しく構成されていません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-222">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="04d8f-223">Key vault は、Azure Key Vault に存在しません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-223">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="04d8f-224">アプリは、キー コンテナーにアクセスする権限はありません。</span><span class="sxs-lookup"><span data-stu-id="04d8f-224">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="04d8f-225">アクセス ポリシーが含まれていない`Get`と`List`アクセス許可。</span><span class="sxs-lookup"><span data-stu-id="04d8f-225">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="04d8f-226">Key vault で構成データ (名前と値のペア) が正しくという名前のないがない、有効期限が切れたか、無効にします。</span><span class="sxs-lookup"><span data-stu-id="04d8f-226">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="04d8f-227">アプリが正しくない key vault の名前 (`Vault`)、Azure AD アプリ Id (`ClientId`)、または Azure AD のキー (`ClientSecret`)。</span><span class="sxs-lookup"><span data-stu-id="04d8f-227">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="04d8f-228">Azure AD のキー (`ClientSecret`) 期限が切れています。</span><span class="sxs-lookup"><span data-stu-id="04d8f-228">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="04d8f-229">構成キー (名) は、ロードしようとしている値用のアプリで正しくないです。</span><span class="sxs-lookup"><span data-stu-id="04d8f-229">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04d8f-230">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="04d8f-230">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="04d8f-231">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="04d8f-231">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="04d8f-232">Microsoft Azure: Key Vault のドキュメント</span><span class="sxs-lookup"><span data-stu-id="04d8f-232">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="04d8f-233">Azure Key Vault のキーを生成し、HSM で保護された転送する方法</span><span class="sxs-lookup"><span data-stu-id="04d8f-233">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="04d8f-234">KeyVaultClient クラス</span><span class="sxs-lookup"><span data-stu-id="04d8f-234">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
