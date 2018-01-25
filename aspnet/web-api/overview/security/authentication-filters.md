---
uid: web-api/overview/security/authentication-filters
title: "ASP.NET Web API 2 の認証フィルター |Microsoft ドキュメント"
author: MikeWasson
description: "認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターをサポートして両方 web API 2 と MVC 5 が若干異なるしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 7c704cc351876b49ec143a49b25cc0ca83876e06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a><span data-ttu-id="326b8-104">ASP.NET Web API 2 の認証フィルター</span><span class="sxs-lookup"><span data-stu-id="326b8-104">Authentication Filters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="326b8-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="326b8-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="326b8-106">認証フィルターは、HTTP 要求を認証するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="326b8-106">An authentication filter is a component that authenticates an HTTP request.</span></span> <span data-ttu-id="326b8-107">認証フィルターをサポート両方 web API 2 と MVC 5 がフィルター インターフェイスの名前付け規則でほとんどの場合、少しが異なります。</span><span class="sxs-lookup"><span data-stu-id="326b8-107">Web API 2 and MVC 5 both support authentication filters, but they differ slightly, mostly in the naming conventions for the filter interface.</span></span> <span data-ttu-id="326b8-108">このトピックでは、Web API 認証フィルターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="326b8-108">This topic describes Web API authentication filters.</span></span>


<span data-ttu-id="326b8-109">認証フィルターを使用して、個々 のコント ローラーまたはアクションの認証方式を設定できます。</span><span class="sxs-lookup"><span data-stu-id="326b8-109">Authentication filters let you set an authentication scheme for individual controllers or actions.</span></span> <span data-ttu-id="326b8-110">このように、アプリは、さまざまな HTTP リソースに対して異なる認証メカニズムをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="326b8-110">That way, your app can support different authentication mechanisms for different HTTP resources.</span></span>

