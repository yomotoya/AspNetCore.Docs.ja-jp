---
title: "はじめに"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: fa6dcbc0627181cd1aca0926fa008f3db907742f
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="introduction"></a><span data-ttu-id="e994d-103">はじめに</span><span class="sxs-lookup"><span data-stu-id="e994d-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="e994d-104">承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="e994d-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="e994d-105">たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。</span><span class="sxs-lookup"><span data-stu-id="e994d-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="e994d-106">管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。</span><span class="sxs-lookup"><span data-stu-id="e994d-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="e994d-107">承認は、認証は、これは、ユーザーがだれを突き止めるのプロセスから独立しており、直角です。</span><span class="sxs-lookup"><span data-stu-id="e994d-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="e994d-108">認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e994d-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="e994d-109">承認の種類</span><span class="sxs-lookup"><span data-stu-id="e994d-109">Authorization Types</span></span>

<span data-ttu-id="e994d-110">ASP.NET Core 承認は、単純な宣言型を提供[ロール](roles.md#security-authorization-role-based)と[豊富なポリシーに基づいた](policies.md#security-authorization-policies-based)モデル。</span><span class="sxs-lookup"><span data-stu-id="e994d-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="e994d-111">承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="e994d-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="e994d-112">命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。</span><span class="sxs-lookup"><span data-stu-id="e994d-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="e994d-113">名前空間</span><span class="sxs-lookup"><span data-stu-id="e994d-113">Namespaces</span></span>

<span data-ttu-id="e994d-114">承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。</span><span class="sxs-lookup"><span data-stu-id="e994d-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
