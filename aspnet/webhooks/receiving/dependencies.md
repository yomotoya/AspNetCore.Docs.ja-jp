---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook レシーバー依存関係 |Microsoft Docs
author: rick-anderson
description: 受信側の依存関係と ASP.NET の Webhook の依存関係挿入します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401186"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="4db39-103">ASP.NET Webhook レシーバー依存関係</span><span class="sxs-lookup"><span data-stu-id="4db39-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="4db39-104">Microsoft ASP.NET Webhook は依存関係の挿入を考慮して設計されています。</span><span class="sxs-lookup"><span data-stu-id="4db39-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="4db39-105">システムのほとんどの依存関係は、依存関係のインジェクション エンジンを使用して別の実装で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="4db39-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="4db39-106">参照してください[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)レシーバー依存関係の一覧についてはします。</span><span class="sxs-lookup"><span data-stu-id="4db39-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="4db39-107">依存関係が登録されていない場合は、既定の実装が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4db39-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="4db39-108">参照してください[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)に対して一連の既定の実装。</span><span class="sxs-lookup"><span data-stu-id="4db39-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
