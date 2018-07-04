---
uid: web-api/overview/security/forms-authentication
title: ASP.NET Web API でフォーム認証 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API でフォーム認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365072"
---
<a name="forms-authentication-in-aspnet-web-api"></a><span data-ttu-id="2f0c6-103">ASP.NET Web API でフォーム認証</span><span class="sxs-lookup"><span data-stu-id="2f0c6-103">Forms Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2f0c6-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2f0c6-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2f0c6-105">フォーム認証では、HTML フォームを使用して、ユーザーの資格情報をサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-105">Forms authentication uses an HTML form to send the user's credentials to the server.</span></span> <span data-ttu-id="2f0c6-106">インターネット標準ではありません。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-106">It is not an Internet standard.</span></span> <span data-ttu-id="2f0c6-107">ユーザーが HTML フォームを操作できるように、フォーム認証は web、web アプリケーションから呼び出される Api の適切なのみ。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-107">Forms authentication is only appropriate for web APIs that are called from a web application, so that the user can interact with the HTML form.</span></span>

| <span data-ttu-id="2f0c6-108">長所</span><span class="sxs-lookup"><span data-stu-id="2f0c6-108">Advantages</span></span> | <span data-ttu-id="2f0c6-109">短所</span><span class="sxs-lookup"><span data-stu-id="2f0c6-109">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="2f0c6-110">実装が容易: ASP.NET に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-110">- Easy to implement: Built into ASP.NET.</span></span> <span data-ttu-id="2f0c6-111">-ASP.NET メンバーシップ プロバイダーは、簡単にユーザー アカウントの管理を使用します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-111">- Uses ASP.NET membership provider, which makes it easy to manage user accounts.</span></span> | <span data-ttu-id="2f0c6-112">-非標準の HTTP 認証機構です。標準の Authorization ヘッダーではなく、HTTP クッキーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-112">- Not a standard HTTP authentication mechanism; uses HTTP cookies instead of the standard Authorization header.</span></span> <span data-ttu-id="2f0c6-113">-クライアントのブラウザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-113">- Requires a browser client.</span></span> <span data-ttu-id="2f0c6-114">-資格情報は、プレーン テキストとして送信されます。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-114">- Credentials are sent as plaintext.</span></span> <span data-ttu-id="2f0c6-115">クロスサイト リクエスト フォージェリ (CSRF); に対して脆弱CSRF 対策メジャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-115">- Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span> <span data-ttu-id="2f0c6-116">-Nonbrowser クライアントから使用するは困難です。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-116">- Difficult to use from nonbrowser clients.</span></span> <span data-ttu-id="2f0c6-117">ログインには、ブラウザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-117">Login requires a browser.</span></span> <span data-ttu-id="2f0c6-118">のユーザー資格情報は、要求で送信されます。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-118">- User credentials are sent in the request.</span></span> <span data-ttu-id="2f0c6-119">-一部のユーザーは、cookie を無効にします。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-119">- Some users disable cookies.</span></span> |

<span data-ttu-id="2f0c6-120">について簡単に、asp.net フォーム認証は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-120">Briefly, forms authentication in ASP.NET works like this:</span></span>

1. <span data-ttu-id="2f0c6-121">クライアントは、認証が必要なリソースを要求します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-121">The client requests a resource that requires authentication.</span></span>
2. <span data-ttu-id="2f0c6-122">ユーザーが認証されていない場合、サーバーは HTTP 302 (検出) を返し、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-122">If the user is not authenticated, the server returns HTTP 302 (Found) and redirects to a login page.</span></span>
3. <span data-ttu-id="2f0c6-123">ユーザーは資格情報を入力し、フォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-123">The user enters credentials and submits the form.</span></span>
4. <span data-ttu-id="2f0c6-124">サーバーは、元の URI にリダイレクトするもう 1 つの HTTP 302 を返します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-124">The server returns another HTTP 302 that redirects back to the original URI.</span></span> <span data-ttu-id="2f0c6-125">この応答には、認証 cookie が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-125">This response includes an authentication cookie.</span></span>
5. <span data-ttu-id="2f0c6-126">クライアントは、もう一度リソースを要求します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-126">The client requests the resource again.</span></span> <span data-ttu-id="2f0c6-127">要求には、サーバー要求を許可するために、認証 cookie が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-127">The request includes the authentication cookie, so the server grants the request.</span></span>

![](forms-authentication/_static/image1.png)

<span data-ttu-id="2f0c6-128">詳細については、次を参照してください。 [、フォーム認証の概要。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2f0c6-128">For more information, see [An Overview of Forms Authentication.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)</span></span>

## <a name="using-forms-authentication-with-web-api"></a><span data-ttu-id="2f0c6-129">フォーム認証を使用して Web API</span><span class="sxs-lookup"><span data-stu-id="2f0c6-129">Using Forms Authentication with Web API</span></span>

<span data-ttu-id="2f0c6-130">フォーム認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「インターネット アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-130">To create an application that uses forms authentication, select the "Internet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="2f0c6-131">このテンプレートは、アカウント管理のための MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-131">This template creates MVC controllers for account management.</span></span> <span data-ttu-id="2f0c6-132">ASP.NET の Fall 2012 更新プログラムで使用できる「シングル ページ アプリケーション」テンプレートを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-132">You can also use the "Single Page Application" template, available in the ASP.NET Fall 2012 Update.</span></span>

<span data-ttu-id="2f0c6-133">使用してアクセスを制限する、web API コント ローラーで、`[Authorize]`属性」の説明に従って[[Authorize] 属性を使用して](authentication-and-authorization-in-aspnet-web-api.md#auth3)します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-133">In your web API controllers, you can restrict access by using the `[Authorize]` attribute, as described in [Using the [Authorize] Attribute](authentication-and-authorization-in-aspnet-web-api.md#auth3).</span></span>

<span data-ttu-id="2f0c6-134">フォーム認証では、セッションの cookie を使用して、要求を認証します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-134">Forms-authentication uses a session cookie to authenticate requests.</span></span> <span data-ttu-id="2f0c6-135">ブラウザーは、web サイトに自動的にすべての関連クッキーを送信します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-135">Browsers automatically send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="2f0c6-136">この機能により、フォーム認証のクロスサイト リクエスト フォージェリ (CSRF) 攻撃を参照する可能性のある脆弱性のある[防止クロスサイト リクエスト フォージェリ (CSRF) 攻撃](preventing-cross-site-request-forgery-csrf-attacks.md)します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-136">This feature makes forms authentication potentially vulnerable to cross-site request forgery (CSRF) attacks See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

<span data-ttu-id="2f0c6-137">フォーム認証では、ユーザーの資格情報は暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-137">Forms authentication does not encrypt the user's credentials.</span></span> <span data-ttu-id="2f0c6-138">そのため、フォーム認証と SSL を使用しない場合は、安全ではありません。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-138">Therefore, forms authentication is not secure unless used with SSL.</span></span> <span data-ttu-id="2f0c6-139">参照してください[Web API の SSL の使用](working-with-ssl-in-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="2f0c6-139">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>
