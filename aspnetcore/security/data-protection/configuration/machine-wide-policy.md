---
title: "マシン全体のポリシー"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="8f408-103">マシン全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="8f408-103">Machine Wide Policy</span></span>

<a name="data-protection-configuration-machinewidepolicy"></a>

<span data-ttu-id="8f408-104">Windows で実行されるときに、データ保護システムにデータの保護を使用するすべてのアプリケーションの既定のコンピューター全体のポリシーを設定するためのサポートが制限されています。</span><span class="sxs-lookup"><span data-stu-id="8f408-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="8f408-105">一般的な考え方としては、管理者が、コンピューター上のすべてのアプリケーションを手動で更新する必要はありません (アルゴリズムまたはキーの使用有効期間) などの既定の設定を変更しようとする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8f408-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="8f408-106">システム管理者は、既定のポリシーを設定できますが、それを適用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8f408-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="8f408-107">アプリケーション開発者は、独自の選択のいずれかの任意の値を常にオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="8f408-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="8f408-108">既定のポリシーは、開発者が指定されていない場合、明示的な値をいくつか特定の設定をアプリケーションにのみ影響します。</span><span class="sxs-lookup"><span data-stu-id="8f408-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="8f408-109">既定のポリシーの設定</span><span class="sxs-lookup"><span data-stu-id="8f408-109">Setting default policy</span></span>

<span data-ttu-id="8f408-110">既定のポリシーを設定するには、管理者は、次のキーの下のシステム レジストリで既知の値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="8f408-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="8f408-111">レジストリ キー:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="8f408-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="8f408-112">64 ビット オペレーティング システムで 32 ビット アプリケーションの動作に影響する場合、忘れずにも構成上のキーの Wow6432Node に相当します。</span><span class="sxs-lookup"><span data-stu-id="8f408-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="8f408-113">サポートされる値は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8f408-113">The supported values are:</span></span>

* <span data-ttu-id="8f408-114">EncryptionType [文字列] には、アルゴリズムは、データ保護に使用する必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="8f408-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="8f408-115">この値は"CNG CBC"、"CNG-GCM"または"Managed"にする必要があります、さらに詳しく記載されて[下](#data-protection-encryption-types)です。</span><span class="sxs-lookup"><span data-stu-id="8f408-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="8f408-116">DefaultKeyLifetime [DWORD] には、新しく生成されたキーの有効期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f408-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="8f408-117">この値は日数で指定され、≥ 7 をする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f408-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="8f408-118">KeyEscrowSinks [文字列] には、キー エスクローを使用する型を指定します。</span><span class="sxs-lookup"><span data-stu-id="8f408-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="8f408-119">この値は、IKeyEscrowSink を実装する型のアセンブリ修飾名を一覧内の各要素がここでは、キー エスクロー シンクのセミコロンで区切られたリストです。</span><span class="sxs-lookup"><span data-stu-id="8f408-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a><span data-ttu-id="8f408-120">暗号化の種類</span><span class="sxs-lookup"><span data-stu-id="8f408-120">Encryption types</span></span>

<span data-ttu-id="8f408-121">Windows CNG によって提供されるサービスとの信頼性の機密性、および HMAC CBC モード対称ブロック暗号を使用するように、システム構成は EncryptionType が"CNG CBC"の場合は、(を参照してください[のカスタムのWindowsCNGアルゴリズムを指定する](overview.md#data-protection-changing-algorithms-cng)詳細)。</span><span class="sxs-lookup"><span data-stu-id="8f408-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="8f408-122">次の追加の値がサポートされているそれぞれに対応して CngCbcAuthenticatedEncryptionSettings 型のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="8f408-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="8f408-123">[String] - EncryptionAlgorithm CNG で認識される対称ブロック暗号アルゴリズムの名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="8f408-124">このアルゴリズムは、CBC モードで表示します。</span><span class="sxs-lookup"><span data-stu-id="8f408-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="8f408-125">[String] - EncryptionAlgorithmProvider アルゴリズム EncryptionAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="8f408-126">[DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称ブロック暗号アルゴリズムを派生させるキー。</span><span class="sxs-lookup"><span data-stu-id="8f408-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="8f408-127">[String] - HashAlgorithm CNG で認識されるハッシュ アルゴリズムの名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="8f408-128">このアルゴリズムは、HMAC モードで表示します。</span><span class="sxs-lookup"><span data-stu-id="8f408-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="8f408-129">[String] - HashAlgorithmProvider アルゴリズムの HashAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="8f408-130">Windows CNG によって提供されるサービスでの機密性、および信頼性 Galois/カウンター モード対称ブロック暗号を使用するように、システム構成は EncryptionType が"CNG GCM"の場合は、(を参照してください[のカスタムのWindowsCNGアルゴリズムを指定する](overview.md#data-protection-changing-algorithms-cng)詳細)。</span><span class="sxs-lookup"><span data-stu-id="8f408-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="8f408-131">次の追加の値がサポートされているそれぞれに対応して CngGcmAuthenticatedEncryptionSettings 型のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="8f408-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="8f408-132">[String] - EncryptionAlgorithm CNG で認識される対称ブロック暗号アルゴリズムの名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="8f408-133">このアルゴリズムが Galois/カウンター モードで開きます。</span><span class="sxs-lookup"><span data-stu-id="8f408-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="8f408-134">[String] - EncryptionAlgorithmProvider アルゴリズム EncryptionAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。</span><span class="sxs-lookup"><span data-stu-id="8f408-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="8f408-135">[DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称ブロック暗号アルゴリズムを派生させるキー。</span><span class="sxs-lookup"><span data-stu-id="8f408-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="8f408-136">EncryptionType が"Managed"場合、システムは、信頼性の機密性、および KeyedHashAlgorithm のマネージ対称アルゴリズムを使用する構成は (を参照してください[を指定するカスタム マネージ アルゴリズム](overview.md#data-protection-changing-algorithms-custom-managed)詳細)。</span><span class="sxs-lookup"><span data-stu-id="8f408-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="8f408-137">次の追加の値がサポートされているそれぞれに対応して ManagedAuthenticatedEncryptionSettings 型のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="8f408-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="8f408-138">[String] - EncryptionAlgorithmType 対称アルゴリズムを実装する型のアセンブリ修飾名。</span><span class="sxs-lookup"><span data-stu-id="8f408-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="8f408-139">[DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称暗号化アルゴリズムを派生させるキー。</span><span class="sxs-lookup"><span data-stu-id="8f408-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="8f408-140">[String] - ValidationAlgorithmType KeyedHashAlgorithm を実装する型のアセンブリ修飾名。</span><span class="sxs-lookup"><span data-stu-id="8f408-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="8f408-141">EncryptionType がその他の値 (null 以外/空) の場合、データ保護システム起動時に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8f408-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="8f408-142">型名 (EncryptionAlgorithmType、ValidationAlgorithmType、KeyEscrowSinks) は、既定のポリシー設定を構成するときに、型が、アプリケーションで使用できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8f408-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="8f408-143">実際には、これは、アプリケーションのデスクトップ CLR で実行されている場合、アセンブリをこれらの型を含む必要がある GACed ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="8f408-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="8f408-144">実行されている ASP.NET Core アプリケーション[.NET Core](https://www.microsoft.com/net/core)、これらの型を含むパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f408-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
