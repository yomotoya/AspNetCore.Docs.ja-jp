---
title: ASP.NET Core での目的の文字列
author: rick-anderson
description: ASP.NET Core データ保護 Api で目的の文字列を使用する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087544"
---
# <a name="purpose-strings-in-aspnet-core"></a><span data-ttu-id="450a3-103">ASP.NET Core での目的の文字列</span><span class="sxs-lookup"><span data-stu-id="450a3-103">Purpose strings in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="450a3-104">コンポーネントを消費しない`IDataProtectionProvider`を一意に渡す必要があります*目的で*パラメーターを`CreateProtector`メソッド。</span><span class="sxs-lookup"><span data-stu-id="450a3-104">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="450a3-105">目的*パラメーター*は、データ保護システムのセキュリティを本質的なルートの暗号化キーが同じ場合でも、暗号化のコンシューマー間の分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="450a3-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="450a3-106">コンシューマーは、目的を指定する場合、目的の文字列内のルートの暗号化キーと共にそのコンシューマーに一意の暗号化のサブキーを派生に使用されます。</span><span class="sxs-lookup"><span data-stu-id="450a3-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="450a3-107">これにより、アプリケーションで他のすべての暗号化サービス コンシューマーからコンシューマーを排除します。 その他のコンポーネントは、そのペイロードを読み取ることではありませんし、その他のコンポーネントのペイロードを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="450a3-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="450a3-108">この分離には、コンポーネントに対する攻撃の実行不可能の全体カテゴリも表示します。</span><span class="sxs-lookup"><span data-stu-id="450a3-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![目的の図の例](purpose-strings/_static/purposes.png)

<span data-ttu-id="450a3-110">上記の図で`IDataProtector`インスタンス A と B**ことはできません**ペイロードを読み取る他ののみ独自です。</span><span class="sxs-lookup"><span data-stu-id="450a3-110">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="450a3-111">秘密にする目的の文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="450a3-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="450a3-112">適切に動作の他のコンポーネントがこれまでしないことを目的の文字列と同じ意味で一意である単純にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="450a3-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="450a3-113">目安、実際にこの情報が競合しないようには、データ保護 Api を利用して、コンポーネントの名前空間と型名を使用します。</span><span class="sxs-lookup"><span data-stu-id="450a3-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="450a3-114">Minting ベアラー トークンを担当する contoso 社製のコンポーネントは、その目的の文字列として Contoso.Security.BearerToken を使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="450a3-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="450a3-115">-さらに良い点 - Contoso.Security.BearerToken.v1、目的の文字列として使用可能性があることもします。</span><span class="sxs-lookup"><span data-stu-id="450a3-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="450a3-116">Contoso.Security.BearerToken.v2 を目的として使用する将来のバージョンは、バージョン番号を追加し、さまざまなバージョン互いから完全に分離されたペイロードの移動しします。</span><span class="sxs-lookup"><span data-stu-id="450a3-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="450a3-117">目的のパラメーターから`CreateProtector`文字列配列では、上記でしたした代わりとして指定されている`[ "Contoso.Security.BearerToken", "v1" ]`します。</span><span class="sxs-lookup"><span data-stu-id="450a3-117">Since the purposes parameter to `CreateProtector` is a string array, the above could've been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="450a3-118">これにより、目的の階層を確立して、データ保護システムでのマルチ テナント シナリオの可能性を開きます。</span><span class="sxs-lookup"><span data-stu-id="450a3-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="450a3-119">コンポーネントは、目的のチェーンの入力の唯一のソースを信頼できないユーザー入力を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="450a3-119">Components shouldn't allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="450a3-120">たとえば、コンポーネントは、セキュリティで保護されたメッセージを格納する責任を負います Contoso.Messaging.SecureMessage を検討してください。</span><span class="sxs-lookup"><span data-stu-id="450a3-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="450a3-121">セキュリティで保護されたメッセージング コンポーネントを呼び出す場合`CreateProtector([ username ])`、悪意のあるユーザーを呼び出すコンポーネントを取得するためにユーザー名"Contoso.Security.BearerToken"を持つアカウントを作成する可能性がありますし、 `CreateProtector([ "Contoso.Security.BearerToken" ])`、誤っているため、セキュリティで保護されたメッセージング認証トークンとして認識される可能性があります mint ペイロードするシステムです。</span><span class="sxs-lookup"><span data-stu-id="450a3-121">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="450a3-122">メッセージング コンポーネントの向上のためチェーンになります`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`、適切な分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="450a3-122">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="450a3-123">によって提供される分離との動作`IDataProtectionProvider`、 `IDataProtector`、目的では、次のとおりとします。</span><span class="sxs-lookup"><span data-stu-id="450a3-123">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="450a3-124">指定された`IDataProtectionProvider`オブジェクト、`CreateProtector`メソッドは、作成、`IDataProtector`両方にオブジェクトが一意に関連付けられている、`IDataProtectionProvider`メソッドに渡された目的でパラメーターを作成するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="450a3-124">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="450a3-125">目的のパラメーターを null にするにはできません。</span><span class="sxs-lookup"><span data-stu-id="450a3-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="450a3-126">(場合は、配列としての目的を指定すると、つまり長さ 0 の配列がすることはできません、配列のすべての要素は null 以外である必要があります。)空の文字列の目的は、技術的には許可されていますをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="450a3-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="450a3-127">2 つの目的の引数は、同じ順序では、(序数の比較子を使用して)、同じ文字列が含まれている場合にのみ等価です。</span><span class="sxs-lookup"><span data-stu-id="450a3-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="450a3-128">1 つの目的の引数は、対応する 1 つの要素の目的で配列と同じです。</span><span class="sxs-lookup"><span data-stu-id="450a3-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="450a3-129">2 つ`IDataProtector`相当から作成されている場合にのみ、オブジェクトが等しい`IDataProtectionProvider`オブジェクトと同じ目的でパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="450a3-129">Two `IDataProtector` objects are equivalent if and only if they're created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="450a3-130">指定された`IDataProtector`オブジェクトへの呼び出し`Unprotect(protectedData)`元に戻ります`unprotectedData`場合にのみ`protectedData := Protect(unprotectedData)`相当する`IDataProtector`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="450a3-130">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="450a3-131">いくつかのコンポーネントが意図的に別のコンポーネントと競合が知られている目的の文字列を選択、ケースを考慮していないことはできます。</span><span class="sxs-lookup"><span data-stu-id="450a3-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="450a3-132">このようなコンポーネントは本質的に悪意のあると見なさ悪意のあるコードがワーカー プロセス内で既に実行されていること、セキュリティ保証を提供するこのシステムはありません。</span><span class="sxs-lookup"><span data-stu-id="450a3-132">Such a component would essentially be considered malicious, and this system isn't intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
