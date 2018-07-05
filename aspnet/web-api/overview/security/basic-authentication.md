---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API の基本的な認証 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で基本認証の使用について説明します。
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829588"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="c0e48-103">ASP.NET Web API の基本認証</span><span class="sxs-lookup"><span data-stu-id="c0e48-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c0e48-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c0e48-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c0e48-105">基本認証が定義されている[RFC 2617、HTTP 認証: 基本認証とダイジェスト アクセス認証](http://www.ietf.org/rfc/rfc2617.txt)します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="c0e48-106">短所</span><span class="sxs-lookup"><span data-stu-id="c0e48-106">Disadvantages</span></span>

- <span data-ttu-id="c0e48-107">ユーザーの資格情報は、要求で送信されます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="c0e48-108">資格情報は、プレーン テキストとして送信されます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="c0e48-109">資格情報は、すべての要求で送信されます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="c0e48-110">ログアウトする方法がない以外のブラウザー セッションを終了します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="c0e48-111">クロスサイト リクエスト フォージェリ (CSRF); に対して脆弱になりますCSRF 対策メジャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="c0e48-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="c0e48-112">長所</span><span class="sxs-lookup"><span data-stu-id="c0e48-112">Advantages</span></span>

- <span data-ttu-id="c0e48-113">インターネットの標準です。</span><span class="sxs-lookup"><span data-stu-id="c0e48-113">Internet standard.</span></span>
- <span data-ttu-id="c0e48-114">すべての主要ブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c0e48-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="c0e48-115">比較的単純なプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="c0e48-115">Relatively simple protocol.</span></span>

<span data-ttu-id="c0e48-116">基本認証はように動作します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="c0e48-117">要求に認証が必要な場合、サーバーは 401 (Unauthorized) を返します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="c0e48-118">応答には、サーバーが基本認証をサポートしていることを示す WWW 認証ヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0e48-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="c0e48-119">クライアントは、Authorization ヘッダーでクライアント資格情報を使って、別の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="c0e48-120">資格情報は、「名前: パスワード」, base64 でエンコードされた文字列としてフォーマットされます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="c0e48-121">資格情報は暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="c0e48-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="c0e48-122">基本認証は、「領域」のコンテキスト内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="c0e48-123">サーバーには、WWW 認証ヘッダー内の領域の名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0e48-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="c0e48-124">ユーザーの資格情報は、その領域内で有効です。</span><span class="sxs-lookup"><span data-stu-id="c0e48-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="c0e48-125">レルムの正確なスコープは、サーバーによって定義されます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="c0e48-126">たとえば、パーティション リソースへの順序でいくつかの領域を定義する場合があります。</span><span class="sxs-lookup"><span data-stu-id="c0e48-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="c0e48-127">資格情報は暗号化されずに送信されて、ため、基本認証が HTTPS 経由でセキュリティで保護されただけです。</span><span class="sxs-lookup"><span data-stu-id="c0e48-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="c0e48-128">参照してください[Web API の SSL の使用](working-with-ssl-in-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="c0e48-129">基本認証は CSRF 攻撃に対する脆弱性もあります。</span><span class="sxs-lookup"><span data-stu-id="c0e48-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="c0e48-130">ユーザーの資格情報を入力すると、ブラウザーに自動的に送信に後続の要求では同じドメインに、セッションの期間の。</span><span class="sxs-lookup"><span data-stu-id="c0e48-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="c0e48-131">これには、AJAX 要求が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-131">This includes AJAX requests.</span></span> <span data-ttu-id="c0e48-132">参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="c0e48-133">IIS を使用した基本認証</span><span class="sxs-lookup"><span data-stu-id="c0e48-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="c0e48-134">IIS 基本認証をサポートしていますが、警告がある: ユーザーが自分の Windows 資格情報に対して認証します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="c0e48-135">つまり、ユーザーでは、サーバーのドメインにアカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="c0e48-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="c0e48-136">パブリックに公開された web サイトでは、通常、ASP.NET メンバーシップ プロバイダーに対する認証にします。</span><span class="sxs-lookup"><span data-stu-id="c0e48-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="c0e48-137">IIS を使用して基本認証を有効にするには、ASP.NET プロジェクトの Web.config の"Windows"を認証モードを設定します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="c0e48-138">このモードでは、IIS は、認証に Windows 資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="c0e48-139">さらに、IIS での基本認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0e48-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="c0e48-140">IIS マネージャーで、機能ビューに移動するには、認証 を選択および基本認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c0e48-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="c0e48-141">Web API プロジェクトで、追加、`[Authorize]`認証を必要とする任意のコント ローラー アクションの属性。</span><span class="sxs-lookup"><span data-stu-id="c0e48-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="c0e48-142">クライアントは、要求に Authorization ヘッダーを設定して自身を認証します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="c0e48-143">ブラウザー クライアントでは、自動的にこの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="c0e48-144">Nonbrowser クライアントは、ヘッダーを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0e48-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="c0e48-145">カスタムのメンバシップを使用した基本認証</span><span class="sxs-lookup"><span data-stu-id="c0e48-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="c0e48-146">前述のように、IIS に組み込まれている基本認証は、Windows 資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="c0e48-147">ホスティング サーバー上のユーザー用のアカウントを作成する必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="c0e48-148">インターネット アプリケーションのユーザー アカウントは通常外部データベースに格納します。</span><span class="sxs-lookup"><span data-stu-id="c0e48-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="c0e48-149">次のコードでどのように基本認証を実行する HTTP モジュールです。</span><span class="sxs-lookup"><span data-stu-id="c0e48-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="c0e48-150">簡単に置き換えることで、ASP.NET メンバーシップ プロバイダーにプラグインできる、`CheckPassword`メソッドで、この例ではダミー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c0e48-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="c0e48-151">Web API 2 での書き込みをする必要があります、[認証フィルター](authentication-filters.md)または[OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/index.md)、HTTP モジュールの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="c0e48-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="c0e48-152">HTTP モジュールを有効にするには、web.config ファイルに、次を追加、 **system.webServer**セクション。</span><span class="sxs-lookup"><span data-stu-id="c0e48-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="c0e48-153">「アセンブリ名」("dll"拡張子を含まない) アセンブリの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c0e48-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="c0e48-154">フォームまたは Windows 認証などの他の認証スキームが無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0e48-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
