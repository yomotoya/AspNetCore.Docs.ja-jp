---
uid: webhooks/receiving/dependencies
title: ASP.NET の Webhook の受信者の依存関係 |Microsoft ドキュメント
author: rick-anderson
description: 受信者の依存関係と ASP.NET の Webhook の依存関係の挿入します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529911"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="c65af-103">ASP.NET の Webhook の受信者の依存関係</span><span class="sxs-lookup"><span data-stu-id="c65af-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="c65af-104">依存関係の挿入を念頭には、Microsoft ASP.NET の Webhook は設計されています。</span><span class="sxs-lookup"><span data-stu-id="c65af-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="c65af-105">システムの依存関係のほとんどは、依存関係の挿入エンジンを使用して別の実装で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="c65af-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="c65af-106">参照してください[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)受信機の依存関係の一覧についてはします。</span><span class="sxs-lookup"><span data-stu-id="c65af-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="c65af-107">依存関係が登録されていない場合は、既定の実装が使用されます。</span><span class="sxs-lookup"><span data-stu-id="c65af-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="c65af-108">参照してください[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)既定の実装の一覧についてはします。</span><span class="sxs-lookup"><span data-stu-id="c65af-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