<span data-ttu-id="326b8-111">この記事で紹介からコードを[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)のサンプル[http://aspnet.codeplex.com](http://aspnet.codeplex.com)です。このサンプルでは、HTTP 基本アクセス認証方式 (RFC 2617) を実装する認証フィルターを示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-111">In this article, I'll show code from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample on [http://aspnet.codeplex.com](http://aspnet.codeplex.com). The sample shows an authentication filter that implements the HTTP Basic Access Authentication scheme (RFC 2617).</span></span> <span data-ttu-id="326b8-112">フィルターがという名前のクラスで実装されている`IdentityBasicAuthenticationAttribute`です。</span><span class="sxs-lookup"><span data-stu-id="326b8-112">The filter is implemented in a class named `IdentityBasicAuthenticationAttribute`.</span></span> <span data-ttu-id="326b8-113">私は表示されませんのすべてのサンプル コード認証フィルターを作成する方法を説明する部分だけです。</span><span class="sxs-lookup"><span data-stu-id="326b8-113">I won't show all of the code from the sample, just the parts that illustrate how to write an authentication filter.</span></span>

## <a name="setting-an-authentication-filter"></a><span data-ttu-id="326b8-114">認証フィルターを設定</span><span class="sxs-lookup"><span data-stu-id="326b8-114">Setting an Authentication Filter</span></span>

<span data-ttu-id="326b8-115">他のフィルターと同様には、認証フィルターは適用されている各コント ローラー、アクション、またはをグローバルにすべての Web API コント ローラーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="326b8-115">Like other filters, authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.</span></span>

<span data-ttu-id="326b8-116">コント ローラーに認証フィルターを適用するには、フィルター属性でコント ローラー クラスを装飾します。</span><span class="sxs-lookup"><span data-stu-id="326b8-116">To apply an authentication filter to a controller, decorate the controller class with the filter attribute.</span></span> <span data-ttu-id="326b8-117">次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのアクションのすべての基本認証を有効にするコント ローラー クラスでフィルターします。</span><span class="sxs-lookup"><span data-stu-id="326b8-117">The following code sets the `[IdentityBasicAuthentication]` filter on a controller class, which enables Basic Authentication for all of the controller's actions.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

<span data-ttu-id="326b8-118">1 つのアクションにフィルターを適用するには、フィルターでアクションを装飾します。</span><span class="sxs-lookup"><span data-stu-id="326b8-118">To apply the filter to one action, decorate the action with the filter.</span></span> <span data-ttu-id="326b8-119">次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのフィルター`Post`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="326b8-119">The following code sets the `[IdentityBasicAuthentication]` filter on the controller's `Post` method.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

<span data-ttu-id="326b8-120">フィルターを適用する、すべての Web API コント ローラーに、追加する**GlobalConfiguration.Filters**です。</span><span class="sxs-lookup"><span data-stu-id="326b8-120">To apply the filter to all Web API controllers, add it to **GlobalConfiguration.Filters**.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a><span data-ttu-id="326b8-121">Web API 認証フィルターを実装します。</span><span class="sxs-lookup"><span data-stu-id="326b8-121">Implementing a Web API Authentication Filter</span></span>

<span data-ttu-id="326b8-122">Web API では認証フィルターを実装して、 [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="326b8-122">In Web API, authentication filters implement the [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface.</span></span> <span data-ttu-id="326b8-123">継承する必要がありますも**System.Attribute**、属性として適用するためにします。</span><span class="sxs-lookup"><span data-stu-id="326b8-123">They should also inherit from **System.Attribute**, in order to be applied as attributes.</span></span>

<span data-ttu-id="326b8-124">**IAuthenticationFilter**インターフェイスが 2 つのメソッドには。</span><span class="sxs-lookup"><span data-stu-id="326b8-124">The **IAuthenticationFilter** interface has two methods:</span></span>

- <span data-ttu-id="326b8-125">**AuthenticateAsync**存在する場合、要求内の資格情報を検証することによって、要求を認証します。</span><span class="sxs-lookup"><span data-stu-id="326b8-125">**AuthenticateAsync** authenticates the request by validating credentials in the request, if present.</span></span>
- <span data-ttu-id="326b8-126">**ChallengeAsync**必要な場合は、HTTP 応答に認証チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="326b8-126">**ChallengeAsync** adds an authentication challenge to the HTTP response, if needed.</span></span>

<span data-ttu-id="326b8-127">これらのメソッドがで定義されている認証フローに対応して[RFC 2612](http://tools.ietf.org/html/rfc2616)と[RFC 2617](http://tools.ietf.org/html/rfc2617):</span><span class="sxs-lookup"><span data-stu-id="326b8-127">These methods correspond to the authentication flow defined in [RFC 2612](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617):</span></span>

1. <span data-ttu-id="326b8-128">クライアントは、承認ヘッダーに資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="326b8-128">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="326b8-129">これは通常、クライアントがサーバーから 401 (未承認) 応答を受信した後に発生します。</span><span class="sxs-lookup"><span data-stu-id="326b8-129">This typically happens after the client receives a 401 (Unauthorized) response from the server.</span></span> <span data-ttu-id="326b8-130">ただし、クライアントは、401 を取得した後だけでなく、要求に資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="326b8-130">However, a client can send credentials with any request, not just after getting a 401.</span></span>
2. <span data-ttu-id="326b8-131">サーバーが資格情報を受け付けない場合は、401 (未承認) 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="326b8-131">If the server does not accept the credentials, it returns a 401 (Unauthorized) response.</span></span> <span data-ttu-id="326b8-132">応答には、1 つまたは複数の課題を表す Www-authenticate ヘッダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="326b8-132">The response includes a Www-Authenticate header that contains one or more challenges.</span></span> <span data-ttu-id="326b8-133">各チャレンジは、サーバーによって認識される認証スキームを指定します。</span><span class="sxs-lookup"><span data-stu-id="326b8-133">Each challenge specifies an authentication scheme recognized by the server.</span></span>

<span data-ttu-id="326b8-134">サーバーでは、匿名の要求から 401 を返すこともできます。</span><span class="sxs-lookup"><span data-stu-id="326b8-134">The server can also return 401 from an anonymous request.</span></span> <span data-ttu-id="326b8-135">実際には、通常、これは、認証プロセスを開始する方法です。</span><span class="sxs-lookup"><span data-stu-id="326b8-135">In fact, that's typically how the authentication process is initiated:</span></span>

1. <span data-ttu-id="326b8-136">クライアントは、匿名の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="326b8-136">The client sends an anonymous request.</span></span>
2. <span data-ttu-id="326b8-137">サーバーでは、401 を返します。</span><span class="sxs-lookup"><span data-stu-id="326b8-137">The server returns 401.</span></span>
3. <span data-ttu-id="326b8-138">クライアントは、資格情報を持つ要求を再送信します。</span><span class="sxs-lookup"><span data-stu-id="326b8-138">The clients resends the request with credentials.</span></span>

<span data-ttu-id="326b8-139">このフローには、両方が含まれます*認証*と*承認*手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="326b8-139">This flow includes both *authentication* and *authorization* steps.</span></span>

- <span data-ttu-id="326b8-140">認証は、クライアントの身元を証明します。</span><span class="sxs-lookup"><span data-stu-id="326b8-140">Authentication proves the identity of the client.</span></span>
- <span data-ttu-id="326b8-141">承認は、クライアントが特定のリソースにアクセスできるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="326b8-141">Authorization determines whether the client can access a particular resource.</span></span>

<span data-ttu-id="326b8-142">Web API では、認証フィルターは、認証が承認されませんを処理します。</span><span class="sxs-lookup"><span data-stu-id="326b8-142">In Web API, authentication filters handle authentication, but not authorization.</span></span> <span data-ttu-id="326b8-143">承認フィルターまたはコント ローラー アクションの内部は、承認を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="326b8-143">Authorization should be done by an authorization filter or inside the controller action.</span></span>

<span data-ttu-id="326b8-144">Web API 2 のパイプラインの流れを次に示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-144">Here is the flow in the Web API 2 pipeline:</span></span>

1. <span data-ttu-id="326b8-145">アクションを呼び出す前に、Web API は、その操作で使用する認証フィルターの一覧を作成します。</span><span class="sxs-lookup"><span data-stu-id="326b8-145">Before invoking an action, Web API creates a list of the authentication filters for that action.</span></span> <span data-ttu-id="326b8-146">これには、アクション スコープ、コント ローラーのスコープ、およびグローバル スコープでフィルターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="326b8-146">This includes filters with action scope, controller scope, and global scope.</span></span>
2. <span data-ttu-id="326b8-147">Web API 呼び出し**AuthenticateAsync**リスト内のすべてのフィルターにします。</span><span class="sxs-lookup"><span data-stu-id="326b8-147">Web API calls **AuthenticateAsync** on every filter in the list.</span></span> <span data-ttu-id="326b8-148">各フィルターは、要求に資格情報を検証できます。</span><span class="sxs-lookup"><span data-stu-id="326b8-148">Each filter can validate credentials in the request.</span></span> <span data-ttu-id="326b8-149">フィルターを作成している場合は任意のフィルターは、資格情報を正常に検証、 **IPrincipal**を要求にアタッチします。</span><span class="sxs-lookup"><span data-stu-id="326b8-149">If any filter successfully validates credentials, the filter creates an **IPrincipal** and attaches it to the request.</span></span> <span data-ttu-id="326b8-150">フィルターは、エラーをこの時点でトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="326b8-150">A filter can also trigger an error at this point.</span></span> <span data-ttu-id="326b8-151">その場合、パイプラインの残りの部分は実行されません。</span><span class="sxs-lookup"><span data-stu-id="326b8-151">If so, the rest of the pipeline does not run.</span></span>
3. <span data-ttu-id="326b8-152">要求は、エラーがないと仮定した場合、残りのパイプラインを経由して流れます。</span><span class="sxs-lookup"><span data-stu-id="326b8-152">Assuming there is no error, the request flows through the rest of the pipeline.</span></span>
4. <span data-ttu-id="326b8-153">Web API が最後に、すべて認証フィルターを呼び出して**ChallengeAsync**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="326b8-153">Finally, Web API calls every authentication filter's **ChallengeAsync** method.</span></span> <span data-ttu-id="326b8-154">フィルターは、必要な場合は、応答にチャレンジを追加するこのメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="326b8-154">Filters use this method to add a challenge to the response, if needed.</span></span> <span data-ttu-id="326b8-155">通常 (必ずではありませんが) ですが、発生する可能性、401 エラーに応答します。</span><span class="sxs-lookup"><span data-stu-id="326b8-155">Typically (but not always) that would happen in response to a 401 error.</span></span>

<span data-ttu-id="326b8-156">次の図は、次の 2 つの可能なケースを示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-156">The following diagrams show two possible cases.</span></span> <span data-ttu-id="326b8-157">最初の例で認証フィルターは、要求を正常に認証、承認フィルターは、要求を承認およびコント ローラーのアクションには、200 (OK) が返されます。</span><span class="sxs-lookup"><span data-stu-id="326b8-157">In the first, the authentication filter successfully authenticates the request, an authorization filter authorizes the request, and the controller action returns 200 (OK).</span></span>

![](authentication-filters/_static/image1.png)

<span data-ttu-id="326b8-158">2 番目の例では、要求を認証する認証フィルターが、承認フィルターが 401 (Unauthorized) を返します。</span><span class="sxs-lookup"><span data-stu-id="326b8-158">In the second example, the authentication filter authenticates the request, but the authorization filter returns 401 (Unauthorized).</span></span> <span data-ttu-id="326b8-159">この場合、コント ローラーのアクションは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="326b8-159">In this case, the controller action is not invoked.</span></span> <span data-ttu-id="326b8-160">認証フィルターは、Www 認証ヘッダーを応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="326b8-160">The authentication filter adds a Www-Authenticate header to the response.</span></span>

![](authentication-filters/_static/image2.png)

<span data-ttu-id="326b8-161">その他の組み合わせも有効である&mdash;など、コント ローラーのアクションは、匿名の要求を許可している場合が起き認証フィルターが、権限がありません。</span><span class="sxs-lookup"><span data-stu-id="326b8-161">Other combinations are possible&mdash;for example, if the controller action allows anonymous requests, you might have an authentication filter but no authorization.</span></span>

## <a name="implementing-the-authenticateasync-method"></a><span data-ttu-id="326b8-162">AuthenticateAsync メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="326b8-162">Implementing the AuthenticateAsync Method</span></span>

<span data-ttu-id="326b8-163">**AuthenticateAsync**メソッドは、要求を認証しようとしています。</span><span class="sxs-lookup"><span data-stu-id="326b8-163">The **AuthenticateAsync** method tries to authenticate the request.</span></span> <span data-ttu-id="326b8-164">メソッドのシグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-164">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

<span data-ttu-id="326b8-165">**AuthenticateAsync**メソッドは、次のいずれかを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="326b8-165">The **AuthenticateAsync** method must do one of the following:</span></span>

1. <span data-ttu-id="326b8-166">何も行われません (何も行いません)。</span><span class="sxs-lookup"><span data-stu-id="326b8-166">Nothing (no-op).</span></span>
2. <span data-ttu-id="326b8-167">作成、 **IPrincipal**し、要求に設定します。</span><span class="sxs-lookup"><span data-stu-id="326b8-167">Create an **IPrincipal** and set it on the request.</span></span>
3. <span data-ttu-id="326b8-168">エラーの結果を設定します。</span><span class="sxs-lookup"><span data-stu-id="326b8-168">Set an error result.</span></span>

<span data-ttu-id="326b8-169">オプション (1) では、要求には、フィルターを理解しているすべての資格情報がありませんでした。</span><span class="sxs-lookup"><span data-stu-id="326b8-169">Option (1) means the request did not have any credentials that the filter understands.</span></span> <span data-ttu-id="326b8-170">オプション (2) では、フィルターでは、要求が正常に認証されます。</span><span class="sxs-lookup"><span data-stu-id="326b8-170">Option (2) means the filter successfully authenticated the request.</span></span> <span data-ttu-id="326b8-171">オプション (3) では、要求は、エラー応答をトリガーする (正しくないパスワード) などの無効な資格情報をいました。</span><span class="sxs-lookup"><span data-stu-id="326b8-171">Option (3) means the request had invalid credentials (like the wrong password), which triggers an error response.</span></span>

<span data-ttu-id="326b8-172">実装するための一般的な概要を次に示します**AuthenticateAsync**です。</span><span class="sxs-lookup"><span data-stu-id="326b8-172">Here is a general outline for implementing **AuthenticateAsync**.</span></span>

1. <span data-ttu-id="326b8-173">要求で資格情報を参照してください。</span><span class="sxs-lookup"><span data-stu-id="326b8-173">Look for credentials in the request.</span></span>
2. <span data-ttu-id="326b8-174">資格情報がない場合は、何もしないし、(何も行いません) を返します。</span><span class="sxs-lookup"><span data-stu-id="326b8-174">If there are no credentials, do nothing and return (no-op).</span></span>
3. <span data-ttu-id="326b8-175">資格情報があるフィルターは、認証スキームを認識しない場合は、何もしないし、(何も行いません) を返します。</span><span class="sxs-lookup"><span data-stu-id="326b8-175">If there are credentials but the filter does not recognize the authentication scheme, do nothing and return (no-op).</span></span> <span data-ttu-id="326b8-176">パイプライン内の別のフィルターは、スキームを理解することがあります。</span><span class="sxs-lookup"><span data-stu-id="326b8-176">Another filter in the pipeline might understand the scheme.</span></span>
4. <span data-ttu-id="326b8-177">フィルターを認識する資格情報がある場合は、認証を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="326b8-177">If there are credentials that the filter understands, try to authenticate them.</span></span>
5. <span data-ttu-id="326b8-178">資格情報が正しくない場合は、設定して 401 を返す`context.ErrorResult`です。</span><span class="sxs-lookup"><span data-stu-id="326b8-178">If the credentials are bad, return 401 by setting `context.ErrorResult`.</span></span>
6. <span data-ttu-id="326b8-179">資格情報が有効な場合は、作成、 **IPrincipal**設定と`context.Principal`です。</span><span class="sxs-lookup"><span data-stu-id="326b8-179">If the credentials are valid, create an **IPrincipal** and set `context.Principal`.</span></span>

<span data-ttu-id="326b8-180">次のコードに示す、 **AuthenticateAsync**メソッドから、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)サンプルです。</span><span class="sxs-lookup"><span data-stu-id="326b8-180">The follow code shows the **AuthenticateAsync** method from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample.</span></span> <span data-ttu-id="326b8-181">コメントは、各手順を示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-181">The comments indicate each step.</span></span> <span data-ttu-id="326b8-182">コードは、いくつかの種類のエラー: ない資格情報、形式が正しくない資格情報、および不適切なユーザー名とパスワードと共に、Authorization ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="326b8-182">The code shows several types of error: An Authorization header with no credentials, malformed credentials, and bad username/password.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a><span data-ttu-id="326b8-183">エラーの結果を設定</span><span class="sxs-lookup"><span data-stu-id="326b8-183">Setting an Error Result</span></span>

<span data-ttu-id="326b8-184">フィルターを設定する必要があります、資格情報が有効でない場合`context.ErrorResult`を**IHttpActionResult**エラー応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="326b8-184">If the credentials are invalid, the filter must set `context.ErrorResult` to an **IHttpActionResult** that creates an error response.</span></span> <span data-ttu-id="326b8-185">詳細については**IHttpActionResult**を参照してください[Web API 2 のアクション結果](../getting-started-with-aspnet-web-api/action-results.md)です。</span><span class="sxs-lookup"><span data-stu-id="326b8-185">For more information about **IHttpActionResult**, see [Action Results in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

<span data-ttu-id="326b8-186">基本認証のサンプルが含まれています、`AuthenticationFailureResult`この目的に適したクラスです。</span><span class="sxs-lookup"><span data-stu-id="326b8-186">The Basic Authentication sample includes an `AuthenticationFailureResult` class that is suitable for this purpose.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a><span data-ttu-id="326b8-187">ChallengeAsync を実装します。</span><span class="sxs-lookup"><span data-stu-id="326b8-187">Implementing ChallengeAsync</span></span>

<span data-ttu-id="326b8-188">目的、 **ChallengeAsync**メソッドは、必要な場合は、応答に認証チャレンジを追加するは。</span><span class="sxs-lookup"><span data-stu-id="326b8-188">The purpose of the **ChallengeAsync** method is to add authentication challenges to the response, if needed.</span></span> <span data-ttu-id="326b8-189">メソッドのシグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="326b8-189">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

<span data-ttu-id="326b8-190">メソッドは、要求パイプライン内のすべての認証フィルターで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="326b8-190">The method is called on every authentication filter in the request pipeline.</span></span>

<span data-ttu-id="326b8-191">理解することが重要**ChallengeAsync**が呼び出された*する前に*HTTP 応答が作成され、コント ローラー アクションの実行前にも可能性があります。</span><span class="sxs-lookup"><span data-stu-id="326b8-191">It's important to understand that **ChallengeAsync** is called *before* the HTTP response is created, and possibly even before the controller action runs.</span></span> <span data-ttu-id="326b8-192">ときに**ChallengeAsync**が呼び出されると、`context.Result`が含まれています、 **IHttpActionResult**、後で、HTTP 応答の作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="326b8-192">When **ChallengeAsync** is called, `context.Result` contains an **IHttpActionResult**, which is used later to create the HTTP response.</span></span> <span data-ttu-id="326b8-193">ため**ChallengeAsync**が呼び出されると、HTTP 応答についての情報がまだ決まっていません。</span><span class="sxs-lookup"><span data-stu-id="326b8-193">So when **ChallengeAsync** is called, you don't know anything about the HTTP response yet.</span></span> <span data-ttu-id="326b8-194">**ChallengeAsync**メソッドの元の値を置き換える必要があります`context.Result`を新しい**IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="326b8-194">The **ChallengeAsync** method should replace the original value of `context.Result` with a new **IHttpActionResult**.</span></span> <span data-ttu-id="326b8-195">これは、 **IHttpActionResult**元をラップする必要があります`context.Result`です。</span><span class="sxs-lookup"><span data-stu-id="326b8-195">This **IHttpActionResult** must wrap the original `context.Result`.</span></span>

![](authentication-filters/_static/image3.png)

<span data-ttu-id="326b8-196">元の名前を付けます**IHttpActionResult** 、*内部の結果*、および新しい**IHttpActionResult** 、*外部結果*です。</span><span class="sxs-lookup"><span data-stu-id="326b8-196">I'll call the original **IHttpActionResult** the *inner result*, and the new **IHttpActionResult** the *outer result*.</span></span> <span data-ttu-id="326b8-197">外側の結果では、次の操作を行います必要があります。</span><span class="sxs-lookup"><span data-stu-id="326b8-197">The outer result must do the following:</span></span>

1. <span data-ttu-id="326b8-198">HTTP 応答を作成する内部の結果を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="326b8-198">Invoke the inner result to create the HTTP response.</span></span>
2. <span data-ttu-id="326b8-199">応答を確認します。</span><span class="sxs-lookup"><span data-stu-id="326b8-199">Examine the response.</span></span>
3. <span data-ttu-id="326b8-200">必要な場合は、応答に認証チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="326b8-200">Add an authentication challenge to the response, if needed.</span></span>

<span data-ttu-id="326b8-201">次の例は、基本認証のサンプルから取得されます。</span><span class="sxs-lookup"><span data-stu-id="326b8-201">The following example is taken from the Basic Authentication sample.</span></span> <span data-ttu-id="326b8-202">定義する、 **IHttpActionResult**の外側の結果。</span><span class="sxs-lookup"><span data-stu-id="326b8-202">It defines an **IHttpActionResult** for the outer result.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

<span data-ttu-id="326b8-203">`InnerResult`プロパティは、内部、保持**IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="326b8-203">The `InnerResult` property holds the inner **IHttpActionResult**.</span></span> <span data-ttu-id="326b8-204">`Challenge`プロパティは、Www 認証ヘッダーを表します。</span><span class="sxs-lookup"><span data-stu-id="326b8-204">The `Challenge` property represents a Www-Authentication header.</span></span> <span data-ttu-id="326b8-205">注意して**ExecuteAsync** 、まず呼び出し`InnerResult.ExecuteAsync`を HTTP 応答を作成し、必要な場合は、チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="326b8-205">Notice that **ExecuteAsync** first calls `InnerResult.ExecuteAsync` to create the HTTP response, and then adds the challenge if needed.</span></span>

<span data-ttu-id="326b8-206">チャレンジを追加する前に、応答コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="326b8-206">Check the response code before adding the challenge.</span></span> <span data-ttu-id="326b8-207">ほとんどの認証スキームのみチャレンジを追加する場合は次のように、応答は 401、します。</span><span class="sxs-lookup"><span data-stu-id="326b8-207">Most authentication schemes only add a challenge if the response is 401, as shown here.</span></span> <span data-ttu-id="326b8-208">ただし、一部の認証スキームでは、成功の応答にチャレンジを追加しないでください。</span><span class="sxs-lookup"><span data-stu-id="326b8-208">However, some authentication schemes do add a challenge to a success response.</span></span> <span data-ttu-id="326b8-209">たとえばを参照してください[Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559)。</span><span class="sxs-lookup"><span data-stu-id="326b8-209">For example, see [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span></span>

<span data-ttu-id="326b8-210">指定された、`AddChallengeOnUnauthorizedResult`クラス、実際のコードで**ChallengeAsync**は単純です。</span><span class="sxs-lookup"><span data-stu-id="326b8-210">Given the `AddChallengeOnUnauthorizedResult` class, the actual code in **ChallengeAsync** is simple.</span></span> <span data-ttu-id="326b8-211">だけ、結果を作成してアタッチすることを`context.Result`です。</span><span class="sxs-lookup"><span data-stu-id="326b8-211">You just create the result and attach it to `context.Result`.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

<span data-ttu-id="326b8-212">注: 基本認証のサンプルを抽象化このロジックをビット拡張メソッドに配置します。</span><span class="sxs-lookup"><span data-stu-id="326b8-212">Note: The Basic Authentication sample abstracts this logic a bit, by placing it in an extension method.</span></span>

## <a name="combining-authentication-filters-with-host-level-authentication"></a><span data-ttu-id="326b8-213">ホスト レベルの認証で認証フィルターを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="326b8-213">Combining Authentication Filters with Host-Level Authentication</span></span>

<span data-ttu-id="326b8-214">「ホスト レベルの認証」では、要求に達すると、Web API フレームワークの前に (IIS など)、ホストによって認証が実行です。</span><span class="sxs-lookup"><span data-stu-id="326b8-214">"Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.</span></span>

<span data-ttu-id="326b8-215">多くの場合、残り、アプリケーションのホスト レベルの認証を有効にするには、Web API コント ローラーに対して無効にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="326b8-215">Often, you may want to to enable host-level authentication for the rest of your application, but disable it for your Web API controllers.</span></span> <span data-ttu-id="326b8-216">たとえば、一般的なシナリオはホスト レベルでは、フォーム認証を有効にするが、Web API のトークン ベースの認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="326b8-216">For example, a typical scenario is to enable Forms Authentication at the host level, but use token-based authentication for Web API.</span></span>

<span data-ttu-id="326b8-217">Web API パイプライン内のホスト レベルの認証を無効にするを呼び出す`config.SuppressHostPrincipal()`構成にします。</span><span class="sxs-lookup"><span data-stu-id="326b8-217">To disable host-level authentication inside the Web API pipeline, call `config.SuppressHostPrincipal()` in your configuration.</span></span> <span data-ttu-id="326b8-218">これが原因で削除する Web API、 **IPrincipal** Web API パイプラインが入力したすべての要求からです。</span><span class="sxs-lookup"><span data-stu-id="326b8-218">This causes Web API to remove the **IPrincipal** from any request that enters the Web API pipeline.</span></span> <span data-ttu-id="326b8-219">実際には、その&quot;解除-認証&quot;要求します。</span><span class="sxs-lookup"><span data-stu-id="326b8-219">Effectively, it &quot;un-authenticates&quot; the request.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="326b8-220">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="326b8-220">Additional Resources</span></span>

<span data-ttu-id="326b8-221">[ASP.NET Web API のセキュリティ フィルターの](https://msdn.microsoft.com/magazine/dn781361.aspx)(MSDN マガジン)</span><span class="sxs-lookup"><span data-stu-id="326b8-221">[ASP.NET Web API Security Filters](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)</span></span>
