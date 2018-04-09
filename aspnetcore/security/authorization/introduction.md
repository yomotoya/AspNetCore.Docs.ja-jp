---
title: ASP.NET Core での承認の概要
author: rick-anderson
description: 認証と承認が ASP.NET Core アプリケーションでどのように動作するかの基本について説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 7ba1966dcaf1ce0510f489cfe0ff11501faffa56
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core での承認の概要

<a name="security-authorization-introduction"></a>

承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。 たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。 管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。

承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。 認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。

## <a name="authorization-types"></a>承認の種類

ASP.NET Core 承認は、単純な宣言型[ロール](xref:security/authorization/roles)およびリッチ[ポリシーに基づく](xref:security/authorization/policies)モデル。 承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。 命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。

## <a name="namespaces"></a>名前空間

承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`、属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。

ドキュメントを参照してください[単純な承認](xref:security/authorization/simple)です。
