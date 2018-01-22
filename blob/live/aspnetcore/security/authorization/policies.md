---
title: "ASP.NET Core でのカスタムのポリシー ベースの承認"
author: rick-anderson
description: "作成および ASP.NET Core アプリケーションの承認要件を適用するカスタム承認ポリシーのハンドラーを使用する方法を説明します。"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="48983-103">カスタムのポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="48983-103">Custom policy-based authorization</span></span>

<span data-ttu-id="48983-104">背後の[ロールベース承認](xref:security/authorization/roles)と[クレーム ベースの承認](xref:security/authorization/claims)要件、要件のハンドラー、および構成済みのポリシーを使用します。</span><span class="sxs-lookup"><span data-stu-id="48983-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="48983-105">これらのビルド ブロックでは、コードで、式の評価の承認をサポートします。</span><span class="sxs-lookup"><span data-stu-id="48983-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="48983-106">結果は、豊富な再利用可能なテスト可能な承認構造です。</span><span class="sxs-lookup"><span data-stu-id="48983-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="48983-107">承認ポリシーは、1 つまたは複数の要件で構成されます。</span><span class="sxs-lookup"><span data-stu-id="48983-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="48983-108">承認サービスの構成の一部として登録されている、`ConfigureServices`のメソッド、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="48983-108">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="48983-109">前の例では、"AtLeast21"ポリシーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="48983-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="48983-110">ある、最小経過期間のでは提供される、要件にパラメーターとして、1 つの要件があります。</span><span class="sxs-lookup"><span data-stu-id="48983-110">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="48983-111">使用してポリシーが適用されます、`[Authorize]`ポリシーの名前を持つ属性。</span><span class="sxs-lookup"><span data-stu-id="48983-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="48983-112">例:</span><span class="sxs-lookup"><span data-stu-id="48983-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="48983-113">必要条件</span><span class="sxs-lookup"><span data-stu-id="48983-113">Requirements</span></span>

<span data-ttu-id="48983-114">承認要件は、現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="48983-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="48983-115">当社"AtLeast21"ポリシーの要件が 1 つのパラメーター&mdash;最小経過期間。</span><span class="sxs-lookup"><span data-stu-id="48983-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="48983-116">要件を実装する`IAuthorizationRequirement`、これは、空のマーカーのインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="48983-116">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="48983-117">パラメーター化の最低年齢の要件は、次のように実装可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48983-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="48983-118">要件は、データまたはプロパティを持つ必要はありません。</span><span class="sxs-lookup"><span data-stu-id="48983-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="48983-119">認証ハンドラー</span><span class="sxs-lookup"><span data-stu-id="48983-119">Authorization handlers</span></span>

<span data-ttu-id="48983-120">認証ハンドラーは、要件のプロパティの評価にします。</span><span class="sxs-lookup"><span data-stu-id="48983-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="48983-121">に対して、指定された要件を評価して、承認ハンドラー`AuthorizationHandlerContext`をアクセスが許可されたかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="48983-121">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="48983-122">要件が持てる[複数のハンドラー](#security-authorization-policies-based-multiple-handlers)です。</span><span class="sxs-lookup"><span data-stu-id="48983-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="48983-123">ハンドラーを継承`AuthorizationHandler<T>`ここで、`T`を処理するための要件です。</span><span class="sxs-lookup"><span data-stu-id="48983-123">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="48983-124">最小経過期間ハンドラーは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="48983-124">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="48983-125">上記のコードは、現在のユーザー プリンシパルは、生年月日が既知で信頼できる発行者によって発行された要求の場合を判断します。</span><span class="sxs-lookup"><span data-stu-id="48983-125">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="48983-126">承認は、クレームが不足している場合に発生することはできません、その場合、完了したタスクが返されます。</span><span class="sxs-lookup"><span data-stu-id="48983-126">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="48983-127">クレームが存在する場合は、ユーザーの年齢が計算されます。</span><span class="sxs-lookup"><span data-stu-id="48983-127">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="48983-128">ユーザーが要求することにより定義されている最小経過期間を満たさない場合承認は成功と見なされます。</span><span class="sxs-lookup"><span data-stu-id="48983-128">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="48983-129">承認が正常に完了、`context.Succeed`が満たされた要件をパラメーターとしてで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="48983-129">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="48983-130">ハンドラーの登録</span><span class="sxs-lookup"><span data-stu-id="48983-130">Handler registration</span></span>

