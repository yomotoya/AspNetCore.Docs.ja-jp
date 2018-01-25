---
title: "キーを持つが失効している保護を解除するペイロード"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core アプリケーションで、失効させたのでキーで保護されたデータの保護を解除する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 08a8ad9b3b3cc2de48751d4149bf39c58954fd90
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>キーを持つが失効している保護を解除するペイロード

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

主に、ASP.NET Core データ保護 Api は社外秘のペイロードの無期限の持続性の目的ではありません。 他のテクノロジと同様に[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](https://docs.microsoft.com/rights-management/)より永続的ストレージのシナリオに適している、およびこれに対応して強力なキー管理機能があります。 ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用して、開発者を禁止することは nothing を使用する必要があります。 キーからは削除されませんキー リングのため`IDataProtector.Unprotect`キーが使用可能な有効な限り、常に、既存のペイロードを回復できます。

開発者として、失効したキーで保護されているデータの保護を解除しようとしました。 ただし、問題が発生した`IDataProtector.Unprotect`ここで例外がスローされます。 で、システムによってこれらの種類のペイロードを簡単に再作成するし、最悪の場合、サイトの利用者しなければならない場合あります再度ログインするのにはのような認証トークンの場合)、短期間または一時的なペイロードには適してにあります。 永続化されたペイロードを持つ、 `Unprotect` throw が許容できないデータ損失につながる可能性があります。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

データ保護システムに含まれるキーが失効した場合でも保護を解除されるペイロードを許可するというシナリオをサポートするために、`IPersistedDataProtector`型です。 インスタンスを取得する`IPersistedDataProtector`のインスタンスを取得するだけ`IDataProtector`try キャストと通常の方法で、`IDataProtector`に`IPersistedDataProtector`です。

> [!NOTE]
> すべて`IDataProtector`インスタンスにキャストできる`IPersistedDataProtector`です。 演算子として、c# の開発者が使用する必要があります。 または障害の場合を適切に処理する準備をほぼ同じ結果が、無効なキャストにより、ランタイム例外を回避する必要があります。

`IPersistedDataProtector`次の API サーフェスを公開します。

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

この API は、(バイト配列) として保護されているペイロードを取得し、保護されていないペイロードを返します。 文字列ベースのオーバー ロードはありません。 Out パラメーター 2 は次のとおりです。

* `requiresMigration`: 実行される場所をこのペイロードを保護するためのキーが古いおよび以降の操作をローリング キーになどのこのペイロードを保護するためのキーは、アクティブな既定のキーでは不要になった場合は true に設定されます。 呼び出し元は、ビジネス ニーズに応じて、ペイロードの再保護を考慮する必要があります。

* `wasRevoked`: このペイロードを保護するためのキーが取り消された場合は true に設定されます。

>[!WARNING]
> 慎重に渡す際に`ignoreRevocationErrors: true`を`DangerousUnprotect`メソッドです。 このメソッドを呼び出した後の場合、`wasRevoked`値が true の場合、このペイロードを保護するためのキーが取り消された、し、ペイロードの信頼性を問題ありとして扱う必要があります。 この場合、のみ操作が続行保護されていないペイロードである本物であり、たとえば別程度保証がある場合そのが、信頼されていない web クライアントによって送信されるのではなく、セキュリティで保護されたデータベースから入手します。

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
