---
title: ASP.NET Core でのカスタム承認ポリシー プロバイダー
author: mjrousos
description: ASP.NET Core アプリでカスタム IAuthorizationPolicyProvider を使用して、承認ポリシーを動的に生成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614410"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="10111-103">ASP.NET Core で IAuthorizationPolicyProvider を使用してカスタム承認ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="10111-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="10111-104">によって[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="10111-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="10111-105">通常使用時に[ポリシー ベースの承認](xref:security/authorization/policies)、呼び出すことによってポリシーが登録されている`AuthorizationOptions.AddPolicy`承認サービスの構成の一部として。</span><span class="sxs-lookup"><span data-stu-id="10111-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="10111-106">一部のシナリオである可能性がありますいない (または不適切) をこの方法ですべての承認ポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="10111-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="10111-107">カスタムを使用するような場合、`IAuthorizationPolicyProvider`承認ポリシーを指定する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="10111-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="10111-108">シナリオの例については、カスタム[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)役立つ可能性がありますが含まれます。</span><span class="sxs-lookup"><span data-stu-id="10111-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="10111-109">外部サービスを使用すると、ポリシーの評価を提供します。</span><span class="sxs-lookup"><span data-stu-id="10111-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="10111-110">(別の部屋番号または例では、年齢層) 用のさまざまなポリシーを使用して、これは無意味で各個々 の承認ポリシーを追加する、`AuthorizationOptions.AddPolicy`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="10111-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="10111-111">(データベース) のような外部データ ソース内の情報に基づいて実行時ポリシーを作成するか、別のメカニズムによって承認要件を動的に決定します。</span><span class="sxs-lookup"><span data-stu-id="10111-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="10111-112">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)から、 [AspNetCore GitHub リポジトリ](https://github.com/aspnet/AspNetCore)します。</span><span class="sxs-lookup"><span data-stu-id="10111-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="10111-113">Aspnet/AspNetCore リポジトリの ZIP ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="10111-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="10111-114">ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="10111-114">Unzip the file.</span></span> <span data-ttu-id="10111-115">移動し、 *src、セキュリティ、サンプル/CustomPolicyProvider*プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="10111-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="10111-116">ポリシーの取得をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="10111-116">Customize policy retrieval</span></span>

<span data-ttu-id="10111-117">ASP.NET Core アプリの実装を使用して、`IAuthorizationPolicyProvider`承認ポリシーを取得するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="10111-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="10111-118">既定では、 [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)登録され、使用します。</span><span class="sxs-lookup"><span data-stu-id="10111-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="10111-119">`DefaultAuthorizationPolicyProvider` ポリシーを返します、`AuthorizationOptions`で提供される、`IServiceCollection.AddAuthorization`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="10111-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="10111-120">この動作をカスタマイズするには、別の登録`IAuthorizationPolicyProvider`アプリの実装[依存関係の注入](xref:fundamentals/dependency-injection)コンテナー。</span><span class="sxs-lookup"><span data-stu-id="10111-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="10111-121">`IAuthorizationPolicyProvider`インターフェイスには、2 つの Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="10111-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="10111-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)承認ポリシーを指定した名前を返します。</span><span class="sxs-lookup"><span data-stu-id="10111-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="10111-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)は既定の承認ポリシーを返します (に対して使用されるポリシー`[Authorize]`ポリシーが指定されていない属性)。</span><span class="sxs-lookup"><span data-stu-id="10111-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="10111-124">これら 2 つの Api を実装すると、承認ポリシーの提供方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="10111-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="10111-125">パラメーター化された属性の例の承認</span><span class="sxs-lookup"><span data-stu-id="10111-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="10111-126">1 つのシナリオ、`IAuthorizationPolicyProvider`役に立ちますカスタムを有効にする`[Authorize]`属性の要件は、パラメーターによって異なります。</span><span class="sxs-lookup"><span data-stu-id="10111-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="10111-127">たとえば、[ポリシー ベースの承認](xref:security/authorization/policies)年齢に基づいた ("AtLeast21") をサンプルとして使用されたポリシーのドキュメント。</span><span class="sxs-lookup"><span data-stu-id="10111-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="10111-128">アプリ内の別のコント ローラー アクションのユーザーに可能にするかどう*異なる*年齢で撮った多くの異なる有効期間に基づくポリシーを設定すると便利かもしれません。</span><span class="sxs-lookup"><span data-stu-id="10111-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="10111-129">異なる年齢に基づいたされるすべてのポリシーに必要なアプリケーションを登録するのではなく`AuthorizationOptions`、動的にカスタム ポリシーを生成する`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="10111-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="10111-130">ポリシーを簡単に使用するためは、ようにカスタム承認属性を持つアクションの注釈を付ける`[MinimumAgeAuthorize(20)]`します。</span><span class="sxs-lookup"><span data-stu-id="10111-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="10111-131">カスタム承認属性</span><span class="sxs-lookup"><span data-stu-id="10111-131">Custom Authorization attributes</span></span>

<span data-ttu-id="10111-132">承認ポリシーは、名前によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="10111-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="10111-133">カスタム`MinimumAgeAuthorizeAttribute`説明されている以前に対応する承認ポリシーの取得に使用できる文字列に引数をマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="10111-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="10111-134">派生することによってこれを行う`AuthorizeAttribute`を行うと、`Age`プロパティ ラップ、`AuthorizeAttribute.Policy`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="10111-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
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

<span data-ttu-id="10111-135">この属性の型が、`Policy`ハード コーディングされたプレフィックスに基づいた文字列 (`"MinimumAge"`) コンス トラクターによって渡された整数とします。</span><span class="sxs-lookup"><span data-stu-id="10111-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="10111-136">同じように他のアクションに適用することができます`Authorize`属性の整数をパラメーターとして受け取る点を除いて。</span><span class="sxs-lookup"><span data-stu-id="10111-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="10111-137">カスタム IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="10111-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="10111-138">カスタム`MinimumAgeAuthorizeAttribute`必要な最小年齢の要求の承認ポリシーに簡単です。</span><span class="sxs-lookup"><span data-stu-id="10111-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="10111-139">次の問題を解決するためを行ってそれらのさまざまな年齢層のすべての承認ポリシーが使用できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="10111-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="10111-140">これは、ような場合、`IAuthorizationPolicyProvider`役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="10111-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="10111-141">使用する場合`MinimumAgeAuthorizationAttribute`、承認ポリシーの名前はパターン`"MinimumAge" + Age`ため、カスタム`IAuthorizationPolicyProvider`で承認ポリシーを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10111-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="10111-142">ポリシーの名前からの経過時間を解析します。</span><span class="sxs-lookup"><span data-stu-id="10111-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="10111-143">使用して`AuthorizationPolicyBuilder`新たに作成するには `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="10111-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="10111-144">経過期間に基づいて、ポリシーに要件を追加する`AuthorizationPolicyBuilder.AddRequirements`します。</span><span class="sxs-lookup"><span data-stu-id="10111-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="10111-145">他のシナリオで使用する場合があります`RequireClaim`、 `RequireRole`、または`RequireUserName`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="10111-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="10111-146">複数の承認ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="10111-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="10111-147">カスタムの使用時に`IAuthorizationPolicyProvider`、実装は、ASP.NET Core は 1 つのインスタンスだけを使用することに留意してください`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="10111-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="10111-148">カスタム プロバイダーが使用されるすべてのポリシー名の承認ポリシーを指定することがない場合は、バックアップのプロバイダーにフォールバックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="10111-148">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="10111-149">たとえば、カスタムの経過期間ポリシーとは、従来のロール ベースのポリシーの取得の両方が必要なアプリケーションを検討してください。</span><span class="sxs-lookup"><span data-stu-id="10111-149">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="10111-150">このようなアプリは、カスタム承認ポリシー プロバイダーを使用できますです。</span><span class="sxs-lookup"><span data-stu-id="10111-150">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="10111-151">ポリシー名を解析しようとします。</span><span class="sxs-lookup"><span data-stu-id="10111-151">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="10111-152">別のポリシー プロバイダーを呼び出す (など`DefaultAuthorizationPolicyProvider`) ポリシーの名前には、年齢が含まれていない場合。</span><span class="sxs-lookup"><span data-stu-id="10111-152">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="10111-153">例では、`IAuthorizationPolicyProvider`に上記の実装を更新することができます、 `DefaultAuthorizationPolicyProvider` (ポリシー名が 'MinimumAge' + 年齢の想定されるパターンと一致しない場合に使用) をそのコンス トラクターでフォールバック ポリシー プロバイダーを作成しています。</span><span class="sxs-lookup"><span data-stu-id="10111-153">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="10111-154">次に、`GetPolicyAsync`を使用するメソッドを更新することができます、 `FallbackPolicyProvider` null を返す代わりにします。</span><span class="sxs-lookup"><span data-stu-id="10111-154">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="10111-155">既定のポリシー</span><span class="sxs-lookup"><span data-stu-id="10111-155">Default policy</span></span>

<span data-ttu-id="10111-156">カスタムの名前付きの承認ポリシーを提供するだけでなく`IAuthorizationPolicyProvider`を実装する必要がある`GetDefaultPolicyAsync`承認ポリシーを提供する`[Authorize]`ポリシー名が指定のない属性です。</span><span class="sxs-lookup"><span data-stu-id="10111-156">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="10111-157">多くの場合、この承認属性のみが必要です、認証されたユーザーへの呼び出しに必要なポリシーを活用するため`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="10111-157">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="10111-158">カスタムのすべての側面と同様`IAuthorizationPolicyProvider`、必要に応じて、これをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="10111-158">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="10111-159">場合によっては、フォールバックから既定のポリシーを取得することが望ましい場合があります`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="10111-159">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="10111-160">必要なポリシー</span><span class="sxs-lookup"><span data-stu-id="10111-160">Required policy</span></span>

<span data-ttu-id="10111-161">カスタム`IAuthorizationPolicyProvider`を実装する必要がある`GetRequiredPolicyAsync`に、必要に応じて、常に必要なポリシーを指定します。</span><span class="sxs-lookup"><span data-stu-id="10111-161">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="10111-162">場合`GetRequiredPolicyAsync`null 以外のポリシーを返します (既定または名前付き) で、他と組み合わせて、ポリシーが要求されたポリシー。</span><span class="sxs-lookup"><span data-stu-id="10111-162">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="10111-163">必要なポリシーが不要な場合、プロバイダーできますだけは null を返すまたはフォールバック プロバイダーに延期します。</span><span class="sxs-lookup"><span data-stu-id="10111-163">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="10111-164">カスタム IAuthorizationPolicyProvider を使用します。</span><span class="sxs-lookup"><span data-stu-id="10111-164">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="10111-165">カスタム ポリシーを使用する、 `IAuthorizationPolicyProvider`、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10111-165">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="10111-166">適切な登録`AuthorizationHandler`依存関係の挿入の種類 (で説明されている[ポリシー ベースの承認](xref:security/authorization/policies#authorization-handlers)) すべてのポリシー ベースの承認のシナリオと同様に、します。</span><span class="sxs-lookup"><span data-stu-id="10111-166">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="10111-167">カスタム登録`IAuthorizationPolicyProvider`アプリの依存関係注入サービス コレクション内の型 (で`Startup.ConfigureServices`) を既定のポリシー プロバイダーを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10111-167">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="10111-168">完全なカスタム`IAuthorizationPolicyProvider`サンプルは、 [aspnet/AuthSamples GitHub リポジトリ](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)します。</span><span class="sxs-lookup"><span data-stu-id="10111-168">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
