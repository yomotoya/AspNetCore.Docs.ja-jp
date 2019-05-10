---
title: ASP.NET Core での一時的なデータ保護プロバイダー
author: rick-anderson
description: ASP.NET Core の一時的なデータ保護プロバイダーの実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895179"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="1b63e-103">ASP.NET Core での一時的なデータ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="1b63e-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="1b63e-104">アプリケーションが、throwaway に必要なシナリオが`IDataProtectionProvider`します。</span><span class="sxs-lookup"><span data-stu-id="1b63e-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="1b63e-105">たとえば、開発者は、1 回限りのコンソール アプリケーションで実験だけ可能性がありますまたはアプリケーション自体は一時的なもの (これがスクリプト化または単体テスト プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="1b63e-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="1b63e-106">これらのシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)パッケージは、型を含む`EphemeralDataProtectionProvider`します。</span><span class="sxs-lookup"><span data-stu-id="1b63e-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="1b63e-107">この種類の基本的な実装を提供する`IDataProtectionProvider`がキーのリポジトリは、メモリ内のみが保持されているし、任意のバッキング ストアに書き込まれますはありません。</span><span class="sxs-lookup"><span data-stu-id="1b63e-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="1b63e-108">各インスタンス`EphemeralDataProtectionProvider`独自の一意のマスター _ キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1b63e-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="1b63e-109">そのため場合、`IDataProtector`をルートと、 `EphemeralDataProtectionProvider` 、保護されたペイロードを生成しますそのペイロードは、同等でのみ保護できます`IDataProtector`(指定された同じ[目的](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes)チェーン) ルートと同じ`EphemeralDataProtectionProvider`。インスタンス。</span><span class="sxs-lookup"><span data-stu-id="1b63e-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="1b63e-110">次のサンプルでは、インスタンス化では、`EphemeralDataProtectionProvider`保護およびデータの保護を解除するために使用するとします。</span><span class="sxs-lookup"><span data-stu-id="1b63e-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
