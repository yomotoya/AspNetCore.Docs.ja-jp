---
uid: web-api/overview/security/forms-authentication
title: ASP.NET Web API でフォーム認証 |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API でフォーム認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508311"
---
<a name="forms-authentication-in-aspnet-web-api"></a><span data-ttu-id="23352-103">ASP.NET Web API でフォーム認証</span><span class="sxs-lookup"><span data-stu-id="23352-103">Forms Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="23352-104">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="23352-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="23352-105">フォーム認証では、HTML フォームを使用して、ユーザーの資格情報をサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="23352-105">Forms authentication uses an HTML form to send the user's credentials to the server.</span></span> <span data-ttu-id="23352-106">インターネット標準ではありません。</span><span class="sxs-lookup"><span data-stu-id="23352-106">It is not an Internet standard.</span></span> <span data-ttu-id="23352-107">フォーム認証のみに適した web Api、web アプリケーションから呼び出されるユーザーが HTML フォームと対話できるようにします。</span><span class="sxs-lookup"><span data-stu-id="23352-107">Forms authentication is only appropriate for web APIs that are called from a web application, so that the user can interact with the HTML form.</span></span>

| <span data-ttu-id="23352-108">長所</span><span class="sxs-lookup"><span data-stu-id="23352-108">Advantages</span></span> | <span data-ttu-id="23352-109">短所</span><span class="sxs-lookup"><span data-stu-id="23352-109">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="23352-110">実装する簡単: ASP.NET に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="23352-110">- Easy to implement: Built into ASP.NET.</span></span> <span data-ttu-id="23352-111">-ユーザー アカウントを管理しやすく ASP.NET メンバーシップ プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="23352-111">- Uses ASP.NET membership provider, which makes it easy to manage user accounts.</span></span> | <span data-ttu-id="23352-112">-標準 HTTP 認証メカニズムです。標準の承認ヘッダーの代わりに、HTTP クッキーを使用します。</span><span class="sxs-lookup"><span data-stu-id="23352-112">- Not a standard HTTP authentication mechanism; uses HTTP cookies instead of the standard Authorization header.</span></span> <span data-ttu-id="23352-113">-クライアント ブラウザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="23352-113">- Requires a browser client.</span></span> <span data-ttu-id="23352-114">資格情報は、プレーン テキストとして送信されます。</span><span class="sxs-lookup"><span data-stu-id="23352-114">- Credentials are sent as plaintext.</span></span> <span data-ttu-id="23352-115">脆弱性にクロスサイト リクエスト フォージェリ (CSRF) です。アンチ CSRF メジャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="23352-115">- Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span> <span data-ttu-id="23352-116">-Nonbrowser クライアントから使用するが困難です。</span><span class="sxs-lookup"><span data-stu-id="23352-116">- Difficult to use from nonbrowser clients.</span></span> <span data-ttu-id="23352-117">ログインには、ブラウザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="23352-117">Login requires a browser.</span></span> <span data-ttu-id="23352-118">のユーザー資格情報は、要求で送信されます。</span><span class="sxs-lookup"><span data-stu-id="23352-118">- User credentials are sent in the request.</span></span> <span data-ttu-id="23352-119">-一部のユーザーには、クッキーが無効にします。</span><span class="sxs-lookup"><span data-stu-id="23352-119">- Some users disable cookies.</span></span> |

<span data-ttu-id="23352-120">簡単に、asp.net フォーム認証は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="23352-120">Briefly, forms authentication in ASP.NET works like this:</span></span>

