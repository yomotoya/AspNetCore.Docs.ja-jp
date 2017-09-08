---
title: "コンシューマー Api の概要"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a><span data-ttu-id="1b680-103">コンシューマー Api の概要</span><span class="sxs-lookup"><span data-stu-id="1b680-103">Consumer APIs overview</span></span>

<span data-ttu-id="1b680-104">IDataProtectionProvider と IDataProtector インターフェイスは、コンシューマーを使用するには、データ保護システムを使用して基本的なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="1b680-104">The IDataProtectionProvider and IDataProtector interfaces are the basic interfaces through which consumers use the data protection system.</span></span> <span data-ttu-id="1b680-105">Microsoft.AspNetCore.DataProtection.Abstractions パッケージ内にあります。</span><span class="sxs-lookup"><span data-stu-id="1b680-105">They are located in the Microsoft.AspNetCore.DataProtection.Abstractions package.</span></span>

## <a name="idataprotectionprovider"></a><span data-ttu-id="1b680-106">IDataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="1b680-106">IDataProtectionProvider</span></span>

<span data-ttu-id="1b680-107">プロバイダーのインターフェイスでは、データ保護システムのルートを表します。</span><span class="sxs-lookup"><span data-stu-id="1b680-107">The provider interface represents the root of the data protection system.</span></span> <span data-ttu-id="1b680-108">保護やデータ保護の解除を直接使用できません。</span><span class="sxs-lookup"><span data-stu-id="1b680-108">It cannot directly be used to protect or unprotect data.</span></span> <span data-ttu-id="1b680-109">代わりに、コンシューマーは、IDataProtectionProvider.CreateProtector(purpose)、ここで、目的は、目的のコンシューマーのユース ケースを説明する文字列を呼び出すことによって IDataProtector への参照を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1b680-109">Instead, the consumer must get a reference to an IDataProtector by calling IDataProtectionProvider.CreateProtector(purpose), where purpose is a string that describes the intended consumer use case.</span></span> <span data-ttu-id="1b680-110">参照してください[目的文字列](purpose-strings.md)を目的として、このパラメーターの適切な値を選択する方法についてははるかにします。</span><span class="sxs-lookup"><span data-stu-id="1b680-110">See [Purpose Strings](purpose-strings.md) for much more information on the intent of this parameter and how to choose an appropriate value.</span></span>

## <a name="idataprotector"></a><span data-ttu-id="1b680-111">IDataProtector</span><span class="sxs-lookup"><span data-stu-id="1b680-111">IDataProtector</span></span>

<span data-ttu-id="1b680-112">プロテクター インターフェイスが CreateProtector への呼び出しによって返され、は、コンシューマーは、実行に使用できるこのインターフェイスは、保護し、操作の保護を解除します。</span><span class="sxs-lookup"><span data-stu-id="1b680-112">The protector interface is returned by a call to CreateProtector, and it is this interface which consumers can use to perform protect and unprotect operations.</span></span>

