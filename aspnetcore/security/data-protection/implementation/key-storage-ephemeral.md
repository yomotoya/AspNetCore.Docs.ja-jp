---
title: "短期のデータ保護プロバイダー"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 6b564f082eefad993ac938407e0f3b657d83fb52
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="09926-103">短期のデータ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="09926-103">Ephemeral data protection providers</span></span>

<a name=data-protection-implementation-key-storage-ephemeral></a>

<span data-ttu-id="09926-104">アプリケーションが throwaway IDataProtectionProvider 必要があるシナリオがあります。</span><span class="sxs-lookup"><span data-stu-id="09926-104">There are scenarios where an application needs a throwaway IDataProtectionProvider.</span></span> <span data-ttu-id="09926-105">たとえば、開発者が 1 回限りのコンソール アプリケーションで試してみるだけ可能性がありますまたはアプリケーション自体は一時的なものがスクリプト化 (、単体テスト プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="09926-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="09926-106">これらのシナリオをサポートするためには、パッケージ Microsoft.AspNetCore.DataProtection には、型 EphemeralDataProtectionProvider が含まれています。</span><span class="sxs-lookup"><span data-stu-id="09926-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection includes a type EphemeralDataProtectionProvider.</span></span> <span data-ttu-id="09926-107">この種類のキーのリポジトリがメモリ内のみが保持されているし、バッキング ストアに書き込んでいない IDataProtectionProvider の基本的な実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="09926-107">This type provides a basic implementation of IDataProtectionProvider whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="09926-108">EphemeralDataProtectionProvider の各インスタンスは、独自の一意のマスター _ キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="09926-108">Each instance of EphemeralDataProtectionProvider uses its own unique master key.</span></span> <span data-ttu-id="09926-109">したがって、IDataProtector、EphemeralDataProtectionProvider をルートとは、保護されているペイロードを生成する場合そのペイロードできますのみするによって保護されていない同等 IDataProtector (指定された同じ[目的](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)チェーン) ルートと同じEphemeralDataProtectionProvider インスタンス。</span><span class="sxs-lookup"><span data-stu-id="09926-109">Therefore, if an IDataProtector rooted at an EphemeralDataProtectionProvider generates a protected payload, that payload can only be unprotected by an equivalent IDataProtector (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same EphemeralDataProtectionProvider instance.</span></span>

<span data-ttu-id="09926-110">次の例は、EphemeralDataProtectionProvider をインスタンス化および保護し、データ保護の解除に使用します。</span><span class="sxs-lookup"><span data-stu-id="09926-110">The following sample demonstrates instantiating an EphemeralDataProtectionProvider and using it to protect and unprotect data.</span></span>

```none
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
