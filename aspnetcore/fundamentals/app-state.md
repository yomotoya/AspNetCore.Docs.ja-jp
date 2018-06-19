---
title: ASP.NET Core のセッションとアプリケーションの状態
author: rick-anderson
description: 要求間でアプリケーションとユーザー (セッション) の状態を維持する手法。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 887aefdeaa45957f7b95bfe8df342eb34d267e3a
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306648"
---
# <a name="session-and-application-state-in-aspnet-core"></a><span data-ttu-id="0e11e-103">ASP.NET Core のセッションとアプリケーションの状態</span><span class="sxs-lookup"><span data-stu-id="0e11e-103">Session and application state in ASP.NET Core</span></span>

<span data-ttu-id="0e11e-104">[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose) 作</span><span class="sxs-lookup"><span data-stu-id="0e11e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="0e11e-105">HTTP はステートレス プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="0e11e-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="0e11e-106">Web サーバーは各 HTTP 要求を非依存の要求として扱い、前の要求からのユーザー値を維持しません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="0e11e-107">この記事では、要求間でアプリケーションとセッションの状態を維持するさまざまな方法について取り上げます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-107">This article discusses different ways to preserve application and session state between requests.</span></span>

## <a name="session-state"></a><span data-ttu-id="0e11e-108">セッション状態</span><span class="sxs-lookup"><span data-stu-id="0e11e-108">Session state</span></span>

<span data-ttu-id="0e11e-109">セッション状態は ASP.NET Core の機能で、これを使用することでユーザーが Web アプリを参照中にユーザー データを保存して格納することができます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="0e11e-110">セッション状態は、サーバー上のディクショナリまたはハッシュ テーブルから構成され、ブラウザーからの要求間でデータを維持します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="0e11e-111">セッション データはキャッシュによりバックアップされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="0e11e-112">ASP.NET Core は、セッション ID を含む Cookie をクライアントに与えることでセッションの状態を維持します。セッション ID は要求ごとにサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="0e11e-113">サーバーはセッション ID を使用し、セッション データを取得します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="0e11e-114">セッション Cookie はブラウザーに固有であるため、ブラウザー間でセッションを共有することはできません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="0e11e-115">セッション Cookie は、ブラウザー セッションが終了したときに初めて削除されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="0e11e-116">Cookie を受け取り、セッションが期限切れになった場合、同じセッション Cookie を使用する新しいセッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-116">If a cookie is received for an expired session, a new session that uses the same session cookie is created.</span></span>

