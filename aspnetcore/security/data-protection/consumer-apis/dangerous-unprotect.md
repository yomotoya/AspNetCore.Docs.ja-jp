---
title: ASP.NET Core でキーを持つが取り消されたペイロードの保護を解除します。
author: rick-anderson
description: ASP.NET Core アプリで、取り消されたためキーで保護されているデータの保護を解除する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897109"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>ASP.NET Core でキーを持つが取り消されたペイロードの保護を解除します。

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET Core データ保護 Api が機密のペイロードの無期限の永続化の主に意図されていません。 などの他のテクノロジ[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](/rights-management/)無制限のストレージのシナリオに適したこれに応じて拡大縮小の強力なキー管理機能があるとします。 ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用してから、開発者を禁止すること何もないです。 キーがので、キー リングから削除しない`IDataProtector.Unprotect`キーが使用可能で有効な限り常に、既存のペイロードを回復できます。

開発者が、失効したキーを持つとして保護されているデータの保護を解除しようとしています。 ただし、問題が発生した`IDataProtector.Unprotect`ここで例外がスローされます。 システムでこれらの種類のペイロードを簡単に再作成するし、サイト訪問者をもう一度ログインする必要な最悪の場合の (認証トークン) のようなペイロードを短期間または一時的な問題があります。 持つ、永続化されたペイロードの`Unprotect`throw が許容できないデータ損失につながる可能性があります。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

失効したキーが発生しても保護するペイロードを許可する際のシナリオをサポートするデータ保護システムに含まれる、`IPersistedDataProtector`型。 インスタンスを取得する`IPersistedDataProtector`のインスタンスが表示されるだけ`IDataProtector`通常の方法でお試しくださいキャスト、`IDataProtector`に`IPersistedDataProtector`します。

> [!NOTE]
> すべて`IDataProtector`インスタンスにキャストできる`IPersistedDataProtector`します。 開発者が使用する必要があります、C#演算子、または似ていますが無効なキャストで引き起こされたランタイムの例外を回避し、障害発生時に適切に処理する準備が必要があります。

`IPersistedDataProtector` 次の API サーフェスを公開します。

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

この API は、(バイト配列) として保護されているペイロードを受け取り、保護されていないペイロードを返します。 文字列ベースのオーバー ロードがありません。 Out パラメーター 2 は次のとおりです。

* `requiresMigration`: 実行される場所をこのペイロードを保護するために使用するキーは、アクティブな既定のキーが不要になった、など、このペイロードを保護するために使用するキーが古いと、以降の操作をローリング キーがある場合は true に設定されます。 呼び出し元は、ビジネス ニーズに応じて、ペイロードの再保護を検討することです。

* `wasRevoked`: このペイロードを保護するために使用するキーが取り消された場合は true に設定されます。

>[!WARNING]
> 渡すときに十分注意`ignoreRevocationErrors: true`を`DangerousUnprotect`メソッド。 このメソッドを呼び出した後の場合、`wasRevoked`値が true の場合、このペイロードを保護するために使用するキーが取り消されたし、ペイロードの信頼性が問題ありとして扱われます。 この場合は、のみ操作が続行、保護されていないペイロードであるを導き出すことなどいくつか別の保証がある場合そのが、信頼されていない web クライアントによって送信されるのではなく、セキュリティで保護されたデータベースから入手します。

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
