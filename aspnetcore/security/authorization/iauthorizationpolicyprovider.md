---
title: ASP.NET Core でのカスタム承認ポリシー プロバイダー
author: mjrousos
description: ASP.NET Core アプリでカスタム IAuthorizationPolicyProvider を使用して、承認ポリシーを動的に生成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ca57a9fd8e3c11f15fe14bbe4538bc748c4c84b6
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444156"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="4e208-103">ASP.NET Core で IAuthorizationPolicyProvider を使用してカスタム承認ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4e208-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="4e208-104">によって[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="4e208-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="4e208-105">通常使用時に[ポリシー ベースの承認](xref:security/authorization/policies)、呼び出すことによってポリシーが登録されている`AuthorizationOptions.AddPolicy`承認サービスの構成の一部として。</span><span class="sxs-lookup"><span data-stu-id="4e208-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="4e208-106">一部のシナリオである可能性がありますいない (または不適切) をこの方法ですべての承認ポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="4e208-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="4e208-107">カスタムを使用するような場合、`IAuthorizationPolicyProvider`承認ポリシーを指定する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="4e208-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="4e208-108">シナリオの例については、カスタム[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)役立つ可能性がありますが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4e208-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="4e208-109">外部サービスを使用すると、ポリシーの評価を提供します。</span><span class="sxs-lookup"><span data-stu-id="4e208-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="4e208-110">(別の部屋番号または例では、年齢層) 用のさまざまなポリシーを使用して、これは無意味で各個々 の承認ポリシーを追加する、`AuthorizationOptions.AddPolicy`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4e208-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="4e208-111">(データベース) のような外部データ ソース内の情報に基づいて実行時ポリシーを作成するか、別のメカニズムによって承認要件を動的に決定します。</span><span class="sxs-lookup"><span data-stu-id="4e208-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="4e208-112">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)から、 [AspNetCore GitHub リポジトリ](https://github.com/aspnet/AspNetCore)します。</span><span class="sxs-lookup"><span data-stu-id="4e208-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="4e208-113">Aspnet/AspNetCore リポジトリの ZIP ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4e208-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="4e208-114">ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="4e208-114">Unzip the file.</span></span> <span data-ttu-id="4e208-115">移動し、 *src、セキュリティ、サンプル/CustomPolicyProvider*プロジェクト フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4e208-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="4e208-116">ポリシーの取得をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="4e208-116">Customize policy retrieval</span></span>

