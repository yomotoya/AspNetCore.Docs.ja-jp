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
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="c23f9-103">ASP.NET Core での承認の概要</span><span class="sxs-lookup"><span data-stu-id="c23f9-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="c23f9-104">承認とは、アクションを決定するプロセス、ユーザーが操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="c23f9-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="c23f9-105">たとえば、管理ユーザーはドキュメント ライブラリの作成、ドキュメントを追加、ドキュメントの編集、およびそれらを削除する許可します。</span><span class="sxs-lookup"><span data-stu-id="c23f9-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="c23f9-106">管理者以外のユーザーが、ライブラリの操作は、ドキュメントを参照する権限がのみ。</span><span class="sxs-lookup"><span data-stu-id="c23f9-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="c23f9-107">承認は、認証から独立しており、直角です。</span><span class="sxs-lookup"><span data-stu-id="c23f9-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="c23f9-108">ただし、承認には、認証メカニズムが必要です。</span><span class="sxs-lookup"><span data-stu-id="c23f9-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="c23f9-109">認証は、よっては、ユーザーのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="c23f9-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="c23f9-110">認証は、現在のユーザーの 1 つまたは複数の id を作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c23f9-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="c23f9-111">承認の種類</span><span class="sxs-lookup"><span data-stu-id="c23f9-111">Authorization types</span></span>

<span data-ttu-id="c23f9-112">ASP.NET Core 承認は、単純な宣言型[ロール](xref:security/authorization/roles)およびリッチ[ポリシーに基づく](xref:security/authorization/policies)モデル。</span><span class="sxs-lookup"><span data-stu-id="c23f9-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="c23f9-113">承認は、要件で表され、ハンドラーが要件に照らして、ユーザーの要求を評価します。</span><span class="sxs-lookup"><span data-stu-id="c23f9-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="c23f9-114">命令型チェックは、単純なポリシーまたはユーザー id と、ユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づいて作成できます。</span><span class="sxs-lookup"><span data-stu-id="c23f9-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="c23f9-115">名前空間</span><span class="sxs-lookup"><span data-stu-id="c23f9-115">Namespaces</span></span>

<span data-ttu-id="c23f9-116">承認コンポーネントを含む、`AuthorizeAttribute`と`AllowAnonymousAttribute`、属性が見つかりましたが、`Microsoft.AspNetCore.Authorization`名前空間。</span><span class="sxs-lookup"><span data-stu-id="c23f9-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="c23f9-117">ドキュメントを参照してください[単純な承認](xref:security/authorization/simple)です。</span><span class="sxs-lookup"><span data-stu-id="c23f9-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
