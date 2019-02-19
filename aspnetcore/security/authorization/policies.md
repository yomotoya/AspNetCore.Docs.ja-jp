---
title: ASP.NET Core でのポリシー ベースの承認
author: rick-anderson
description: 作成し、ASP.NET Core アプリでの承認要件を適用するための承認ポリシーのハンドラーを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: be4812487c92a16c44e3983b234bc9e31be65190
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410390"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="fa4d4-103">ASP.NET Core でのポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="fa4d4-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="fa4d4-104">実際には、下にある[ロールベースの承認](xref:security/authorization/roles)と[クレームに基づく承認](xref:security/authorization/claims)要件、要件ハンドラー、および事前構成済みポリシーを使用します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="fa4d4-105">これらのビルディング ブロックでは、コードでの評価の承認の式をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="fa4d4-106">結果より豊富な再利用可能なテスト可能な承認構造です。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="fa4d4-107">承認ポリシーは、1 つまたは複数の要件で構成されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="fa4d4-108">承認サービスの構成の一部としてに登録されている、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="fa4d4-109">上記の例では、"AtLeast21"ポリシーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="fa4d4-110">1 つの要件がある&mdash;の最小経過期間が、要件にパラメーターとして指定されています。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="fa4d4-111">ポリシーを使用して、`[Authorize]`ポリシーの名前を持つ属性です。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="fa4d4-112">例:</span><span class="sxs-lookup"><span data-stu-id="fa4d4-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="fa4d4-113">必要条件</span><span class="sxs-lookup"><span data-stu-id="fa4d4-113">Requirements</span></span>

<span data-ttu-id="fa4d4-114">承認要件を現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="fa4d4-115">要件が 1 つのパラメーターを"AtLeast21"ポリシーで&mdash;最小経過期間。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="fa4d4-116">要件を実装して[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)、空のマーカー インターフェイスであります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="fa4d4-117">パラメーター化の最小経過期間の要件は、次のように実装できます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="fa4d4-118">承認ポリシーに複数の承認要件が含まれている場合、すべての要件は、ポリシー評価を成功させるために渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="fa4d4-119">つまり、複数の承認要件を 1 つの承認ポリシーに追加がで扱われます、 **AND**ごと。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4d4-120">要件にデータまたはプロパティを持つ必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="fa4d4-121">承認ハンドラー</span><span class="sxs-lookup"><span data-stu-id="fa4d4-121">Authorization handlers</span></span>

