---
title: "キーの保存時の暗号化"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護キーの暗号化の実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 0a62a1a10e578e59e1d80579d80779d4dcf1658a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="38c83-103">キーの保存時の暗号化</span><span class="sxs-lookup"><span data-stu-id="38c83-103">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="38c83-104">既定では、データ保護システム[ヒューリスティックを使用して](xref:security/data-protection/configuration/default-settings)キー マテリアルを暗号化する方法を決定する残りの部分で暗号化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38c83-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="38c83-105">開発者は、ヒューリスティックをオーバーライドし、残りの部分でのキーの暗号化方法を手動で指定できます。</span><span class="sxs-lookup"><span data-stu-id="38c83-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="38c83-106">Rest メカニズムで明示的なキーの暗号化を指定する場合、データ保護システムは、ヒューリスティックが提供される既定のキー記憶域メカニズムを登録解除します。</span><span class="sxs-lookup"><span data-stu-id="38c83-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="38c83-107">行う必要があります[キー記憶域の明示的なメカニズムが指定](key-storage-providers.md#data-protection-implementation-key-storage-providers)、それ以外の場合、データ保護システムは起動しません。</span><span class="sxs-lookup"><span data-stu-id="38c83-107">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="38c83-108">データ保護システムは、次の 3 つのボックスでキーの暗号化メカニズムに付属します。</span><span class="sxs-lookup"><span data-stu-id="38c83-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="38c83-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="38c83-109">Windows DPAPI</span></span>

<span data-ttu-id="38c83-110">*このメカニズムは、Windows でのみ使用可能です。*</span><span class="sxs-lookup"><span data-stu-id="38c83-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="38c83-111">キー マテリアルを使用して暗号化されます Windows DPAPI を使用すると[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)ストレージに永続化される前にします。</span><span class="sxs-lookup"><span data-stu-id="38c83-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="38c83-112">DPAPI は、現在のコンピューターの外部で読み込みは行われませんデータの適切な暗号化メカニズム (ただしこれは Active Directory までこれらのキーをバックアップすることです。 を参照してください[DPAPI および移動ユーザー プロファイル](https://support.microsoft.com/kb/309408/#6))。</span><span class="sxs-lookup"><span data-stu-id="38c83-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="38c83-113">たとえば DPAPI キーの暗号化を構成します。</span><span class="sxs-lookup"><span data-stu-id="38c83-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="38c83-114">場合`ProtectKeysWithDpapi`は、現在の Windows ユーザー アカウントは、永続化されたキー マテリアルを解読できますのみパラメーターなしで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="38c83-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="38c83-115">ように (だけでなく、現在のユーザー アカウント) のコンピューター上の任意のユーザー アカウントが、キー マテリアルを解読できないする必要があるを指定することができます必要に応じて、次の例です。</span><span class="sxs-lookup"><span data-stu-id="38c83-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="38c83-116">X.509 証明書</span><span class="sxs-lookup"><span data-stu-id="38c83-116">X.509 certificate</span></span>

<span data-ttu-id="38c83-117">*このメカニズムでは利用できません`.NET Core 1.0`または`1.1`です。*</span><span class="sxs-lookup"><span data-stu-id="38c83-117">*This mechanism isn't available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="38c83-118">アプリケーションを複数のコンピューターに分散された場合、は、残りの部分でのキーの暗号化にこの証明書を使用するアプリケーションを構成して、コンピューター間で共有の X.509 証明書を配布する便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="38c83-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="38c83-119">例については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="38c83-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="38c83-120">.NET Framework の制限により CAPI 秘密キーを持つ証明書のみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="38c83-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="38c83-121">参照してください[証明書ベースの暗号化に Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下これらの制限に対処する方法についてです。</span><span class="sxs-lookup"><span data-stu-id="38c83-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="38c83-122">Windows DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="38c83-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="38c83-123">*このメカニズムは Windows 8 でのみ利用可能/Windows Server 2012 以降。*</span><span class="sxs-lookup"><span data-stu-id="38c83-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="38c83-124">Windows 8 以降では、オペレーティング システムは、DPAPI NG (CNG DPAPI とも呼ばれます) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="38c83-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="38c83-125">Microsoft は、その使用方法のシナリオを次のようにレイアウトします。</span><span class="sxs-lookup"><span data-stu-id="38c83-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="38c83-126">クラウド コンピューティング、ただし、多くの場合、必要がありますコンテンツでは暗号化された 1 つのコンピューターが別の復号化されます。</span><span class="sxs-lookup"><span data-stu-id="38c83-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="38c83-127">Microsoft はこのため、Windows 8 以降、クラウドのシナリオを使用するように、比較的簡単な API を使用する概念を拡張します。</span><span class="sxs-lookup"><span data-stu-id="38c83-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="38c83-128">DPAPI NG と呼ばれるこの新しい API では、一連の適切な認証および承認後に別のコンピューターに保護解除に使用できるプリンシパルを保護することによって、機密データ (キー、パスワード、キー マテリアル) とメッセージを安全に共有することができます。</span><span class="sxs-lookup"><span data-stu-id="38c83-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="38c83-129">[CNG DPAPI について](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="38c83-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="38c83-130">プリンシパルは、保護記述子ルールとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="38c83-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="38c83-131">検討してください、次の例では、する暗号化キー マテリアルのみ、ドメインに参加しているユーザー指定の SID を持つには、キー マテリアルが復号化できるようにします。</span><span class="sxs-lookup"><span data-stu-id="38c83-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="38c83-132">パラメーターなしのオーバー ロードもあります`ProtectKeysWithDpapiNG`です。</span><span class="sxs-lookup"><span data-stu-id="38c83-132">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="38c83-133">これは、ルールを指定するための便利なメソッド"SID とマイニング ="で、私は現在の Windows ユーザー アカウントの SID。</span><span class="sxs-lookup"><span data-stu-id="38c83-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="38c83-134">このシナリオでは、AD ドメイン コント ローラーは DPAPI NG 操作で使用される暗号化キーの配布を担当します。</span><span class="sxs-lookup"><span data-stu-id="38c83-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="38c83-135">ターゲット ユーザーは、(その id では、プロセスは実行されている) を指定した任意のドメインに参加しているコンピューターから暗号化されたペイロードを解読することはできます。</span><span class="sxs-lookup"><span data-stu-id="38c83-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="38c83-136">証明書ベースの暗号化に Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="38c83-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="38c83-137">Windows 8.1 で実行している場合/Windows Server 2012 R2 以降を使用することもできます Windows DPAPI NG 証明書ベースの暗号化を実行するでアプリケーションが実行されている場合でも[.NET Core](https://www.microsoft.com/net/core)です。</span><span class="sxs-lookup"><span data-stu-id="38c83-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="38c83-138">これを利用するルール記述子文字列を使用"証明書 HashId:thumbprint を ="で、拇印は、16 進でエンコードされた SHA1 証明書の拇印を使用します。</span><span class="sxs-lookup"><span data-stu-id="38c83-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="38c83-139">例については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="38c83-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="38c83-140">このリポジトリに設定されているすべてのアプリケーションは、Windows 8.1 で実行されている必要があります/Windows Server 2012 R2 以降をこのキーを解読します。</span><span class="sxs-lookup"><span data-stu-id="38c83-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="38c83-141">カスタムのキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="38c83-141">Custom key encryption</span></span>

<span data-ttu-id="38c83-142">カスタムを提供することによって、開発者が独自のキーの暗号化メカニズムを指定するインボックス メカニズムが適切でない場合`IXmlEncryptor`です。</span><span class="sxs-lookup"><span data-stu-id="38c83-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
