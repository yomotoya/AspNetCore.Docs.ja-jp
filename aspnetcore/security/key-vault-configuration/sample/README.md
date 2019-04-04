---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012735"
---
# <a name="key-vault-configuration-provider-sample-app"></a><span data-ttu-id="f5f59-101">Key Vault 構成プロバイダーのサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="f5f59-101">Key Vault Configuration Provider Sample App</span></span>

<span data-ttu-id="f5f59-102">このサンプルでは、Azure Key Vault 構成プロバイダーの使用を示します。</span><span class="sxs-lookup"><span data-stu-id="f5f59-102">This sample illustrates the use of the Azure Key Vault Configuration Provider.</span></span>

<span data-ttu-id="f5f59-103">サンプルの実行によって 2 つのモードのいずれかで、`#define`の上部にあるステートメント、 *Program.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f5f59-103">The sample runs in one of two modes determined by the `#define` statement at the top of the *Program.cs* file.</span></span> <span data-ttu-id="f5f59-104">手順については、次を参照してください[サンプル コードでプリプロセッサ ディレクティブ](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):。</span><span class="sxs-lookup"><span data-stu-id="f5f59-104">For instructions, see [Preprocessor directives in sample code](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):</span></span>

* `Certificate` <span data-ttu-id="f5f59-105">&ndash; Azure Key Vault に格納されているアクセス シークレットを Azure Key Vault のクライアント ID と X.509 証明書の使用を示します。</span><span class="sxs-lookup"><span data-stu-id="f5f59-105">&ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="f5f59-106">このバージョンのサンプルは、Azure App Service または ASP.NET Core アプリのサービスを提供できる任意のホストに展開されている任意の場所から実行できます。</span><span class="sxs-lookup"><span data-stu-id="f5f59-106">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* `Managed` <span data-ttu-id="f5f59-107">&ndash; Azure の使用方法を示します[管理対象サービス Id](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)アプリのコードまたは構成で資格情報なしの Azure AD 認証を使用した Azure Key Vault にアプリを認証します。</span><span class="sxs-lookup"><span data-stu-id="f5f59-107">&ndash; Demonstrates how to use Azure's [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials in the app's code or configuration.</span></span> <span data-ttu-id="f5f59-108">Azure AD のクライアント ID とシークレットは、Azure Key Vault に対する認証をアプリの必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f5f59-108">An Azure AD Client ID and Secret aren't required for the app to authenticate with Azure Key Vault.</span></span> <span data-ttu-id="f5f59-109">このサンプルは、管理対象 Id scearnio を探索する Azure App Service にデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5f59-109">This sample must be deployed to Azure App Service to explore the Managed Identity scearnio.</span></span>

<span data-ttu-id="f5f59-110">詳細については、次を参照してください。 [Azure Key Vault 構成プロバイダー](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="f5f59-110">For more information, see [Azure Key Vault Configuration Provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).</span></span>
