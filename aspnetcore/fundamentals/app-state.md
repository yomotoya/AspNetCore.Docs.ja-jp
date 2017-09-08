---
title: "ASP.NET Core でのセッションおよびアプリケーションの状態"
author: rick-anderson
description: "要求間で維持アプリケーションとユーザー (セッション) 状態にアプローチです。"
keywords: "アプリケーションの状態、セッション状態、クエリ文字列での ASP.NET Core の投稿します。"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bdc93b2c06b7f0314b5bf49e0e3ea5aa3c1eb3cf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="c18a9-104">ASP.NET Core でのセッションおよびアプリケーションの状態の概要</span><span class="sxs-lookup"><span data-stu-id="c18a9-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="c18a9-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](http://ardalis.com)、および[Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="c18a9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](http://ardalis.com), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="c18a9-106">HTTP は、ステートレス プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="c18a9-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="c18a9-107">Web サーバーでは、独立した要求として、各 HTTP 要求を処理し、以前の要求からユーザーの値を保持しません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-107">A  web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="c18a9-108">この記事では、アプリケーションと要求間でセッション状態を保持するためにさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="c18a9-109">セッションの状態</span><span class="sxs-lookup"><span data-stu-id="c18a9-109">Session state</span></span>

<span data-ttu-id="c18a9-110">セッション状態は、保存し、ユーザーが web アプリを参照中にユーザー データの格納に使用できる ASP.NET Core の機能です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="c18a9-111">サーバー上のディクショナリまたはハッシュ テーブルで構成される、セッション状態は、ブラウザーからの要求間でデータを保持します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="c18a9-112">セッション データは、キャッシュによってバックアップされます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="c18a9-113">ASP.NET Core は、クライアントには各要求を使用してサーバーに送信されたセッション ID が含まれた cookie を提供することにより、セッション状態を維持します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="c18a9-114">サーバーは、セッション データをフェッチするのにセッション ID を使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="c18a9-115">セッション cookie はブラウザーに固有であるためのブラウザーでセッションを共有することはできません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="c18a9-116">ブラウザー セッションの終了時にのみ、セッション cookie が削除されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="c18a9-117">期限切れのセッションの cookie を受信すると、同じセッションの cookie を使用する新しいセッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="c18a9-118">サーバーは、最後の要求の後に限られた時間のセッションを保持します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="c18a9-119">セッションのタイムアウトを設定するか、20 分間の既定値を使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="c18a9-120">セッション状態は、特定のセッションに固有でが完全に永続化する必要はありませんユーザー データの格納に最適です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="c18a9-121">呼び出すか、データをバッキング ストアから削除`Session.Clear`データ ストアに、セッションが期限切れにするか。</span><span class="sxs-lookup"><span data-stu-id="c18a9-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="c18a9-122">サーバーは、ブラウザーを閉じているときに、またはセッションの cookie が削除されたときに認識されません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="c18a9-123">セッションでは、機密データを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="c18a9-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="c18a9-124">クライアント可能性がありますいないブラウザーを閉じて、セッションの cookie をクリア (および一部のブラウザーが windows の間でセッション cookie を維持する。)。</span><span class="sxs-lookup"><span data-stu-id="c18a9-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="c18a9-125">また、セッションできない可能性があります。 1 人のユーザーに制限次のユーザーは、同じセッションと続ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="c18a9-126">メモリ内のセッション プロバイダーは、ローカル サーバー上のセッション データを格納します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="c18a9-127">サーバー ファームで web アプリを実行する場合は、特定のサーバーには、各セッションを関連付けるにスティッキー セッションを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="c18a9-128">Windows Azure Web サイトのプラットフォームでは、スティッキー セッション アプリケーション要求ルーティング処理 (ARR) が既定値です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="c18a9-129">ただし、スティッキー セッションはスケーラビリティに影響を与えるし、web アプリの更新プログラムが複雑になることができます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="c18a9-130">良いオプションは、Redis を使用する、またはスティッキー セッションを必要としないが、SQL Server の分散をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="c18a9-131">詳細については、次を参照してください。[分散キャッシュを使用して作業](xref:performance/caching/distributed)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="c18a9-132">サービス プロバイダーの設定の詳細については、「[構成セッション](#configuring-session)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<span data-ttu-id="c18a9-133">このセクションの残りの部分では、ユーザー データを格納するためのオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-133">The remainder of this section describes the options for storing user data.</span></span>

<a name="temp"></a>
### <a name="tempdata"></a><span data-ttu-id="c18a9-134">TempData</span><span class="sxs-lookup"><span data-stu-id="c18a9-134">TempData</span></span>

<span data-ttu-id="c18a9-135">ASP.NET Core MVC を公開、 [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData)プロパティを[コント ローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-135">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="c18a9-136">このプロパティは、読み取りが可能になるまで、データを格納します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-136">This property stores data until it is read.</span></span> <span data-ttu-id="c18a9-137">`Keep`と`Peek`メソッドは、データが削除の確認に使用できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-137">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c18a9-138">`TempData`1 つの要求より多くのデータが必要なときにこのプロパティの値はリダイレクト、特に便利です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-138">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="c18a9-139">`TempData`セッション状態の上に作成されています。</span><span class="sxs-lookup"><span data-stu-id="c18a9-139">`TempData` is built on top of session state.</span></span> 

## <a name="cookie-based-tempdata-provider"></a><span data-ttu-id="c18a9-140">Cookie ベース TempData プロバイダー</span><span class="sxs-lookup"><span data-stu-id="c18a9-140">Cookie-based TempData provider</span></span> 

<span data-ttu-id="c18a9-141">ASP.NET Core 1.1 以降では、cookie にユーザーの TempData を格納するのに、クッキー ベース TempData プロバイダーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-141">In ASP.NET Core 1.1 and higher, you can use the cookie-based TempData provider to store a user's TempData in a cookie.</span></span> <span data-ttu-id="c18a9-142">Cookie ベースの TempData プロバイダーを有効にするには登録、`CookieTempDataProvider`サービス`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c18a9-142">To enable the  cookie-based TempData provider, register the `CookieTempDataProvider` service in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

<span data-ttu-id="c18a9-143">Cookie のデータがでエンコードされた、 [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-143">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span></span> <span data-ttu-id="c18a9-144">Cookie が暗号化されており、チャンク、ため、1 つの cookie サイズの制限は適用されません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-144">Because the cookie is encrypted and chunked, the single cookie size limit does not apply.</span></span> <span data-ttu-id="c18a9-145">Cookie のデータが圧縮されていないため、暗号化されたデータの圧縮が問題につながるセキュリティなど、 [CRIME](https://en.wikipedia.org/wiki/CRIME_(security_exploit))と[侵害](https://en.wikipedia.org/wiki/BREACH_(security_exploit))攻撃です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-145">The cookie data is not compressed, because compressing encryped data can lead to security problems such as the [CRIME](https://en.wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://en.wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="c18a9-146">Cookie ベースの TempData プロバイダーの詳細については、次を参照してください。 [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-146">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

### <a name="query-strings"></a><span data-ttu-id="c18a9-147">クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="c18a9-147">Query strings</span></span>

<span data-ttu-id="c18a9-148">新しい要求のクエリ文字列に追加して 1 つの要求からで、限られた量のデータを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-148">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="c18a9-149">これは、電子メールやソーシャル ネットワークを介して共有する埋め込みの状態を持つリンクを許可する永続的な方法で状態をキャプチャするために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-149">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="c18a9-150">ただし、このためを使用しないでクエリ文字列機密性の高いデータ。</span><span class="sxs-lookup"><span data-stu-id="c18a9-150">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="c18a9-151">簡単に共有されているだけでなくクエリ文字列にデータを含めることができます作成の営業案件[クロスサイト リクエスト フォージェリ (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))攻撃は、ユーザーが認証中に悪意のあるサイトにアクセスするを騙してことができます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-151">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="c18a9-152">攻撃者は、アプリからユーザー データを盗む、またはユーザーの代理として、悪意のあるアクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-152">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="c18a9-153">保持されているアプリケーションまたはセッション状態は、CSRF 攻撃から保護する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-153">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="c18a9-154">CSRF 攻撃の詳細については、次を参照してください。 [ASP.NET Core で防止サイト間で要求の偽造防止 (XSRF/CSRF) 攻撃](../security/anti-request-forgery.md)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-154">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

### <a name="post-data-and-hidden-fields"></a><span data-ttu-id="c18a9-155">Post データと非表示フィールド</span><span class="sxs-lookup"><span data-stu-id="c18a9-155">Post data and hidden fields</span></span>

<span data-ttu-id="c18a9-156">データは、隠しフォーム フィールドに保存されているし、次の要求にポストされたことができます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-156">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="c18a9-157">これは、複数ページのフォームで共通です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-157">This is common in multipage forms.</span></span> <span data-ttu-id="c18a9-158">ただし、クライアントは、データを改ざんする可能性があります、ため、サーバー必要があります常に再検証します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-158">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

### <a name="cookies"></a><span data-ttu-id="c18a9-159">クッキー</span><span class="sxs-lookup"><span data-stu-id="c18a9-159">Cookies</span></span>

<span data-ttu-id="c18a9-160">Cookie は、web アプリケーションでユーザーに固有のデータを格納する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-160">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="c18a9-161">要求ごとに cookie が送信されるため、サイズを最小限に抑える必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-161">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="c18a9-162">理想的には、識別子のみは、サーバーに格納されている実際のデータを cookie に保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-162">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="c18a9-163">ほとんどのブラウザーは、cookie を 4096 バイトに制限します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-163">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="c18a9-164">さらに、限定された数の cookie のみが各ドメインで使用できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-164">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="c18a9-165">クッキーは、改ざんされる可能性がありますが、ためには、サーバーで検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-165">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="c18a9-166">クライアントでクッキーの持続性は、ユーザーが操作の対象と有効期限は、クライアント上のデータの持続性の最も持続性のある形式では通常。</span><span class="sxs-lookup"><span data-stu-id="c18a9-166">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="c18a9-167">Cookie は、多くの場合、既知のユーザーのコンテンツをカスタマイズするパーソナル化の使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-167">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="c18a9-168">ユーザーを識別のみほとんどの場合で認証されていないため、cookie にユーザー名、アカウント名、または (GUID) などの一意のユーザー ID を格納することによって通常 cookie を保護できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-168">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="c18a9-169">サイトのユーザーのパーソナル化インフラストラクチャにアクセスするのに、cookie を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-169">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

### <a name="httpcontextitems"></a><span data-ttu-id="c18a9-170">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="c18a9-170">HttpContext.Items</span></span>

<span data-ttu-id="c18a9-171">`Items`コレクションが格納されるデータに適した場所に必要な 1 つ特定の要求を処理中にのみです。</span><span class="sxs-lookup"><span data-stu-id="c18a9-171">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="c18a9-172">コレクションの内容は、各要求の後に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-172">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="c18a9-173">`Items`コレクションを最適な使用のコンポーネントまたはミドルウェア手段としてを通信時に、要求でさまざまなポイントで動作があり、パラメーターを渡す直接的な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-173">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="c18a9-174">詳細については、次を参照してください。 [HttpContext.Items 扱う](#working-with-httpcontextitems)、この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-174">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

### <a name="cache"></a><span data-ttu-id="c18a9-175">キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c18a9-175">Cache</span></span>

<span data-ttu-id="c18a9-176">キャッシュは、格納およびデータを取得する効率的な方法です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-176">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="c18a9-177">時間と他の考慮事項に基づいて、キャッシュされた項目の有効期間を制御できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-177">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="c18a9-178">詳細については[キャッシュ](../performance/caching/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-178">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name=session></a>

## <a name="configuring-session"></a><span data-ttu-id="c18a9-179">セッションの構成</span><span class="sxs-lookup"><span data-stu-id="c18a9-179">Configuring Session</span></span>

<span data-ttu-id="c18a9-180">`Microsoft.AspNetCore.Session`パッケージは、セッション状態を管理するためのミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-180">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="c18a9-181">セッションのミドルウェアを有効にする`Startup`含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-181">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="c18a9-182">いずれか、 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)メモリ キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-182">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="c18a9-183">`IDistributedCache`実装は、セッションのバッキング ストアとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-183">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="c18a9-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)呼び出すには、NuGet パッケージ"Microsoft.AspNetCore.Session"する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="c18a9-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="c18a9-186">次のコードでは、メモリ内のセッション プロバイダーを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-186">The following code shows how to set up the in-memory session provider.</span></span>

<span data-ttu-id="c18a9-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span><span class="sxs-lookup"><span data-stu-id="c18a9-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span></span>

<span data-ttu-id="c18a9-188">セッションを参照する`HttpContext`がインストールされ構成されているとします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-188">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="c18a9-189">アクセスしようとする`Session`する前に`UseSession`が呼び出されると、例外`InvalidOperationException: Session has not been configured for this application or request`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-189">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="c18a9-190">新しいを作成しようとすると`Session`(つまり、セッションの cookie が作成されていません) への書き込みを開始した後、`Response`ストリーム、例外`InvalidOperationException: The session cannot be established after the response has started`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-190">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="c18a9-191">Web サーバー ログには、例外が見つかりませんブラウザーに表示されません。</span><span class="sxs-lookup"><span data-stu-id="c18a9-191">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="c18a9-192">セッションを非同期的に読み込む</span><span class="sxs-lookup"><span data-stu-id="c18a9-192">Loading Session asynchronously</span></span> 

<span data-ttu-id="c18a9-193">ASP.NET Core の既定のセッション プロバイダーが、基になるからセッションのレコードを読み込みます[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)ストア場合に非同期的にのみ、 [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)メソッドが明示的にする前に呼び出されます `TryGetValue`、 `Set`、または`Remove`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c18a9-193">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="c18a9-194">場合`LoadAsync`は呼び出されません最初に、基になるセッション レコードが同期的に読み込まれると、アプリの標準化機能する可能性のある影響を与える可能性です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-194">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="c18a9-195">このパターンを適用するアプリケーションを表示するには、ラップ、 [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore)と[DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)例外をスローするバージョンでの実装、`LoadAsync`メソッドではありません前に呼び出される`TryGetValue`、 `Set`、または`Remove`です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-195">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="c18a9-196">ラップされたバージョンをサービス コンテナーに登録します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-196">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="c18a9-197">実装の詳細</span><span class="sxs-lookup"><span data-stu-id="c18a9-197">Implementation Details</span></span>

<span data-ttu-id="c18a9-198">セッションは、追跡し、1 つのブラウザーからの要求を特定する cookie を使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-198">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="c18a9-199">既定では、この cookie が名前付き"です。パスを使用して AspNet.Session"、「/」です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-199">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="c18a9-200">Cookie の既定値は、ドメインを指定しないので、利用できるがないクライアント側スクリプトを作成 ページで (ため`CookieHttpOnly`の既定値は`true`)。</span><span class="sxs-lookup"><span data-stu-id="c18a9-200">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="c18a9-201">セッションの既定値をオーバーライドする`SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="c18a9-201">To override session defaults, use `SessionOptions`:</span></span>

<span data-ttu-id="c18a9-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span><span class="sxs-lookup"><span data-stu-id="c18a9-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span></span>

<span data-ttu-id="c18a9-203">サーバーを使用して、`IdleTimeout`セッションできる期間アイドル状態の内容を破棄する前に決定するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c18a9-203">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="c18a9-204">このプロパティは、cookie の有効期限の独立しました。</span><span class="sxs-lookup"><span data-stu-id="c18a9-204">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="c18a9-205">セッションのミドルウェア (読み取りまたはに書き込まれます) を通過する各要求では、タイムアウト値をリセットします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-205">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="c18a9-206">`Session`は*ロックしない*両方で、最後の 1 つのセッションの内容の変更を試みる次の 2 つの要求が 1 つをオーバーライドする場合、します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-206">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="c18a9-207">`Session`として実装された、*一貫性のあるセッション*、つまり、すべての内容が一緒に格納されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-207">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="c18a9-208">セッション (異なるキー) のさまざまな部分を変更している 2 つの要求が互いに影響することがありますもします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-208">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

## <a name="setting-and-getting-session-values"></a><span data-ttu-id="c18a9-209">設定またはセッションの値を取得します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-209">Setting and getting Session values</span></span>

<span data-ttu-id="c18a9-210">セッションを介してへのアクセス、`Session`プロパティ`HttpContext`です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-210">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="c18a9-211">このプロパティは、 [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)実装します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-211">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="c18a9-212">設定または int 型と文字列を取得する次の例を示しています。</span><span class="sxs-lookup"><span data-stu-id="c18a9-212">The following example shows setting and getting an int and a string:</span></span>

<span data-ttu-id="c18a9-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c18a9-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="c18a9-214">次の拡張メソッドを追加する場合は、設定およびセッションにシリアル化可能なオブジェクトを取得できます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-214">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

<span data-ttu-id="c18a9-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span><span class="sxs-lookup"><span data-stu-id="c18a9-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span></span>

<span data-ttu-id="c18a9-216">次の例では、設定し、シリアル化可能なオブジェクトを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-216">The following sample shows how to set and get a serializable object:</span></span>

<span data-ttu-id="c18a9-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c18a9-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span></span>


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="c18a9-218">HttpContext.Items の操作</span><span class="sxs-lookup"><span data-stu-id="c18a9-218">Working with HttpContext.Items</span></span>

<span data-ttu-id="c18a9-219">`HttpContext`抽象化では、型のディクショナリ コレクションのサポート`IDictionary<object, object>`という`Items`です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-219">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="c18a9-220">先頭からこのコレクションは使用可能な*HttpRequest*要求ごとの最後に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-220">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="c18a9-221">キー付きのエントリに値を割り当てるか、特定のキーの値を要求することによってアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-221">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="c18a9-222">以下のサンプルで[ミドルウェア](middleware.md)追加`isVerified`を`Items`コレクション。</span><span class="sxs-lookup"><span data-stu-id="c18a9-222">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="c18a9-223">パイプラインで後では、他のミドルウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-223">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="c18a9-224">1 つのアプリでのみ使用されるミドルウェアの`string`キーは許容されます。</span><span class="sxs-lookup"><span data-stu-id="c18a9-224">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="c18a9-225">ただし、ミドルウェア アプリケーション間で共有されるは、キーの競合の可能性を回避するのに一意のオブジェクトのキーを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-225">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="c18a9-226">複数のアプリケーション間で動作する必要があるミドルウェアを開発している場合は、次のように、ミドルウェア クラスで定義されている一意のオブジェクト キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-226">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="c18a9-227">その他のコードに格納されている値にアクセスできます`HttpContext.Items`ミドルウェア クラスによって公開キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-227">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="c18a9-228">この方法では、コード内の複数の場所の「マジック文字列」の繰り返しを排除することの利点もあります。</span><span class="sxs-lookup"><span data-stu-id="c18a9-228">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name=appstate-errors></a>

## <a name="application-state-data"></a><span data-ttu-id="c18a9-229">アプリケーション状態データ</span><span class="sxs-lookup"><span data-stu-id="c18a9-229">Application state data</span></span>

<span data-ttu-id="c18a9-230">使用して[依存性の注入](xref:fundamentals/dependency-injection)すべてのユーザーにデータを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c18a9-230">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="c18a9-231">データを含むサービスを定義する (たとえば、という名前のクラス`MyAppData`)。</span><span class="sxs-lookup"><span data-stu-id="c18a9-231">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="c18a9-232">サービス クラスを追加`ConfigureServices`(たとえば`services.AddSingleton<MyAppData>();`)。</span><span class="sxs-lookup"><span data-stu-id="c18a9-232">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="c18a9-233">各コント ローラーで、データ サービス クラスを利用します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-233">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="c18a9-234">セッションを使用する場合の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="c18a9-234">Common errors when working with session</span></span>

* <span data-ttu-id="c18a9-235">「'Microsoft.AspNetCore.Session.DistributedSessionStore' をアクティブ化しようとしました。 型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' のサービスを解決するのにはできません。」</span><span class="sxs-lookup"><span data-stu-id="c18a9-235">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="c18a9-236">これは、少なくとも 1 つの構成に失敗により発生通常`IDistributedCache`実装します。</span><span class="sxs-lookup"><span data-stu-id="c18a9-236">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="c18a9-237">詳細については、次を参照してください。[分散キャッシュを使用して作業](xref:performance/caching/distributed)と[メモリ キャッシュに](xref:performance/caching/memory)です。</span><span class="sxs-lookup"><span data-stu-id="c18a9-237">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c18a9-238">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="c18a9-238">Additional Resources</span></span>


* [<span data-ttu-id="c18a9-239">このドキュメントで使用するサンプル コード</span><span class="sxs-lookup"><span data-stu-id="c18a9-239">Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
