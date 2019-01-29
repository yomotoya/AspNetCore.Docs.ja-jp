---
title: ASP.NET Core でのセッションとアプリの状態
author: rick-anderson
description: 異なる要求の間でセッションとアプリの状態を維持する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: a510e4f49e158203dd7c5e1e0bd28472541f7925
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836338"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="73442-103">ASP.NET Core でのセッションとアプリの状態</span><span class="sxs-lookup"><span data-stu-id="73442-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="73442-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="73442-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="73442-105">HTTP はステートレス プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="73442-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="73442-106">手順を追加しないと、HTTP 要求は独立したメッセージであり、ユーザーの値やアプリの状態は保持されません。</span><span class="sxs-lookup"><span data-stu-id="73442-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="73442-107">この記事では、要求と要求の間でユーザー データとアプリの状態を保持するための複数の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="73442-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="73442-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="73442-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="73442-109">状態管理</span><span class="sxs-lookup"><span data-stu-id="73442-109">State management</span></span>

<span data-ttu-id="73442-110">状態は、いくつかの方法で格納することができます。</span><span class="sxs-lookup"><span data-stu-id="73442-110">State can be stored using several approaches.</span></span> <span data-ttu-id="73442-111">このトピックではそれぞれの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="73442-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="73442-112">格納の方法</span><span class="sxs-lookup"><span data-stu-id="73442-112">Storage approach</span></span> | <span data-ttu-id="73442-113">格納のメカニズム</span><span class="sxs-lookup"><span data-stu-id="73442-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="73442-114">Cookie</span><span class="sxs-lookup"><span data-stu-id="73442-114">Cookies</span></span>](#cookies) | <span data-ttu-id="73442-115">HTTP Cookie (サーバー側アプリのコードを使って格納されるデータを含む場合があります)</span><span class="sxs-lookup"><span data-stu-id="73442-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="73442-116">セッション状態</span><span class="sxs-lookup"><span data-stu-id="73442-116">Session state</span></span>](#session-state) | <span data-ttu-id="73442-117">HTTP Cookie とサーバー側アプリ コード</span><span class="sxs-lookup"><span data-stu-id="73442-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="73442-118">TempData</span><span class="sxs-lookup"><span data-stu-id="73442-118">TempData</span></span>](#tempdata) | <span data-ttu-id="73442-119">HTTP Cookie またはセッション状態</span><span class="sxs-lookup"><span data-stu-id="73442-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="73442-120">クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="73442-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="73442-121">HTTP クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="73442-121">HTTP query strings</span></span> |
| [<span data-ttu-id="73442-122">非表示フィールド</span><span class="sxs-lookup"><span data-stu-id="73442-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="73442-123">HTTP フォーム フィールド</span><span class="sxs-lookup"><span data-stu-id="73442-123">HTTP form fields</span></span> |
| [<span data-ttu-id="73442-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="73442-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="73442-125">サーバー側アプリ コード</span><span class="sxs-lookup"><span data-stu-id="73442-125">Server-side app code</span></span> |
| [<span data-ttu-id="73442-126">キャッシュ</span><span class="sxs-lookup"><span data-stu-id="73442-126">Cache</span></span>](#cache) | <span data-ttu-id="73442-127">サーバー側アプリ コード</span><span class="sxs-lookup"><span data-stu-id="73442-127">Server-side app code</span></span> |
| [<span data-ttu-id="73442-128">依存性の注入</span><span class="sxs-lookup"><span data-stu-id="73442-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="73442-129">サーバー側アプリ コード</span><span class="sxs-lookup"><span data-stu-id="73442-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="73442-130">クッキー</span><span class="sxs-lookup"><span data-stu-id="73442-130">Cookies</span></span>

<span data-ttu-id="73442-131">Cookie は、要求と要求の間でデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="73442-131">Cookies store data across requests.</span></span> <span data-ttu-id="73442-132">Cookie は要求ごとに送信されるために、そのサイズは最小に抑える必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="73442-133">理想的には、識別子だけを Cookie に格納し、データはアプリで格納します。</span><span class="sxs-lookup"><span data-stu-id="73442-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="73442-134">ほとんどのブラウザーで Cookie のサイズは 4096 バイトに制限されています。</span><span class="sxs-lookup"><span data-stu-id="73442-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="73442-135">ドメインごとに使用できる Cookie の数も制限されています。</span><span class="sxs-lookup"><span data-stu-id="73442-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="73442-136">Cookie は改ざんされる可能性があるため、アプリで検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="73442-137">Cookie は、ユーザーが削除でき、クライアント上で期限切れになります。</span><span class="sxs-lookup"><span data-stu-id="73442-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="73442-138">ただし、Cookie は一般に、クライアントでデータを永続化するときに最も持続性のある形式です。</span><span class="sxs-lookup"><span data-stu-id="73442-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="73442-139">Cookie は、多くの場合、パーソナル化に利用されます。既知のユーザーのためにコンテンツをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="73442-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="73442-140">ユーザーは識別されるだけであり、ほとんどの場合は認証されません。</span><span class="sxs-lookup"><span data-stu-id="73442-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="73442-141">Cookie には、ユーザーの名前、アカウント名、または一意ユーザー ID (GUID など) を格納できます。</span><span class="sxs-lookup"><span data-stu-id="73442-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="73442-142">その後は、優先される Web サイトの背景色など、ユーザーの個人用設定に Cookie を使ってアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="73442-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="73442-143">Cookie を発行し、プライバシーの問題を扱うときは、[欧州連合の一般データ保護規則 (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) に留意してください。</span><span class="sxs-lookup"><span data-stu-id="73442-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="73442-144">詳細については、「[General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr)」(ASP.NET Core での一般データ保護規則 (GDPR) のサポート) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="73442-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="73442-145">セッション状態</span><span class="sxs-lookup"><span data-stu-id="73442-145">Session state</span></span>

<span data-ttu-id="73442-146">セッション状態は、ユーザーが Web アプリを参照している期間中ユーザー データを格納するための ASP.NET Core のシナリオです。</span><span class="sxs-lookup"><span data-stu-id="73442-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="73442-147">セッション状態では、アプリで管理されているストアを使用して、クライアントからの要求間でデータを保持します。</span><span class="sxs-lookup"><span data-stu-id="73442-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="73442-148">セッション データはキャッシュによってバックアップされ、一時的なデータと見なされます。セッション データがなくても、サイトは機能し続けられる必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span> <span data-ttu-id="73442-149">重要なアプリケーションのデータはユーザー データベースに格納し、パフォーマンスの最適化としてのみセッションでキャッシュする必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-149">Critical application data should be stored in the user database and cached in session only as a performance optimization.</span></span>

> [!NOTE]
> <span data-ttu-id="73442-150">[SignalR Hub](xref:signalr/hubs) は HTTP コンテキストとは独立して実行する可能性があるため、[SignalR](xref:signalr/index) アプリではセッションはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="73442-150">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="73442-151">このようなことは、たとえば、長いポーリング要求が HTTP コンテキストの有効期間を超えてハブによって開かれている場合に発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="73442-151">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="73442-152">ASP.NET Core は、セッション ID を含む Cookie をクライアントに提供することで、セッションの状態を維持します。Cookie は要求ごとにサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="73442-152">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="73442-153">アプリは、セッション ID を使用してセッション データをフェッチします。</span><span class="sxs-lookup"><span data-stu-id="73442-153">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="73442-154">セッション状態は次の動作を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-154">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="73442-155">セッション Cookie はブラウザーに固有であるため、セッションはブラウザー間では共有されません。</span><span class="sxs-lookup"><span data-stu-id="73442-155">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="73442-156">セッション Cookie は、ブラウザー セッションが終了するときに削除されます。</span><span class="sxs-lookup"><span data-stu-id="73442-156">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="73442-157">Cookie を受け取り、セッションが期限切れになった場合、同じセッション Cookie を使用する新しいセッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="73442-157">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="73442-158">空のセッションは保持されません。要求と要求の間でセッションを維持するには、少なくとも 1 つの値をセッションが持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-158">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="73442-159">セッションが保持されないと、新しい要求ごとに新しいセッション ID が生成されます。</span><span class="sxs-lookup"><span data-stu-id="73442-159">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="73442-160">アプリは、最後の要求から限られた時間だけセッションを維持します。</span><span class="sxs-lookup"><span data-stu-id="73442-160">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="73442-161">アプリでは、セッション タイムアウトを設定するか、既定値の 20 分を使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-161">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="73442-162">セッション状態は、特定のセッションに固有であるが、セッション間で永続的に保持する必要のないユーザー データの格納に最適です。</span><span class="sxs-lookup"><span data-stu-id="73442-162">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="73442-163">セッション データは、[ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) の実装が呼び出されるか、セッションが期限切れになると、削除されます。</span><span class="sxs-lookup"><span data-stu-id="73442-163">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="73442-164">クライアント ブラウザーが閉じられたこと、またはクライアントでセッション Cookie が削除されるか期限切れになったことを、アプリ コードに通知する既定のメカニズムはありません。</span><span class="sxs-lookup"><span data-stu-id="73442-164">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>
<span data-ttu-id="73442-165">ASP.NET Core MVC と Razor ページのテンプレートには、[一般データ保護規制 (GDPR) のサポート](xref:security/gdpr)が含まれます。</span><span class="sxs-lookup"><span data-stu-id="73442-165">The ASP.NET Core MVC and Razor pages templates include support for [General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span> <span data-ttu-id="73442-166">[セッション状態 Cookie は不可欠ではありません](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential)。追跡が無効になっている場合は、セッション状態は機能しません。</span><span class="sxs-lookup"><span data-stu-id="73442-166">[Session state cookies aren't essential](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), session state isn't functional when tracking is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="73442-167">セッション状態には機密データを保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="73442-167">Don't store sensitive data in session state.</span></span> <span data-ttu-id="73442-168">ユーザーがブラウザーを閉じず、セッション Cookie がクリアされない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="73442-168">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="73442-169">一部のブラウザーでは、ブラウザー ウィンドウの間で有効なセッションの Cookie が維持されます。</span><span class="sxs-lookup"><span data-stu-id="73442-169">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="73442-170">セッションが 1 人のユーザーに制限されず、次のユーザーが同じセッション Cookie でアプリの閲覧を続けることがあります。</span><span class="sxs-lookup"><span data-stu-id="73442-170">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="73442-171">メモリ内キャッシュ プロバイダーは、アプリが存在するサーバーのメモリにセッション データを格納します。</span><span class="sxs-lookup"><span data-stu-id="73442-171">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="73442-172">サーバー ファームのシナリオでは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="73442-172">In a server farm scenario:</span></span>

* <span data-ttu-id="73442-173">"*固定セッション*" を使用して、個々のサーバー上の特定のアプリのインスタンスに、各セッションを結び付けます。</span><span class="sxs-lookup"><span data-stu-id="73442-173">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="73442-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) は[アプリケーション要求ルーティング処理 (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) を使って、既定で固定セッションを強制的に使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="73442-175">ただし、固定セッションは拡張性に影響を与え、Web アプリの更新を複雑にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="73442-175">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="73442-176">もっとよい方法は、Redis または SQL Server の分散キャッシュを使用することで、固定セッションを必要としません。</span><span class="sxs-lookup"><span data-stu-id="73442-176">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="73442-177">詳細については、「<xref:performance/caching/distributed>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73442-177">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="73442-178">セッション Cookie は [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) によって暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="73442-178">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="73442-179">各コンピューターでセッション Cookie を読み取るには、データ保護を適切に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-179">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="73442-180">詳細については、<xref:security/data-protection/introduction> および[キー ストレージ プロバイダー](xref:security/data-protection/implementation/key-storage-providers)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="73442-180">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="73442-181">セッション状態を構成する</span><span class="sxs-lookup"><span data-stu-id="73442-181">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73442-182">[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) に含まれる [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) パッケージは、セッション状態を管理するためのミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="73442-182">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="73442-183">セッション ミドルウェアを有効にするには、`Startup` に次が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-183">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73442-184">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) パッケージは、セッション状態を管理するためのミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="73442-184">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="73442-185">セッション ミドルウェアを有効にするには、`Startup` に次が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-185">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="73442-186">いずれかの [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) メモリ キャッシュ。</span><span class="sxs-lookup"><span data-stu-id="73442-186">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="73442-187">`IDistributedCache` 実装はセッションのバックアップ ストアとして利用されます。</span><span class="sxs-lookup"><span data-stu-id="73442-187">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="73442-188">詳細については、「<xref:performance/caching/distributed>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73442-188">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="73442-189">`ConfigureServices` での [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) の呼び出し。</span><span class="sxs-lookup"><span data-stu-id="73442-189">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="73442-190">`Configure` での [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) の呼び出し。</span><span class="sxs-lookup"><span data-stu-id="73442-190">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="73442-191">次のコードでは、`IDistributedCache` の既定のメモリ内実装でメモリ内セッション プロバイダーを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-191">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="73442-192">ミドルウェアの順序が重要です。</span><span class="sxs-lookup"><span data-stu-id="73442-192">The order of middleware is important.</span></span> <span data-ttu-id="73442-193">上の例では、`UseMvc` の後で `UseSession` を呼び出すと、`InvalidOperationException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="73442-193">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="73442-194">詳細については、[ミドルウェアの順序](xref:fundamentals/middleware/index#order)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="73442-194">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="73442-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) は、セッション状態を構成した後で使用できます。</span><span class="sxs-lookup"><span data-stu-id="73442-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="73442-196">`UseSession` を呼び出前に `HttpContext.Session` にアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="73442-196">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="73442-197">アプリが応答ストリームへの書き込みを開始した後では、新しいセッション Cookie を含む新しいセッションを作成できません。</span><span class="sxs-lookup"><span data-stu-id="73442-197">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="73442-198">例外は Web サーバー ログに記録され、ブラウザーには表示されません。</span><span class="sxs-lookup"><span data-stu-id="73442-198">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="73442-199">セッション状態を非同期的に読み込む</span><span class="sxs-lookup"><span data-stu-id="73442-199">Load session state asynchronously</span></span>

<span data-ttu-id="73442-200">[TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue)、[Set](/dotnet/api/microsoft.aspnetcore.http.isession.set)、または [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) メソッドの前に、[ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) メソッドが明示的に呼び出された場合にのみ、ASP.NET Core の既定のセッション プロバイダーは、基になっている [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) バッキング ストアからセッション レコードを非同期に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="73442-200">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="73442-201">`LoadAsync` を最初に呼び出さないと、基になっているセッション レコードは同期的に読み込まれ、パフォーマンスが大幅に低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="73442-201">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="73442-202">アプリにこのパターンを強制させるには、`TryGetValue`、`Set`、または `Remove` の前に `LoadAsync` メソッドが呼び出されない場合に例外をスローするバージョンで、[DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) および [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) の実装をラップします。</span><span class="sxs-lookup"><span data-stu-id="73442-202">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="73442-203">ラップしたバージョンをサービス コンテナーに登録します。</span><span class="sxs-lookup"><span data-stu-id="73442-203">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="73442-204">セッション オプション</span><span class="sxs-lookup"><span data-stu-id="73442-204">Session options</span></span>

<span data-ttu-id="73442-205">セッションの既定値をオーバーライドするには、[SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions) を使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-205">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="73442-206">オプション</span><span class="sxs-lookup"><span data-stu-id="73442-206">Option</span></span> | <span data-ttu-id="73442-207">説明</span><span class="sxs-lookup"><span data-stu-id="73442-207">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="73442-208">Cookie</span><span class="sxs-lookup"><span data-stu-id="73442-208">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="73442-209">Cookie の作成に使用される設定を決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-209">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="73442-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) の既定値は [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="73442-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) の既定値は [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="73442-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) の既定値は [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="73442-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) の既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="73442-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="73442-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) の既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="73442-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="73442-215">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="73442-215">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="73442-216">`IdleTimeout` は、内容を破棄されることなくセッションがアイドル状態になっていることのできる最大時間を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-216">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="73442-217">セッションへのアクセスがあるたびに、タイムアウトはリセットされます。</span><span class="sxs-lookup"><span data-stu-id="73442-217">Each session access resets the timeout.</span></span> <span data-ttu-id="73442-218">この設定はセッションの内容にのみ適用され、Cookie には適用されません。</span><span class="sxs-lookup"><span data-stu-id="73442-218">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="73442-219">既定値は 20 分です。</span><span class="sxs-lookup"><span data-stu-id="73442-219">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="73442-220">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="73442-220">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="73442-221">ストアからのセッションの読み込み、またはストアに戻すコミットに対して許容される最大時間です。</span><span class="sxs-lookup"><span data-stu-id="73442-221">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="73442-222">この設定は非同期操作にのみ適用できます。</span><span class="sxs-lookup"><span data-stu-id="73442-222">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="73442-223">このタイムアウトは、[InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) を使用して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="73442-223">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="73442-224">既定値は 1 分です。</span><span class="sxs-lookup"><span data-stu-id="73442-224">The default is 1 minute.</span></span> |

<span data-ttu-id="73442-225">セッションは Cookie を利用し、1 つのブラウザーからの要求を追跡し、識別します。</span><span class="sxs-lookup"><span data-stu-id="73442-225">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="73442-226">既定では、この Cookie は `.AspNetCore.Session` という名前になり、パス `/` を使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-226">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="73442-227">Cookie の既定値ではドメインが指定されないため、ページのクライアント側スクリプトには使用できません ([HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) の既定値が `true` になるため)。</span><span class="sxs-lookup"><span data-stu-id="73442-227">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="73442-228">オプション</span><span class="sxs-lookup"><span data-stu-id="73442-228">Option</span></span> | <span data-ttu-id="73442-229">説明</span><span class="sxs-lookup"><span data-stu-id="73442-229">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="73442-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="73442-230">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="73442-231">Cookie の作成に使用されるドメインを決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-231">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="73442-232">既定では、`CookieDomain` は設定されません。</span><span class="sxs-lookup"><span data-stu-id="73442-232">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="73442-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="73442-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="73442-234">クライアント側 JavaScript が Cookie にアクセスするのをブラウザーが許可する必要があるかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-234">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="73442-235">既定値の `true` は、Cookie が HTTP 要求に対して渡されるだけであり、ページ上のスクリプトでは使用できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="73442-235">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="73442-236">CookieName</span><span class="sxs-lookup"><span data-stu-id="73442-236">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="73442-237">セッション ID の保持に使われる Cookie の名前を決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-237">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="73442-238">既定値は [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-238">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="73442-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="73442-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="73442-240">Cookie の作成に使用されるパスを決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-240">Determines the path used to create the cookie.</span></span> <span data-ttu-id="73442-241">既定値は [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-241">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="73442-242">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="73442-242">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="73442-243">Cookie を HTTPS 要求のみで送信するかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-243">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="73442-244">既定値は [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`) です。</span><span class="sxs-lookup"><span data-stu-id="73442-244">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="73442-245">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="73442-245">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="73442-246">`IdleTimeout` は、内容を破棄されることなくセッションがアイドル状態になっていることのできる最大時間を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-246">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="73442-247">セッションへのアクセスがあるたびに、タイムアウトはリセットされます。</span><span class="sxs-lookup"><span data-stu-id="73442-247">Each session access resets the timeout.</span></span> <span data-ttu-id="73442-248">これはセッションの内容にのみ適用され、Cookie には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="73442-248">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="73442-249">既定値は 20 分です。</span><span class="sxs-lookup"><span data-stu-id="73442-249">The default is 20 minutes.</span></span> |

<span data-ttu-id="73442-250">セッションは Cookie を利用し、1 つのブラウザーからの要求を追跡し、識別します。</span><span class="sxs-lookup"><span data-stu-id="73442-250">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="73442-251">既定では、この Cookie は `.AspNet.Session` という名前になり、パス `/` を使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-251">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="73442-252">Cookie セッションの既定値をオーバーライドするには、`SessionOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-252">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="73442-253">アプリは [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) プロパティを使用し、サーバーのキャッシュ内の内容が破棄されるまでのセッションのアイドル時間を決定します。</span><span class="sxs-lookup"><span data-stu-id="73442-253">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="73442-254">このプロパティは Cookie の有効期限に依存しません。</span><span class="sxs-lookup"><span data-stu-id="73442-254">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="73442-255">要求が[セッション ミドルウェア](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware)を通過するたびにタイムアウトがリセットされます。</span><span class="sxs-lookup"><span data-stu-id="73442-255">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="73442-256">セッション状態は "*ロックなし*" です。</span><span class="sxs-lookup"><span data-stu-id="73442-256">Session state is *non-locking*.</span></span> <span data-ttu-id="73442-257">2 つの要求がセッションの内容を同時に変更しようとした場合、最後の要求が最初の要求をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="73442-257">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="73442-258">`Session` は*一貫性のあるセッション*として実装されます。つまり、コンテンツは全部まとめて保管されます。</span><span class="sxs-lookup"><span data-stu-id="73442-258">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="73442-259">2 つの要求が異なるセッション値を変更しようとしたとき、最後の要求が最初の要求によって行われたセッションの変更をオーバーライドすることがあります。</span><span class="sxs-lookup"><span data-stu-id="73442-259">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="73442-260">セッション値の設定および取得</span><span class="sxs-lookup"><span data-stu-id="73442-260">Set and get Session values</span></span>

<span data-ttu-id="73442-261">セッション状態には、Razor Pages の [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) クラスから、または MVC の [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) クラスの [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) でアクセスします。</span><span class="sxs-lookup"><span data-stu-id="73442-261">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="73442-262">このプロパティは [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 実装です。</span><span class="sxs-lookup"><span data-stu-id="73442-262">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73442-263">`ISession` の実装では、整数値や文字列値を設定および取得するための複数の拡張メソッドが提供されています。</span><span class="sxs-lookup"><span data-stu-id="73442-263">The `ISession` implementation provides several extension methods to set and retrieve integer and string values.</span></span> <span data-ttu-id="73442-264">[Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) パッケージがプロジェクトによって参照されている場合、拡張メソッドは [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 名前空間内にあります (拡張メソッドにアクセスするには、`using Microsoft.AspNetCore.Http;` ステートメントを追加します)。</span><span class="sxs-lookup"><span data-stu-id="73442-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="73442-265">どちらのパッケージも、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="73442-265">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73442-266">`ISession` の実装では、整数値や文字列値を設定および取得するための複数の拡張メソッドが提供されています。</span><span class="sxs-lookup"><span data-stu-id="73442-266">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="73442-267">[Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) パッケージがプロジェクトによって参照されている場合、拡張メソッドは [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 名前空間内にあります (拡張メソッドにアクセスするには、`using Microsoft.AspNetCore.Http;` ステートメントを追加します)。</span><span class="sxs-lookup"><span data-stu-id="73442-267">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="73442-268">`ISession` 拡張メソッド:</span><span class="sxs-lookup"><span data-stu-id="73442-268">`ISession` extension methods:</span></span>

* [<span data-ttu-id="73442-269">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="73442-269">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="73442-270">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="73442-270">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="73442-271">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="73442-271">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="73442-272">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="73442-272">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="73442-273">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="73442-273">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="73442-274">次の例では、`IndexModel.SessionKeyName` キー (サンプル アプリ内の `_Name`) のセッション値を Razor Pages ページで取得します。</span><span class="sxs-lookup"><span data-stu-id="73442-274">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="73442-275">次の例では、整数および文字列を設定および取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-275">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="73442-276">分散キャッシュのシナリオを有効にするには、メモリ内キャッシュを使用する場合であっても、すべてのセッション データをシリアル化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-276">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="73442-277">最小限の文字列と数値のシリアライザーが提供されています ([ISession](/dotnet/api/microsoft.aspnetcore.http.isession) のメソッドおよびの拡張メソッドをご覧ください)。</span><span class="sxs-lookup"><span data-stu-id="73442-277">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="73442-278">複合型は、JSON などの別のメカニズムを使ってユーザーがシリアル化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-278">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="73442-279">シリアル化可能なオブジェクトを設定および取得するには、次の拡張メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="73442-279">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73442-280">次の例では、シリアル化可能オブジェクトを拡張メソッドで設定および取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-280">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="73442-281">TempData</span><span class="sxs-lookup"><span data-stu-id="73442-281">TempData</span></span>

<span data-ttu-id="73442-282">ASP.NET Core は、[Razor Pages ページ モデルの TempData プロパティ](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata)または [MVC コントローラーの TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata) を公開します。</span><span class="sxs-lookup"><span data-stu-id="73442-282">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="73442-283">このプロパティは、読み取られるまでデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="73442-283">This property stores data until it's read.</span></span> <span data-ttu-id="73442-284">[Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) メソッドと [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) メソッドは、削除せずにデータを確認するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="73442-284">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="73442-285">TempData は特に、複数の要求にデータが必要な場合のリダイレクトに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="73442-285">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="73442-286">TempData は、Cookie またはセッション状態を使って、TempData プロバイダーによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="73442-286">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="73442-287">TempData プロバイダー</span><span class="sxs-lookup"><span data-stu-id="73442-287">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73442-288">ASP.NET Core 2.0 以降では、Cookie に TempData を保存するために Cookie ベースの TempData プロバイダーが既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="73442-288">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="73442-289">Cookie データは [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) を使用して暗号化され、[Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) でエンコードされ、チャンクされます。</span><span class="sxs-lookup"><span data-stu-id="73442-289">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="73442-290">Cookie はチャンクされるため、ASP.NET Core 1.x の 1 Cookie のサイズ上限は適用されません。</span><span class="sxs-lookup"><span data-stu-id="73442-290">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="73442-291">暗号化されているデータを圧縮すると、[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 攻撃や [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻撃など、セキュリティ上の問題を起す可能性があるため、Cookie データは圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="73442-291">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="73442-292">Cookie ベース TempData プロバイダーの詳細については、「[CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73442-292">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73442-293">ASP.NET Core 1.0 と 1.1 では、セッション状態 TempData プロバイダーが既定のプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="73442-293">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="73442-294">TempData プロバイダーを選択する</span><span class="sxs-lookup"><span data-stu-id="73442-294">Choose a TempData provider</span></span>

<span data-ttu-id="73442-295">TempData プロバイダーを選択するときの考慮事項:</span><span class="sxs-lookup"><span data-stu-id="73442-295">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="73442-296">アプリは既にセッション状態を使っているかどうか。</span><span class="sxs-lookup"><span data-stu-id="73442-296">Does the app already use session state?</span></span> <span data-ttu-id="73442-297">使っている場合、(データのサイズを除き) セッション状態 TempData プロバイダーがそのアプリにコストを追加することはありません。</span><span class="sxs-lookup"><span data-stu-id="73442-297">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="73442-298">アプリでは、比較的少量のデータに対して (最大 500 バイト) TempData がわずかばかり使用されているか。</span><span class="sxs-lookup"><span data-stu-id="73442-298">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="73442-299">該当する場合、Cookie TempData プロバイダーは TempData を送信する要求ごとに少額のコストを追加します。</span><span class="sxs-lookup"><span data-stu-id="73442-299">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="73442-300">該当しない場合、セッション状態 TempData プロバイダーは便利かもしれません。TempData が尽きるまで、要求のたびに大量のデータをラウンドトリップすることが回避されます。</span><span class="sxs-lookup"><span data-stu-id="73442-300">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="73442-301">アプリは複数サーバーのサーバー ファームで実行しているか。</span><span class="sxs-lookup"><span data-stu-id="73442-301">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="73442-302">そうである場合は、データ保護の外部で Cookie TempData プロバイダーを使用するために、追加の構成は必要ありません (<xref:security/data-protection/introduction> および[キー ストレージ プロバイダー](xref:security/data-protection/implementation/key-storage-providers)に関する記事を参照)。</span><span class="sxs-lookup"><span data-stu-id="73442-302">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="73442-303">ほとんどの Web クライアント (Web ブラウザーなど) は、各 Cookie の最大サイズ、Cookie の合計数、または両方に上限を課します。</span><span class="sxs-lookup"><span data-stu-id="73442-303">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="73442-304">Cookie TempData プロバイダーを使用するとき、アプリでそれらの上限が超えないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="73442-304">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="73442-305">データの合計サイズを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="73442-305">Consider the total size of the data.</span></span> <span data-ttu-id="73442-306">暗号化とチャンクによる Cookie のサイズの増加を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="73442-306">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="73442-307">TempData プロバイダーを構成する</span><span class="sxs-lookup"><span data-stu-id="73442-307">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73442-308">Cookie ベース TempData プロバイダーは既定で有効になります。</span><span class="sxs-lookup"><span data-stu-id="73442-308">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="73442-309">セッション ベースの TempData プロバイダーを有効にするには、[AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) 拡張メソッドを使います。</span><span class="sxs-lookup"><span data-stu-id="73442-309">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73442-310">次の `Startup` クラス コードでは、セッション ベース TempData プロバイダーが構成されます。</span><span class="sxs-lookup"><span data-stu-id="73442-310">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="73442-311">ミドルウェアの順序が重要です。</span><span class="sxs-lookup"><span data-stu-id="73442-311">The order of middleware is important.</span></span> <span data-ttu-id="73442-312">上の例では、`UseMvc` の後で `UseSession` を呼び出すと、`InvalidOperationException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="73442-312">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="73442-313">詳細については、[ミドルウェアの順序](xref:fundamentals/middleware/index#order)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="73442-313">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73442-314">.NET Framework が対象で、セッション ベースの TempData プロバイダーを使う場合は、[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="73442-314">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="73442-315">クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="73442-315">Query strings</span></span>

<span data-ttu-id="73442-316">限られた量のデータを要求間で渡すことができます。新しい要求のクエリ文字列にそれを追加します。</span><span class="sxs-lookup"><span data-stu-id="73442-316">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="73442-317">これは、リンクと埋め込まれた状態がメールまたはソーシャル ネットワークを通して共有されるよう、永久的に状態をキャプチャするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="73442-317">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="73442-318">URL クエリ文字列はパブリックであるため、機密データにはクエリ文字列を使わないでください。</span><span class="sxs-lookup"><span data-stu-id="73442-318">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="73442-319">意図しない共有が発生するだけでなく、クエリ文字列にデータを含めると、[クロスサイト リクエスト フォージェリ (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻撃の機会を与えてしまいます。認証中、ユーザーをだまして悪意のあるサイトに誘導します。</span><span class="sxs-lookup"><span data-stu-id="73442-319">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="73442-320">攻撃者はアプリからユーザー データを盗んだり、ユーザーになりすまして悪意のある行為を行ったりできます。</span><span class="sxs-lookup"><span data-stu-id="73442-320">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="73442-321">保存されるアプリまたはセッション状態は CSRF 攻撃を防ぐ必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-321">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="73442-322">詳細については、「[Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery)」(クロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73442-322">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="73442-323">非表示フィールド</span><span class="sxs-lookup"><span data-stu-id="73442-323">Hidden fields</span></span>

<span data-ttu-id="73442-324">データは非表示フォーム フィールドに保存したり、次の要求で転記したりできます。</span><span class="sxs-lookup"><span data-stu-id="73442-324">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="73442-325">複数ページ フォームでは、これは一般的です。</span><span class="sxs-lookup"><span data-stu-id="73442-325">This is common in multi-page forms.</span></span> <span data-ttu-id="73442-326">クライアントがデータを改ざんする可能性があるため、アプリで非表示フィールドに格納されているデータを常に再検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-326">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="73442-327">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="73442-327">HttpContext.Items</span></span>

<span data-ttu-id="73442-328">1 つの要求を処理している間データを格納するには、[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) コレクションを使います。</span><span class="sxs-lookup"><span data-stu-id="73442-328">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="73442-329">要求が処理された後、コレクションの内容は破棄されます。</span><span class="sxs-lookup"><span data-stu-id="73442-329">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="73442-330">`Items` コレクションは、コンポーネントまたはミドルウェアが要求の間の異なる時点で動作し、パラメーターを受け渡す直接的な方法がない場合に、コンポーネントとミドルウェアが通信できるようにするためによく使われます。</span><span class="sxs-lookup"><span data-stu-id="73442-330">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="73442-331">次の例では、[ミドルウェア](xref:fundamentals/middleware/index)により `isVerified` が `Items` コレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="73442-331">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="73442-332">後のパイプラインで、別のミドルウェアが `isVerified` の値にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="73442-332">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="73442-333">1 つのアプリでのみ使用されるミドルウェアの場合、`string` キーが許容されます。</span><span class="sxs-lookup"><span data-stu-id="73442-333">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="73442-334">アプリのインスタンス間で共有されるミドルウェアでは、キーの競合を避けるため、一意のオブジェクト キーを使う必要があります。</span><span class="sxs-lookup"><span data-stu-id="73442-334">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="73442-335">次の例では、ミドルウェア クラスで定義されている一意のオブジェクト キーを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="73442-335">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="73442-336">その他のコードは、ミドルウェア クラスで公開されるキーを利用し、`HttpContext.Items` に格納されている値にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="73442-336">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="73442-337">この方法には、コード内でキーの文字列を使用する必要がないという利点もあります。</span><span class="sxs-lookup"><span data-stu-id="73442-337">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="73442-338">キャッシュ</span><span class="sxs-lookup"><span data-stu-id="73442-338">Cache</span></span>

<span data-ttu-id="73442-339">キャッシュは、データを保存し、取得する効率的な方法です。</span><span class="sxs-lookup"><span data-stu-id="73442-339">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="73442-340">アプリでは、キャッシュされた項目の有効期間を制御できます。</span><span class="sxs-lookup"><span data-stu-id="73442-340">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="73442-341">キャッシュされたデータは、特定の要求、ユーザー、またはセッションに関連付けられていません。</span><span class="sxs-lookup"><span data-stu-id="73442-341">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="73442-342">**他のユーザーの要求によって取得される可能性があるので、ユーザー固有データをキャッシュしないように注意してください。**</span><span class="sxs-lookup"><span data-stu-id="73442-342">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="73442-343">詳細については、「<xref:performance/caching/response>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73442-343">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="73442-344">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="73442-344">Dependency Injection</span></span>

<span data-ttu-id="73442-345">[依存関係の注入](xref:fundamentals/dependency-injection)を利用し、すべてのユーザーがデータを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="73442-345">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="73442-346">データを含むサービスを定義します。</span><span class="sxs-lookup"><span data-stu-id="73442-346">Define a service containing the data.</span></span> <span data-ttu-id="73442-347">たとえば、`MyAppData` という名前のクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="73442-347">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="73442-348">サービス クラスを `Startup.ConfigureServices` に追加します。</span><span class="sxs-lookup"><span data-stu-id="73442-348">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="73442-349">データ サービス クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="73442-349">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="73442-350">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="73442-350">Common errors</span></span>

* <span data-ttu-id="73442-351">"'Microsoft.AspNetCore.Session.DistributedSessionStore' を起動しようとしましたが、型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' のサービスを解決できません。"</span><span class="sxs-lookup"><span data-stu-id="73442-351">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="73442-352">これは通常、少なくとも 1 つの `IDistributedCache` 実装で構成に失敗したことで発生します。</span><span class="sxs-lookup"><span data-stu-id="73442-352">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="73442-353">詳細については、次のトピックを参照してください。 <xref:performance/caching/distributed> および <xref:performance/caching/memory></span><span class="sxs-lookup"><span data-stu-id="73442-353">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="73442-354">セッション ミドルウェアがセッションを永続化できなかった場合 (バッキング ストアを利用できない場合など)、ミドルウェアは例外をログに記録し、要求は普通に続行されます。</span><span class="sxs-lookup"><span data-stu-id="73442-354">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="73442-355">これにより、予期しない動作が発生します。</span><span class="sxs-lookup"><span data-stu-id="73442-355">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="73442-356">たとえば、ユーザーがセッションでショッピング カートを格納します。</span><span class="sxs-lookup"><span data-stu-id="73442-356">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="73442-357">ユーザーはアイテムをカートに追加しますが、コミットが失敗します。</span><span class="sxs-lookup"><span data-stu-id="73442-357">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="73442-358">アプリはこの失敗を認識しないので、アイテムがカートに追加されたことをユーザーに伝えますが、これは正しくありません。</span><span class="sxs-lookup"><span data-stu-id="73442-358">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="73442-359">エラーを確認するための推奨される方法は、アプリがセッションへの書き込みを終了したら、アプリ コードから `await feature.Session.CommitAsync();` を呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="73442-359">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="73442-360">バッキング ストアが利用できない場合、`CommitAsync` は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="73442-360">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="73442-361">`CommitAsync` が失敗した場合、アプリは例外を処理できます。</span><span class="sxs-lookup"><span data-stu-id="73442-361">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="73442-362">`LoadAsync` は、データ ストアが利用できない場合に同じ条件で例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="73442-362">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73442-363">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="73442-363">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
