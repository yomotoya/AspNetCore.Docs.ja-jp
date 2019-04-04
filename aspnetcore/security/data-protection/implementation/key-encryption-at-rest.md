---
title: ASP.NET Core での保存時のキーの暗号化
author: rick-anderson
description: ASP.NET Core データ保護キーの保存時の暗号化の実装の詳細について説明します。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219291"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="5032d-103">ASP.NET Core での保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="5032d-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="5032d-104">データ保護システム[検出メカニズムを使用して、既定で](xref:security/data-protection/configuration/default-settings)キーを暗号化する方法を決定する残りの部分で暗号化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5032d-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="5032d-105">開発者は、検出メカニズムをオーバーライドし、保存時のキーの暗号化方法を手動で指定できます。</span><span class="sxs-lookup"><span data-stu-id="5032d-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="5032d-106">明示的な指定した場合[キーの永続化される場所](xref:security/data-protection/implementation/key-storage-providers)、データ保護システム登録メカニズムの残りの部分で既定のキーの暗号化を解除します。</span><span class="sxs-lookup"><span data-stu-id="5032d-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="5032d-107">その結果、キーは、残りの部分では暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="5032d-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="5032d-108">お勧めする[明示的なキーの暗号化メカニズムを指定](xref:security/data-protection/implementation/key-encryption-at-rest)運用環境のデプロイ。</span><span class="sxs-lookup"><span data-stu-id="5032d-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="5032d-109">保存時の暗号化メカニズムのオプションは、このトピックで説明します。</span><span class="sxs-lookup"><span data-stu-id="5032d-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="5032d-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5032d-110">Azure Key Vault</span></span>

<span data-ttu-id="5032d-111">キーを保管する[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)、構成を使用してシステム[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="5032d-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="5032d-112">詳細については、[ASP.NET Core データ保護の構成: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5032d-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="5032d-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="5032d-113">Windows DPAPI</span></span>

<span data-ttu-id="5032d-114">**Windows の展開にのみ適用されます。**</span><span class="sxs-lookup"><span data-stu-id="5032d-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="5032d-115">キー マテリアルがで暗号化された Windows DPAPI を使用すると、 [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata)ストレージに永続化される前にします。</span><span class="sxs-lookup"><span data-stu-id="5032d-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="5032d-116">DPAPI は、現在のコンピューターの外部ではない読み取り専用データの適切な暗号化メカニズム (が Active Directory までこれらのキーをバックアップすることは、参照してください[DPAPI および移動ユーザー プロファイル](https://support.microsoft.com/kb/309408/#6))。</span><span class="sxs-lookup"><span data-stu-id="5032d-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="5032d-117">DPAPI の保存時のキーの暗号化を構成するには、いずれかを呼び出して、 [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="5032d-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="5032d-118">場合`ProtectKeysWithDpapi`は、現在の Windows ユーザー アカウントは、永続化されたキー リングを解読できますのみパラメーターなしで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5032d-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="5032d-119">必要に応じて、(現在のユーザー アカウントだけでなく) コンピューター上の任意のユーザー アカウントが、キー リングを解読できることを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="5032d-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="5032d-120">X.509 証明書</span><span class="sxs-lookup"><span data-stu-id="5032d-120">X.509 certificate</span></span>

<span data-ttu-id="5032d-121">アプリは複数のマシンに分散され場合、マシン間で共有の X.509 証明書を配布して、保存時のキーの暗号化に証明書を使用するホスト型アプリを構成する便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="5032d-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="5032d-122">.NET Framework の制限により、CAPI 秘密キーを含む証明書のみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5032d-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="5032d-123">これらの制限に対処する方法については、次の内容を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5032d-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="5032d-124">Windows の DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="5032d-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="5032d-125">**このメカニズムは、Windows 8/Windows Server 2012 以降でのみ使用できます。**</span><span class="sxs-lookup"><span data-stu-id="5032d-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="5032d-126">Windows 8 以降、Windows OS には、DPAPI NG (CNG DPAPI とも呼ばれます) がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5032d-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="5032d-127">詳細については、[CNG DPAPI について](/windows/desktop/SecCNG/cng-dpapi)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5032d-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="5032d-128">プリンシパルは、保護の記述子ルールとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="5032d-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="5032d-129">呼び出す次の例では[ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)、のみ、指定された SID を持つドメインに参加しているユーザーには、キー リングが復号化できます。</span><span class="sxs-lookup"><span data-stu-id="5032d-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="5032d-130">パラメーターなしのオーバー ロードも`ProtectKeysWithDpapiNG`します。</span><span class="sxs-lookup"><span data-stu-id="5032d-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="5032d-131">この便利なメソッドを使用して、ルールを指定する"SID = {CURRENT_ACCOUNT_SID}"ここで、 *CURRENT_ACCOUNT_SID*は現在の Windows ユーザー アカウントの SID。</span><span class="sxs-lookup"><span data-stu-id="5032d-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="5032d-132">このシナリオでは、AD ドメイン コント ローラーは、DPAPI NG 操作によって使用される暗号化キーを配布する責任を負います。</span><span class="sxs-lookup"><span data-stu-id="5032d-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="5032d-133">ターゲット ユーザーは、(プロセスは、その id の下で実行している) を指定した任意のドメインに参加しているコンピューターから暗号化されたペイロードを解読できます。</span><span class="sxs-lookup"><span data-stu-id="5032d-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="5032d-134">証明書ベースの暗号化では、Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="5032d-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="5032d-135">Windows 8.1/Windows Server 2012 R2 で、アプリが実行されている場合は後で、NG-Windows DPAPI を使用して証明書ベースの暗号化を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="5032d-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="5032d-136">ルールの記述子の文字列を使用して"証明書 HashId:THUMBPRINT ="ここで、*拇印*証明書の 16 進エンコードされた SHA1 拇印します。</span><span class="sxs-lookup"><span data-stu-id="5032d-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="5032d-137">Windows 8.1/windows Server 2012 R2、または、キーを解読するには、後で、このリポジトリで参照されているすべてのアプリを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5032d-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="5032d-138">カスタム キーの暗号化</span><span class="sxs-lookup"><span data-stu-id="5032d-138">Custom key encryption</span></span>

<span data-ttu-id="5032d-139">組み込みのメカニズムがない適切な場合、開発者がカスタムを提供することで独自のキーの暗号化メカニズムを指定できます[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)します。</span><span class="sxs-lookup"><span data-stu-id="5032d-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
