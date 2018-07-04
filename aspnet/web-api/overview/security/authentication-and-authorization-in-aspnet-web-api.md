---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: ASP.NET Web API で認証と承認 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で認証と承認の概要を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 981eebeaa1daaf85cb90a52f073c88cb71099edb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365621"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a><span data-ttu-id="14e1b-103">ASP.NET Web API で認証と承認</span><span class="sxs-lookup"><span data-stu-id="14e1b-103">Authentication and Authorization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="14e1b-104">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="14e1b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="14e1b-105">Web API を作成したが、それへのアクセスを制御するようになりました。</span><span class="sxs-lookup"><span data-stu-id="14e1b-105">You've created a web API, but now you want to control access to it.</span></span> <span data-ttu-id="14e1b-106">この一連の記事では、承認されていないユーザーから web API をセキュリティで保護するためのいくつかのオプションを紹介します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-106">In this series of articles, we'll look at some options for securing a web API from unauthorized users.</span></span> <span data-ttu-id="14e1b-107">このシリーズは、認証と承認の両方について説明します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-107">This series will cover both authentication and authorization.</span></span>

- <span data-ttu-id="14e1b-108">*認証*ユーザーの id が把握することです。</span><span class="sxs-lookup"><span data-stu-id="14e1b-108">*Authentication* is knowing the identity of the user.</span></span> <span data-ttu-id="14e1b-109">たとえば、Alice は自分のユーザー名とパスワード、ログイン、サーバーでは、パスワードを使用して、Alice の認証。</span><span class="sxs-lookup"><span data-stu-id="14e1b-109">For example, Alice logs in with her username and password, and the server uses the password to authenticate Alice.</span></span>
- <span data-ttu-id="14e1b-110">*承認*アクションを実行するユーザーが許可されているかどうかを決めることです。</span><span class="sxs-lookup"><span data-stu-id="14e1b-110">*Authorization* is deciding whether a user is allowed to perform an action.</span></span> <span data-ttu-id="14e1b-111">たとえば、Alice、リソースを取得するが、リソースを作成できません権があるのです。</span><span class="sxs-lookup"><span data-stu-id="14e1b-111">For example, Alice has permission to get a resource but not create a resource.</span></span>

<span data-ttu-id="14e1b-112">シリーズの最初の記事では、ASP.NET Web API で認証と承認の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-112">The first article in the series gives a general overview of authentication and authorization in ASP.NET Web API.</span></span> <span data-ttu-id="14e1b-113">その他のトピックでは、Web API の一般的な認証シナリオについて説明します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-113">Other topics describe common authentication scenarios for Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="14e1b-114">このシリーズを確認し、貴重なフィードバックを提供するユーザーに協力してくれた: Rick Anderson、Levi Broderick、Barry Dorrans、Tom Dykstra、Hongmei Ge、David Matson、Daniel Roth、Tim Teebken します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-114">Thanks to the people who reviewed this series and provided valuable feedback: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.</span></span>


## <a name="authentication"></a><span data-ttu-id="14e1b-115">認証</span><span class="sxs-lookup"><span data-stu-id="14e1b-115">Authentication</span></span>

<span data-ttu-id="14e1b-116">Web API では、ホストでその認証を行いますを前提としています。</span><span class="sxs-lookup"><span data-stu-id="14e1b-116">Web API assumes that authentication happens in the host.</span></span> <span data-ttu-id="14e1b-117">Web ホスト、ホスト認証のための HTTP モジュールを使用して、IIS です。</span><span class="sxs-lookup"><span data-stu-id="14e1b-117">For web-hosting, the host is IIS, which uses HTTP modules for authentication.</span></span> <span data-ttu-id="14e1b-118">IIS または ASP.NET に組み込まれている認証モジュールのいずれかを使用するプロジェクトを構成したり、カスタム認証を実行する独自の HTTP モジュールを記述できます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-118">You can configure your project to use any of the authentication modules built in to IIS or ASP.NET, or write your own HTTP module to perform custom authentication.</span></span>

