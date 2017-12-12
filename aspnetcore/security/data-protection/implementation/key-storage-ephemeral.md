---
title: "短期のデータ保護プロバイダー"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core 短期データ保護プロバイダーの実装の詳細について説明します。"
keywords: "ASP.NET Core、データ保護、短期プロバイダー"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 51d4aaa3a669763c2e388a8186ebeaec6b57e77a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="bda68-104">短期のデータ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="bda68-104">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="bda68-105">アプリケーションが、throwaway が必要なシナリオがある`IDataProtectionProvider`です。</span><span class="sxs-lookup"><span data-stu-id="bda68-105">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="bda68-106">たとえば、開発者が 1 回限りのコンソール アプリケーションで試してみるだけ可能性がありますまたはアプリケーション自体は一時的なものがスクリプト化 (、単体テスト プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="bda68-106">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="bda68-107">これらのシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)パッケージには、型が含まれています。`EphemeralDataProtectionProvider`です。</span><span class="sxs-lookup"><span data-stu-id="bda68-107">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="bda68-108">この種類の基本的な実装を提供する`IDataProtectionProvider`がキーのリポジトリがメモリ内のみが保持されているし、バッキング ストアに書き込むはありません。</span><span class="sxs-lookup"><span data-stu-id="bda68-108">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="bda68-109">各インスタンス`EphemeralDataProtectionProvider`独自の一意のマスター _ キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="bda68-109">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="bda68-110">したがって場合、`IDataProtector`をルートと、`EphemeralDataProtectionProvider`保護対象のペイロードが生成されますそのペイロードは同等でのみ保護しない`IDataProtector`(指定された同じ[目的](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)チェーン) ルートと同じ`EphemeralDataProtectionProvider`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bda68-110">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="bda68-111">次の例では、インスタンス化を示しています、`EphemeralDataProtectionProvider`を保護し、データ保護の解除に使用するとします。</span><span class="sxs-lookup"><span data-stu-id="bda68-111">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
