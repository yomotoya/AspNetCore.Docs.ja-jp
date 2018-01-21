---
title: "短期のデータ保護プロバイダー"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core 短期データ保護プロバイダーの実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a>短期のデータ保護プロバイダー

<a name="data-protection-implementation-key-storage-ephemeral"></a>

アプリケーションが、throwaway が必要なシナリオがある`IDataProtectionProvider`です。 たとえば、開発者が 1 回限りのコンソール アプリケーションで試してみるだけ可能性がありますまたはアプリケーション自体は一時的なものがスクリプト化 (、単体テスト プロジェクト)。 これらのシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)パッケージには、型が含まれています。`EphemeralDataProtectionProvider`です。 この種類の基本的な実装を提供する`IDataProtectionProvider`がキーのリポジトリがメモリ内のみが保持されているし、バッキング ストアに書き込むはありません。

各インスタンス`EphemeralDataProtectionProvider`独自の一意のマスター _ キーを使用します。 したがって場合、`IDataProtector`をルートと、`EphemeralDataProtectionProvider`保護対象のペイロードが生成されますそのペイロードは同等でのみ保護しない`IDataProtector`(指定された同じ[目的](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)チェーン) ルートと同じ`EphemeralDataProtectionProvider`インスタンス。

次の例では、インスタンス化を示しています、`EphemeralDataProtectionProvider`を保護し、データ保護の解除に使用するとします。

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
