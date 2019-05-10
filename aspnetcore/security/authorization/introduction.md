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
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="e927b-103">ASP.NET Core での承認の概要</span><span class="sxs-lookup"><span data-stu-id="e927b-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="e927b-104">承認アクションを決定するプロセスとは、ユーザーが操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="e927b-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="e927b-105">たとえば、ドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する管理ユーザーが許可されています。</span><span class="sxs-lookup"><span data-stu-id="e927b-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="e927b-106">管理者以外のユーザーが、ライブラリの操作はドキュメントの読み取り権限のみ。</span><span class="sxs-lookup"><span data-stu-id="e927b-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="e927b-107">承認は、認証から独立して直交してです。</span><span class="sxs-lookup"><span data-stu-id="e927b-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="e927b-108">ただし、承認には、認証メカニズムが必要です。</span><span class="sxs-lookup"><span data-stu-id="e927b-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="e927b-109">認証は、ユーザーがだれかを確認するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="e927b-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="e927b-110">認証は、現在のユーザーの 1 つまたは複数の id を作成できます。</span><span class="sxs-lookup"><span data-stu-id="e927b-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="e927b-111">承認の種類</span><span class="sxs-lookup"><span data-stu-id="e927b-111">Authorization types</span></span>

<span data-ttu-id="e927b-112">ASP.NET Core の承認は、単純な宣言型[ロール](xref:security/authorization/roles)と豊富な[ポリシーに基づく](xref:security/authorization/policies)モデル。</span><span class="sxs-lookup"><span data-stu-id="e927b-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="e927b-113">承認は、要件で表され、ハンドラーは、要件に照らして、ユーザーの要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="e927b-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="e927b-114">命令型のチェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。</span><span class="sxs-lookup"><span data-stu-id="e927b-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="e927b-115">名前空間</span><span class="sxs-lookup"><span data-stu-id="e927b-115">Namespaces</span></span>

<span data-ttu-id="e927b-116">承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`内の属性にある、`Microsoft.AspNetCore.Authorization`名前空間。</span><span class="sxs-lookup"><span data-stu-id="e927b-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="e927b-117">上のドキュメントを参照して[単純な承認](xref:security/authorization/simple)します。</span><span class="sxs-lookup"><span data-stu-id="e927b-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