<span data-ttu-id="1b680-113">一部のデータを保護するには、保護するメソッドにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="1b680-113">To protect a piece of data, pass the data to the Protect method.</span></span> <span data-ttu-id="1b680-114">どの変換 byte[] -> byte[]、メソッドを定義する基本インターフェイスですがありますも、オーバー ロード (拡張メソッドとして提供) の文字列に変換する文字列]-> [です。</span><span class="sxs-lookup"><span data-stu-id="1b680-114">The basic interface defines a method which converts byte[] -> byte[], but there is also an overload (provided as an extension method) which converts string -> string.</span></span> <span data-ttu-id="1b680-115">2 つの方法で提供されるセキュリティは同じです。開発者は、いずれかのオーバー ロードは、ユース ケースに対して最も便利な方法を選択してください。</span><span class="sxs-lookup"><span data-stu-id="1b680-115">The security offered by the two methods is identical; the developer should choose whichever overload is most convenient for their use case.</span></span> <span data-ttu-id="1b680-116">選択すると、オーバー ロードの保護によって返される値に関係なくメソッドはようになりました (enciphered および不正使用防止機能も使用可能な)、および保護アプリケーションを使用すると、信頼されていないクライアントに送信できます。</span><span class="sxs-lookup"><span data-stu-id="1b680-116">Irrespective of the overload chosen, the value returned by the Protect method is now protected (enciphered and tamper-proofed), and the application can send it to an untrusted client.</span></span>

<span data-ttu-id="1b680-117">保護されていた一部のデータの保護を解除するには、保護の解除メソッドに、保護されたデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="1b680-117">To unprotect a previously-protected piece of data, pass the protected data to the Unprotect method.</span></span> <span data-ttu-id="1b680-118">(Byte[] がある-開発者の利便性のためのベースと文字列ベースのオーバー ロードします)。保護されたペイロードは、この同じ IDataProtector を保護するに旧の呼び出しによって生成された、Unprotect メソッドは、元の保護されていないペイロードを返します。</span><span class="sxs-lookup"><span data-stu-id="1b680-118">(There are byte[]-based and string-based overloads for developer convenience.) If the protected payload was generated by an earlier call to Protect on this same IDataProtector, the Unprotect method will return the original unprotected payload.</span></span> <span data-ttu-id="1b680-119">保護されたペイロードは、改ざんされたまたは異なる IDataProtector によって生成された、Unprotect メソッド CryptographicException がスローされます。</span><span class="sxs-lookup"><span data-stu-id="1b680-119">If the protected payload has been tampered with or was produced by a different IDataProtector, the Unprotect method will throw CryptographicException.</span></span>

<span data-ttu-id="1b680-120">異なる IDataProtector と同一の概念は、目的の概念に関連付けます。</span><span class="sxs-lookup"><span data-stu-id="1b680-120">The concept of same vs. different IDataProtector ties back to the concept of purpose.</span></span> <span data-ttu-id="1b680-121">IDataProtector の 2 つのインスタンスが同じルート IDataProtectionProvider からが IDataProtectionProvider.CreateProtector への呼び出しでそれぞれ異なる目的の文字列を使用して生成されたかどうかと見なされる[異なるプロテクター](purpose-strings.md)、し、もう一方によって生成されるペイロードの保護を解除する 1 つはできません。</span><span class="sxs-lookup"><span data-stu-id="1b680-121">If two IDataProtector instances were generated from the same root IDataProtectionProvider but via different purpose strings in the call to IDataProtectionProvider.CreateProtector, then they are considered [different protectors](purpose-strings.md), and one will not be able to unprotect payloads generated by the other.</span></span>

## <a name="consuming-these-interfaces"></a><span data-ttu-id="1b680-122">これらのインターフェイスを使用</span><span class="sxs-lookup"><span data-stu-id="1b680-122">Consuming these interfaces</span></span>

<span data-ttu-id="1b680-123">DI 対応のコンポーネントの使用目的は、コンポーネントが、コンス トラクターで IDataProtectionProvider パラメーターを受け取るし、コンポーネントがインスタンス化されるとき、DI システムがそのこのサービスを自動的に提供します。</span><span class="sxs-lookup"><span data-stu-id="1b680-123">For a DI-aware component, the intended usage is that the component take an IDataProtectionProvider parameter in its constructor and that the DI system automatically provides this service when the component is instantiated.</span></span>

> [!NOTE]
> <span data-ttu-id="1b680-124">一部のアプリケーション (コンソール アプリケーション、ASP.NET 4.x アプリケーションなど) があります DI に対応するため、ここで説明する機構を使用できません。</span><span class="sxs-lookup"><span data-stu-id="1b680-124">Some applications (such as console applications or ASP.NET 4.x applications) might not be DI-aware so cannot use the mechanism described here.</span></span> <span data-ttu-id="1b680-125">これらのシナリオを参照してください、[非 DI 対応したシナリオ](../configuration/non-di-scenarios.md)DI を経由せず IDataProtection プロバイダーのインスタンスを取得の詳細については、ドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="1b680-125">For these scenarios consult the [Non DI Aware Scenarios](../configuration/non-di-scenarios.md) document for more information on getting an instance of an IDataProtection provider without going through DI.</span></span>

<span data-ttu-id="1b680-126">次の例では、3 つの概念を示しています。</span><span class="sxs-lookup"><span data-stu-id="1b680-126">The following sample demonstrates three concepts:</span></span>

1. <span data-ttu-id="1b680-127">[データ保護システムに追加する](../configuration/overview.md)サービス コンテナー</span><span class="sxs-lookup"><span data-stu-id="1b680-127">[Adding the data protection system](../configuration/overview.md) to the service container,</span></span>

2. <span data-ttu-id="1b680-128">DI を使用して、IDataProtectionProvider のインスタンスを取得し、</span><span class="sxs-lookup"><span data-stu-id="1b680-128">Using DI to receive an instance of an IDataProtectionProvider, and</span></span>

3. <span data-ttu-id="1b680-129">IDataProtectionProvider から IDataProtector を作成し、保護し、データ保護の解除に使用します。</span><span class="sxs-lookup"><span data-stu-id="1b680-129">Creating an IDataProtector from an IDataProtectionProvider and using it to protect and unprotect data.</span></span>

<span data-ttu-id="1b680-130">[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]</span><span class="sxs-lookup"><span data-stu-id="1b680-130">[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]</span></span>

<span data-ttu-id="1b680-131">Microsoft.AspNetCore.DataProtection.Abstractions パッケージには、開発者便宜を図って IServiceProvider.GetDataProtector 拡張メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1b680-131">The package Microsoft.AspNetCore.DataProtection.Abstractions contains an extension method IServiceProvider.GetDataProtector as a developer convenience.</span></span> <span data-ttu-id="1b680-132">これは、単一の操作を使用しているサービス プロバイダーからの IDataProtectionProvider の取得と IDataProtectionProvider.CreateProtector を呼び出すことの両方としてカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="1b680-132">It encapsulates as a single operation both retrieving an IDataProtectionProvider from the service provider and calling IDataProtectionProvider.CreateProtector.</span></span> <span data-ttu-id="1b680-133">次の例では、その使用法を示します。</span><span class="sxs-lookup"><span data-stu-id="1b680-133">The following sample demonstrates its usage.</span></span>

<span data-ttu-id="1b680-134">[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]</span><span class="sxs-lookup"><span data-stu-id="1b680-134">[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]</span></span>

>[!TIP]
> <span data-ttu-id="1b680-135">IDataProtectionProvider と IDataProtector のインスタンスが、スレッド セーフである複数の呼び出し元のです。</span><span class="sxs-lookup"><span data-stu-id="1b680-135">Instances of IDataProtectionProvider and IDataProtector are thread-safe for multiple callers.</span></span> <span data-ttu-id="1b680-136">ものではコンポーネントが CreateProtector 呼び出しを経由して IDataProtector への参照を取得したら、その参照は保護する複数の呼び出しを使用し、Unprotect Unprotect.A 呼び出しは、保護されているペイロードができない場合は CryptographicException にスローされます。検証するか、データを解読します。</span><span class="sxs-lookup"><span data-stu-id="1b680-136">It is intended that once a component gets a reference to an IDataProtector via a call to CreateProtector, it will use that reference for multiple calls to Protect and Unprotect.A call to Unprotect will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="1b680-137">一部のコンポーネントがエラーを無視することも中に保護を解除します。認証 cookie が読み取られますコンポーネント可能性がありますこのエラーを処理およびこれがなかった cookie まったくかのように、要求を処理ではなく完全要求に失敗します。</span><span class="sxs-lookup"><span data-stu-id="1b680-137">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="1b680-138">この動作をするコンポーネントは、すべての例外を受け入れるではなく CryptographicException をキャッチする必要があります具体的には。</span><span class="sxs-lookup"><span data-stu-id="1b680-138">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
