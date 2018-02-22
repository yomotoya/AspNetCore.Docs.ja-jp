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
# <a name="introduction"></a><span data-ttu-id="f9792-103">はじめに</span><span class="sxs-lookup"><span data-stu-id="f9792-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="f9792-104">承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f9792-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="f9792-105">たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。</span><span class="sxs-lookup"><span data-stu-id="f9792-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="f9792-106">管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。</span><span class="sxs-lookup"><span data-stu-id="f9792-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="f9792-107">承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。</span><span class="sxs-lookup"><span data-stu-id="f9792-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="f9792-108">認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f9792-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="f9792-109">承認の種類</span><span class="sxs-lookup"><span data-stu-id="f9792-109">Authorization types</span></span>

<span data-ttu-id="f9792-110">ASP.NET Core 承認は、単純な宣言型[ロール](roles.md)およびリッチ[ポリシーに基づく](policies.md)モデル。</span><span class="sxs-lookup"><span data-stu-id="f9792-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="f9792-111">承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="f9792-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="f9792-112">命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。</span><span class="sxs-lookup"><span data-stu-id="f9792-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="f9792-113">名前空間</span><span class="sxs-lookup"><span data-stu-id="f9792-113">Namespaces</span></span>

<span data-ttu-id="f9792-114">承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`、属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。</span><span class="sxs-lookup"><span data-stu-id="f9792-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="f9792-115">ドキュメントを参照してください[単純な承認](xref:security/authorization/simple)です。</span><span class="sxs-lookup"><span data-stu-id="f9792-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
