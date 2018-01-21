---
title: "承認の概要"
author: rick-anderson
description: "このドキュメントでは、承認の基本的な説明し、承認が ASP.NET Core に関連付ける方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 6f4f1fb4f2776db10a1640049885e31e9a54011a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction"></a><span data-ttu-id="5e054-103">はじめに</span><span class="sxs-lookup"><span data-stu-id="5e054-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="5e054-104">承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="5e054-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="5e054-105">たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。</span><span class="sxs-lookup"><span data-stu-id="5e054-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="5e054-106">管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。</span><span class="sxs-lookup"><span data-stu-id="5e054-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="5e054-107">承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。</span><span class="sxs-lookup"><span data-stu-id="5e054-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="5e054-108">認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5e054-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="5e054-109">承認の種類</span><span class="sxs-lookup"><span data-stu-id="5e054-109">Authorization Types</span></span>

<span data-ttu-id="5e054-110">ASP.NET Core 承認は、単純な宣言型を提供[ロール](roles.md)と[豊富なポリシーに基づいた](policies.md)モデル。</span><span class="sxs-lookup"><span data-stu-id="5e054-110">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="5e054-111">承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="5e054-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="5e054-112">命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。</span><span class="sxs-lookup"><span data-stu-id="5e054-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="5e054-113">名前空間</span><span class="sxs-lookup"><span data-stu-id="5e054-113">Namespaces</span></span>

<span data-ttu-id="5e054-114">承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。</span><span class="sxs-lookup"><span data-stu-id="5e054-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
