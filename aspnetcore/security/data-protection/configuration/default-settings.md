---
title: "キー管理と有効期間"
author: rick-anderson
description: "キー管理と有効期間について説明します。"
keywords: "ASP.NET Core キー管理を DPAPI データ保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: c361af7d336fc0f7651e5d2f28d71515e2949c65
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="key-management-and-lifetime"></a><span data-ttu-id="dfa81-104">キー管理と有効期間</span><span class="sxs-lookup"><span data-stu-id="dfa81-104">Key management and lifetime</span></span>

<a name=data-protection-default-settings></a>

## <a name="key-management"></a><span data-ttu-id="dfa81-105">キー管理</span><span class="sxs-lookup"><span data-stu-id="dfa81-105">Key Management</span></span>

<span data-ttu-id="dfa81-106">システムしようと、運用環境を検出し、適切な 0 の動作の既定の設定を提供します。</span><span class="sxs-lookup"><span data-stu-id="dfa81-106">The system tries to detect its operational environment and provide good zero-configuration behavioral defaults.</span></span> <span data-ttu-id="dfa81-107">使用されるヒューリスティックのとおりです。</span><span class="sxs-lookup"><span data-stu-id="dfa81-107">The heuristic used is as follows.</span></span>

1. <span data-ttu-id="dfa81-108">場合は、システムは、Azure Web サイトでホストされている、キーが"%home%\asp.net\dataprotection-keys"フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-108">If the system is being hosted in Azure Web Sites, keys are persisted to the "%HOME%\ASP.NET\DataProtection-Keys" folder.</span></span> <span data-ttu-id="dfa81-109">このフォルダーはネットワーク ストレージによってバックアップされは、アプリケーションをホストしているすべてのマシン間で同期します。</span><span class="sxs-lookup"><span data-stu-id="dfa81-109">This folder is backed by network storage and is synchronized across all machines hosting the application.</span></span> <span data-ttu-id="dfa81-110">残りの部分では、キーは保護されません。</span><span class="sxs-lookup"><span data-stu-id="dfa81-110">Keys are not protected at rest.</span></span> <span data-ttu-id="dfa81-111">このフォルダーは、単一のデプロイ スロット内のアプリケーションのすべてのインスタンスにキー リングを提供します。</span><span class="sxs-lookup"><span data-stu-id="dfa81-111">This folder supplies the key ring to all instances of an application in a single deployment slot.</span></span> <span data-ttu-id="dfa81-112">ステージングと運用環境などの別のデプロイ スロットは、キーのリングを共有していません。</span><span class="sxs-lookup"><span data-stu-id="dfa81-112">Separate deployment slots, such as Staging and Production, will not share a key ring.</span></span> <span data-ttu-id="dfa81-113">たとえば実稼働環境にステージングのスワップまたは A を使用して、デプロイ スロット間で交換する場合に/テスト B、データ保護を使用して任意のシステムは、前のスロット内のキーのリングを使用して格納されたデータの暗号化を解除することはできません。</span><span class="sxs-lookup"><span data-stu-id="dfa81-113">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any system using data protection will not be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="dfa81-114">これは、そのクッキーを保護するデータの保護を使用するように、標準の ASP.NET cookie ミドルウェアを使用する ASP.NET アプリケーションからログアウトするユーザーにつながります。</span><span class="sxs-lookup"><span data-stu-id="dfa81-114">This will lead to users being logged out of an ASP.NET application that uses the standard ASP.NET cookie middleware, as it uses data protection to protect its cookies.</span></span> <span data-ttu-id="dfa81-115">スロットに依存しないキーリングを希望する場合は、Azure Blob ストレージ、Azure Key Vault SQL ストアなど、外部キー リング プロバイダーを使用または Redis キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="dfa81-115">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

2. <span data-ttu-id="dfa81-116">ユーザー プロファイルを使用できる場合は、キーが"%localappdata%\asp.net\dataprotection-keys"フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-116">If the user profile is available, keys are persisted to the "%LOCALAPPDATA%\ASP.NET\DataProtection-Keys" folder.</span></span> <span data-ttu-id="dfa81-117">さらに、オペレーティング システムが Windows の場合は、DPAPI を使用して残りの部分で、暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-117">Additionally, if the operating system is Windows, they'll be encrypted at rest using DPAPI.</span></span>

