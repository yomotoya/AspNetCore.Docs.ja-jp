---
title: ASP.NET Core でのカスタム承認ポリシー プロバイダー
author: mjrousos
description: ASP.NET Core アプリケーションでカスタム IAuthorizationPolicyProvider を使用して、承認ポリシーを動的に生成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 218d7a495655598046671093c0cfe7b9622aca5e
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077603"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>ASP.NET Core で IAuthorizationPolicyProvider を使用して、カスタム承認ポリシーのプロバイダー 

によって[Mike Rousos](https://github.com/mjrousos)

通常を使用する場合[ポリシー ベースの承認](xref:security/authorization/policies)、呼び出すことによりポリシーが登録されている`AuthorizationOptions.AddPolicy`承認サービス構成の一部として。 一部のシナリオである可能性がありますいない不可能 (または不適切) をこの方法ですべての承認ポリシーを登録します。 カスタムを使用するような場合、`IAuthorizationPolicyProvider`承認ポリシーを提供する方法を制御します。

シナリオの例については、カスタム[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)役立つことがありますが含まれます。

* 外部サービスを使用すると、ポリシーの評価を提供します。
* 広範囲のポリシーを使用して、(別の部屋番号の例については、歳)、これをなさない各を使用して個々 の承認ポリシーを追加する、`AuthorizationOptions.AddPolicy`呼び出します。
* (データベースなど)、外部データ ソースの情報に基づいて実行時ポリシーを作成するか、別のメカニズムによって承認要件を動的に決定します。

## <a name="customizing-policy-retrieval"></a>ポリシーの取得をカスタマイズします。

ASP.NET Core アプリケーションの実装を使用して、`IAuthorizationPolicyProvider`承認ポリシーを取得するインターフェイスです。 既定では、 [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)登録され、使用できます。 `DefaultAuthorizationPolicyProvider` ポリシーを返します、`AuthorizationOptions`で提供される、`IServiceCollection.AddAuthorization`呼び出します。

この動作をカスタマイズするには、別の登録をすることによって`IAuthorizationPolicyProvider`アプリの実装[依存性の注入](xref:fundamentals/dependency-injection)コンテナーです。 

`IAuthorizationPolicyProvider`インターフェイスには、2 つの Api が含まれています。

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)指定した名前の承認ポリシーを返します。
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0)既定の承認ポリシーを返します (に対して使用されるポリシー`[Authorize]`ポリシーが指定されていない属性)。 

これら 2 つの Api を実装すると、承認ポリシーの指定方法をカスタマイズできます。

## <a name="parameterized-authorize-attribute-example"></a>パラメーター化された属性の例の承認

1 つのシナリオで`IAuthorizationPolicyProvider`役に立ちますで有効にしてカスタム`[Authorize]`属性が持つ要件が、パラメーターに依存します。 たとえば、[ポリシー ベースの承認](xref:security/authorization/policies)ドキュメントについては、年齢に基づいた ("AtLeast21") ポリシーは、サンプルとして使用されました。 かどうか、アプリ内の別のコント ローラー アクション可能になります。 のユーザーに*異なる*年齢、多くのさまざまな有効期間に基づくポリシーを持つ便利な場合があります。 異なる年齢に基づいたされるすべてのポリシーに必要なアプリケーションの登録ではなく`AuthorizationOptions`、動的にカスタム ポリシーを生成する`IAuthorizationPolicyProvider`です。 ポリシーをより簡単に使用するには、注釈を付けて、アクションと同様に、カスタム承認属性を持つ`[MinimumAgeAuthorize(20)]`します。

## <a name="custom-authorization-attributes"></a>カスタム承認属性

承認ポリシーは、その名前で識別されます。 カスタム`MinimumAgeAuthorizeAttribute`説明されている以前に対応する承認ポリシーを取得するために使用する文字列に引数をマップする必要があります。 派生することによってこれを行う`AuthorizeAttribute`を行うと、`Age`プロパティ ラップ、`AuthorizeAttribute.Policy`プロパティです。

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

