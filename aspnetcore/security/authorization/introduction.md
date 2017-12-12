---
title: "承認の概要"
author: rick-anderson
description: "このドキュメントでは、承認の基本的な説明し、承認が ASP.NET Core に関連付ける方法について説明します。"
keywords: "ASP.NET Core、承認"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a>はじめに

<a name="security-authorization-introduction"></a>

承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。 たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。 管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。

承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。 認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。

## <a name="authorization-types"></a>承認の種類

ASP.NET Core 承認は、単純な宣言型を提供[ロール](roles.md)と[豊富なポリシーに基づいた](policies.md)モデル。 承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。 命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。

## <a name="namespaces"></a>名前空間

承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。