<span data-ttu-id="0e11e-117">サーバーは、最後の要求から限られた時間だけセッションを維持します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="0e11e-118">セッション タイムアウトを設定するか、20 分という既定値を使用してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-118">Either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="0e11e-119">セッション状態は、特定のセッションに固有であるが、永久的に維持する必要がないユーザー データの格納に最適です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-119">Session state is ideal for storing user data that's specific to a particular session but doesn't need to be persisted permanently.</span></span> <span data-ttu-id="0e11e-120">`Session.Clear` を呼び出したときか、データ ストアでセッションが期限切れになったとき、バックアップ ストアからデータが削除されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-120">Data is deleted from the backing store either when calling `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="0e11e-121">サーバーでは、ブラウザーが閉じられことやセッション Cookie が削除されたことが認識されません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="0e11e-122">セッションでは機密データを保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="0e11e-123">クライアントでブラウザーが閉じられず、セッション Cookie が消去されないことがあります (ブラウザーによっては、ウィンドウ間でセッション Cookie が有効になります)。</span><span class="sxs-lookup"><span data-stu-id="0e11e-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="0e11e-124">また、セッションが 1 人のユーザーに制限されないことがあります。次のユーザーが同じセッションを続けることがあります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="0e11e-125">メモリ内セッション プロバイダーは、ローカル サーバー上でセッション データを保存します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="0e11e-126">サーバー ファームで Web アプリを実行する予定であれば、固定セッションを使用し、各セッションを特定のサーバーに結び付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="0e11e-127">Windows Azure Web Sites プラットフォームは、初期設定で固定セッションを使用します (アプリケーション要求ルーティング処理または ARR)。</span><span class="sxs-lookup"><span data-stu-id="0e11e-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="0e11e-128">ただし、固定セッションは拡張性に影響を与え、Web アプリの更新を複雑にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="0e11e-129">良い選択肢は、Redis または SQL Server の分散キャッシュを使用することです。固定セッションを必要としません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="0e11e-130">詳細については、[分散キャッシュの使用](xref:performance/caching/distributed)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-130">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="0e11e-131">サービス プロバイダーの設定方法については、この記事の「[セッションを構成する](#configuring-session)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

## <a name="tempdata"></a><span data-ttu-id="0e11e-132">TempData</span><span class="sxs-lookup"><span data-stu-id="0e11e-132">TempData</span></span>

<span data-ttu-id="0e11e-133">ASP.NET Core MVC は[コントローラー](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)上で [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-133">ASP.NET Core MVC exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="0e11e-134">このプロパティは、読み取られるまでデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-134">This property stores data until it's read.</span></span> <span data-ttu-id="0e11e-135">`Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0e11e-136">`TempData` は特に、複数の要求にデータが必要な場合のリダイレクトに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="0e11e-137">`TempData` は、Cookie やセッション状態を利用することなどで、TempData プロバイダーによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="0e11e-138">TempData プロバイダー</span><span class="sxs-lookup"><span data-stu-id="0e11e-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e11e-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0e11e-140">ASP.NET Core 2.0 以降、Cookie に TempData を保存するために Cookie ベースの TempData プロバイダーが既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="0e11e-141">Cookie データは [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) を使用して暗号化され、[Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) でエンコードされ、チャンクされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-141">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="0e11e-142">Cookie はチャンクされるため、ASP.NET Core 1.x の 1 Cookie のサイズ上限は適用されません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-142">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="0e11e-143">暗号化されているデータを圧縮すると、[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 攻撃や [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻撃など、セキュリティ上の問題を起す可能性があるため、Cookie データは圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-143">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="0e11e-144">Cookie ベース TempData プロバイダーの詳細については、「[CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e11e-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0e11e-146">ASP.NET Core 1.0 と 1.1 では、セッション状態 TempData プロバイダーが既定です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

---

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="0e11e-147">TempData プロバイダーを選択する</span><span class="sxs-lookup"><span data-stu-id="0e11e-147">Choosing a TempData provider</span></span>

<span data-ttu-id="0e11e-148">TempData プロバイダーを選択するときの考慮事項:</span><span class="sxs-lookup"><span data-stu-id="0e11e-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="0e11e-149">そのアプリケーションでセッション状態が他の目的のために既に使用されていないか。</span><span class="sxs-lookup"><span data-stu-id="0e11e-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="0e11e-150">使用されている場合、(データのサイズを除き) セッション状態 TempData プロバイダーがそのアプリケーションにコストを追加することはありません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="0e11e-151">そのアプリケーションでは、比較的少量のデータに対して (最大 500 バイト) TempData がわずかばかり使用されているだけではないか。</span><span class="sxs-lookup"><span data-stu-id="0e11e-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="0e11e-152">該当する場合、Cookie TempData プロバイダーは TempData を送信する要求ごとに少額のコストを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="0e11e-153">該当しない場合、セッション状態 TempData プロバイダーは便利かもしれません。TempData が尽きるまで、要求のたびに大量のデータをラウンドトリップすることが回避されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="0e11e-154">そのアプリケーションは Web ファーム (複数のサーバー) で実行するのか。</span><span class="sxs-lookup"><span data-stu-id="0e11e-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="0e11e-155">その場合、Cookie TempData プロバイダーを使用するために追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="0e11e-156">ほとんどの Web クライアント (Web ブラウザーなど) は、各 Cookie の最大サイズ、Cookie の合計数、または両方に上限を課します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="0e11e-157">そのため、Cookie TempData プロバイダーを使用するとき、アプリでそれらの上限が超えないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="0e11e-158">データの合計サイズを考慮し、暗号化やチャンク化のオーバーヘッドを計算します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="0e11e-159">TempData プロバイダーを構成する</span><span class="sxs-lookup"><span data-stu-id="0e11e-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e11e-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="0e11e-161">Cookie ベース TempData プロバイダーは既定で有効になります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="0e11e-162">次の `Startup` クラス コードでは、セッション ベース TempData プロバイダーが構成されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e11e-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="0e11e-164">次の `Startup` クラス コードでは、セッション ベース TempData プロバイダーが構成されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="0e11e-165">ミドルウェア コンポーネントの場合、順序が重要です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="0e11e-166">先の例では、`UseMvcWithDefaultRoute` の後に `UseSession` が呼び出されたとき、型 `InvalidOperationException` の例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="0e11e-167">詳細については、[ミドルウェアの順序付け](xref:fundamentals/middleware/index#ordering)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-167">See [Middleware Ordering](xref:fundamentals/middleware/index#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0e11e-168">.NET Framework が対象で、セッション ベースのプロバイダーを使用するとき、[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="0e11e-169">クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="0e11e-169">Query strings</span></span>

<span data-ttu-id="0e11e-170">限られた量のデータを要求間で渡すことができます。新しい要求のクエリ文字列にそれを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-170">You can pass a limited amount of data from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="0e11e-171">これは、リンクと埋め込まれた状態がメールまたはソーシャル ネットワークを通して共有されるよう、永久的に状態をキャプチャするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="0e11e-172">ただし、この理由から、機密データにはクエリ文字列で絶対に使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="0e11e-173">クエリ文字列にデータを含めると、共有が簡単になりますが、[クロスサイト リクエスト フォージェリ (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻撃の機会を与えてしまいます。認証中、ユーザーをだまして悪意のあるサイトに誘導します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="0e11e-174">攻撃者はアプリからユーザー データを盗んだり、ユーザーになりすまして悪意のある行為を行ったりできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="0e11e-175">保存されるアプリケーションまたはセッション状態は CSRF 攻撃を防ぐ必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="0e11e-176">CSRF 攻撃の詳細については、「[Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery)」(クロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-176">For more information on CSRF attacks, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="0e11e-177">データや非表示フィールドの転記</span><span class="sxs-lookup"><span data-stu-id="0e11e-177">Post data and hidden fields</span></span>

<span data-ttu-id="0e11e-178">データは非表示フォーム フィールドに保存したり、次の要求で転記したりできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="0e11e-179">複数ページ フォームでは、これは一般的です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-179">This is common in multi-page forms.</span></span> <span data-ttu-id="0e11e-180">ただし、クライアントがデータを改ざんする可能性があるため、サーバーは常に再検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span>

## <a name="cookies"></a><span data-ttu-id="0e11e-181">クッキー</span><span class="sxs-lookup"><span data-stu-id="0e11e-181">Cookies</span></span>

<span data-ttu-id="0e11e-182">Cookie は、web アプリケーションでユーザー固有のデータを格納する方法です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="0e11e-183">Cookie は要求ごとに送信されるために、そのサイズは最小に抑える必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="0e11e-184">理想的には、識別子だけを Cookie に格納し、実際のデータはサーバーに保存します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="0e11e-185">ほとんどのブラウザーで Cookie は 4096 バイトに制限されています。</span><span class="sxs-lookup"><span data-stu-id="0e11e-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="0e11e-186">また、ドメインごとに限定された数の Cookie のみを利用できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-186">In addition, only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="0e11e-187">Cookie は改ざんされる可能性があるため、サーバーで検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="0e11e-188">クライアント上の Cookie の永続性はユーザーの操作や有効期限に左右されるため、一般的に、クライアントのデータ永続性としては最も長持ちする形態となります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="0e11e-189">Cookie は、多くの場合、パーソナル化に利用されます。既知のユーザーのためにコンテンツをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="0e11e-190">ほとんどの場合、ユーザーは識別されるだけで本人確認されないため、一般的に、Cookie にユーザー名、アカウント名、一意のユーザー ID (GUID など) を保存することで Cookie を安全にできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="0e11e-191">その後、Cookie を使用し、サイトのユーザー パーソナル化インフラストラクチャにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="0e11e-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="0e11e-192">HttpContext.Items</span></span>

<span data-ttu-id="0e11e-193">`Items` コレクションは、ある特定の要求の処理時にのみ必要なデータの格納に最適な場所です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="0e11e-194">コレクションのコンテンツは各要求の後に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="0e11e-195">`Items` コレクションは、コンポーネントまたはミドルウェアが要求中に異なる時点で作動するがパラメーターを渡す直接的な方法がないとき、通信方法として最適です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="0e11e-196">詳細については、この記事の「[HttpContext.Items を使用する](#working-with-httpcontextitems)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-196">For more information, see [Work with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="0e11e-197">キャッシュ</span><span class="sxs-lookup"><span data-stu-id="0e11e-197">Cache</span></span>

<span data-ttu-id="0e11e-198">キャッシュは、データを保存し、取得する効率的な方法です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="0e11e-199">時間やその他の考慮事項に基づき、キャッシュされる項目の有効期限を制御できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="0e11e-200">[キャッシュの方法](../performance/caching/index.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-200">Learn more about [how to cache](../performance/caching/index.md).</span></span>

## <a name="working-with-session-state"></a><span data-ttu-id="0e11e-201">セッション状態の使用する</span><span class="sxs-lookup"><span data-stu-id="0e11e-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="0e11e-202">セッションを構成する</span><span class="sxs-lookup"><span data-stu-id="0e11e-202">Configuring Session</span></span>

<span data-ttu-id="0e11e-203">`Microsoft.AspNetCore.Session` パッケージは、セッション状態を管理するためのミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="0e11e-204">セッション ミドルウェアを有効にするには、`Startup` に次が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="0e11e-205">いずれかの [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) メモリ キャッシュ。</span><span class="sxs-lookup"><span data-stu-id="0e11e-205">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="0e11e-206">`IDistributedCache` 実装はセッションのバックアップ ストアとして利用されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="0e11e-207">[AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 呼び出し。これには NuGet パッケージ "Microsoft.AspNetCore.Session" が必要です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-207">[AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="0e11e-208">[UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 呼び出し。</span><span class="sxs-lookup"><span data-stu-id="0e11e-208">[UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="0e11e-209">次のコードでは、メモリ内セッション プロバイダーの設定方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e11e-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e11e-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="0e11e-212">インストールと構成の後、`HttpContext` からセッションを参照できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="0e11e-213">`UseSession` が呼び出される前に `Session` にアクセスする場合、例外 `InvalidOperationException: Session has not been configured for this application or request` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="0e11e-214">`Response` ストリームへの書き込みを既に開始した後に新しい `Session` を作成しようとする場合、例外 `InvalidOperationException: The session cannot be established after the response has started` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="0e11e-215">例外は Web サーバー ログにあります。ブラウザーには表示されません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="0e11e-216">セッションを非同期で読み込む</span><span class="sxs-lookup"><span data-stu-id="0e11e-216">Loading Session asynchronously</span></span>

<span data-ttu-id="0e11e-217">ASP.NET Core の既定のセッション プロバイダーは、`TryGetValue`、`Set`、または `Remove` メソッドの前に [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) メソッドが明示的に呼び出された場合にのみ、基礎となる [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) ストアから非同期的にセッション レコードを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="0e11e-218">`LoadAsync` が最初に呼び出されない場合、基礎となるセッション レコードが同期的に読み込まれます。これはアプリの拡張機能に影響を与える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="0e11e-219">アプリケーションにこのパターンを強制させるには、`TryGetValue`、`Set`、または `Remove` の前に `LoadAsync` メソッドが呼び出されない場合に例外をスローするバージョンで [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) と [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 実装をラップします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="0e11e-220">ラップしたバージョンをサービス コンテナーに登録します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="0e11e-221">実装詳細</span><span class="sxs-lookup"><span data-stu-id="0e11e-221">Implementation details</span></span>

<span data-ttu-id="0e11e-222">セッションは Cookie を利用し、1 つのブラウザーからの要求を追跡し、識別します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="0e11e-223">既定では、この Cookie には ".AspNet.Session" という名前が付き、"/" というパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="0e11e-224">Cookie の既定値でドメインが指定されない場合、ページのクライアント側スクリプトで利用できません (`CookieHttpOnly` の初期設定が `true` となるため)。</span><span class="sxs-lookup"><span data-stu-id="0e11e-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="0e11e-225">セッションの既定値をオーバーライドするには、`SessionOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0e11e-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0e11e-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0e11e-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="0e11e-228">サーバーは `IdleTimeout` プロパティを使用し、コンテンツが破棄されるまでのセッションのアイドル時間を決定します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="0e11e-229">このプロパティは Cookie の有効期限に依存しません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="0e11e-230">(読み取りまたは書き込みで) 要求がセッション ミドルウェアを通過するたびにタイムアウトがリセットされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="0e11e-231">`Session` は*ロックなし*のため、2 つの要求がセッションのコンテンツを変更しようとすると、最後の要求が最初の要求をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="0e11e-232">`Session` は*一貫性のあるセッション*として実装されます。つまり、コンテンツは全部まとめて保管されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="0e11e-233">2 つの要求がセッションのさまざまなパーツ (さまざまなキー) を変更する場合、互いに影響を及ぼすことがあります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="0e11e-234">セッション値の設定および取得</span><span class="sxs-lookup"><span data-stu-id="0e11e-234">Set and get Session values</span></span>

<span data-ttu-id="0e11e-235">セッションは、`Context.Session` を使用して Razor ページまたはビューからアクセスします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-235">Session is accessed from a Razor Page or view with `Context.Session`:</span></span>

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

<span data-ttu-id="0e11e-236">セッションは、`PageModel` クラスまたは `HttpContext.Session` を使用したコントローラーからアクセスします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-236">Session is accessed from a `PageModel` class or controller with `HttpContext.Session`.</span></span> <span data-ttu-id="0e11e-237">このプロパティは [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 実装です。</span><span class="sxs-lookup"><span data-stu-id="0e11e-237">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="0e11e-238">次の例では、int と文字列が設定され、取得されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-238">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="0e11e-239">次の拡張メソッドを追加すると、シリアル化可能なオブジェクトを Session に設定し、取得できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-239">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="0e11e-240">次のサンプルでは、シリアル化可能なオブジェクトを設定し、取得する方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-240">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a><span data-ttu-id="0e11e-241">HttpContext.Items を使用する</span><span class="sxs-lookup"><span data-stu-id="0e11e-241">Working with HttpContext.Items</span></span>

<span data-ttu-id="0e11e-242">`HttpContext` 抽象化では、`Items` と呼ばれている、型 `IDictionary<object, object>` のディクショナリ コレクションがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-242">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="0e11e-243">このコレクションは *HttpRequest* の開始から利用できます。各要求の終わりに破棄されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-243">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="0e11e-244">キー付きエントリに値を割り当てるか、特定のキーの値を要求することでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-244">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="0e11e-245">次のサンプルでは、[Middleware](xref:fundamentals/middleware/index) により `isVerified` が `Items` コレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-245">In the following sample, [Middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="0e11e-246">後のパイプラインで別のミドルウェアがこれにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-246">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

<span data-ttu-id="0e11e-247">1 つのアプリでのみ使用されるミドルウェアの場合、`string` キーが許容されます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-247">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="0e11e-248">ただし、アプリケーション間で共有されるミドルウェアの場合、キーの競合を回避するために、一意のオブジェクト キーを利用してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-248">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="0e11e-249">複数のアプリケーションで動作させるミドルウェアを開発する場合、下の図のように、ミドルウェア クラスに定義されている一意のオブジェクト キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-249">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="0e11e-250">その他のコードは、ミドルウェア クラスで公開されるキーを利用し、`HttpContext.Items` に格納されている値にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-250">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="0e11e-251">この手法には、コード内の複数の場所で "鍵になる文字列" を繰り返すことをなくすという利点もあります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-251">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

## <a name="application-state-data"></a><span data-ttu-id="0e11e-252">アプリケーション状態データ</span><span class="sxs-lookup"><span data-stu-id="0e11e-252">Application state data</span></span>

<span data-ttu-id="0e11e-253">[依存関係の注入](xref:fundamentals/dependency-injection)を利用し、すべてのユーザーがデータを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0e11e-253">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="0e11e-254">データに含まれているサービスを定義します (たとえば、`MyAppData` という名前のクラス)。</span><span class="sxs-lookup"><span data-stu-id="0e11e-254">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. <span data-ttu-id="0e11e-255">サービス クラスを `ConfigureServices` に追加します (`services.AddSingleton<MyAppData>();` など)。</span><span class="sxs-lookup"><span data-stu-id="0e11e-255">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>

3. <span data-ttu-id="0e11e-256">各コントローラーのデータ サービス クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-256">Consume the data service class in each controller:</span></span>

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

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="0e11e-257">セッションの使用時の一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="0e11e-257">Common errors when working with session</span></span>

* <span data-ttu-id="0e11e-258">"'Microsoft.AspNetCore.Session.DistributedSessionStore' を起動しようとしましたが、型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' のサービスを解決できません。"</span><span class="sxs-lookup"><span data-stu-id="0e11e-258">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="0e11e-259">これは通常、少なくとも 1 つの `IDistributedCache` 実装で構成に失敗したことで発生します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-259">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="0e11e-260">詳細については、[分散キャッシュの使用](xref:performance/caching/distributed)と、[メモリ内キャッシュ](xref:performance/caching/memory)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e11e-260">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="0e11e-261">セッション ミドルウェアがセッションを永続化できなかった場合 (データベースが利用できなかったなど)、例外をログに記録し、受け取られます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-261">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="0e11e-262">要求は通常どおり続行され、まったく予想できない動作につながります。</span><span class="sxs-lookup"><span data-stu-id="0e11e-262">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="0e11e-263">典型的な例:</span><span class="sxs-lookup"><span data-stu-id="0e11e-263">A typical example:</span></span>

<span data-ttu-id="0e11e-264">誰かがセッションに買い物カゴを保管します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-264">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="0e11e-265">アイテムを追加しますが、コミットが失敗します。</span><span class="sxs-lookup"><span data-stu-id="0e11e-265">The user adds an item but the commit fails.</span></span> <span data-ttu-id="0e11e-266">アプリはこの失敗を認識せず、"アイテムが追加されました" というメッセージを伝えますが、それは本当ではありません。</span><span class="sxs-lookup"><span data-stu-id="0e11e-266">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="0e11e-267">このようなエラーを確認するために推奨される方法は、セッションへの書き込みを終えた時点でアプリ コードから `await feature.Session.CommitAsync();` を呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="0e11e-267">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="0e11e-268">その後、エラーを自由に処理できます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-268">Then you can do what you like with the error.</span></span> <span data-ttu-id="0e11e-269">`LoadAsync` を呼び出すとき、同様の動作が行われます。</span><span class="sxs-lookup"><span data-stu-id="0e11e-269">It works the same way when calling `LoadAsync`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="0e11e-270">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0e11e-270">Additional resources</span></span>

* [<span data-ttu-id="0e11e-271">ASP.NET Core 1.x: このドキュメントで使用されたサンプル コード</span><span class="sxs-lookup"><span data-stu-id="0e11e-271">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="0e11e-272">ASP.NET Core 2.x: このドキュメントで使用されたサンプル コード</span><span class="sxs-lookup"><span data-stu-id="0e11e-272">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