3. <span data-ttu-id="dfa81-118">アプリケーションが IIS でホストされている場合、キーは、ワーカー プロセス アカウントにのみ ACLed である特殊なレジストリ キー HKLM レジストリに保存されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-118">If the application is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that is ACLed only to the worker process account.</span></span> <span data-ttu-id="dfa81-119">キーは DPAPI を使用して保存時に暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-119">Keys are encrypted at rest using DPAPI.</span></span>

4. <span data-ttu-id="dfa81-120">いずれの条件に一致する場合、現在のプロセスの外部キーは保存されません。</span><span class="sxs-lookup"><span data-stu-id="dfa81-120">If none of these conditions matches, keys are not persisted outside of the current process.</span></span> <span data-ttu-id="dfa81-121">プロセスがシャット ダウン、生成されたすべてのキーが失われます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-121">When the process shuts down, all generated keys will be lost.</span></span>

<span data-ttu-id="dfa81-122">開発者は常にフル コントロールであり、キーの格納場所と方法をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-122">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="dfa81-123">上記の最初の 3 つのオプションには、方法と似ていますほとんどのアプリケーションの適切な既定値が必要があります、ASP.NET<machineKey>過去で自動生成ルーチンが機能していた。</span><span class="sxs-lookup"><span data-stu-id="dfa81-123">The first three options above should good defaults for most applications similar to how the ASP.NET <machineKey> auto-generation routines worked in the past.</span></span> <span data-ttu-id="dfa81-124">最終的にフォール フェールバック オプションは、唯一のシナリオを指定する開発者を本当に必要とする[構成](overview.md)キーの永続化したいが、このフォールバックはまれな状況でのみ発生する場合は、先行します。</span><span class="sxs-lookup"><span data-stu-id="dfa81-124">The final, fall back option is the only scenario that truly requires the developer to specify [configuration](overview.md) upfront if they want key persistence, but this fall-back would only occur in rare situations.</span></span>

>[!WARNING]
> <span data-ttu-id="dfa81-125">開発者は、このヒューリスティックをオーバーライドし、特定のキー リポジトリにデータ保護システムを指す、残りの部分でのキーの自動暗号化は無効になります。</span><span class="sxs-lookup"><span data-stu-id="dfa81-125">If the developer overrides this heuristic and points the data protection system at a specific key repository, automatic encryption of keys at rest will be disabled.</span></span> <span data-ttu-id="dfa81-126">残りの部分では、保護を使用して再度有効になる可能性が[構成](overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="dfa81-126">At rest protection can be re-enabled via [configuration](overview.md).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="dfa81-127">キーの有効期間</span><span class="sxs-lookup"><span data-stu-id="dfa81-127">Key Lifetime</span></span>

<span data-ttu-id="dfa81-128">既定でキーには、90 日間有効期間があります。</span><span class="sxs-lookup"><span data-stu-id="dfa81-128">Keys by default have a 90-day lifetime.</span></span> <span data-ttu-id="dfa81-129">キーが期限切れになったときに、システムは自動的に新しいキーを生成し、アクティブ キーとして新しいキーを設定します。</span><span class="sxs-lookup"><span data-stu-id="dfa81-129">When a key expires, the system will automatically generate a new key and set the new key as the active key.</span></span> <span data-ttu-id="dfa81-130">提供終了になったキーがシステムに残っている限り、それらに保護されている任意のデータを復号化することができます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-130">As long as retired keys remain on the system you will still be able to decrypt any data protected with them.</span></span> <span data-ttu-id="dfa81-131">参照してください[キー管理](../implementation/key-management.md#data-protection-implementation-key-management-expiration)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="dfa81-131">See [key management](../implementation/key-management.md#data-protection-implementation-key-management-expiration) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="dfa81-132">既定のアルゴリズム</span><span class="sxs-lookup"><span data-stu-id="dfa81-132">Default Algorithms</span></span>

<span data-ttu-id="dfa81-133">使用される既定のペイロード保護アルゴリズムは AES 256-CBC 機密性と HMACSHA256 の信頼性です。</span><span class="sxs-lookup"><span data-stu-id="dfa81-133">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="dfa81-134">90 日ごとにロールバックされます、512 ビット マスター _ キーをペイロードあたりごとにこれらのアルゴリズムに使用される 2 つのサブ キーを派生させるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="dfa81-134">A 512-bit master key, rolled every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="dfa81-135">参照してください[サブキーを派生](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="dfa81-135">See [subkey derivation](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) for more information.</span></span>
