---
title: 目的の階層と ASP.NET Core でのマルチ テナント
author: rick-anderson
description: ASP.NET Core データ保護 Api に関連する、目的の文字列の階層とマルチ テナント機能について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/10/2018
ms.locfileid: "41830227"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>目的の階層と ASP.NET Core でのマルチ テナント

以降、`IDataProtector`は暗黙的にも、`IDataProtectionProvider`目的を連結することができます。 この意味で`provider.CreateProtector([ "purpose1", "purpose2" ])`と等価`provider.CreateProtector("purpose1").CreateProtector("purpose2")`します。

これにより、データ保護システムを通じていくつか興味深い階層リレーションシップ。 前の例で[Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)、SecureMessage コンポーネントを呼び出すことができます`provider.CreateProtector("Contoso.Messaging.SecureMessage")`1 回前払いプライベートに結果をキャッシュおよび`_myProvider`フィールド。 今後の保護機能への呼び出しを使用して作成できます`_myProvider.CreateProtector("User: username")`、されこれらの保護機能は、個々 のメッセージを保護するために使用されます。

これを反転もできます。 どのホストに独自の認証と状態管理システムで構成できます (妥当なは、CMS よう) 複数のテナントとテナントごとの 1 つの論理アプリケーションを検討してください。 包括的なアプリケーションが 1 つのマスター プロバイダーと呼び出す`provider.CreateProtector("Tenant 1")`と`provider.CreateProtector("Tenant 2")`各テナントに独自の分離スライスのデータ保護システムを提供します。 テナントが独自のニーズに基づいて個々 の保護機能が独自派生させることしますが、どの程度難しいしようとすると関係なくことはできませんを作成するプロテクターが競合する、他のテナントで、システムで。 視覚的に、これは次のようです。

![マルチ テナント機能の目的](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> 包括的なアプリケーション制御を個々 のテナント Api で利用およびテナントが、サーバー上の任意のコードを実行できないことを前提です。 かどうか、テナントが任意のコードを実行できる、分離の保証を中断するプライベート リフレクションを実行する可能性がありますまたはマスター_キー生成情報を直接読み取るし、どのようなサブキーの派生する可能性がありますがだけが必要です。

データ保護システムは、実際には既定のボックスの構成でのマルチ テナント機能の一種を使用します。 既定では、マスター_キー生成情報は、ワーカー プロセス アカウントのユーザー プロファイル フォルダー (または、IIS アプリケーション プール id のレジストリ) に格納されます。 実際には、単一のアカウントを使用して、複数のアプリケーションを実行する非常に一般的ないて、これらすべてのアプリケーションがマスター_キー生成情報を共有しまうためです。 これを解決するには、データ保護システムは自動的に、全体的な目的のチェーンの最初の要素として、アプリケーションあたり一意の識別子を挿入します。 この暗黙的な目的では[個々 のアプリケーションの分離](xref:security/data-protection/configuration/overview#per-application-isolation)から、システム、および保護機能の作成プロセス内で一意のテナントは、上の図と同じ外観と各アプリケーションを効果的に扱うことにより別です。
