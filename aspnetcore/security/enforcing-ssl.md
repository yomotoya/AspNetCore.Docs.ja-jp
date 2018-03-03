---
title: "ASP.NET Core アプリケーションで HTTPS を適用します。"
author: rick-anderson
description: "Web アプリで ASP.NET Core HTTPS/TLS を必要とする方法を示します。"
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="f8158-103">ASP.NET Core アプリケーションで HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="f8158-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="f8158-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8158-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8158-105">このドキュメントで説明する方法。</span><span class="sxs-lookup"><span data-stu-id="f8158-105">This document shows how to:</span></span>

- <span data-ttu-id="f8158-106">すべての要求には HTTPS が必要です。</span><span class="sxs-lookup"><span data-stu-id="f8158-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="f8158-107">すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f8158-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="f8158-108">操作を行います**いない**使用`RequireHttpsAttribute`機密情報を受信する Web Api にします。</span><span class="sxs-lookup"><span data-stu-id="f8158-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="f8158-109">`RequireHttpsAttribute` HTTP ステータス コードを使用して、HTTP から HTTPS へのブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f8158-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="f8158-110">API クライアントの理解または HTTP から HTTPS へのリダイレクトに従うことがありますできません。</span><span class="sxs-lookup"><span data-stu-id="f8158-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="f8158-111">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="f8158-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="f8158-112">Web Api には、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f8158-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="f8158-113">HTTP をリッスンしません。</span><span class="sxs-lookup"><span data-stu-id="f8158-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="f8158-114">ステータス コード 400 (Bad Request) との接続を閉じていない要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="f8158-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="f8158-115">HTTPS が必要</span><span class="sxs-lookup"><span data-stu-id="f8158-115">Require HTTPS</span></span>

<span data-ttu-id="f8158-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS を要求するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f8158-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="f8158-117">`[RequireHttpsAttribute]` 装飾できるは、メソッドまたはコント ローラーまたはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="f8158-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="f8158-118">属性をグローバルに適用するには、次のコードを追加`ConfigureServices`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="f8158-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="f8158-119">前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="f8158-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="f8158-120">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f8158-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="f8158-121">詳細については、次を参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。</span><span class="sxs-lookup"><span data-stu-id="f8158-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="f8158-122">HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="f8158-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="f8158-123">適用する、`[RequireHttps]`属性をすべてのコント ローラー/Razor ページされていないグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="f8158-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="f8158-124">保証できません、`[RequireHttps]`属性は、新しいコント ローラーおよび Razor ページが追加されたときに適用します。</span><span class="sxs-lookup"><span data-stu-id="f8158-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>