この属性の型が、`Policy`文字列プレフィックスに基づく、ハード コーディングされた (`"MinimumAge"`) し、コンス トラクターを使用して渡された整数。

同じ方法で他のアクションに適用することができます`Authorize`整数パラメーターとして受け取る点を除いての属性です。

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>カスタム IAuthorizationPolicyProvider

カスタム`MinimumAgeAuthorizeAttribute`簡単に要求承認ポリシーの最小経過期間が必要なのです。 次の問題を解決することを確認、さまざまな年齢層のすべての承認ポリシーが使用できることです。 これは、ような場合、`IAuthorizationPolicyProvider`役に立ちます。

使用する場合`MinimumAgeAuthorizationAttribute`、承認ポリシーの名前は、パターンに従いますが`"MinimumAge" + Age`ため、カスタム`IAuthorizationPolicyProvider`によって承認ポリシーを生成する必要があります。

* ポリシー名からの年齢を解析します。
* 使用して`AuthorizationPolicyBuilder`を新規作成するには `AuthorizationPolicy`
* ポリシーへの要件を追加、年齢に基づいて`AuthorizationPolicyBuilder.AddRequirements`です。 他のシナリオで使用する場合があります`RequireClaim`、 `RequireRole`、または`RequireUserName`代わりにします。

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

## <a name="multiple-authorization-policy-providers"></a>複数の承認ポリシー プロバイダー

ユーザー設定を使用するときに`IAuthorizationPolicyProvider`実装では、ASP.NET Core は 1 つのインスタンスだけを使用する点に注意して`IAuthorizationPolicyProvider`です。 カスタム プロバイダーがすべてのポリシー名の承認ポリシーを指定することがない場合は、バックアップのプロバイダーに戻る必要があります。 ポリシー名の既定のポリシーに由来するものがあります`[Authorize]`名前のない属性です。

たとえば、カスタムの経過期間ポリシーと従来のロール ベースのポリシーの取得の両方に必要なアプリケーションを検討します。 このようなアプリは、カスタム承認ポリシー プロバイダーを使用してでしたです。

* ポリシー名を解析を試みます。 
* 別のポリシー プロバイダーを呼び出す (と同様に`DefaultAuthorizationPolicyProvider`) ポリシーの名前には、年齢が含まれていない場合。

## <a name="default-policy"></a>既定のポリシー

カスタムの名前付きの承認ポリシーを提供するだけでなく`IAuthorizationPolicyProvider`を実装する必要がある`GetDefaultPolicyAsync`の承認ポリシーを提供する`[Authorize]`ポリシー名が指定のない属性です。

多くの場合、この承認属性のみが必要です、認証されたユーザーへの呼び出しに必要なポリシーを行うことができますので`RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

カスタムのすべての側面と同様に`IAuthorizationPolicyProvider`、必要に応じて、これには、カスタマイズすることができます。 場合によっては：

* 既定の承認ポリシーを使用ない可能性があります。
* フォールバックに既定のポリシー取得を委任できます`IAuthorizationPolicyProvider`です。

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>カスタム IAuthorizationPolicyProvider を使用します。

カスタム ポリシーを使用する、 `IAuthorizationPolicyProvider`、する必要があります。

* 適切な登録`AuthorizationHandler`依存関係の挿入を持つ型 (」に記載[ポリシー ベースの承認](xref:security/authorization/policies#authorization-handlers))、すべてのポリシー ベースの承認のシナリオと同様です。
* カスタム登録`IAuthorizationPolicyProvider`アプリの依存関係の挿入サービス コレクション内の型 (で`Startup.ConfigureServices`) を置き換える既定のポリシー プロバイダー。

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

完全なカスタム`IAuthorizationPolicyProvider`サンプルで使用できる、 [aspnet/AuthSamples GitHub リポジトリ](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider)です。
