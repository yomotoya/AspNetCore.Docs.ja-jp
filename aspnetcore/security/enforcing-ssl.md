---
title: "ASP.NET Core アプリケーションで SSL を適用します。"
author: rick-anderson
description: "Web アプリの ASP.NET Core での SSL を要求する方法を示します"
keywords: "ASP.NET Core、SSL、HTTPS、RequireHttpsAttribute、IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="d646b-104">ASP.NET Core アプリケーションで SSL を適用します。</span><span class="sxs-lookup"><span data-stu-id="d646b-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="d646b-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d646b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d646b-106">このドキュメントで説明する方法。</span><span class="sxs-lookup"><span data-stu-id="d646b-106">This document shows how to:</span></span>

- <span data-ttu-id="d646b-107">(HTTPS 要求のみ) のすべての要求に対して SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="d646b-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="d646b-108">すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d646b-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="d646b-109">SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="d646b-109">Require SSL</span></span>

<span data-ttu-id="d646b-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL を要求するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d646b-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="d646b-111">コント ローラーまたはこの属性を持つメソッドを装飾することができますか、次のようにグローバルに適用できます。</span><span class="sxs-lookup"><span data-stu-id="d646b-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="d646b-112">次のコードを追加`ConfigureServices`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="d646b-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="d646b-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="d646b-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span></span>

<span data-ttu-id="d646b-114">上記の強調表示されたコードでは、すべての要求を使用して必要があります`HTTPS`、したがって HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="d646b-114">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="d646b-115">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d646b-115">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="d646b-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span><span class="sxs-lookup"><span data-stu-id="d646b-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span></span>

<span data-ttu-id="d646b-117">参照してください[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d646b-117">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="d646b-118">HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="d646b-118">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="d646b-119">適用する、`[RequireHttps]`属性をすべてのコント ローラーがグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされない。</span><span class="sxs-lookup"><span data-stu-id="d646b-119">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="d646b-120">保証できない場合、アプリに追加された新しいコント ローラーを忘れずに適用、`[RequireHttps]`属性。</span><span class="sxs-lookup"><span data-stu-id="d646b-120">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="d646b-121">SSL または HTTPS の IIS Express を設定します。</span><span class="sxs-lookup"><span data-stu-id="d646b-121">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="d646b-122">参照してください[ASP.NET Core での開発の HTTPS を設定](xref:security/https#iisxpress)です。</span><span class="sxs-lookup"><span data-stu-id="d646b-122">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
