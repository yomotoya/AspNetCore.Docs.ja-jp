---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2 での認証フィルター |Microsoft Docs
author: MikeWasson
description: 認証フィルターは、HTTP 要求を認証するコンポーネントです。 認証フィルターを両方サポート web API 2 と MVC 5 がこれらとは若干.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: be2dcb246597f90ed7f00b2cf647b92e44aa254c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385678"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a><span data-ttu-id="5e72f-104">ASP.NET Web API 2 での認証フィルター</span><span class="sxs-lookup"><span data-stu-id="5e72f-104">Authentication Filters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="5e72f-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e72f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5e72f-106">認証フィルターは、HTTP 要求を認証するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="5e72f-106">An authentication filter is a component that authenticates an HTTP request.</span></span> <span data-ttu-id="5e72f-107">認証フィルターを両方サポート web API 2 と MVC 5 がフィルター インターフェイスの名前付け規則のほとんどの場合、少しが異なります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-107">Web API 2 and MVC 5 both support authentication filters, but they differ slightly, mostly in the naming conventions for the filter interface.</span></span> <span data-ttu-id="5e72f-108">このトピックでは、Web API 認証フィルターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-108">This topic describes Web API authentication filters.</span></span>


<span data-ttu-id="5e72f-109">認証フィルターを使用して、個々 のコント ローラーまたはアクションの認証方式を設定できます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-109">Authentication filters let you set an authentication scheme for individual controllers or actions.</span></span> <span data-ttu-id="5e72f-110">これにより、アプリは、さまざまな HTTP リソースを異なる認証メカニズムをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-110">That way, your app can support different authentication mechanisms for different HTTP resources.</span></span>

