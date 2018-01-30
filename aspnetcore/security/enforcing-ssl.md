---
title: "ASP.NET Core アプリケーションで SSL を適用します。"
author: rick-anderson
description: "Web アプリの ASP.NET Core での SSL を要求する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2e0a2f4732e574c80ceef8fd21a530a11aef254c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="47a94-103">ASP.NET Core アプリケーションで SSL を適用します。</span><span class="sxs-lookup"><span data-stu-id="47a94-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="47a94-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47a94-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47a94-105">このドキュメントで説明する方法。</span><span class="sxs-lookup"><span data-stu-id="47a94-105">This document shows how to:</span></span>

- <span data-ttu-id="47a94-106">(HTTPS 要求のみ) のすべての要求に対して SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="47a94-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="47a94-107">すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="47a94-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="47a94-108">SSL を必須にする</span><span class="sxs-lookup"><span data-stu-id="47a94-108">Require SSL</span></span>

<span data-ttu-id="47a94-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL を要求するために使用します。</span><span class="sxs-lookup"><span data-stu-id="47a94-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="47a94-110">コント ローラーまたはこの属性を持つメソッドを装飾することができますか、次のようにグローバルに適用できます。</span><span class="sxs-lookup"><span data-stu-id="47a94-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="47a94-111">次のコードを追加`ConfigureServices`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="47a94-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="47a94-112">上記の強調表示されたコードでは、すべての要求を使用して必要があります`HTTPS`、したがって HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="47a94-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="47a94-113">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="47a94-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="47a94-114">参照してください[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="47a94-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="47a94-115">HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="47a94-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="47a94-116">適用する、`[RequireHttps]`属性すべてのコント ローラーをグローバルに HTTPS を必要とすると安全はありません。</span><span class="sxs-lookup"><span data-stu-id="47a94-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="47a94-117">保証できない場合、アプリに追加された新しいコント ローラーを忘れずに適用、`[RequireHttps]`属性。</span><span class="sxs-lookup"><span data-stu-id="47a94-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
