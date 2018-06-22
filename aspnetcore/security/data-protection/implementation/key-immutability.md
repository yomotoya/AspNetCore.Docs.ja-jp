---
title: キーの ASP.NET Core の不変性とキーの設定
author: rick-anderson
description: ASP.NET Core データ保護キー不変 Api の実装の詳細を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279455"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>キーの ASP.NET Core の不変性とキーの設定

オブジェクトは、バッキング ストアに永続化した後、形式は永久に固定されます。 バッキング ストアに新しいデータを追加することができますが、既存のデータを変更しないことができます。 この動作の主な目的は、データの破損を防ぐためです。

この動作の 1 つの結果は、こと、キーは、バッキング ストアに書き込まれますが後、は変更できません。 作成、アクティブ化、および有効期限の日付は変更不可能を使用して取り消すことができますが、`IKeyManager`です。 さらに、その基になるアルゴリズム情報、マスター_キー生成情報、および rest プロパティでの暗号化も変更できません。

これらの変更はありません、キーを生成するのいずれかに明示的な呼び出しによって次の時間まで有効に移動する、開発者は、キーの永続性に影響する設定を変更する場合`IKeyManager.CreateNewKey`またはシステム経由で、データ保護の独自[自動キー生成](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)動作します。 キーの永続性に影響する設定は次のとおりです。

* [既定キーの有効期間](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Rest メカニズムでキーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [キーに含まれるアルゴリズム情報](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

をこれらの設定、次の自動キーのローリング時間よりも前に開始する必要がある場合は、明示的な呼び出しを行うことを検討してください`IKeyManager.CreateNewKey`に新しいキーの作成を強制します。 明示的なライセンス認証日を指定してください ({今すぐ + 2 日間} は、目安変更が反映されるまでの時間を許可する) と呼び出しの有効期限。

>[!TIP]
> リポジトリと接触しているすべてのアプリケーションで、同じ設定を指定する必要があります、`IDataProtectionBuilder`拡張メソッド。 それ以外の場合、永続化されたキーのプロパティは、キーの生成のルーチンを呼び出す特定のアプリケーションに依存するされます。
