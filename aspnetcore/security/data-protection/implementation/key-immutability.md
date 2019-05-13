---
title: キーの不変性と ASP.NET Core でのキーの設定
author: rick-anderson
description: ASP.NET Core データ保護キーの不変の Api の実装の詳細について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892289"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>キーの不変性と ASP.NET Core でのキーの設定

オブジェクトは、バッキング ストアに永続化されるとその表現は永久に修正されます。 新しいデータは、バッキング ストアに追加できますが、既存のデータを変更しないことができます。 この動作の主な目的は、データの破損を防ぐためです。

この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。 その作成、ライセンス認証、および有効期限の日付変更できないを使用して取り消すことができる、`IKeyManager`します。 さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。

開発者は、キーの永続性に影響を与える設定を変更する場合これらの変更には触れません効果、キーを生成すると、いずれかを明示的に呼び出すによって次の時間まで`IKeyManager.CreateNewKey`またはシステム経由で、データ保護の独自[自動キー生成](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)動作します。 キーの永続性に影響する設定は次のとおりです。

* [既定キーの有効期間](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Rest のメカニズムでは、キーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)

* [キー内に含まれているアルゴリズムの情報](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

これらの設定、次の自動キーのローリング時間よりも前に開始するために必要な場合は、明示的な呼び出しを行うことを検討してください`IKeyManager.CreateNewKey`新しいキーの作成を強制します。 明示的にアクティブ化日付を指定してください ({0} 今すぐ + 2 日} は、変更が反映されるまでの時間を許可する適切な目安) と呼び出しの有効期限の日付。

>[!TIP]
> リポジトリの手を加えることのすべてのアプリケーションが使用して、同じ設定を指定する必要があります、`IDataProtectionBuilder`拡張メソッド。 それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存するされます。
