---
title: ASP.NET Core でのデータ保護 Api の概要します。
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用して保護すると、アプリのデータを復号化する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838115"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="819f2-103">ASP.NET Core でのデータ保護 Api の概要します。</span><span class="sxs-lookup"><span data-stu-id="819f2-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="819f2-104">その最も簡単な保護データには、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="819f2-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="819f2-105">データ保護プロバイダーからのデータ保護機能を作成します。</span><span class="sxs-lookup"><span data-stu-id="819f2-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="819f2-106">呼び出す、`Protect`を保護するデータを持つメソッド。</span><span class="sxs-lookup"><span data-stu-id="819f2-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="819f2-107">呼び出す、`Unprotect`をプレーン テキストに戻す有効にするデータを持つメソッド。</span><span class="sxs-lookup"><span data-stu-id="819f2-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="819f2-108">ほとんどのフレームワークと ASP.NET Core SignalR などのアプリ モデルは既にデータ保護システムを構成し、依存関係の挿入を使用してアクセスするサービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="819f2-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="819f2-109">次の例では、依存関係の挿入サービス コンテナーを構成して、データ保護スタックの登録、DI を使用してデータ保護プロバイダーの受信、保護機能と保護し、保護を解除するデータの作成を示します。</span><span class="sxs-lookup"><span data-stu-id="819f2-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="819f2-110">プロテクターを作成するときに、1 つまたは複数を指定する必要があります[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)します。</span><span class="sxs-lookup"><span data-stu-id="819f2-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="819f2-111">目的の文字列は、消費者間の分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="819f2-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="819f2-112">たとえば、「緑色」の目的の文字列で作成された保護機能は、「紫」の目的を持つプロテクターによって提供されるデータの保護を解除できないでしょう。</span><span class="sxs-lookup"><span data-stu-id="819f2-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="819f2-113">インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="819f2-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="819f2-114">意図したものをコンポーネントへの参照を取得したら、`IDataProtector`呼び出しに`CreateProtector`、複数の呼び出しの参照が使用されます`Protect`と`Unprotect`します。</span><span class="sxs-lookup"><span data-stu-id="819f2-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="819f2-115">呼び出し`Unprotect`CryptographicException 保護されたペイロードの検証または解読できない場合がスローされます。</span><span class="sxs-lookup"><span data-stu-id="819f2-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="819f2-116">一部のコンポーネントがエラーを無視することが中に保護を解除します。認証 cookie を読み取るコンポーネント可能性がありますこのエラーを処理しがなかった cookie すべての場合と、要求を処理ではなくも、最初からの要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="819f2-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="819f2-117">この動作をするコンポーネントは、すべての例外の飲み込みではなく CryptographicException を明示的にキャッチする必要があります。</span><span class="sxs-lookup"><span data-stu-id="819f2-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
