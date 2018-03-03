---
title: "保護されたペイロードの有効期間を制限します。"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 Api を使用して保護されているペイロードの有効期間を制限する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: d631851b5b933d75c37a308f492840e3442e6f1a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>保護されたペイロードの有効期間を制限します。

アプリケーション開発者が一定期間の後に期限切れになる保護されているペイロードを作成しようとした、シナリオがあります。 たとえば、保護されたペイロードは必要があるだけ有効な 1 つの 1 時間以内のパスワード リセット トークンを表す場合があります。 ことが確かに、埋め込みの有効期限日を含む独自のペイロード形式を作成する開発者およびこれを行うか、上級開発者が望まけれども、開発者の大部分の有効期限のこれらの管理を拡張できる面倒

開発者、ユーザー向けのパッケージを簡単に作成する[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)一定期間の後に自動的に期限切れのペイロードを作成するためのユーティリティ Api が含まれています。 これらの Api がオフのハング、`ITimeLimitedDataProtector`型です。

## <a name="api-usage"></a>API の使用方法

`ITimeLimitedDataProtector`インターフェイスは、コア インターフェイスの保護や、時間制限/自己期限切れ間近のペイロードを復号化します。 インスタンスを作成する、 `ITimeLimitedDataProtector`、最初、通常のインスタンスを必要があります[IDataProtector](overview.md)特定の目的で構築します。 1 回、`IDataProtector`インスタンスが使用可能な呼び出し、`IDataProtector.ToTimeLimitedDataProtector`有効期限の組み込み機能を備えたの保護機能を取得する拡張メソッド。

`ITimeLimitedDataProtector` 以下の API サーフェスと拡張メソッドを公開します。

* CreateProtector (文字列目的): ITimeLimitedDataProtector - この API は、既存のような`IDataProtectionProvider.CreateProtector`を作成するために使用できる点で[チェーンを目的](purpose-strings.md)ルート期間限定の保護機能からです。

* Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]

* Protect(byte[] plaintext, TimeSpan lifetime) : byte[]

* Protect(byte[] plaintext) : byte[]

* (文字列プレーン テキスト、DateTimeOffset の有効期限) を保護する: 文字列

* 保護 (文字列のプレーン テキスト、TimeSpan の有効期間): 文字列

* (文字列のプレーン テキスト) を保護する: 文字列

コアに加えて`Protect`プレーン テキストのみを実行するメソッド、ペイロードの有効期限の日付を指定することを許可する新しいオーバー ロードがあります。 絶対日付として有効期限の日付を指定することができます (を使用して、 `DateTimeOffset`) または相対時刻として (現在のシステムからを使用して、後で、 `TimeSpan`)。 有効期限を受け取らない指定できるオーバー ロードが呼び出されると、ペイロードが期限切れにしないと見なされます。

* Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]

* Unprotect(byte[] protectedData) : byte[]

* (DateTimeOffset の有効期限を文字列 protectedData) の保護を解除: 文字列

* (文字列 protectedData) の保護を解除: 文字列

`Unprotect`メソッドには、元の保護されていないデータが返されます。 ペイロードがまだ失効しておらず、絶対有効期限が省略可能な出力元の保護されていないデータと共にパラメーターとして返されます。 ペイロードが有効期限が切れて Unprotect メソッドのすべてのオーバー ロードは CryptographicException をスローします。

>[!WARNING]
> これがこれらの Api を使用して、永続的または長期の永続化を必要とするペイロードを保護するにはお勧めできません。 「ことを許容できるは回復不能に永続的に、1 か月後に保護されているペイロードですか?」 適切な経験則; として使用できます。回答がある場合、開発者検討してくださいありません代替 Api。

使用して下記のサンプル、 [DI 以外のコード パス](../configuration/non-di-scenarios.md)データ保護システムをインスタンス化するためです。 このサンプルを実行するには、Microsoft.AspNetCore.DataProtection.Extensions パッケージへの参照が最初に追加されたことを確認します。

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
