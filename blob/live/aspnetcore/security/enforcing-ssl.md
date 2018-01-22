---
title: "ASP.NET Core アプリケーションで SSL を適用します。"
author: rick-anderson
description: "Web アプリの ASP.NET Core での SSL を要求する方法を示します"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="5bdd5-103">ASP.NET Core アプリケーションで SSL を適用します。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="5bdd5-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5bdd5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5bdd5-105">このドキュメントで説明する方法。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-105">This document shows how to:</span></span>

- <span data-ttu-id="5bdd5-106">(HTTPS 要求のみ) のすべての要求に対して SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="5bdd5-107">すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="5bdd5-108">SSL を必須にする</span><span class="sxs-lookup"><span data-stu-id="5bdd5-108">Require SSL</span></span>

<span data-ttu-id="5bdd5-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL を要求するために使用します。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="5bdd5-110">コント ローラーまたはこの属性を持つメソッドを装飾することができますか、次のようにグローバルに適用できます。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="5bdd5-111">次のコードを追加`ConfigureServices`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="5bdd5-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="5bdd5-112">上記の強調表示されたコードでは、すべての要求を使用して必要があります`HTTPS`、したがって HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="5bdd5-113">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="5bdd5-114">参照してください[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="5bdd5-115">HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="5bdd5-116">適用する、`[RequireHttps]`属性をすべてのコント ローラーがグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされない。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="5bdd5-117">保証できない場合、アプリに追加された新しいコント ローラーを忘れずに適用、`[RequireHttps]`属性。</span><span class="sxs-lookup"><span data-stu-id="5bdd5-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