<span data-ttu-id="4e208-117">ASP.NET Core アプリの実装を使用して、`IAuthorizationPolicyProvider`承認ポリシーを取得するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="4e208-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="4e208-118">既定では、 [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)登録され、使用します。</span><span class="sxs-lookup"><span data-stu-id="4e208-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="4e208-119">`DefaultAuthorizationPolicyProvider` ポリシーを返します、`AuthorizationOptions`で提供される、`IServiceCollection.AddAuthorization`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4e208-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="4e208-120">この動作をカスタマイズするには、別の登録`IAuthorizationPolicyProvider`アプリの実装[依存関係の注入](xref:fundamentals/dependency-injection)コンテナー。</span><span class="sxs-lookup"><span data-stu-id="4e208-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="4e208-121">`IAuthorizationPolicyProvider`インターフェイスには、2 つの Api が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4e208-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="4e208-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)承認ポリシーを指定した名前を返します。</span><span class="sxs-lookup"><span data-stu-id="4e208-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="4e208-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)は既定の承認ポリシーを返します (に対して使用されるポリシー`[Authorize]`ポリシーが指定されていない属性)。</span><span class="sxs-lookup"><span data-stu-id="4e208-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="4e208-124">これら 2 つの Api を実装すると、承認ポリシーの提供方法をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="4e208-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="4e208-125">パラメーター化された属性の例の承認</span><span class="sxs-lookup"><span data-stu-id="4e208-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="4e208-126">1 つのシナリオ、`IAuthorizationPolicyProvider`役に立ちますカスタムを有効にする`[Authorize]`属性の要件は、パラメーターによって異なります。</span><span class="sxs-lookup"><span data-stu-id="4e208-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="4e208-127">たとえば、[ポリシー ベースの承認](xref:security/authorization/policies)年齢に基づいた ("AtLeast21") をサンプルとして使用されたポリシーのドキュメント。</span><span class="sxs-lookup"><span data-stu-id="4e208-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="4e208-128">アプリ内の別のコント ローラー アクションのユーザーに可能にするかどう*異なる*年齢で撮った多くの異なる有効期間に基づくポリシーを設定すると便利かもしれません。</span><span class="sxs-lookup"><span data-stu-id="4e208-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="4e208-129">異なる年齢に基づいたされるすべてのポリシーに必要なアプリケーションを登録するのではなく`AuthorizationOptions`、動的にカスタム ポリシーを生成する`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="4e208-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="4e208-130">ポリシーを簡単に使用するためは、ようにカスタム承認属性を持つアクションの注釈を付ける`[MinimumAgeAuthorize(20)]`します。</span><span class="sxs-lookup"><span data-stu-id="4e208-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="4e208-131">カスタム承認属性</span><span class="sxs-lookup"><span data-stu-id="4e208-131">Custom Authorization Attributes</span></span>

<span data-ttu-id="4e208-132">承認ポリシーは、名前によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="4e208-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="4e208-133">カスタム`MinimumAgeAuthorizeAttribute`説明されている以前に対応する承認ポリシーの取得に使用できる文字列に引数をマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e208-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="4e208-134">派生することによってこれを行う`AuthorizeAttribute`を行うと、`Age`プロパティ ラップ、`AuthorizeAttribute.Policy`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4e208-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="4e208-135">この属性の型が、`Policy`ハード コーディングされたプレフィックスに基づいた文字列 (`"MinimumAge"`) コンス トラクターによって渡された整数とします。</span><span class="sxs-lookup"><span data-stu-id="4e208-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="4e208-136">同じように他のアクションに適用することができます`Authorize`属性の整数をパラメーターとして受け取る点を除いて。</span><span class="sxs-lookup"><span data-stu-id="4e208-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="4e208-137">カスタム IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="4e208-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="4e208-138">カスタム`MinimumAgeAuthorizeAttribute`必要な最小年齢の要求の承認ポリシーに簡単です。</span><span class="sxs-lookup"><span data-stu-id="4e208-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="4e208-139">次の問題を解決するためを行ってそれらのさまざまな年齢層のすべての承認ポリシーが使用できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4e208-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="4e208-140">これは、ような場合、`IAuthorizationPolicyProvider`役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="4e208-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="4e208-141">使用する場合`MinimumAgeAuthorizationAttribute`、承認ポリシーの名前はパターン`"MinimumAge" + Age`ため、カスタム`IAuthorizationPolicyProvider`で承認ポリシーを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e208-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="4e208-142">ポリシーの名前からの経過時間を解析します。</span><span class="sxs-lookup"><span data-stu-id="4e208-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="4e208-143">使用して`AuthorizationPolicyBuilder`新たに作成するには `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="4e208-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="4e208-144">経過期間に基づいて、ポリシーに要件を追加する`AuthorizationPolicyBuilder.AddRequirements`します。</span><span class="sxs-lookup"><span data-stu-id="4e208-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="4e208-145">他のシナリオで使用する場合があります`RequireClaim`、 `RequireRole`、または`RequireUserName`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4e208-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="4e208-146">複数の承認ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4e208-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="4e208-147">カスタムの使用時に`IAuthorizationPolicyProvider`、実装は、ASP.NET Core は 1 つのインスタンスだけを使用することに留意してください`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="4e208-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="4e208-148">カスタム プロバイダーがすべてのポリシー名の承認ポリシーを指定することがない場合は、バックアップのプロバイダーにフォールバックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e208-148">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="4e208-149">既定のポリシーからポリシーの名前が含まれます`[Authorize]`名前のない属性です。</span><span class="sxs-lookup"><span data-stu-id="4e208-149">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="4e208-150">たとえば、カスタムの経過期間ポリシーとは、従来のロール ベースのポリシーの取得の両方に必要なアプリケーションを検討してください。</span><span class="sxs-lookup"><span data-stu-id="4e208-150">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="4e208-151">このようなアプリは、カスタム承認ポリシー プロバイダーを使用できますです。</span><span class="sxs-lookup"><span data-stu-id="4e208-151">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="4e208-152">ポリシー名を解析しようとします。</span><span class="sxs-lookup"><span data-stu-id="4e208-152">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="4e208-153">別のポリシー プロバイダーを呼び出す (など`DefaultAuthorizationPolicyProvider`) ポリシーの名前には、年齢が含まれていない場合。</span><span class="sxs-lookup"><span data-stu-id="4e208-153">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="4e208-154">既定のポリシー</span><span class="sxs-lookup"><span data-stu-id="4e208-154">Default policy</span></span>

<span data-ttu-id="4e208-155">カスタムの名前付きの承認ポリシーを提供するだけでなく`IAuthorizationPolicyProvider`を実装する必要がある`GetDefaultPolicyAsync`承認ポリシーを提供する`[Authorize]`ポリシー名が指定のない属性です。</span><span class="sxs-lookup"><span data-stu-id="4e208-155">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="4e208-156">多くの場合、この承認属性のみが必要です、認証されたユーザーへの呼び出しに必要なポリシーを活用するため`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="4e208-156">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="4e208-157">カスタムのすべての側面と同様`IAuthorizationPolicyProvider`、必要に応じて、これをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="4e208-157">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="4e208-158">場合によっては：</span><span class="sxs-lookup"><span data-stu-id="4e208-158">In some cases:</span></span>

* <span data-ttu-id="4e208-159">既定の承認ポリシーを使用しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="4e208-159">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="4e208-160">フォールバックに既定のポリシーの取得を委任できます`IAuthorizationPolicyProvider`します。</span><span class="sxs-lookup"><span data-stu-id="4e208-160">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="4e208-161">カスタム IAuthorizationPolicyProvider を使用します。</span><span class="sxs-lookup"><span data-stu-id="4e208-161">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="4e208-162">カスタム ポリシーを使用する、 `IAuthorizationPolicyProvider`、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e208-162">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="4e208-163">適切な登録`AuthorizationHandler`依存関係の挿入の種類 (で説明されている[ポリシー ベースの承認](xref:security/authorization/policies#authorization-handlers)) すべてのポリシー ベースの承認のシナリオと同様に、します。</span><span class="sxs-lookup"><span data-stu-id="4e208-163">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="4e208-164">カスタム登録`IAuthorizationPolicyProvider`アプリの依存関係注入サービス コレクション内の型 (で`Startup.ConfigureServices`) を既定のポリシー プロバイダーを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4e208-164">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="4e208-165">完全なカスタム`IAuthorizationPolicyProvider`サンプルは、 [aspnet/AuthSamples GitHub リポジトリ](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)します。</span><span class="sxs-lookup"><span data-stu-id="4e208-165">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
