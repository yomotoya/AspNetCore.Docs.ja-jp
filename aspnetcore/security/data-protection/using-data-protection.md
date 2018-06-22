---
title: ASP.NET Core のデータ保護 Api の概要します。
author: rick-anderson
description: 保護して、アプリ内のデータを復号化の ASP.NET Core データ保護 Api を使用する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: ab2551d87d1a2cd22e9f421cabe0288311cb2ec3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275750"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="69f12-103">ASP.NET Core のデータ保護 Api の概要します。</span><span class="sxs-lookup"><span data-stu-id="69f12-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="69f12-104">その最も簡単な保護データで、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="69f12-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="69f12-105">データ保護プロバイダーからのデータ保護機能を作成します。</span><span class="sxs-lookup"><span data-stu-id="69f12-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="69f12-106">呼び出す、`Protect`を保護するデータを持つメソッドです。</span><span class="sxs-lookup"><span data-stu-id="69f12-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="69f12-107">呼び出す、`Unprotect`プレーン テキストに戻るに変換するデータを持つメソッドです。</span><span class="sxs-lookup"><span data-stu-id="69f12-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="69f12-108">ほとんどのフレームワークと ASP.NET SignalR などのアプリ モデルは既にデータ保護システムを設定し、依存関係の挿入を使用してアクセスするサービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="69f12-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="69f12-109">次の例は、依存関係の挿入のサービス コンテナーを構成して、データ保護スタックの登録、DI 経由でデータ保護プロバイダーを受信、保護機能と保護し、保護を解除するデータの作成</span><span class="sxs-lookup"><span data-stu-id="69f12-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="69f12-110">プロテクターを作成する場合は、1 つ以上を指定する必要があります[目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)です。</span><span class="sxs-lookup"><span data-stu-id="69f12-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="69f12-111">目的の文字列は、消費者間の分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="69f12-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="69f12-112">たとえば、「緑」の目的の文字列で作成された、保護機能できなくなります「紫」の目的を使用して、保護機能によって提供されるデータの保護を解除します。</span><span class="sxs-lookup"><span data-stu-id="69f12-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="69f12-113">インスタンス`IDataProtectionProvider`と`IDataProtector`は複数の呼び出し元のスレッド セーフです。</span><span class="sxs-lookup"><span data-stu-id="69f12-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="69f12-114">目的としていること、コンポーネントへの参照を取得したら、`IDataProtector`呼び出しを経由して`CreateProtector`、その参照に複数の呼び出しは使用`Protect`と`Unprotect`です。</span><span class="sxs-lookup"><span data-stu-id="69f12-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="69f12-115">呼び出し`Unprotect`保護されているペイロードを検証または解読できない場合 CryptographicException がスローされます。</span><span class="sxs-lookup"><span data-stu-id="69f12-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="69f12-116">一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。</span><span class="sxs-lookup"><span data-stu-id="69f12-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="69f12-117">この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。</span><span class="sxs-lookup"><span data-stu-id="69f12-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
