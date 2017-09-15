---
title: "ASP.NET Core のデータ保護"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="9a7fe-103">ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装</span><span class="sxs-lookup"><span data-stu-id="9a7fe-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="9a7fe-104">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="9a7fe-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="9a7fe-105">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="9a7fe-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="9a7fe-106">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="9a7fe-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="9a7fe-107">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="9a7fe-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="9a7fe-108">目的文字列</span><span class="sxs-lookup"><span data-stu-id="9a7fe-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="9a7fe-109">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="9a7fe-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="9a7fe-110">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="9a7fe-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="9a7fe-111">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="9a7fe-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="9a7fe-112">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="9a7fe-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="9a7fe-113">構成</span><span class="sxs-lookup"><span data-stu-id="9a7fe-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="9a7fe-114">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="9a7fe-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="9a7fe-115">既定の設定</span><span class="sxs-lookup"><span data-stu-id="9a7fe-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="9a7fe-116">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="9a7fe-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="9a7fe-117">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="9a7fe-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="9a7fe-118">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="9a7fe-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="9a7fe-119">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="9a7fe-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="9a7fe-120">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="9a7fe-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="9a7fe-121">その他の API</span><span class="sxs-lookup"><span data-stu-id="9a7fe-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="9a7fe-122">実装</span><span class="sxs-lookup"><span data-stu-id="9a7fe-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="9a7fe-123">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="9a7fe-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="9a7fe-124">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="9a7fe-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="9a7fe-125">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="9a7fe-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="9a7fe-126">キーの管理</span><span class="sxs-lookup"><span data-stu-id="9a7fe-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="9a7fe-127">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="9a7fe-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="9a7fe-128">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="9a7fe-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="9a7fe-129">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="9a7fe-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="9a7fe-130">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="9a7fe-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="9a7fe-131">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="9a7fe-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="9a7fe-132">互換性</span><span class="sxs-lookup"><span data-stu-id="9a7fe-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="9a7fe-133">アプリケーション間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="9a7fe-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="9a7fe-134">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="9a7fe-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
