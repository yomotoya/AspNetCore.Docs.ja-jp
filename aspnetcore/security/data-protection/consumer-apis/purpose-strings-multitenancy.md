---
title: "ASP.NET Core で目的の文字列"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9d18c287-e0e6-4541-b09c-7fed45c902d9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: dd87d8bcaf0056b322908e9a3ef75678f603e1e6
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>目的の階層と ASP.NET Core におけるマルチ テナント機能

IDataProtector も暗黙的に、IDataProtectionProvider であるために、目的に連結することができます。 この意味プロバイダー。CreateProtector (["purpose1"、"purpose2"]) は、プロバイダーと同じです。CreateProtector("purpose1") です。CreateProtector("purpose2") です。

これにより、データ保護システムを通じていくつか興味深い階層リレーションシップです。 先ほどの例の[Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose)、SecureMessage コンポーネントは、プロバイダーを呼び出すことができます。CreateProtector("Contoso.Messaging.SecureMessage") したらアップ フロントとプライベート _myProvider フィールドに結果をキャッシュします。 _MyProvider.CreateProtector への呼び出しを使用して、将来の保護機能を作成し、ことができます ("ユーザー: ユーザー名")、これらの保護機能が個々 のメッセージを保護するため使用されます。

これを反転もできます。 独自の認証および状態管理システム (CMS 必然的に) 複数のテナント、および各テナントを構成できますホストの 1 つの論理アプリケーションを検討してください。 包括的なアプリケーションが 1 つのマスター プロバイダーとプロバイダーを呼び出します。CreateProtector (以下"Tenant 1") とプロバイダー。CreateProtector ("Tenant 2") に、データ保護システムの分離された独自スライスを各テナントに付与します。 テナントは独自のニーズに基づく独自個々 の保護機能を派生し、でしたとどの程度厳密に関係なく、作成はできません衝突の保護機能、他のテナントでシステムで。 視覚的に下として表されます。

![マルチ テナント機能の目的](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> 包括的なアプリケーションを制御する Api は、個々 のテナントを使用し、テナントが、サーバー上の任意のコードを実行できないことを前提です。 テナントが任意のコードを実行できる、分離の保証を中断するプライベート リフレクションを実行する可能性がありますかマスター_キー生成情報を直接読み取るし、どのようなサブキーを派生する可能性がありますがだけ状況が該当します。

データ保護システムは、既定の既定の構成におけるマルチ テナント機能の並べ替えを実際に使用します。 既定では、マスター_キー生成情報は、ワーカー プロセス アカウントのユーザー プロファイル フォルダー (または IIS アプリケーション プール id のレジストリ、) に格納されます。 実際には、単一のアカウントを使用して、複数のアプリケーションを実行する非常に一般的なマスター_キー生成情報の共有をこれらすべてのアプリケーションが終了するため。 これを解決するには、データ保護システムは、全体的な目的のチェーンの最初の要素としてごとのアプリケーション固有の識別子を自動的に挿入されます。 この暗黙的な目的の機能を[個々 のアプリケーションの分離](../configuration/overview.md#data-protection-configuration-per-app-isolation)上の図と同じで、システム、および保護機能の作成プロセス内で一意のテナントとして各アプリケーションを効果的に扱うことで別のです。