<span data-ttu-id="5e72f-111">この記事でのコードを紹介します、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)でサンプル[ http://aspnet.codeplex.com](http://aspnet.codeplex.com)します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-111">In this article, I'll show code from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample on [http://aspnet.codeplex.com](http://aspnet.codeplex.com).</span></span> <span data-ttu-id="5e72f-112">このサンプルでは、(RFC 2617) HTTP 基本アクセス認証スキームを実装する認証フィルターを示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-112">The sample shows an authentication filter that implements the HTTP Basic Access Authentication scheme (RFC 2617).</span></span> <span data-ttu-id="5e72f-113">フィルターがという名前のクラスで実装された`IdentityBasicAuthenticationAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-113">The filter is implemented in a class named `IdentityBasicAuthenticationAttribute`.</span></span> <span data-ttu-id="5e72f-114">見せしませんのサンプル コードはすべて認証フィルターを記述する方法を説明する部分だけです。</span><span class="sxs-lookup"><span data-stu-id="5e72f-114">I won't show all of the code from the sample, just the parts that illustrate how to write an authentication filter.</span></span>

## <a name="setting-an-authentication-filter"></a><span data-ttu-id="5e72f-115">認証フィルターの設定</span><span class="sxs-lookup"><span data-stu-id="5e72f-115">Setting an Authentication Filter</span></span>

<span data-ttu-id="5e72f-116">他のフィルターのような認証フィルターは、適用されたコント ローラーごと、アクション、またはにグローバルにすべての Web API コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="5e72f-116">Like other filters, authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.</span></span>

<span data-ttu-id="5e72f-117">コント ローラーには、認証フィルターを適用するには、フィルター属性でコント ローラー クラスを装飾します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-117">To apply an authentication filter to a controller, decorate the controller class with the filter attribute.</span></span> <span data-ttu-id="5e72f-118">次のコード セット、`[IdentityBasicAuthentication]`コント ローラー クラスは、すべてのコント ローラーのアクションの基本認証を有効にするフィルター。</span><span class="sxs-lookup"><span data-stu-id="5e72f-118">The following code sets the `[IdentityBasicAuthentication]` filter on a controller class, which enables Basic Authentication for all of the controller's actions.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

<span data-ttu-id="5e72f-119">1 つのアクションには、フィルターを適用するには、フィルターでアクションを装飾します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-119">To apply the filter to one action, decorate the action with the filter.</span></span> <span data-ttu-id="5e72f-120">次のコード セット、`[IdentityBasicAuthentication]`コント ローラーのフィルター`Post`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5e72f-120">The following code sets the `[IdentityBasicAuthentication]` filter on the controller's `Post` method.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

<span data-ttu-id="5e72f-121">追加するすべての Web API コント ローラーに、フィルターを適用する**GlobalConfiguration.Filters**します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-121">To apply the filter to all Web API controllers, add it to **GlobalConfiguration.Filters**.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a><span data-ttu-id="5e72f-122">Web API の認証フィルターを実装します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-122">Implementing a Web API Authentication Filter</span></span>

<span data-ttu-id="5e72f-123">Web api では、認証フィルターを実装、 [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="5e72f-123">In Web API, authentication filters implement the [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface.</span></span> <span data-ttu-id="5e72f-124">継承する必要がありますも**System.Attribute**、属性として適用するためにします。</span><span class="sxs-lookup"><span data-stu-id="5e72f-124">They should also inherit from **System.Attribute**, in order to be applied as attributes.</span></span>

<span data-ttu-id="5e72f-125">**IAuthenticationFilter**インターフェイスが 2 つのメソッドには。</span><span class="sxs-lookup"><span data-stu-id="5e72f-125">The **IAuthenticationFilter** interface has two methods:</span></span>

- <span data-ttu-id="5e72f-126">**AuthenticateAsync**存在する場合、要求で資格情報を検証することで要求を認証します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-126">**AuthenticateAsync** authenticates the request by validating credentials in the request, if present.</span></span>
- <span data-ttu-id="5e72f-127">**ChallengeAsync**必要な場合に、HTTP 応答に認証チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-127">**ChallengeAsync** adds an authentication challenge to the HTTP response, if needed.</span></span>

<span data-ttu-id="5e72f-128">これらのメソッドで定義されている認証フローに対応して[RFC 2612](http://tools.ietf.org/html/rfc2616)と[RFC 2617](http://tools.ietf.org/html/rfc2617):</span><span class="sxs-lookup"><span data-stu-id="5e72f-128">These methods correspond to the authentication flow defined in [RFC 2612](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617):</span></span>

1. <span data-ttu-id="5e72f-129">クライアントは、Authorization ヘッダーで、資格情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-129">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="5e72f-130">これは通常、クライアントがサーバーから 401 (未承認) 応答を受信した後に発生します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-130">This typically happens after the client receives a 401 (Unauthorized) response from the server.</span></span> <span data-ttu-id="5e72f-131">ただし、クライアントは、401 を取得した後だけでなく、すべての要求で資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-131">However, a client can send credentials with any request, not just after getting a 401.</span></span>
2. <span data-ttu-id="5e72f-132">サーバーが資格情報を受け付けない場合は、401 (未承認) 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-132">If the server does not accept the credentials, it returns a 401 (Unauthorized) response.</span></span> <span data-ttu-id="5e72f-133">応答には、1 つまたは複数の課題を含む Www-authenticate ヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5e72f-133">The response includes a Www-Authenticate header that contains one or more challenges.</span></span> <span data-ttu-id="5e72f-134">各チャレンジでは、サーバーによって認識される認証方式を指定します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-134">Each challenge specifies an authentication scheme recognized by the server.</span></span>

<span data-ttu-id="5e72f-135">サーバーでは、匿名の要求から 401 を返すこともできます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-135">The server can also return 401 from an anonymous request.</span></span> <span data-ttu-id="5e72f-136">実際には、通常、これは、認証プロセスを開始する方法です。</span><span class="sxs-lookup"><span data-stu-id="5e72f-136">In fact, that's typically how the authentication process is initiated:</span></span>

1. <span data-ttu-id="5e72f-137">クライアントは、匿名の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-137">The client sends an anonymous request.</span></span>
2. <span data-ttu-id="5e72f-138">サーバーは、401 を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-138">The server returns 401.</span></span>
3. <span data-ttu-id="5e72f-139">クライアントは、資格情報を持つ要求を再送信します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-139">The clients resends the request with credentials.</span></span>

<span data-ttu-id="5e72f-140">このフローでは、両方に含まれる*認証*と*承認*手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-140">This flow includes both *authentication* and *authorization* steps.</span></span>

- <span data-ttu-id="5e72f-141">認証では、クライアントの身元を証明します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-141">Authentication proves the identity of the client.</span></span>
- <span data-ttu-id="5e72f-142">承認は、クライアントが特定のリソースにアクセスできるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-142">Authorization determines whether the client can access a particular resource.</span></span>

<span data-ttu-id="5e72f-143">Web api では、認証フィルター処理が、認証、承認ではありません。</span><span class="sxs-lookup"><span data-stu-id="5e72f-143">In Web API, authentication filters handle authentication, but not authorization.</span></span> <span data-ttu-id="5e72f-144">承認フィルターまたはコント ローラー アクション内では、承認を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-144">Authorization should be done by an authorization filter or inside the controller action.</span></span>

<span data-ttu-id="5e72f-145">Web API 2 のパイプラインで、フローを示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-145">Here is the flow in the Web API 2 pipeline:</span></span>

1. <span data-ttu-id="5e72f-146">アクションを呼び出す前に、Web API は、そのアクションの認証フィルターの一覧を作成します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-146">Before invoking an action, Web API creates a list of the authentication filters for that action.</span></span> <span data-ttu-id="5e72f-147">これには、アクションの範囲、コント ローラーのスコープ、およびグローバル スコープを使用したフィルターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-147">This includes filters with action scope, controller scope, and global scope.</span></span>
2. <span data-ttu-id="5e72f-148">Web API 呼び出し**AuthenticateAsync**一覧内のすべてのフィルターにします。</span><span class="sxs-lookup"><span data-stu-id="5e72f-148">Web API calls **AuthenticateAsync** on every filter in the list.</span></span> <span data-ttu-id="5e72f-149">各フィルターは、要求に資格情報を検証できます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-149">Each filter can validate credentials in the request.</span></span> <span data-ttu-id="5e72f-150">任意のフィルターに資格情報が正常に検証する場合、フィルターを作成、 **IPrincipal**要求にアタッチします。</span><span class="sxs-lookup"><span data-stu-id="5e72f-150">If any filter successfully validates credentials, the filter creates an **IPrincipal** and attaches it to the request.</span></span> <span data-ttu-id="5e72f-151">フィルターはこの時点でエラーをトリガーもできます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-151">A filter can also trigger an error at this point.</span></span> <span data-ttu-id="5e72f-152">そうである場合、パイプラインの残りの部分は実行されません。</span><span class="sxs-lookup"><span data-stu-id="5e72f-152">If so, the rest of the pipeline does not run.</span></span>
3. <span data-ttu-id="5e72f-153">エラーがないと仮定すると、要求は、パイプラインの残りの部分をフローします。</span><span class="sxs-lookup"><span data-stu-id="5e72f-153">Assuming there is no error, the request flows through the rest of the pipeline.</span></span>
4. <span data-ttu-id="5e72f-154">最後に、Web API 呼び出しはすべて認証フィルターの**ChallengeAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="5e72f-154">Finally, Web API calls every authentication filter's **ChallengeAsync** method.</span></span> <span data-ttu-id="5e72f-155">フィルターは、必要な場合、応答にチャレンジを追加するこのメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-155">Filters use this method to add a challenge to the response, if needed.</span></span> <span data-ttu-id="5e72f-156">通常 (が常にではありません) ですが、発生 401 エラーに応答します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-156">Typically (but not always) that would happen in response to a 401 error.</span></span>

<span data-ttu-id="5e72f-157">次の図は、2 つの可能なケースを示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-157">The following diagrams show two possible cases.</span></span> <span data-ttu-id="5e72f-158">最初の例では、認証フィルターは、要求を正常に認証、承認フィルターは、要求を承認およびコント ローラー アクションは、200 (OK) を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-158">In the first, the authentication filter successfully authenticates the request, an authorization filter authorizes the request, and the controller action returns 200 (OK).</span></span>

![](authentication-filters/_static/image1.png)

<span data-ttu-id="5e72f-159">2 番目の例では、認証フィルターは、要求を認証、承認フィルターは 401 (Unauthorized) を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-159">In the second example, the authentication filter authenticates the request, but the authorization filter returns 401 (Unauthorized).</span></span> <span data-ttu-id="5e72f-160">この場合、コント ローラー アクションは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="5e72f-160">In this case, the controller action is not invoked.</span></span> <span data-ttu-id="5e72f-161">認証フィルターは、Www 認証ヘッダーを応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-161">The authentication filter adds a Www-Authenticate header to the response.</span></span>

![](authentication-filters/_static/image2.png)

<span data-ttu-id="5e72f-162">その他の組み合わせが可能です。&mdash;など、コント ローラー アクションは、匿名の要求を許可している場合する必要がありますが、認証フィルター、承認なし。</span><span class="sxs-lookup"><span data-stu-id="5e72f-162">Other combinations are possible&mdash;for example, if the controller action allows anonymous requests, you might have an authentication filter but no authorization.</span></span>

## <a name="implementing-the-authenticateasync-method"></a><span data-ttu-id="5e72f-163">AuthenticateAsync メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-163">Implementing the AuthenticateAsync Method</span></span>

<span data-ttu-id="5e72f-164">**AuthenticateAsync**メソッドは、要求を認証しようとしています。</span><span class="sxs-lookup"><span data-stu-id="5e72f-164">The **AuthenticateAsync** method tries to authenticate the request.</span></span> <span data-ttu-id="5e72f-165">メソッド シグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-165">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

<span data-ttu-id="5e72f-166">**AuthenticateAsync**メソッドは、次のいずれかを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-166">The **AuthenticateAsync** method must do one of the following:</span></span>

1. <span data-ttu-id="5e72f-167">Nothing (操作なし)。</span><span class="sxs-lookup"><span data-stu-id="5e72f-167">Nothing (no-op).</span></span>
2. <span data-ttu-id="5e72f-168">作成、 **IPrincipal**し、要求に設定します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-168">Create an **IPrincipal** and set it on the request.</span></span>
3. <span data-ttu-id="5e72f-169">エラーの結果を設定します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-169">Set an error result.</span></span>

<span data-ttu-id="5e72f-170">オプション (1) では、要求には、フィルターを認識する資格情報がありませんでした。</span><span class="sxs-lookup"><span data-stu-id="5e72f-170">Option (1) means the request did not have any credentials that the filter understands.</span></span> <span data-ttu-id="5e72f-171">オプション (2) では、フィルターは、要求を正常に認証します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-171">Option (2) means the filter successfully authenticated the request.</span></span> <span data-ttu-id="5e72f-172">オプション (3) では、要求は、エラー応答をトリガーする (正しくないパスワード) のような無効な資格情報をいました。</span><span class="sxs-lookup"><span data-stu-id="5e72f-172">Option (3) means the request had invalid credentials (like the wrong password), which triggers an error response.</span></span>

<span data-ttu-id="5e72f-173">実装するための一般的な概要を次に示します**AuthenticateAsync**します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-173">Here is a general outline for implementing **AuthenticateAsync**.</span></span>

1. <span data-ttu-id="5e72f-174">要求で資格情報を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5e72f-174">Look for credentials in the request.</span></span>
2. <span data-ttu-id="5e72f-175">資格情報がない場合は、何もしないし、(操作なし) を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-175">If there are no credentials, do nothing and return (no-op).</span></span>
3. <span data-ttu-id="5e72f-176">資格情報が、フィルターは、認証スキームを認識しない場合、何もしないし、(操作なし) を返します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-176">If there are credentials but the filter does not recognize the authentication scheme, do nothing and return (no-op).</span></span> <span data-ttu-id="5e72f-177">パイプライン内の別のフィルターは、スキームを理解する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-177">Another filter in the pipeline might understand the scheme.</span></span>
4. <span data-ttu-id="5e72f-178">フィルターを認識する資格情報がある場合は、認証をお試しください。</span><span class="sxs-lookup"><span data-stu-id="5e72f-178">If there are credentials that the filter understands, try to authenticate them.</span></span>
5. <span data-ttu-id="5e72f-179">資格情報が正しくない場合は、401 を返すを設定して`context.ErrorResult`します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-179">If the credentials are bad, return 401 by setting `context.ErrorResult`.</span></span>
6. <span data-ttu-id="5e72f-180">資格情報が有効な場合は、作成、 **IPrincipal**設定と`context.Principal`します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-180">If the credentials are valid, create an **IPrincipal** and set `context.Principal`.</span></span>

<span data-ttu-id="5e72f-181">次のコードに示す、 **AuthenticateAsync**からメソッド、[基本認証](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)サンプル。</span><span class="sxs-lookup"><span data-stu-id="5e72f-181">The follow code shows the **AuthenticateAsync** method from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample.</span></span> <span data-ttu-id="5e72f-182">コメントは、各手順を示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-182">The comments indicate each step.</span></span> <span data-ttu-id="5e72f-183">コードでは、いくつかの種類のエラーを示しています。: ない資格情報、形式が正しくない資格情報、および不適切なユーザー名/パスワードで An Authorization ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="5e72f-183">The code shows several types of error: An Authorization header with no credentials, malformed credentials, and bad username/password.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a><span data-ttu-id="5e72f-184">エラーの結果を設定</span><span class="sxs-lookup"><span data-stu-id="5e72f-184">Setting an Error Result</span></span>

<span data-ttu-id="5e72f-185">資格情報が有効でない場合、フィルターを設定する必要があります`context.ErrorResult`を**IHttpActionResult**エラー応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-185">If the credentials are invalid, the filter must set `context.ErrorResult` to an **IHttpActionResult** that creates an error response.</span></span> <span data-ttu-id="5e72f-186">詳細については**IHttpActionResult**を参照してください[Web API 2 でアクションの結果](../getting-started-with-aspnet-web-api/action-results.md)します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-186">For more information about **IHttpActionResult**, see [Action Results in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

<span data-ttu-id="5e72f-187">基本認証のサンプルが含まれています、`AuthenticationFailureResult`はこの目的に適したクラスです。</span><span class="sxs-lookup"><span data-stu-id="5e72f-187">The Basic Authentication sample includes an `AuthenticationFailureResult` class that is suitable for this purpose.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a><span data-ttu-id="5e72f-188">ChallengeAsync を実装します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-188">Implementing ChallengeAsync</span></span>

<span data-ttu-id="5e72f-189">目的、 **ChallengeAsync**メソッドは、必要な場合、応答に認証チャレンジを追加するは。</span><span class="sxs-lookup"><span data-stu-id="5e72f-189">The purpose of the **ChallengeAsync** method is to add authentication challenges to the response, if needed.</span></span> <span data-ttu-id="5e72f-190">メソッド シグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-190">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

<span data-ttu-id="5e72f-191">メソッドは、要求パイプライン内のすべての認証フィルターで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-191">The method is called on every authentication filter in the request pipeline.</span></span>

<span data-ttu-id="5e72f-192">理解することが重要**ChallengeAsync**呼びます*する前に*HTTP 応答が作成、され、コント ローラー アクションを実行する前にも可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-192">It's important to understand that **ChallengeAsync** is called *before* the HTTP response is created, and possibly even before the controller action runs.</span></span> <span data-ttu-id="5e72f-193">ときに**ChallengeAsync**が呼び出され、`context.Result`が含まれています、 **IHttpActionResult**、HTTP 応答を作成する後で使用します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-193">When **ChallengeAsync** is called, `context.Result` contains an **IHttpActionResult**, which is used later to create the HTTP response.</span></span> <span data-ttu-id="5e72f-194">したがって**ChallengeAsync**が呼び出されると、何もわからない HTTP 応答についてまだします。</span><span class="sxs-lookup"><span data-stu-id="5e72f-194">So when **ChallengeAsync** is called, you don't know anything about the HTTP response yet.</span></span> <span data-ttu-id="5e72f-195">**ChallengeAsync**メソッドの元の値を置き換える必要があります`context.Result`を新しい**IHttpActionResult**します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-195">The **ChallengeAsync** method should replace the original value of `context.Result` with a new **IHttpActionResult**.</span></span> <span data-ttu-id="5e72f-196">これは、 **IHttpActionResult**元をラップする必要があります`context.Result`します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-196">This **IHttpActionResult** must wrap the original `context.Result`.</span></span>

![](authentication-filters/_static/image3.png)

<span data-ttu-id="5e72f-197">元と呼ぶことに**IHttpActionResult** 、*内部結果*、および新しい**IHttpActionResult** 、*外部結果*。</span><span class="sxs-lookup"><span data-stu-id="5e72f-197">I'll call the original **IHttpActionResult** the *inner result*, and the new **IHttpActionResult** the *outer result*.</span></span> <span data-ttu-id="5e72f-198">外部の結果では、次の操作を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-198">The outer result must do the following:</span></span>

1. <span data-ttu-id="5e72f-199">HTTP 応答を作成する内部の結果を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-199">Invoke the inner result to create the HTTP response.</span></span>
2. <span data-ttu-id="5e72f-200">応答を確認します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-200">Examine the response.</span></span>
3. <span data-ttu-id="5e72f-201">必要な場合は、応答に認証チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-201">Add an authentication challenge to the response, if needed.</span></span>

<span data-ttu-id="5e72f-202">次の例は、基本認証のサンプルから取得されます。</span><span class="sxs-lookup"><span data-stu-id="5e72f-202">The following example is taken from the Basic Authentication sample.</span></span> <span data-ttu-id="5e72f-203">定義、 **IHttpActionResult**の外側の結果。</span><span class="sxs-lookup"><span data-stu-id="5e72f-203">It defines an **IHttpActionResult** for the outer result.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

<span data-ttu-id="5e72f-204">`InnerResult`プロパティを保持する内部**IHttpActionResult**します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-204">The `InnerResult` property holds the inner **IHttpActionResult**.</span></span> <span data-ttu-id="5e72f-205">`Challenge`プロパティは、Www 認証ヘッダーを表します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-205">The `Challenge` property represents a Www-Authentication header.</span></span> <span data-ttu-id="5e72f-206">注意して**ExecuteAsync**まず呼び出し`InnerResult.ExecuteAsync`HTTP 応答を作成するために必要な場合に、チャレンジを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-206">Notice that **ExecuteAsync** first calls `InnerResult.ExecuteAsync` to create the HTTP response, and then adds the challenge if needed.</span></span>

<span data-ttu-id="5e72f-207">チャレンジを追加する前に、応答コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-207">Check the response code before adding the challenge.</span></span> <span data-ttu-id="5e72f-208">ほとんどの認証方式は、チャレンジを追加するだけ応答は、次に示すように、401 が場合。</span><span class="sxs-lookup"><span data-stu-id="5e72f-208">Most authentication schemes only add a challenge if the response is 401, as shown here.</span></span> <span data-ttu-id="5e72f-209">ただし、一部の認証スキームでは、成功応答にチャレンジを追加しないでください。</span><span class="sxs-lookup"><span data-stu-id="5e72f-209">However, some authentication schemes do add a challenge to a success response.</span></span> <span data-ttu-id="5e72f-210">たとえばを参照してください[ネゴシエート](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。</span><span class="sxs-lookup"><span data-stu-id="5e72f-210">For example, see [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span></span>

<span data-ttu-id="5e72f-211">指定された、`AddChallengeOnUnauthorizedResult`クラス、実際のコードで**ChallengeAsync**は単純です。</span><span class="sxs-lookup"><span data-stu-id="5e72f-211">Given the `AddChallengeOnUnauthorizedResult` class, the actual code in **ChallengeAsync** is simple.</span></span> <span data-ttu-id="5e72f-212">だけ、結果を作成およびアタッチ先`context.Result`します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-212">You just create the result and attach it to `context.Result`.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

<span data-ttu-id="5e72f-213">注: 基本認証のサンプルを抽象化このロジック、拡張メソッドに配置します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-213">Note: The Basic Authentication sample abstracts this logic a bit, by placing it in an extension method.</span></span>

## <a name="combining-authentication-filters-with-host-level-authentication"></a><span data-ttu-id="5e72f-214">ホスト レベルの認証を使用した認証フィルターを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="5e72f-214">Combining Authentication Filters with Host-Level Authentication</span></span>

<span data-ttu-id="5e72f-215">「ホスト レベルの認証」では前の要求に達すると、Web API フレームワークに、(IIS) などのホストによって認証が実行です。</span><span class="sxs-lookup"><span data-stu-id="5e72f-215">"Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.</span></span>

<span data-ttu-id="5e72f-216">多くの場合に、アプリケーションの残りのホスト レベルの認証を有効にするが、Web API コント ローラーに対して無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5e72f-216">Often, you may want to enable host-level authentication for the rest of your application, but disable it for your Web API controllers.</span></span> <span data-ttu-id="5e72f-217">たとえば、一般的なシナリオはホスト レベルでは、フォーム認証を有効にするが、Web API のトークン ベースの認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-217">For example, a typical scenario is to enable Forms Authentication at the host level, but use token-based authentication for Web API.</span></span>

<span data-ttu-id="5e72f-218">Web API パイプライン内のホスト レベルの認証を無効にするには、呼び出す`config.SuppressHostPrincipal()`構成します。</span><span class="sxs-lookup"><span data-stu-id="5e72f-218">To disable host-level authentication inside the Web API pipeline, call `config.SuppressHostPrincipal()` in your configuration.</span></span> <span data-ttu-id="5e72f-219">これが原因で削除する Web API、 **IPrincipal** Web API パイプラインが入力したすべての要求から。</span><span class="sxs-lookup"><span data-stu-id="5e72f-219">This causes Web API to remove the **IPrincipal** from any request that enters the Web API pipeline.</span></span> <span data-ttu-id="5e72f-220">実際には、その&quot;解除-認証&quot;要求。</span><span class="sxs-lookup"><span data-stu-id="5e72f-220">Effectively, it &quot;un-authenticates&quot; the request.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="5e72f-221">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5e72f-221">Additional Resources</span></span>

<span data-ttu-id="5e72f-222">[ASP.NET Web API のセキュリティ フィルター](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN マガジン)</span><span class="sxs-lookup"><span data-stu-id="5e72f-222">[ASP.NET Web API Security Filters](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)</span></span>
