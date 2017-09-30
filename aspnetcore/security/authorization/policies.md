---
title: "カスタムのポリシー ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 5021b5d20f6d9b9a4d8889f25b5e41f2c9306f64
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="ce24f-103">カスタムのポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="ce24f-103">Custom Policy-Based Authorization</span></span>

<a name=security-authorization-policies-based></a>

<span data-ttu-id="ce24f-104">背後の[ロールの承認](roles.md#security-authorization-role-based)と[承認を要求](claims.md#security-authorization-claims-based)要件の使用、要件とポリシーを事前構成済みのハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-104">Underneath the covers the [role authorization](roles.md#security-authorization-role-based) and [claims authorization](claims.md#security-authorization-claims-based) make use of a requirement, a handler for the requirement and a pre-configured policy.</span></span> <span data-ttu-id="ce24f-105">これらの構成要素を使用すると、承認の評価を許可する、豊富なため、再利用可能なコードと簡単にテストが容易な承認の構造を高速にできます。</span><span class="sxs-lookup"><span data-stu-id="ce24f-105">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="ce24f-106">承認ポリシーを 1 つまたは複数の要件で構成およびに登録されているアプリケーションの起動時に認証サービスの構成の一部として`ConfigureServices`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ce24f-106">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

<span data-ttu-id="ce24f-107">ここで、最小経過期間のこれが渡されるパラメーターとして、要件、1 つの要求で、"Over21"ポリシーが作成されたかを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ce24f-107">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="ce24f-108">使用してポリシーが適用される、`Authorize`属性名を指定して、ポリシー、たとえば;</span><span class="sxs-lookup"><span data-stu-id="ce24f-108">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a><span data-ttu-id="ce24f-109">要件</span><span class="sxs-lookup"><span data-stu-id="ce24f-109">Requirements</span></span>

<span data-ttu-id="ce24f-110">承認要件は、現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="ce24f-110">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="ce24f-111">最小経過期間ポリシーで、要件があるが 1 つのパラメーター、最小経過期間です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-111">In our Minimum Age policy the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="ce24f-112">要件を実装する必要があります`IAuthorizationRequirement`です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-112">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="ce24f-113">これは、空、マーカーのインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ce24f-113">This is an empty, marker interface.</span></span> <span data-ttu-id="ce24f-114">パラメーター化の最低年齢の要件が実装されているとおりです。</span><span class="sxs-lookup"><span data-stu-id="ce24f-114">A parameterized minimum age requirement might be implemented as follows;</span></span>

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

<span data-ttu-id="ce24f-115">要件は、データまたはプロパティを持つ必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ce24f-115">A requirement doesn't need to have data or properties.</span></span>

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a><span data-ttu-id="ce24f-116">認証ハンドラー</span><span class="sxs-lookup"><span data-stu-id="ce24f-116">Authorization Handlers</span></span>

<span data-ttu-id="ce24f-117">認証ハンドラーは、要件のプロパティの評価にします。</span><span class="sxs-lookup"><span data-stu-id="ce24f-117">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="ce24f-118">認証ハンドラーが、指定されたに対して評価する必要があります`AuthorizationHandlerContext`承認が許可されたかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-118">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="ce24f-119">要件が持てる[複数のハンドラー](policies.md#security-authorization-policies-based-multiple-handlers)です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-119">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="ce24f-120">ハンドラーを継承する必要があります`AuthorizationHandler<T>`T は、要件を処理します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-120">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name=security-authorization-handler-example></a>

<span data-ttu-id="ce24f-121">最小経過期間ハンドラーは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ce24f-121">The minimum age handler might look like this:</span></span>

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="ce24f-122">前のコードで最初におる場合、現在のユーザー プリンシパルは、生年月日が発行したことが分かって発行者との信頼の要求を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ce24f-122">In the code above we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="ce24f-123">クレームが存在しない場合は、私たちを返すようお承認できません。</span><span class="sxs-lookup"><span data-stu-id="ce24f-123">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="ce24f-124">どれだけ古いユーザーが調べる、要求した場合と、要件ごとに渡された最小経過期間を満たしている場合、承認が成功しました。</span><span class="sxs-lookup"><span data-stu-id="ce24f-124">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="ce24f-125">承認が成功した後と呼んで`context.Succeed()`をパラメーターとして正常に実行されている要件に渡すことです。</span><span class="sxs-lookup"><span data-stu-id="ce24f-125">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name=security-authorization-policies-based-handler-registration></a>

<span data-ttu-id="ce24f-126">ハンドラーを構成中に、サービスのコレクションにたとえば; 登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce24f-126">Handlers must be registered in the services collection during configuration, for example;</span></span>

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

<span data-ttu-id="ce24f-127">使用して各ハンドラーがサービスのコレクションに追加された`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`ハンドラー クラスを渡します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-127">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="ce24f-128">ハンドラーの取得と必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="ce24f-128">What should a handler return?</span></span>

<span data-ttu-id="ce24f-129">表示、[ハンドラーの例](policies.md#security-authorization-handler-example)を`Handle()`メソッドを持たない戻り値は、どのように成功または失敗を示すことですか?</span><span class="sxs-lookup"><span data-stu-id="ce24f-129">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="ce24f-130">ハンドラーは、呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証済みです。</span><span class="sxs-lookup"><span data-stu-id="ce24f-130">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="ce24f-131">ハンドラーは、同じ要件に対する他のハンドラーが正常に終了すると、一般に、エラーを処理する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ce24f-131">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="ce24f-132">要件の他のハンドラーが成功する場合でも、エラーを保証、するために呼び出す`context.Fail`です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-132">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="ce24f-133">内部ハンドラーと呼ばれる要素に関係なく、ポリシー要件が必要な場合に、要件のすべてのハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ce24f-133">Regardless of what you call inside your handler all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="ce24f-134">これにより、要件を常に行われるログ記録など、副作用がある場合でも`context.Fail()`は別のハンドラーで呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="ce24f-134">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="ce24f-135">複数のハンドラーを必要条件の目的は</span><span class="sxs-lookup"><span data-stu-id="ce24f-135">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="ce24f-136">評価である必要がある場合に、**または**ごとに 1 つの要件の複数のハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-136">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="ce24f-137">たとえば、マイクロソフトでは、キーのカードでのみ開くことがドアがあります。</span><span class="sxs-lookup"><span data-stu-id="ce24f-137">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="ce24f-138">キー カードを自宅に移動すると、受付は一時的なステッカーを出力し、ドアを開きます。</span><span class="sxs-lookup"><span data-stu-id="ce24f-138">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="ce24f-139">このシナリオでは 1 つの要求は*EnterBuilding*が複数のハンドラーが各いずれか 1 つの要件を確認します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-139">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="ce24f-140">ここが両方のハンドラーを想定して[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)ポリシーと評価される場合、`EnterBuildingRequirement`ポリシーの評価が成功かハンドラーが成功した場合。</span><span class="sxs-lookup"><span data-stu-id="ce24f-140">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="ce24f-141">ポリシーを満たすために、func を使用します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-141">Using a func to fulfill a policy</span></span>

<span data-ttu-id="ce24f-142">可能性がありますの機会でポリシーを満たすことがコードで明示する単純です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-142">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="ce24f-143">だけを指定することは、`Func<AuthorizationHandlerContext, bool>`を使用してポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。</span><span class="sxs-lookup"><span data-stu-id="ce24f-143">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="ce24f-144">たとえば、前`BadgeEntryHandler`; 次のように書き換えることができます</span><span class="sxs-lookup"><span data-stu-id="ce24f-144">For example the previous `BadgeEntryHandler` could be rewritten as follows;</span></span>

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="ce24f-145">ハンドラーで MVC 要求コンテキストにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="ce24f-145">Accessing MVC Request Context In Handlers</span></span>

<span data-ttu-id="ce24f-146">`Handle`承認ハンドラーで実装する必要がありますのメソッドが 2 つのパラメーター、`AuthorizationContext`と`Requirement`を処理します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-146">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="ce24f-147">MVC または Jabbr などのフレームワークを自由に任意のオブジェクトを追加、`Resource`プロパティを`AuthorizationContext`余分な情報を通過します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-147">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="ce24f-148">たとえば MVC がのインスタンスを渡す`Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext`HttpContext、RouteData およびそのすべてのアクセスに使用されるリソースのプロパティでは他の MVC を提供します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-148">For example MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="ce24f-149">使用、`Resource`プロパティは、フレームワークに固有です。</span><span class="sxs-lookup"><span data-stu-id="ce24f-149">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="ce24f-150">内の情報を使用して、`Resource`プロパティが特定のフレームワークを承認ポリシーを制限します。</span><span class="sxs-lookup"><span data-stu-id="ce24f-150">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="ce24f-151">キャストする必要があります、`Resource`プロパティを使用して、`as`コードがクラッシュしないことを確認するキーワード、およびチェックインのキャストが成功で`InvalidCastExceptions`; 他のフレームワークで実行されたとき</span><span class="sxs-lookup"><span data-stu-id="ce24f-151">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
