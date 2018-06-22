---
title: ASP.NET Core でのカスタム承認ポリシー プロバイダー
author: mjrousos
description: ASP.NET Core アプリケーションでカスタム IAuthorizationPolicyProvider を使用して、承認ポリシーを動的に生成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277258"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="ca853-103">ASP.NET Core で IAuthorizationPolicyProvider を使用して、カスタム承認ポリシーのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="ca853-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="ca853-104">によって[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="ca853-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="ca853-105">通常を使用する場合[ポリシー ベースの承認](xref:security/authorization/policies)、呼び出すことによりポリシーが登録されている`AuthorizationOptions.AddPolicy`承認サービス構成の一部として。</span><span class="sxs-lookup"><span data-stu-id="ca853-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="ca853-106">一部のシナリオである可能性がありますいない不可能 (または不適切) をこの方法ですべての承認ポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="ca853-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="ca853-107">カスタムを使用するような場合、`IAuthorizationPolicyProvider`承認ポリシーを提供する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="ca853-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="ca853-108">シナリオの例については、カスタム[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)役立つことがありますが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ca853-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="ca853-109">外部サービスを使用すると、ポリシーの評価を提供します。</span><span class="sxs-lookup"><span data-stu-id="ca853-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="ca853-110">広範囲のポリシーを使用して、(別の部屋番号の例については、歳)、これをなさない各を使用して個々 の承認ポリシーを追加する、`AuthorizationOptions.AddPolicy`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ca853-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="ca853-111">(データベースなど)、外部データ ソースの情報に基づいて実行時ポリシーを作成するか、別のメカニズムによって承認要件を動的に決定します。</span><span class="sxs-lookup"><span data-stu-id="ca853-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="ca853-112">ポリシーの取得をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ca853-112">Customizing policy retrieval</span></span>

<span data-ttu-id="ca853-113">ASP.NET Core アプリケーションの実装を使用して、`IAuthorizationPolicyProvider`承認ポリシーを取得するインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="ca853-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="ca853-114">既定では、 [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)登録され、使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca853-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="ca853-115">`DefaultAuthorizationPolicyProvider` ポリシーを返します、`AuthorizationOptions`で提供される、`IServiceCollection.AddAuthorization`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ca853-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="ca853-116">この動作をカスタマイズするには、別の登録をすることによって`IAuthorizationPolicyProvider`アプリの実装[依存性の注入](xref:fundamentals/dependency-injection)コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="ca853-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="ca853-117">`IAuthorizationPolicyProvider`インターフェイスには、2 つの Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ca853-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="ca853-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)指定した名前の承認ポリシーを返します。</span><span class="sxs-lookup"><span data-stu-id="ca853-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="ca853-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0)既定の承認ポリシーを返します (に対して使用されるポリシー`[Authorize]`ポリシーが指定されていない属性)。</span><span class="sxs-lookup"><span data-stu-id="ca853-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="ca853-120">これら 2 つの Api を実装すると、承認ポリシーの指定方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="ca853-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="ca853-121">パラメーター化された属性の例の承認</span><span class="sxs-lookup"><span data-stu-id="ca853-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="ca853-122">1 つのシナリオで`IAuthorizationPolicyProvider`役に立ちますで有効にしてカスタム`[Authorize]`属性が持つ要件が、パラメーターに依存します。</span><span class="sxs-lookup"><span data-stu-id="ca853-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="ca853-123">たとえば、[ポリシー ベースの承認](xref:security/authorization/policies)ドキュメントについては、年齢に基づいた ("AtLeast21") ポリシーは、サンプルとして使用されました。</span><span class="sxs-lookup"><span data-stu-id="ca853-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="ca853-124">かどうか、アプリ内の別のコント ローラー アクション可能になります。 のユーザーに*異なる*年齢、多くのさまざまな有効期間に基づくポリシーを持つ便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="ca853-125">異なる年齢に基づいたされるすべてのポリシーに必要なアプリケーションの登録ではなく`AuthorizationOptions`、動的にカスタム ポリシーを生成する`IAuthorizationPolicyProvider`です。</span><span class="sxs-lookup"><span data-stu-id="ca853-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="ca853-126">ポリシーをより簡単に使用するには、注釈を付けて、アクションと同様に、カスタム承認属性を持つ`[MinimumAgeAuthorize(20)]`します。</span><span class="sxs-lookup"><span data-stu-id="ca853-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="ca853-127">カスタム承認属性</span><span class="sxs-lookup"><span data-stu-id="ca853-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="ca853-128">承認ポリシーは、その名前で識別されます。</span><span class="sxs-lookup"><span data-stu-id="ca853-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="ca853-129">カスタム`MinimumAgeAuthorizeAttribute`説明されている以前に対応する承認ポリシーを取得するために使用する文字列に引数をマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="ca853-130">派生することによってこれを行う`AuthorizeAttribute`を行うと、`Age`プロパティ ラップ、`AuthorizeAttribute.Policy`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ca853-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="ca853-131">この属性の型が、`Policy`文字列プレフィックスに基づく、ハード コーディングされた (`"MinimumAge"`) し、コンス トラクターを使用して渡された整数。</span><span class="sxs-lookup"><span data-stu-id="ca853-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="ca853-132">同じ方法で他のアクションに適用することができます`Authorize`整数パラメーターとして受け取る点を除いての属性です。</span><span class="sxs-lookup"><span data-stu-id="ca853-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="ca853-133">カスタム IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="ca853-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="ca853-134">カスタム`MinimumAgeAuthorizeAttribute`簡単に要求承認ポリシーの最小経過期間が必要なのです。</span><span class="sxs-lookup"><span data-stu-id="ca853-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="ca853-135">次の問題を解決することを確認、さまざまな年齢層のすべての承認ポリシーが使用できることです。</span><span class="sxs-lookup"><span data-stu-id="ca853-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="ca853-136">これは、ような場合、`IAuthorizationPolicyProvider`役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="ca853-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="ca853-137">使用する場合`MinimumAgeAuthorizationAttribute`、承認ポリシーの名前は、パターンに従いますが`"MinimumAge" + Age`ため、カスタム`IAuthorizationPolicyProvider`によって承認ポリシーを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="ca853-138">ポリシー名からの年齢を解析します。</span><span class="sxs-lookup"><span data-stu-id="ca853-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="ca853-139">使用して`AuthorizationPolicyBuiler`を新規作成するには `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="ca853-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="ca853-140">ポリシーへの要件を追加、年齢に基づいて`AuthorizationPolicyBuilder.AddRequirements`です。</span><span class="sxs-lookup"><span data-stu-id="ca853-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="ca853-141">他のシナリオで使用する場合があります`RequireClaim`、 `RequireRole`、または`RequireUserName`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="ca853-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="ca853-142">複数の承認ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="ca853-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="ca853-143">ユーザー設定を使用するときに`IAuthorizationPolicyProvider`実装では、ASP.NET Core は 1 つのインスタンスだけを使用する点に注意して`IAuthorizationPolicyProvider`です。</span><span class="sxs-lookup"><span data-stu-id="ca853-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="ca853-144">カスタム プロバイダーがすべてのポリシー名の承認ポリシーを指定することがない場合は、バックアップのプロバイダーに戻る必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="ca853-145">ポリシー名の既定のポリシーに由来するものがあります`[Authorize]`名前のない属性です。</span><span class="sxs-lookup"><span data-stu-id="ca853-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="ca853-146">たとえば、カスタムの経過期間ポリシーと従来のロール ベースのポリシーの取得の両方に必要なアプリケーションを検討します。</span><span class="sxs-lookup"><span data-stu-id="ca853-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="ca853-147">このようなアプリは、カスタム承認ポリシー プロバイダーを使用してでしたです。</span><span class="sxs-lookup"><span data-stu-id="ca853-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="ca853-148">ポリシー名を解析を試みます。</span><span class="sxs-lookup"><span data-stu-id="ca853-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="ca853-149">別のポリシー プロバイダーを呼び出す (と同様に`DefaultAuthorizationPolicyProvider`) ポリシーの名前には、年齢が含まれていない場合。</span><span class="sxs-lookup"><span data-stu-id="ca853-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="ca853-150">既定のポリシー</span><span class="sxs-lookup"><span data-stu-id="ca853-150">Default policy</span></span>

<span data-ttu-id="ca853-151">カスタムの名前付きの承認ポリシーを提供するだけでなく`IAuthorizationPolicyProvider`を実装する必要がある`GetDefaultPolicyAsync`の承認ポリシーを提供する`[Authorize]`ポリシー名が指定のない属性です。</span><span class="sxs-lookup"><span data-stu-id="ca853-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="ca853-152">多くの場合、この承認属性のみが必要です、認証されたユーザーへの呼び出しに必要なポリシーを行うことができますので`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="ca853-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="ca853-153">カスタムのすべての側面と同様に`IAuthorizationPolicyProvider`、必要に応じて、これには、カスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="ca853-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="ca853-154">場合によっては：</span><span class="sxs-lookup"><span data-stu-id="ca853-154">In some cases:</span></span>

* <span data-ttu-id="ca853-155">既定の承認ポリシーを使用ない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="ca853-156">フォールバックに既定のポリシー取得を委任できます`IAuthorizationPolicyProvider`です。</span><span class="sxs-lookup"><span data-stu-id="ca853-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="ca853-157">カスタム IAuthorizationPolicyProvider を使用します。</span><span class="sxs-lookup"><span data-stu-id="ca853-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="ca853-158">カスタム ポリシーを使用する、 `IAuthorizationPolicyProvider`、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca853-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="ca853-159">適切な登録`AuthorizationHandler`依存関係の挿入を持つ型 (」に記載[ポリシー ベースの承認](xref:security/authorization/policies#authorization-handlers))、すべてのポリシー ベースの承認のシナリオと同様です。</span><span class="sxs-lookup"><span data-stu-id="ca853-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="ca853-160">カスタム登録`IAuthorizationPolicyProvider`アプリの依存関係の挿入サービス コレクション内の型 (で`Startup.ConfigureServices`) を置き換える既定のポリシー プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="ca853-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="ca853-161">完全なカスタム`IAuthorizationPolicyProvider`サンプルで使用できる、 [aspnet/AuthSamples GitHub リポジトリ](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider)です。</span><span class="sxs-lookup"><span data-stu-id="ca853-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