<span data-ttu-id="14e1b-119">作成、ホストが、ユーザーを認証するとき、*プリンシパル*は、 [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)コードが実行されているセキュリティ コンテキストを表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="14e1b-119">When the host authenticates the user, it creates a *principal*, which is an [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) object that represents the security context under which code is running.</span></span> <span data-ttu-id="14e1b-120">ホストを設定して現在のスレッドにプリンシパルをアタッチします**Thread.CurrentPrincipal**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-120">The host attaches the principal to the current thread by setting **Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="14e1b-121">プリンシパルが含まれていますが、関連付けられている**Identity**ユーザーに関する情報を含むオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="14e1b-121">The principal contains an associated **Identity** object that contains information about the user.</span></span> <span data-ttu-id="14e1b-122">ユーザーが認証されている場合、 **Identity.IsAuthenticated**プロパティが返す**true**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-122">If the user is authenticated, the **Identity.IsAuthenticated** property returns **true**.</span></span> <span data-ttu-id="14e1b-123">匿名の要求に対して**IsAuthenticated**返します**false**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-123">For anonymous requests, **IsAuthenticated** returns **false**.</span></span> <span data-ttu-id="14e1b-124">プリンシパルの詳細については、次を参照してください。[ロール ベース セキュリティ](https://msdn.microsoft.com/library/shz8h065.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-124">For more information about principals, see [Role-Based Security](https://msdn.microsoft.com/library/shz8h065.aspx).</span></span>

### <a name="http-message-handlers-for-authentication"></a><span data-ttu-id="14e1b-125">認証のための HTTP メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="14e1b-125">HTTP Message Handlers for Authentication</span></span>

<span data-ttu-id="14e1b-126">ホストの認証を使用する代わりに認証ロジックを配置することができます、 [HTTP メッセージ ハンドラー](../advanced/http-message-handlers.md)します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-126">Instead of using the host for authentication, you can put authentication logic into an [HTTP message handler](../advanced/http-message-handlers.md).</span></span> <span data-ttu-id="14e1b-127">その場合は、メッセージ ハンドラーでは、HTTP 要求を検査し、プリンシパルを設定します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-127">In that case, the message handler examines the HTTP request and sets the principal.</span></span>

<span data-ttu-id="14e1b-128">認証メッセージ ハンドラーを使用するにとする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="14e1b-128">When should you use message handlers for authentication?</span></span> <span data-ttu-id="14e1b-129">一部のトレードオフについて次に示します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-129">Here are some tradeoffs:</span></span>

- <span data-ttu-id="14e1b-130">HTTP モジュールでは、ASP.NET パイプラインを通過するすべての要求が表示されます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-130">An HTTP module sees all requests that go through the ASP.NET pipeline.</span></span> <span data-ttu-id="14e1b-131">メッセージ ハンドラーには、Web API にルーティングされる要求のみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-131">A message handler only sees requests that are routed to Web API.</span></span>
- <span data-ttu-id="14e1b-132">-ルート メッセージ ハンドラーでは、特定のルートへの認証方式を適用できる設定できます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-132">You can set per-route message handlers, which lets you apply an authentication scheme to a specific route.</span></span>
- <span data-ttu-id="14e1b-133">HTTP モジュールは、IIS に固有です。</span><span class="sxs-lookup"><span data-stu-id="14e1b-133">HTTP modules are specific to IIS.</span></span> <span data-ttu-id="14e1b-134">メッセージ ハンドラーは、web ホスティングと自己ホスト両方で使えるように、ホストに依存しないをします。</span><span class="sxs-lookup"><span data-stu-id="14e1b-134">Message handlers are host-agnostic, so they can be used with both web-hosting and self-hosting.</span></span>
- <span data-ttu-id="14e1b-135">HTTP モジュールは、IIS ログ、監査、および具合に参加します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-135">HTTP modules participate in IIS logging, auditing, and so on.</span></span>
- <span data-ttu-id="14e1b-136">HTTP モジュールは、パイプラインの前で実行します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-136">HTTP modules run earlier in the pipeline.</span></span> <span data-ttu-id="14e1b-137">メッセージ ハンドラーでの認証を処理する場合、プリンシパルはまで設定されません、ハンドラーが実行されます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-137">If you handle authentication in a message handler, the principal does not get set until the handler runs.</span></span> <span data-ttu-id="14e1b-138">さらに、応答が、メッセージ ハンドラーを離れると、プリンシパルは、以前のプリンシパルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="14e1b-138">Moreover, the principal reverts back to the previous principal when the response leaves the message handler.</span></span>

<span data-ttu-id="14e1b-139">一般に、自己ホストをサポートするために不要な場合は、HTTP モジュールはより適切なオプションには。</span><span class="sxs-lookup"><span data-stu-id="14e1b-139">Generally, if you don't need to support self-hosting, an HTTP module is a better option.</span></span> <span data-ttu-id="14e1b-140">自己ホストをサポートする必要がある場合は、メッセージ ハンドラーを検討してください。</span><span class="sxs-lookup"><span data-stu-id="14e1b-140">If you need to support self-hosting, consider a message handler.</span></span>

### <a name="setting-the-principal"></a><span data-ttu-id="14e1b-141">プリンシパルの設定</span><span class="sxs-lookup"><span data-stu-id="14e1b-141">Setting the Principal</span></span>

<span data-ttu-id="14e1b-142">アプリケーションでは、任意のカスタム認証ロジックを実行する場合は、2 つの場所にプリンシパルを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="14e1b-142">If your application performs any custom authentication logic, you must set the principal on two places:</span></span>

- <span data-ttu-id="14e1b-143">**Thread.CurrentPrincipal**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-143">**Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="14e1b-144">このプロパティは、.net では、スレッドのプリンシパルを設定する標準的な方法です。</span><span class="sxs-lookup"><span data-stu-id="14e1b-144">This property is the standard way to set the thread's principal in .NET.</span></span>
- <span data-ttu-id="14e1b-145">**ロール**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-145">**HttpContext.Current.User**.</span></span> <span data-ttu-id="14e1b-146">このプロパティは、ASP.NET に固有です。</span><span class="sxs-lookup"><span data-stu-id="14e1b-146">This property is specific to ASP.NET.</span></span>

<span data-ttu-id="14e1b-147">次のコードでは、プリンシパルを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-147">The following code shows how to set the principal:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="14e1b-148">Web ホストの両方の場所で、プリンシパルを設定する必要があります。それ以外の場合、セキュリティ コンテキストは、一貫性のないなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="14e1b-148">For web-hosting, you must set the principal in both places; otherwise the security context may become inconsistent.</span></span> <span data-ttu-id="14e1b-149">自己ホスト、ただし、 **HttpContext.Current**が null です。</span><span class="sxs-lookup"><span data-stu-id="14e1b-149">For self-hosting, however, **HttpContext.Current** is null.</span></span> <span data-ttu-id="14e1b-150">コードがホストに依存しないのためには、そのため、null チェックを割り当てる前に**HttpContext.Current**に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="14e1b-150">To ensure your code is host-agnostic, therefore, check for null before assigning to **HttpContext.Current**, as shown.</span></span>

## <a name="authorization"></a><span data-ttu-id="14e1b-151">承認</span><span class="sxs-lookup"><span data-stu-id="14e1b-151">Authorization</span></span>

<span data-ttu-id="14e1b-152">承認の発生、パイプラインの後半で、コント ローラーに近いです。</span><span class="sxs-lookup"><span data-stu-id="14e1b-152">Authorization happens later in the pipeline, closer to the controller.</span></span> <span data-ttu-id="14e1b-153">リソースへのアクセスを付与する場合より詳細な選択を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-153">That lets you make more granular choices when you grant access to resources.</span></span>

- <span data-ttu-id="14e1b-154">*承認フィルター*コント ローラー アクションの前に実行します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-154">*Authorization filters* run before the controller action.</span></span> <span data-ttu-id="14e1b-155">要求が許可されていない場合、フィルターが、エラー応答を返すし、アクションは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="14e1b-155">If the request is not authorized, the filter returns an error response, and the action is not invoked.</span></span>
- <span data-ttu-id="14e1b-156">コント ローラーのアクション内から現在のプリンシパルを取得できます、 **ApiController.User**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="14e1b-156">Within a controller action, you can get the current principal from the **ApiController.User** property.</span></span> <span data-ttu-id="14e1b-157">など、ユーザー名に基づくリソースの一覧をフィルターする可能性がありますにそのユーザーに属しているリソースのみを返します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-157">For example, you might filter a list of resources based on the user name, returning only those resources that belong to that user.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a><span data-ttu-id="14e1b-158">使用して、[承認] 属性</span><span class="sxs-lookup"><span data-stu-id="14e1b-158">Using the [Authorize] Attribute</span></span>

<span data-ttu-id="14e1b-159">Web API は、組み込みの承認フィルターを提供します。 [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-159">Web API provides a built-in authorization filter, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx).</span></span> <span data-ttu-id="14e1b-160">このフィルターは、ユーザーが認証されたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-160">This filter checks whether the user is authenticated.</span></span> <span data-ttu-id="14e1b-161">それ以外の場合は、アクションを呼び出すことがなく HTTP 状態コード 401 (Unauthorized) を返します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-161">If not, it returns HTTP status code 401 (Unauthorized), without invoking the action.</span></span>

<span data-ttu-id="14e1b-162">コント ローラー レベル、または個々 のアクションのレベルでグローバルにフィルターを適用することができます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-162">You can apply the filter globally, at the controller level, or at the level of individual actions.</span></span>

<span data-ttu-id="14e1b-163">**グローバルに**: すべての Web API コント ローラーのアクセスを制限するには、追加、 **AuthorizeAttribute**グローバル フィルターの一覧にフィルター。</span><span class="sxs-lookup"><span data-stu-id="14e1b-163">**Globally**: To restrict access for every Web API controller, add the **AuthorizeAttribute** filter to the global filter list:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="14e1b-164">**コント ローラー**: 特定のコント ローラーへのアクセスを制限するには、コント ローラーに属性として、フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-164">**Controller**: To restrict access for a specific controller, add the filter as an attribute to the controller:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="14e1b-165">**アクション**: 特定の操作に対してアクセスを制限するには、アクション メソッドに属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-165">**Action**: To restrict access for specific actions, add the attribute to the action method:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="14e1b-166">また、コント ローラーを制限しを使用して、特定のアクションへの匿名アクセスを許可、`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="14e1b-166">Alternatively, you can restrict the controller and then allow anonymous access to specific actions, by using the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="14e1b-167">次の例では、`Post`メソッドが制限されていますが、`Get`メソッドは匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-167">In the following example, the `Post` method is restricted, but the `Get` method allows anonymous access.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="14e1b-168">前の例では、フィルターが認証されたユーザーに制限されているメソッド; へのアクセスを許可します。匿名ユーザーのみが保持されます。特定のユーザーまたは特定のロールのユーザーへのアクセスを制限することもできます。</span><span class="sxs-lookup"><span data-stu-id="14e1b-168">In the previous examples, the filter allows any authenticated user to access the restricted methods; only anonymous users are kept out. You can also limit access to specific users or to users in specific roles:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="14e1b-169">**AuthorizeAttribute**に Web API コント ローラーのフィルターがある、 **System.Web.Http**名前空間。</span><span class="sxs-lookup"><span data-stu-id="14e1b-169">The **AuthorizeAttribute** filter for Web API controllers is located in the **System.Web.Http** namespace.</span></span> <span data-ttu-id="14e1b-170">MVC コント ローラーのようなフィルターがある、 **System.Web.Mvc**名前空間は、Web API コント ローラーと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="14e1b-170">There is a similar filter for MVC controllers in the **System.Web.Mvc** namespace, which is not compatible with Web API controllers.</span></span>


### <a name="custom-authorization-filters"></a><span data-ttu-id="14e1b-171">カスタム承認フィルター</span><span class="sxs-lookup"><span data-stu-id="14e1b-171">Custom Authorization Filters</span></span>

<span data-ttu-id="14e1b-172">カスタム承認フィルターを作成するには、これらの型のいずれかから派生します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-172">To write a custom authorization filter, derive from one of these types:</span></span>

- <span data-ttu-id="14e1b-173">**AuthorizeAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-173">**AuthorizeAttribute**.</span></span> <span data-ttu-id="14e1b-174">現在のユーザーとユーザーのロールに基づいて承認ロジックを実行するには、このクラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-174">Extend this class to perform authorization logic based on the current user and the user's roles.</span></span>
- <span data-ttu-id="14e1b-175">**AuthorizationFilterAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-175">**AuthorizationFilterAttribute**.</span></span> <span data-ttu-id="14e1b-176">現在のユーザーまたはロールに対しては必ずしも基づいていない同期承認ロジックを実行するには、このクラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-176">Extend this class to perform synchronous authorization logic that is not necessarily based on the current user or role.</span></span>
- <span data-ttu-id="14e1b-177">**IAuthorizationFilter**します。</span><span class="sxs-lookup"><span data-stu-id="14e1b-177">**IAuthorizationFilter**.</span></span> <span data-ttu-id="14e1b-178">非同期の承認ロジックを実行するには、このインターフェイスを実装します。たとえば、承認ロジックが非同期 I/O またはネットワーク呼び出しを行った場合。</span><span class="sxs-lookup"><span data-stu-id="14e1b-178">Implement this interface to perform asynchronous authorization logic; for example, if your authorization logic makes asynchronous I/O or network calls.</span></span> <span data-ttu-id="14e1b-179">(から派生する方が簡単では、承認ロジックが CPU バインドの場合は、 **AuthorizationFilterAttribute**のため、非同期のメソッドを記述する必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="14e1b-179">(If your authorization logic is CPU-bound, it is simpler to derive from **AuthorizationFilterAttribute**, because then you don't need to write an asynchronous method.)</span></span>

<span data-ttu-id="14e1b-180">次の図は、クラスの階層構造、 **AuthorizeAttribute**クラス。</span><span class="sxs-lookup"><span data-stu-id="14e1b-180">The following diagram shows the class hierarchy for the **AuthorizeAttribute** class.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a><span data-ttu-id="14e1b-181">コント ローラー アクション内での承認</span><span class="sxs-lookup"><span data-stu-id="14e1b-181">Authorization Inside a Controller Action</span></span>

<span data-ttu-id="14e1b-182">場合によっては、続行して、プリンシパルに基づく動作を変更するには、要求を許可する場合があります。</span><span class="sxs-lookup"><span data-stu-id="14e1b-182">In some cases, you might allow a request to proceed, but change the behavior based on the principal.</span></span> <span data-ttu-id="14e1b-183">などを返す情報は、ユーザーのロールに応じて変更可能性があります。</span><span class="sxs-lookup"><span data-stu-id="14e1b-183">For example, the information that you return might change depending on the user's role.</span></span> <span data-ttu-id="14e1b-184">コント ローラー メソッド内から現在のプリンシパルを取得できます、 **ApiController.User**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="14e1b-184">Within a controller method, you can get the current principle from the **ApiController.User** property.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