1. <span data-ttu-id="23352-121">クライアントは、認証が必要なリソースを要求します。</span><span class="sxs-lookup"><span data-stu-id="23352-121">The client requests a resource that requires authentication.</span></span>
2. <span data-ttu-id="23352-122">ユーザーが認証されていない場合、サーバーは HTTP 302 (検出) を返し、ログイン ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="23352-122">If the user is not authenticated, the server returns HTTP 302 (Found) and redirects to a login page.</span></span>
3. <span data-ttu-id="23352-123">ユーザーは、資格情報を入力し、フォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="23352-123">The user enters credentials and submits the form.</span></span>
4. <span data-ttu-id="23352-124">サーバーでは、元の URI にリダイレクトする別の HTTP 302 を返します。</span><span class="sxs-lookup"><span data-stu-id="23352-124">The server returns another HTTP 302 that redirects back to the original URI.</span></span> <span data-ttu-id="23352-125">この応答には、認証 cookie が含まれています。</span><span class="sxs-lookup"><span data-stu-id="23352-125">This response includes an authentication cookie.</span></span>
5. <span data-ttu-id="23352-126">クライアントは、リソースを再び要求します。</span><span class="sxs-lookup"><span data-stu-id="23352-126">The client requests the resource again.</span></span> <span data-ttu-id="23352-127">要求には、サーバー要求を許可するための認証クッキーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="23352-127">The request includes the authentication cookie, so the server grants the request.</span></span>

![](forms-authentication/_static/image1.png)

<span data-ttu-id="23352-128">詳細については、次を参照してください。 [、フォーム認証の概要です。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)</span><span class="sxs-lookup"><span data-stu-id="23352-128">For more information, see [An Overview of Forms Authentication.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)</span></span>

## <a name="using-forms-authentication-with-web-api"></a><span data-ttu-id="23352-129">フォーム認証を使用して Web API</span><span class="sxs-lookup"><span data-stu-id="23352-129">Using Forms Authentication with Web API</span></span>

<span data-ttu-id="23352-130">フォーム認証を使用するアプリケーションを作成するには、MVC 4 プロジェクト ウィザードで、「インターネット アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="23352-130">To create an application that uses forms authentication, select the "Internet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="23352-131">このテンプレートでは、アカウント管理用の MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="23352-131">This template creates MVC controllers for account management.</span></span> <span data-ttu-id="23352-132">また、ASP.NET Fall 2012 更新プログラムで使用可能な「単一ページ アプリケーション」テンプレートを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="23352-132">You can also use the "Single Page Application" template, available in the ASP.NET Fall 2012 Update.</span></span>

<span data-ttu-id="23352-133">Web API コント ローラーを使用してアクセスを制限することができます、`[Authorize]`属性」の説明に従って[[Authorize] 属性を使用して](authentication-and-authorization-in-aspnet-web-api.md#auth3)です。</span><span class="sxs-lookup"><span data-stu-id="23352-133">In your web API controllers, you can restrict access by using the `[Authorize]` attribute, as described in [Using the [Authorize] Attribute](authentication-and-authorization-in-aspnet-web-api.md#auth3).</span></span>

<span data-ttu-id="23352-134">フォーム認証では、要求の認証に、セッションの cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="23352-134">Forms-authentication uses a session cookie to authenticate requests.</span></span> <span data-ttu-id="23352-135">ブラウザーは、web サイトに関連するすべての cookie を自動的に送信します。</span><span class="sxs-lookup"><span data-stu-id="23352-135">Browsers automatically send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="23352-136">この機能により、フォーム認証のクロスサイト リクエスト フォージェリ (CSRF) 攻撃を参照する可能性のある脆弱な[防止クロスサイト リクエスト フォージェリ (CSRF) 攻撃](preventing-cross-site-request-forgery-csrf-attacks.md)です。</span><span class="sxs-lookup"><span data-stu-id="23352-136">This feature makes forms authentication potentially vulnerable to cross-site request forgery (CSRF) attacks See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

<span data-ttu-id="23352-137">フォーム認証では、ユーザーの資格情報は暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="23352-137">Forms authentication does not encrypt the user's credentials.</span></span> <span data-ttu-id="23352-138">したがって、フォーム認証は SSL と共に使用しない限り、セキュリティで保護されたではありません。</span><span class="sxs-lookup"><span data-stu-id="23352-138">Therefore, forms authentication is not secure unless used with SSL.</span></span> <span data-ttu-id="23352-139">参照してください[Web API での SSL の操作](working-with-ssl-in-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="23352-139">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>