<span data-ttu-id="fa4d4-122">承認ハンドラーは、要件のプロパティの評価を担当します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="fa4d4-123">に対して、指定された要件を評価して、承認ハンドラー [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)へのアクセスが許可されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="fa4d4-124">要件を持つことができます[複数のハンドラー](#security-authorization-policies-based-multiple-handlers)します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="fa4d4-125">ハンドラーを継承できます[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)ここで、`TRequirement`が処理に必要です。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="fa4d4-126">ハンドラーを実装または、 [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)要件の 1 つ以上の型を処理します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="fa4d4-127">要件の 1 つのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="fa4d4-128">最小経過期間のハンドラーが 1 つの要求を利用して一対一のリレーションシップの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="fa4d4-129">上記のコードでは、現在のユーザー プリンシパル生年月日が既知または信頼された発行者によって発行される要求がある場合を決定します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="fa4d4-130">承認は、クレームが見つからない場合に発生することはできません、その場合、完了したタスクが返されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="fa4d4-131">クレームが存在する場合は、ユーザーの年齢が計算されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="fa4d4-132">ユーザーが要求することにより定義されている最小経過期間を満たしている場合は、承認が成功したと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="fa4d4-133">承認が成功すると、`context.Succeed`を唯一のパラメーターとして満足要件とが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="fa4d4-134">複数の要件のハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="fa4d4-135">アクセス許可のハンドラーが 3 つの異なるタイプの要件を処理できる一対多リレーションシップの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="fa4d4-136">上記のコードを走査[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;いない要件を含むプロパティは成功としてマークします。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="fa4d4-137">`ReadPermission`要件、ユーザーは、要求されたリソースにアクセスする所有者またはスポンサーにある必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="fa4d4-138">場合、`EditPermission`または`DeletePermission`要件、そのユーザーが要求されたリソースにアクセスする所有者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="fa4d4-139">ハンドラーの登録</span><span class="sxs-lookup"><span data-stu-id="fa4d4-139">Handler registration</span></span>

<span data-ttu-id="fa4d4-140">ハンドラーは、構成中にサービスのコレクションに登録されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="fa4d4-141">例:</span><span class="sxs-lookup"><span data-stu-id="fa4d4-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="fa4d4-142">各ハンドラーは呼び出すことによってサービスのコレクションに追加`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-142">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="fa4d4-143">ハンドラーを返すどのような必要があります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-143">What should a handler return?</span></span>

<span data-ttu-id="fa4d4-144">なお、`Handle`メソッドで、[ハンドラーの例](#security-authorization-handler-example)値を返しません。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-144">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="fa4d4-145">成功または失敗が示される状態ですか。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-145">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="fa4d4-146">ハンドラーを呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証されました。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-146">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="fa4d4-147">ハンドラーと同じ要件の他のハンドラーが成功する可能性が一般的には、エラーを処理する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-147">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="fa4d4-148">場合でも、その他の要件ハンドラーで成功をエラーを確実に呼び出す`context.Fail`します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-148">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="fa4d4-149">設定すると`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)プロパティ (ASP.NET Core 1.1 で利用可能な以降) を実行せずにハンドラーの実行時に`context.Fail`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-149">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="fa4d4-150">`InvokeHandlersAfterFailure` 既定値は`true`、すべてのハンドラーが呼び出される場合。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-150">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="fa4d4-151">これにより、常に実行される、ログ記録などの側効果を作成するための要件も`context.Fail`が別のハンドラーで呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-151">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="fa4d4-152">複数のハンドラーを要件の対象は</span><span class="sxs-lookup"><span data-stu-id="fa4d4-152">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="fa4d4-153">評価上に存在する必要がある場合に、**または**ごと、1 つの要件の複数のハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-153">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="fa4d4-154">たとえば、Microsoft では、キーのカードでのみ開くことがドアがあります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-154">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="fa4d4-155">キー カードを自宅でままにした場合、受付は一時ステッカーを出力し、ドアが開きます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-155">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="fa4d4-156">このシナリオでは唯一の要件がある*BuildingEntry*が、複数のハンドラーがあり、それぞれ 1 つの要件を確認します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-156">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="fa4d4-157">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="fa4d4-157">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="fa4d4-158">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fa4d4-158">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="fa4d4-159">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fa4d4-159">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="fa4d4-160">両方のハンドラーを[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-160">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="fa4d4-161">いずれかのハンドラーは、ときに、ポリシーが成功した場合は、評価、`BuildingEntryRequirement`ポリシーの評価が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-161">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="fa4d4-162">ポリシーを満たす func を使用します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-162">Using a func to fulfill a policy</span></span>

<span data-ttu-id="fa4d4-163">どのを満たすことで、ポリシーがコードで表現する単純な状況である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-163">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="fa4d4-164">指定することは、`Func<AuthorizationHandlerContext, bool>`でポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-164">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="fa4d4-165">たとえば、以前`BadgeEntryHandler`次のように書き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-165">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="fa4d4-166">ハンドラーで MVC 要求のコンテキストへのアクセス</span><span class="sxs-lookup"><span data-stu-id="fa4d4-166">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="fa4d4-167">`HandleRequirementAsync`承認ハンドラーで実装するメソッドが 2 つのパラメーター: `AuthorizationHandlerContext` 、`TRequirement`開梱を行う。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-167">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="fa4d4-168">MVC または Jabbr などのフレームワークは、任意のオブジェクトを追加する自由、`Resource`プロパティを`AuthorizationHandlerContext`余分な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-168">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="fa4d4-169">たとえば、MVC のインスタンスを渡します[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)で、`Resource`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-169">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="fa4d4-170">このプロパティにアクセスできます`HttpContext`、 `RouteData`、MVC および Razor ページによって提供される他すべてのものとします。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-170">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="fa4d4-171">使用、`Resource`プロパティは、特定のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-171">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="fa4d4-172">情報を使用して、`Resource`プロパティは、特定のフレームワークに独自の承認ポリシーを制限します。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-172">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="fa4d4-173">キャストする必要があります、`Resource`プロパティを使用して、`is`キーワード、コードがクラッシュしないことを確認して、キャストが成功したことを確認およびで、`InvalidCastException`他のフレームワークで実行した場合。</span><span class="sxs-lookup"><span data-stu-id="fa4d4-173">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
