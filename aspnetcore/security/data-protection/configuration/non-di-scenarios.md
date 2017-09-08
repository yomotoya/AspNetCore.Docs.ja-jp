---
title: "非 DI 対応したシナリオ"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a><span data-ttu-id="aa917-103">非 DI 対応したシナリオ</span><span class="sxs-lookup"><span data-stu-id="aa917-103">Non DI aware scenarios</span></span>

<span data-ttu-id="aa917-104">データ保護システムが正常に設計された[サービス コンテナーに追加する](../consumer-apis/overview.md)DI メカニズムを使用して依存コンポーネントに提供するとします。</span><span class="sxs-lookup"><span data-stu-id="aa917-104">The data protection system is normally designed [to be added to a service container](../consumer-apis/overview.md) and to be provided to dependent components via a DI mechanism.</span></span> <span data-ttu-id="aa917-105">ただし、場合によっては、これを実現できない、システムを既存のアプリケーションにインポートする際に特にがあります。</span><span class="sxs-lookup"><span data-stu-id="aa917-105">However, there may be some cases where this is not feasible, especially when importing the system into an existing application.</span></span>

<span data-ttu-id="aa917-106">これらのシナリオをサポートするためには、パッケージ Microsoft.AspNetCore.DataProtection.Extensions は具象型システムを使用して、データ保護 DI に固有のコード パスを経由せずに簡単な手段が提供される DataProtectionProvider を提供します。</span><span class="sxs-lookup"><span data-stu-id="aa917-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection.Extensions provides a concrete type DataProtectionProvider which offers a simple way to use the data protection system without going through DI-specific code paths.</span></span> <span data-ttu-id="aa917-107">型自体が IDataProtectionProvider を実装し、このプロバイダーの暗号化キーの格納場所 DirectoryInfo を提供することと同じくらい簡単にを構築します。</span><span class="sxs-lookup"><span data-stu-id="aa917-107">The type itself implements IDataProtectionProvider, and constructing it is as easy as providing a DirectoryInfo where this provider's cryptographic keys should be stored.</span></span>

<span data-ttu-id="aa917-108">例:</span><span class="sxs-lookup"><span data-stu-id="aa917-108">For example:</span></span>

<span data-ttu-id="aa917-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span><span class="sxs-lookup"><span data-stu-id="aa917-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span></span>

>[!WARNING]
> <span data-ttu-id="aa917-110">既定では、DataProtectionProvider 具象型では暗号化されません生のキー マテリアル前に、ファイル システムに保存します。</span><span class="sxs-lookup"><span data-stu-id="aa917-110">By default the DataProtectionProvider concrete type does not encrypt raw key material before persisting it to the file system.</span></span> <span data-ttu-id="aa917-111">これは、ネットワークへの開発者のポイントを共有している、シナリオをサポートする場合、データ保護システムに自動的を推測できません rest での適切なキーの暗号化メカニズムです。</span><span class="sxs-lookup"><span data-stu-id="aa917-111">This is to support scenarios where the developer points to a network share, in which case the data protection system cannot automatically deduce an appropriate at-rest key encryption mechanism.</span></span>
>
><span data-ttu-id="aa917-112">さらに、DataProtectionProvider 具象型は[アプリケーションの分離](overview.md#data-protection-configuration-per-app-isolation)既定では、すべてのアプリケーション、同じキー ディレクトリで参照されている共有できますペイロードその用途のパラメーターと一致する限りです。</span><span class="sxs-lookup"><span data-stu-id="aa917-112">Additionally, the DataProtectionProvider concrete type does not [isolate applications](overview.md#data-protection-configuration-per-app-isolation) by default, so all applications pointed at the same key directory can share payloads as long as their purpose parameters match.</span></span>

<span data-ttu-id="aa917-113">必要な場合は、アプリケーション開発者はこれらの両方で対処できます。</span><span class="sxs-lookup"><span data-stu-id="aa917-113">The application developer can address both of these if desired.</span></span> <span data-ttu-id="aa917-114">DataProtectionProvider コンス トラクターを受け入れる、[オプションの構成のコールバック](overview.md#data-protection-configuration-callback)これは、システムの動作の調整を使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa917-114">The DataProtectionProvider constructor accepts an [optional configuration callback](overview.md#data-protection-configuration-callback) which can be used to tweak the behaviors of the system.</span></span> <span data-ttu-id="aa917-115">以下のサンプルは SetApplicationName、明示的な呼び出しを使用して復元中の分離を示し、自動的に Windows DPAPI を使用して永続化されたキーを暗号化するシステムの構成も示します。</span><span class="sxs-lookup"><span data-stu-id="aa917-115">The sample below demonstrates restoring isolation via an explicit call to SetApplicationName, and it also demonstrates configuring the system to automatically encrypt persisted keys using Windows DPAPI.</span></span> <span data-ttu-id="aa917-116">ディレクトリは、UNC 共有をポイントしている場合に関連するすべてのコンピューター間で共有の証明書を配布して、呼び出しを経由して代わりに証明書ベースの暗号化を使用するシステムを構成する[ProtectKeysWithCertificate](overview.md#configuring-x509-certificate)です。</span><span class="sxs-lookup"><span data-stu-id="aa917-116">If the directory points to a UNC share, you may wish to distribute a shared certificate across all relevant machines and to configure the system to use certificate-based encryption instead via a call to [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).</span></span>

<span data-ttu-id="aa917-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span><span class="sxs-lookup"><span data-stu-id="aa917-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span></span>

>[!TIP]
> <span data-ttu-id="aa917-118">DataProtectionProvider 具象型のインスタンスでは、作成する手間がかかります。</span><span class="sxs-lookup"><span data-stu-id="aa917-118">Instances of the DataProtectionProvider concrete type are expensive to create.</span></span> <span data-ttu-id="aa917-119">アプリケーションは、この種類の複数のインスタンスを保持し、すべて同一のキー記憶域ディレクトリにポイントしている場合は、アプリケーションのパフォーマンスが低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aa917-119">If an application maintains multiple instances of this type and if they're all pointing at the same key storage directory, application performance may be degraded.</span></span> <span data-ttu-id="aa917-120">使用目的は、アプリケーション開発者がこの型を 1 回インスタンス化し、この参照を 1 つを再利用を可能な限り保持することができます。</span><span class="sxs-lookup"><span data-stu-id="aa917-120">The intended usage is that the application developer instantiate this type once then keep reusing this single reference as much as possible.</span></span> <span data-ttu-id="aa917-121">DataProtectionProvider 型およびこれから作成されたすべての IDataProtector インスタンスは、スレッド セーフである複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="aa917-121">The DataProtectionProvider type and all IDataProtector instances created from it are thread-safe for multiple callers.</span></span>
