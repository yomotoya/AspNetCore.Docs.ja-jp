---
title: "キーを持つが失効している保護を解除するペイロード"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d176515792045545add66ba5aedb0358d8bdc70
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>キーを持つが失効している保護を解除するペイロード

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

主に、ASP.NET Core データ保護 Api は社外秘のペイロードの無期限の持続性の目的ではありません。 他のテクノロジと同様に[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](https://docs.microsoft.com/rights-management/)より永続的ストレージのシナリオに適している、およびこれに対応して強力なキー管理機能があります。 ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用して、開発者を禁止することは nothing を使用する必要があります。 キーは、キーが使用可能な有効な場合に限り、IDataProtector.Unprotect は既存ペイロードを回復常にできるようににことはありませんキー リングから削除されます。

ただし、開発者が IDataProtector.Unprotect はここで例外をスローすると、失効したキーで保護されているデータの保護を解除しようとするときに問題が生じます。 で、システムによってこれらの種類のペイロードを簡単に再作成するし、最悪の場合、サイトの利用者しなければならない場合あります再度ログインするのにはのような認証トークンの場合)、短期間または一時的なペイロードには適してにあります。 永続化されたペイロードのスロー Unprotect を持つ許容できないデータが失われる可能性があります。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

失効したキーが発生した場合でも保護を解除されるペイロードを許可するというシナリオをサポートするためには、データ保護システムには、IPersistedDataProtector 型が含まれています。 IPersistedDataProtector のインスタンスを取得するには、通常の方法で IDataProtector のインスタンスを取得し、IPersistedDataProtector に IDataProtector をキャストしてみてください。

> [!NOTE]
> IDataProtector のすべてのインスタンスは、IPersistedDataProtector にキャストすることができます。 演算子として、c# の開発者が使用する必要があります。 または障害の場合を適切に処理する準備をほぼ同じ結果が、無効なキャストにより、ランタイム例外を回避する必要があります。

IPersistedDataProtector は、次の API サーフェスを公開します。

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

この API は、(バイト配列) として保護されているペイロードを取得し、保護されていないペイロードを返します。 文字列ベースのオーバー ロードはありません。 Out パラメーター 2 は次のとおりです。

* requiresMigration: 実行される場所をこのペイロードを保護するためのキーが古いおよび以降の操作をローリング キーになどのこのペイロードを保護するためのキーは、アクティブな既定のキーでは不要になった場合は true に設定されます。 呼び出し元は、ビジネス ニーズに応じて、ペイロードの再保護を考慮する必要があります。

* wasRevoked: このペイロードを保護するためのキーが取り消された場合は true に設定されます。

>[!WARNING]
> IgnoreRevocationErrors を渡す場合は、十分注意を練習: DangerousUnprotect メソッドの場合は true です。 このメソッドを呼び出した後、wasRevoked 値が true の場合、このペイロードを保護するためのキーが取り消された、およびペイロードの信頼性を問題ありとして扱う必要があります。 ここでのみ操作が続行保護されていないペイロードでいくつか別の保証がある場合、本物である、ことなどの信頼されていない web クライアントによって送信されるのではなく、セキュリティで保護されたデータベースから受信します。

[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
