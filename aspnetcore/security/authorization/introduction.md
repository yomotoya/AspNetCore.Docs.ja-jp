---
title: "承認の概要"
author: rick-anderson
description: "このドキュメントでは、承認の基本的な説明し、承認が ASP.NET Core に関連付ける方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>はじめに

<a name="security-authorization-introduction"></a>

承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。 たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。 管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。

承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。 認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。

## <a name="authorization-types"></a>承認の種類

ASP.NET Core 承認は、単純な宣言型[ロール](roles.md)およびリッチ[ポリシーに基づく](policies.md)モデル。 承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。 命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。

## <a name="namespaces"></a>名前空間

承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`、属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。

ドキュメントを参照してください[単純な承認](xref:security/authorization/simple)です。
