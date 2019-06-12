---
title: ASP.NET Core でのポリシー ベースの承認
author: rick-anderson
description: 作成し、ASP.NET Core アプリでの承認要件を適用するための承認ポリシーのハンドラーを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837348"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c1a55-103">ASP.NET Core でのポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="c1a55-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c1a55-104">実際には、下にある[ロールベースの承認](xref:security/authorization/roles)と[クレームに基づく承認](xref:security/authorization/claims)要件、要件ハンドラー、および事前構成済みポリシーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c1a55-105">これらのビルディング ブロックでは、コードでの評価の承認の式をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c1a55-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c1a55-106">結果より豊富な再利用可能なテスト可能な承認構造です。</span><span class="sxs-lookup"><span data-stu-id="c1a55-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c1a55-107">承認ポリシーは、1 つまたは複数の要件で構成されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c1a55-108">承認サービスの構成の一部としてに登録されている、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c1a55-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="c1a55-109">上記の例では、"AtLeast21"ポリシーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c1a55-110">1 つの要件がある&mdash;の最小経過期間が、要件にパラメーターとして指定されています。</span><span class="sxs-lookup"><span data-stu-id="c1a55-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="c1a55-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="c1a55-111">IAuthorizationService</span></span> 

<span data-ttu-id="c1a55-112">承認が成功したかどうかを決定する、プライマリ サービスが<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="c1a55-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="c1a55-113">上記のコードには 2 つの方法が強調表示、 [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="c1a55-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> メソッドのない、および承認が成功したかどうかを追跡するためのメカニズムを使用して、マーカー サービスです。</span><span class="sxs-lookup"><span data-stu-id="c1a55-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="c1a55-115">各<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>要件が満たされた場合にチェックを担当します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="c1a55-116"><xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>クラスは、ハンドラーの使用要件を満たしているかどうかをマークします。</span><span class="sxs-lookup"><span data-stu-id="c1a55-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="c1a55-117">次のコードが簡略化された (およびコメントによる注釈付き) には、承認サービスの既定の実装。</span><span class="sxs-lookup"><span data-stu-id="c1a55-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="c1a55-118">次のコードは、一般的な`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1a55-118">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

<span data-ttu-id="c1a55-119">使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>または`[Authorize(Policy = "Something"]`承認します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something"]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="c1a55-120">MVC コント ローラーにポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="c1a55-121">Razor ページを使用している場合は、次を参照してください。 [Razor ページにポリシーを適用する](#applying-policies-to-razor-pages)このドキュメントで。</span><span class="sxs-lookup"><span data-stu-id="c1a55-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="c1a55-122">使用して、コント ローラーに適用されるポリシー、`[Authorize]`ポリシーの名前を持つ属性です。</span><span class="sxs-lookup"><span data-stu-id="c1a55-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1a55-123">例:</span><span class="sxs-lookup"><span data-stu-id="c1a55-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="c1a55-124">Razor ページにポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="c1a55-125">使用して Razor ページにポリシーが適用されます、`[Authorize]`ポリシーの名前を持つ属性です。</span><span class="sxs-lookup"><span data-stu-id="c1a55-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1a55-126">例えば:</span><span class="sxs-lookup"><span data-stu-id="c1a55-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="c1a55-127">使用して Razor ページにポリシーが適用もできます、[承認規則](xref:security/authorization/razor-pages-authorization)します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="c1a55-128">必要条件</span><span class="sxs-lookup"><span data-stu-id="c1a55-128">Requirements</span></span>

<span data-ttu-id="c1a55-129">承認要件を現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="c1a55-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c1a55-130">要件が 1 つのパラメーターを"AtLeast21"ポリシーで&mdash;最小経過期間。</span><span class="sxs-lookup"><span data-stu-id="c1a55-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c1a55-131">要件を実装して[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)、空のマーカー インターフェイスであります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c1a55-132">パラメーター化の最小経過期間の要件は、次のように実装できます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="c1a55-133">承認ポリシーに複数の承認要件が含まれている場合、すべての要件は、ポリシー評価を成功させるために渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="c1a55-134">つまり、複数の承認要件を 1 つの承認ポリシーに追加がで扱われます、 **AND**ごと。</span><span class="sxs-lookup"><span data-stu-id="c1a55-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="c1a55-135">要件にデータまたはプロパティを持つ必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c1a55-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c1a55-136">承認ハンドラー</span><span class="sxs-lookup"><span data-stu-id="c1a55-136">Authorization handlers</span></span>