<span data-ttu-id="48983-131">ハンドラーは、構成中にサービスのコレクションに登録されます。</span><span class="sxs-lookup"><span data-stu-id="48983-131">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="48983-132">例:</span><span class="sxs-lookup"><span data-stu-id="48983-132">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="48983-133">呼び出すことによって、サービスのコレクションに各ハンドラーが追加された`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`です。</span><span class="sxs-lookup"><span data-stu-id="48983-133">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="48983-134">ハンドラーの取得と必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="48983-134">What should a handler return?</span></span>

<span data-ttu-id="48983-135">なお、`Handle`メソッドで、[ハンドラーの例](#security-authorization-handler-example)値を返しません。</span><span class="sxs-lookup"><span data-stu-id="48983-135">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="48983-136">成功または失敗が示されている状態ですか。</span><span class="sxs-lookup"><span data-stu-id="48983-136">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="48983-137">ハンドラーは、呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証済みです。</span><span class="sxs-lookup"><span data-stu-id="48983-137">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="48983-138">ハンドラーは、同じ要件に対する他のハンドラーが正常に終了すると、一般に、エラーを処理する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="48983-138">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="48983-139">保証するための障害、他の要件のハンドラーが成功する場合でも、呼び出す`context.Fail`です。</span><span class="sxs-lookup"><span data-stu-id="48983-139">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="48983-140">新機能を呼び出すと、ハンドラー内に関係なく、ポリシー要件が必要な場合に、要件のすべてのハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="48983-140">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="48983-141">これにより、要件を常に行われるログ記録など、副作用がある場合でも`context.Fail()`は別のハンドラーで呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="48983-141">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="48983-142">複数のハンドラーを必要条件の目的は</span><span class="sxs-lookup"><span data-stu-id="48983-142">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="48983-143">評価である必要がある場合に、**または**単位、1 つの要件の複数のハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="48983-143">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="48983-144">たとえば、マイクロソフトでは、キーのカードでのみ開くことがドアがあります。</span><span class="sxs-lookup"><span data-stu-id="48983-144">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="48983-145">キー カードを自宅に移動すると、受付は一時的なステッカーを出力し、ドアを開きます。</span><span class="sxs-lookup"><span data-stu-id="48983-145">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="48983-146">このシナリオでは 1 つの要件が、 *BuildingEntry*が複数のハンドラーが各いずれか 1 つの要件を確認します。</span><span class="sxs-lookup"><span data-stu-id="48983-146">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="48983-147">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="48983-147">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="48983-148">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="48983-148">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="48983-149">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="48983-149">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="48983-150">両方のハンドラー[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)です。</span><span class="sxs-lookup"><span data-stu-id="48983-150">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="48983-151">いずれかのハンドラーは、ときに、ポリシーが成功した場合は、評価、`BuildingEntryRequirement`ポリシーの評価が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="48983-151">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="48983-152">ポリシーを満たすために、func を使用します。</span><span class="sxs-lookup"><span data-stu-id="48983-152">Using a func to fulfill a policy</span></span>

<span data-ttu-id="48983-153">どので満たすことで、ポリシーがコードで明示する単純な状況である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48983-153">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="48983-154">指定することは、`Func<AuthorizationHandlerContext, bool>`を使用してポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。</span><span class="sxs-lookup"><span data-stu-id="48983-154">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="48983-155">たとえば、前の`BadgeEntryHandler`次のように書き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="48983-155">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="48983-156">ハンドラーで MVC 要求コンテキストにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="48983-156">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="48983-157">`HandleRequirementAsync`承認ハンドラーで実装するメソッドが 2 つのパラメーター:`AuthorizationHandlerContext`と`TRequirement`を処理します。</span><span class="sxs-lookup"><span data-stu-id="48983-157">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="48983-158">MVC または Jabbr などのフレームワークを自由に任意のオブジェクトを追加、`Resource`プロパティを`AuthorizationHandlerContext`余分な情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="48983-158">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="48983-159">たとえば、MVC のインスタンスを渡す[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)で、`Resource`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="48983-159">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="48983-160">このプロパティへのアクセスを提供する`HttpContext`、 `RouteData`、MVC および Razor ページによって提供される他すべてのものとします。</span><span class="sxs-lookup"><span data-stu-id="48983-160">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="48983-161">使用、`Resource`プロパティは、フレームワークに固有です。</span><span class="sxs-lookup"><span data-stu-id="48983-161">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="48983-162">内の情報を使用して、`Resource`プロパティは特定のフレームワークを承認ポリシーを制限します。</span><span class="sxs-lookup"><span data-stu-id="48983-162">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="48983-163">キャストする必要があります、`Resource`プロパティを使用して、`as`キーワード、し確認コードがクラッシュしないことを確認するキャストが成功すると、`InvalidCastException`他のフレームワークで実行されたとき。</span><span class="sxs-lookup"><span data-stu-id="48983-163">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
