---
title: "短期のデータ保護プロバイダー"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: ee8dccac3ba990b110758042192779426b01fc53
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="ephemeral-data-protection-providers"></a>短期のデータ保護プロバイダー

<a name="data-protection-implementation-key-storage-ephemeral"></a>

アプリケーションが throwaway IDataProtectionProvider 必要があるシナリオがあります。 たとえば、開発者が 1 回限りのコンソール アプリケーションで試してみるだけ可能性がありますまたはアプリケーション自体は一時的なものがスクリプト化 (、単体テスト プロジェクト)。 これらのシナリオをサポートするためには、パッケージ Microsoft.AspNetCore.DataProtection には、型 EphemeralDataProtectionProvider が含まれています。 この種類のキーのリポジトリがメモリ内のみが保持されているし、バッキング ストアに書き込んでいない IDataProtectionProvider の基本的な実装を提供します。

EphemeralDataProtectionProvider の各インスタンスは、独自の一意のマスター _ キーを使用します。 したがって、IDataProtector、EphemeralDataProtectionProvider をルートとは、保護されているペイロードを生成する場合そのペイロードできますのみするによって保護されていない同等 IDataProtector (指定された同じ[目的](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)チェーン) ルートと同じEphemeralDataProtectionProvider インスタンス。

次の例は、EphemeralDataProtectionProvider をインスタンス化および保護し、データ保護の解除に使用します。

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
