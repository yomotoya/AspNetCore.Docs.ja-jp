---
title: "保護されたペイロードの有効期間を制限します。"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>保護されたペイロードの有効期間を制限します。

アプリケーション開発者が一定期間の後に期限切れになる保護されているペイロードを作成しようとした、シナリオがあります。 たとえば、保護されたペイロードは必要があるだけ有効な 1 つの 1 時間以内のパスワード リセット トークンを表す場合があります。 ことが確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者およびこれを行うか、上級開発者が望まけれども、開発者の大部分の有効期限のこれらの管理を拡張できる面倒

この簡単にするため、開発者向けの Microsoft.AspNetCore.DataProtection.Extensions パッケージには一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。 これらの Api は、ITimeLimitedDataProtector 型から切断します。

## <a name="api-usage"></a>API の使用方法

ITimeLimitedDataProtector インターフェイスは、保護し、時間制限を復号化/ペイロードを自己期限切れのコア インターフェイスです。 ITimeLimitedDataProtector のインスタンスを作成するには、最初、通常のインスタンスを必要がありますが[IDataProtector](overview.md)特定の目的で構築します。 IDataProtector インスタンスが使用可能、メソッドを呼び出して IDataProtector.ToTimeLimitedDataProtector 拡張機能を有効期限の組み込み機能を備えた、保護機能を取得します。

ITimeLimitedDataProtector は、以下の API サーフェスと拡張メソッドを公開します。

* CreateProtector (文字列目的): を作成するために使用できる点で、ITimeLimitedDataProtector この API は既存の IDataProtectionProvider.CreateProtector に似ています[チェーンを目的](purpose-strings.md)ルート期間限定の保護機能からです。

* (バイト:operator[] プレーン テキスト、DateTimeOffset の有効期限) を保護する: byte[]

* 保護する (バイト:operator[] プレーン テキスト、TimeSpan の有効期間): byte[]

* (バイト:operator[] プレーン テキスト) を保護する: byte[]

* (文字列プレーン テキスト、DateTimeOffset の有効期限) を保護する: 文字列

* 保護 (文字列のプレーン テキスト、TimeSpan の有効期間): 文字列

* (文字列のプレーン テキスト) を保護する: 文字列

プレーン テキストのみを実行するコア保護メソッド、ペイロードの有効期限の日付を指定することを許可する新しいオーバー ロードがあります。 (現在のシステム時刻、TimeSpan を使用して) からの相対的な時間として、または (、DateTimeOffset) を使用して絶対日付として、有効期限の日付を指定できます。 有効期限を受け取らない指定できるオーバー ロードが呼び出されると、ペイロードが期限切れにしないと見なされます。

* (バイト:operator[] protectedData、DateTimeOffset の有効期限アウト) の保護を解除: byte[]

* (バイト:operator[] protectedData) の保護を解除: byte[]

* (DateTimeOffset の有効期限を文字列 protectedData) の保護を解除: 文字列

* (文字列 protectedData) の保護を解除: 文字列

Unprotect メソッドには、元の保護されていないデータが返されます。 ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。 ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。

>[!WARNING]
> これらの Api を使用して、永続的または長期の永続化を必要とするペイロードを保護することはお勧めしません。 「ことを許容できるは回復不能に永続的に、1 か月後に保護されているペイロードですか?」 適切な経験則; として使用できます。回答がある場合、開発者検討してくださいありません代替 Api。

使用して下記のサンプル、 [DI 以外のコード パス](../configuration/non-di-scenarios.md)データ保護システムをインスタンス化するためです。 このサンプルを実行するには、Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照が最初に追加されたことを確認します。

[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