<span data-ttu-id="c1a55-137">承認ハンドラーは、要件のプロパティの評価を担当します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c1a55-138">に対して、指定された要件を評価して、承認ハンドラー [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)へのアクセスが許可されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c1a55-139">要件を持つことができます[複数のハンドラー](#security-authorization-policies-based-multiple-handlers)します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c1a55-140">ハンドラーを継承できます[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)ここで、`TRequirement`が処理に必要です。</span><span class="sxs-lookup"><span data-stu-id="c1a55-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c1a55-141">ハンドラーを実装または、 [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)要件の 1 つ以上の型を処理します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c1a55-142">要件の 1 つのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c1a55-143">最小経過期間のハンドラーが 1 つの要求を利用して一対一のリレーションシップの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c1a55-144">上記のコードでは、現在のユーザー プリンシパル生年月日が既知または信頼された発行者によって発行される要求がある場合を決定します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c1a55-145">承認は、クレームが見つからない場合に発生することはできません、その場合、完了したタスクが返されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c1a55-146">クレームが存在する場合は、ユーザーの年齢が計算されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c1a55-147">ユーザーが要求することにより定義されている最小経過期間を満たしている場合は、承認が成功したと見なされます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c1a55-148">承認が成功すると、`context.Succeed`を唯一のパラメーターとして満足要件とが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c1a55-149">複数の要件のハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c1a55-150">アクセス許可のハンドラーが 3 つの異なるタイプの要件を処理できる一対多リレーションシップの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c1a55-151">上記のコードを走査[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;いない要件を含むプロパティは成功としてマークします。</span><span class="sxs-lookup"><span data-stu-id="c1a55-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c1a55-152">`ReadPermission`要件、ユーザーは、要求されたリソースにアクセスする所有者またはスポンサーにある必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c1a55-153">場合、`EditPermission`または`DeletePermission`要件、そのユーザーが要求されたリソースにアクセスする所有者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c1a55-154">ハンドラーの登録</span><span class="sxs-lookup"><span data-stu-id="c1a55-154">Handler registration</span></span>

<span data-ttu-id="c1a55-155">ハンドラーは、構成中にサービスのコレクションに登録されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c1a55-156">例:</span><span class="sxs-lookup"><span data-stu-id="c1a55-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="c1a55-157">上記のコードを登録`MinimumAgeHandler`を呼び出すことによって、シングルトンとして`services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="c1a55-158">組み込みのいずれかを使用してハンドラーを登録できる[サービスの有効期間](xref:fundamentals/dependency-injection#service-lifetimes)します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c1a55-159">ハンドラーを返すどのような必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-159">What should a handler return?</span></span>

<span data-ttu-id="c1a55-160">なお、`Handle`メソッドで、[ハンドラーの例](#security-authorization-handler-example)値を返しません。</span><span class="sxs-lookup"><span data-stu-id="c1a55-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c1a55-161">成功または失敗が示される状態ですか。</span><span class="sxs-lookup"><span data-stu-id="c1a55-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c1a55-162">ハンドラーを呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証されました。</span><span class="sxs-lookup"><span data-stu-id="c1a55-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c1a55-163">ハンドラーと同じ要件の他のハンドラーが成功する可能性が一般的には、エラーを処理する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c1a55-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c1a55-164">場合でも、その他の要件ハンドラーで成功をエラーを確実に呼び出す`context.Fail`します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c1a55-165">ハンドラーを呼び出す場合`context.Succeed`または`context.Fail`、他のすべてのハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="c1a55-166">これにより、別のハンドラーが正常に検証または要件の失敗した場合でも行われるログ記録などの副作用を生成するための要件です。</span><span class="sxs-lookup"><span data-stu-id="c1a55-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="c1a55-167">設定すると`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)プロパティ (ASP.NET Core 1.1 で利用可能な以降) を実行せずにハンドラーの実行時に`context.Fail`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c1a55-168">`InvokeHandlersAfterFailure` 既定値は`true`、すべてのハンドラーが呼び出される場合。</span><span class="sxs-lookup"><span data-stu-id="c1a55-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="c1a55-169">認証が失敗した場合でも、承認ハンドラーは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c1a55-170">複数のハンドラーを要件の対象は</span><span class="sxs-lookup"><span data-stu-id="c1a55-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c1a55-171">評価上に存在する必要がある場合に、**または**ごと、1 つの要件の複数のハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c1a55-172">たとえば、Microsoft では、キーのカードでのみ開くことがドアがあります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c1a55-173">キー カードを自宅でままにした場合、受付は一時ステッカーを出力し、ドアが開きます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c1a55-174">このシナリオでは唯一の要件がある*BuildingEntry*が、複数のハンドラーがあり、それぞれ 1 つの要件を確認します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c1a55-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c1a55-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c1a55-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1a55-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c1a55-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1a55-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c1a55-178">両方のハンドラーを[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c1a55-179">いずれかのハンドラーは、ときに、ポリシーが成功した場合は、評価、`BuildingEntryRequirement`ポリシーの評価が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="c1a55-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c1a55-180">ポリシーを満たす func を使用します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c1a55-181">どのを満たすことで、ポリシーがコードで表現する単純な状況である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c1a55-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c1a55-182">指定することは、`Func<AuthorizationHandlerContext, bool>`でポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。</span><span class="sxs-lookup"><span data-stu-id="c1a55-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c1a55-183">たとえば、以前`BadgeEntryHandler`次のように書き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="c1a55-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c1a55-184">ハンドラーで MVC 要求のコンテキストへのアクセス</span><span class="sxs-lookup"><span data-stu-id="c1a55-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c1a55-185">`HandleRequirementAsync`承認ハンドラーで実装するメソッドが 2 つのパラメーター: `AuthorizationHandlerContext` 、`TRequirement`開梱を行う。</span><span class="sxs-lookup"><span data-stu-id="c1a55-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c1a55-186">MVC または Jabbr などのフレームワークは、任意のオブジェクトを追加する自由、`Resource`プロパティを`AuthorizationHandlerContext`余分な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c1a55-187">たとえば、MVC のインスタンスを渡します[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)で、`Resource`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1a55-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c1a55-188">このプロパティにアクセスできます`HttpContext`、 `RouteData`、MVC および Razor ページによって提供される他すべてのものとします。</span><span class="sxs-lookup"><span data-stu-id="c1a55-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c1a55-189">使用、`Resource`プロパティは、特定のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c1a55-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c1a55-190">情報を使用して、`Resource`プロパティは、特定のフレームワークに独自の承認ポリシーを制限します。</span><span class="sxs-lookup"><span data-stu-id="c1a55-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c1a55-191">キャストする必要があります、`Resource`プロパティを使用して、`is`キーワード、コードがクラッシュしないことを確認して、キャストが成功したことを確認およびで、`InvalidCastException`他のフレームワークで実行した場合。</span><span class="sxs-lookup"><span data-stu-id="c1a55-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
