---
title: ASP.NET Core での承認の概要
author: rick-anderson
description: 承認と ASP.NET Core アプリでの承認のしくみの基礎について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896979"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core での承認の概要

<a name="security-authorization-introduction"></a>

承認アクションを決定するプロセスとは、ユーザーが操作を実行できます。 たとえば、ドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する管理ユーザーが許可されています。 管理者以外のユーザーが、ライブラリの操作はドキュメントの読み取り権限のみ。

承認は、認証から独立して直交してです。 ただし、承認には、認証メカニズムが必要です。 認証は、ユーザーがだれかを確認するプロセスです。 認証は、現在のユーザーの 1 つまたは複数の id を作成できます。

## <a name="authorization-types"></a>承認の種類

ASP.NET Core の承認は、単純な宣言型[ロール](xref:security/authorization/roles)と豊富な[ポリシーに基づく](xref:security/authorization/policies)モデル。 承認は、要件で表され、ハンドラーは、要件に照らして、ユーザーの要求を評価します。 命令型のチェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。

## <a name="namespaces"></a>名前空間

承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`内の属性にある、`Microsoft.AspNetCore.Authorization`名前空間。

上のドキュメントを参照して[単純な承認](xref:security/authorization/simple)します。
