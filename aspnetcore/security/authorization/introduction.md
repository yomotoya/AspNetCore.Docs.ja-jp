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
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="a858a-103">ASP.NET Core での承認の概要</span><span class="sxs-lookup"><span data-stu-id="a858a-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="a858a-104">承認とは、ユーザーが何を実行できるかを決定するプロセスのことです。</span><span class="sxs-lookup"><span data-stu-id="a858a-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="a858a-105">例として、管理者ユーザーはドキュメントライブラリーの作成、ドキュメントの追加、ドキュメントの編集、およびそれらを削除することが許可されています。</span><span class="sxs-lookup"><span data-stu-id="a858a-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="a858a-106">管理者ではないユーザーは、ドキュメントを読む権限しかありません。</span><span class="sxs-lookup"><span data-stu-id="a858a-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="a858a-107">承認は直交性があり、認証から独立しています。</span><span class="sxs-lookup"><span data-stu-id="a858a-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="a858a-108">しかし、承認には認証のメカニズムが必須です。</span><span class="sxs-lookup"><span data-stu-id="a858a-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="a858a-109">認証は、ユーザーが誰であるかを確認するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="a858a-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="a858a-110">認証は、現在のユーザーに対して1つ以上のIDを作成します。</span><span class="sxs-lookup"><span data-stu-id="a858a-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="a858a-111">承認の種類</span><span class="sxs-lookup"><span data-stu-id="a858a-111">Authorization types</span></span>

<span data-ttu-id="a858a-112">ASP.NET Core の承認は、シンプルで宣言的な[ロールベース](xref:security/authorization/roles)と、リッチな[ポリシーベース](xref:security/authorization/policies)のモデルが提供されています。</span><span class="sxs-lookup"><span data-stu-id="a858a-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="a858a-113">承認は、要件の中で表され、ハンドラーが要件に対するユーザーのクレームを評価します。</span><span class="sxs-lookup"><span data-stu-id="a858a-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="a858a-114">命令的なチェックはシンプルなポリシー、または、ユーザーのIDとユーザーがアクセスしようとするリソースのプロパティの両方を評価するポリシーに基づくことができます。</span><span class="sxs-lookup"><span data-stu-id="a858a-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="a858a-115">名前空間</span><span class="sxs-lookup"><span data-stu-id="a858a-115">Namespaces</span></span>

<span data-ttu-id="a858a-116">`AuthorizeAttribute` および `AllowAnonymousAttribute` 属性を含む承認のコンポーネントは、 `Microsoft.AspNetCore.Authorization` 名前空間にあります。</span><span class="sxs-lookup"><span data-stu-id="a858a-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="a858a-117">[シンプルな承認](xref:security/authorization/simple)をご参照ください。</span><span class="sxs-lookup"><span data-stu-id="a858a-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
