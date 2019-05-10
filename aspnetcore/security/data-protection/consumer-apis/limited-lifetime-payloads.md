---
title: ASP.NET Core で保護されたペイロードの有効期間を制限します。
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用して保護されたペイロードの有効期間を制限する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897119"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>ASP.NET Core で保護されたペイロードの有効期間を制限します。

一定期間の後に期限切れになる保護されているペイロードを作成するアプリケーション開発者が必要があるシナリオがあります。 たとえば、保護されたペイロードは、必ず有効な 1 時間分のパスワード リセット トークンを表すことができます。 確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者可能性がありますと高度な開発者はとにかく、そうことができますが、大多数の開発者のこれらの有効期限の管理を拡張できる面倒です。

マイクロソフト開発者向け、パッケージを簡単に確認する[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。 これらの Api がオフのハング、`ITimeLimitedDataProtector`型。

## <a name="api-usage"></a>API の使用方法

`ITimeLimitedDataProtector`インターフェイスは、主要なインターフェイスの保護と時間制限付き/期限切れ間近自己のペイロードの保護を解除します。 インスタンスを作成する、 `ITimeLimitedDataProtector`、通常のインスタンスを必要がありますまず[IDataProtector](xref:security/data-protection/consumer-apis/overview)特定の目的で構築します。 1 回、`IDataProtector`インスタンスが使用可能な呼び出し、`IDataProtector.ToTimeLimitedDataProtector`有効期限の組み込み機能を備えたのプロテクターを取得する拡張メソッド。

`ITimeLimitedDataProtector` 次の API サーフェスと拡張機能メソッドを公開します。

* CreateProtector(string purpose) :ITimeLimitedDataProtector - この API は、既存のような`IDataProtectionProvider.CreateProtector`を作成するために使用できる点で[チェーンを目的](xref:security/data-protection/consumer-apis/purpose-strings)ルート期間限定の保護機能からです。

* Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]

* Protect(byte[] plaintext, TimeSpan lifetime) : byte[]

* Protect(byte[] plaintext) : byte[]

* (文字列プレーン テキスト、DateTimeOffset の有効期限) の保護: 文字列

* Protect(string plaintext, TimeSpan lifetime) : string

* Protect(string plaintext) : string

コアだけでなく`Protect`プレーン テキストのみを実行するメソッドが、ペイロードの有効期限の日付を指定することは新しいオーバー ロードがあります。 有効期限の日付を検証する絶対的日付として指定できます (を使用して、 `DateTimeOffset`) または相対時刻として (現在のシステムからを使用して、後で、 `TimeSpan`)。 有効期限を受け取りません。 指定できるオーバー ロードを呼び出すと、ペイロードが決して期限切れになると見なされます。

* (Byte protectedData、DateTimeOffset の有効期限を) の保護を解除: byte[]

* Unprotect(byte[] protectedData) : byte[]

* (DateTimeOffset の有効期限を文字列 protectedData) 保護の解除: 文字列

* Unprotect(string protectedData) : string

`Unprotect`メソッドは、元の保護されていないデータを返します。 ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。 ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。

>[!WARNING]
> これらの Api を使用して、長期間または無期限の永続化を必要とするペイロードを保護することをお勧めはできません。 「ことを許容できる 1 か月後にある回復可能な完全に保護されたペイロードのでしょうか。」 目安; として使用できます。場合、答えは、開発者検討してくださいない代替の Api。

使用して次のサンプル、 [DI 以外のコード パス](xref:security/data-protection/configuration/non-di-scenarios)データ保護システムをインスタンス化するためです。 このサンプルを実行するには、まず Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照を追加したことを確認します。

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
