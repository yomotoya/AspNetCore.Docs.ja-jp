---
title: "キーの不変性と設定を変更します。"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a>キーの不変性と設定を変更します。

オブジェクトは、バッキング ストアに永続化した後、形式は永久に固定されます。 バッキング ストアに新しいデータを追加することができますが、既存のデータを変更しないことができます。 この動作の主な目的は、データの破損を防ぐためです。

この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。 IKeyManager を使用して、取り消すことができますが、その作成、アクティブ化、および有効期限の日付を変更ことはありませんできます。 さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。

場合は、開発者は、キーの永続性に影響する設定を変更、それらの変更にならない効果 IKeyManager.CreateNewKey を明示的に呼び出すを介してまたはシステム経由で、データ保護の独自のいずれか、キーが生成された次の時間まで[自動キーの生成](key-management.md#data-protection-implementation-key-management)動作します。 キーの永続性に影響する設定は次のとおりです。

* [既定キーの有効期間](key-management.md#data-protection-implementation-key-management)

* [Rest メカニズムでキーの暗号化](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [キーに含まれるアルゴリズム情報](../configuration/overview.md#data-protection-changing-algorithms)

これらの設定、次の自動キーのローリング時間よりも前に開始する場合は、IKeyManager.CreateNewKey に新しいキーの作成を強制の明示的な呼び出しを行うことを検討してください。 明示的なライセンス認証日を指定してください ({今すぐ + 2 日間} は、目安変更が反映されるまでの時間を許可する) と呼び出しの有効期限。

>[!TIP]
> リポジトリと接触しているすべてのアプリケーションが IDataProtectionBuilder 拡張メソッドで同じ設定を指定する必要があります、それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存します。.
