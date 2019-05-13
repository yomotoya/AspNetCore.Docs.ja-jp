---
title: ASP.NET Core でのカスタム承認ポリシー プロバイダー
author: mjrousos
description: ASP.NET Core アプリでカスタム IAuthorizationPolicyProvider を使用して、承認ポリシーを動的に生成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896359"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>ASP.NET Core で IAuthorizationPolicyProvider を使用してカスタム承認ポリシー プロバイダー 

によって[Mike Rousos](https://github.com/mjrousos)

通常使用時に[ポリシー ベースの承認](xref:security/authorization/policies)、呼び出すことによってポリシーが登録されている`AuthorizationOptions.AddPolicy`承認サービスの構成の一部として。 一部のシナリオである可能性がありますいない (または不適切) をこの方法ですべての承認ポリシーを登録します。 カスタムを使用するような場合、`IAuthorizationPolicyProvider`承認ポリシーを指定する方法を制御します。

シナリオの例については、カスタム[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)役立つ可能性がありますが含まれます。

* 外部サービスを使用すると、ポリシーの評価を提供します。
* (別の部屋番号または例では、年齢層) 用のさまざまなポリシーを使用して、これは無意味で各個々 の承認ポリシーを追加する、`AuthorizationOptions.AddPolicy`呼び出します。
* (データベース) のような外部データ ソース内の情報に基づいて実行時ポリシーを作成するか、別のメカニズムによって承認要件を動的に決定します。

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)から、 [AspNetCore GitHub リポジトリ](https://github.com/aspnet/AspNetCore)します。 Aspnet/AspNetCore リポジトリの ZIP ファイルをダウンロードします。 ファイルを解凍します。 移動し、 *src、セキュリティ、サンプル/CustomPolicyProvider*プロジェクト フォルダーです。

## <a name="customize-policy-retrieval"></a>ポリシーの取得をカスタマイズします。

ASP.NET Core アプリの実装を使用して、`IAuthorizationPolicyProvider`承認ポリシーを取得するインターフェイス。 既定では、 [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)登録され、使用します。 `DefaultAuthorizationPolicyProvider` ポリシーを返します、`AuthorizationOptions`で提供される、`IServiceCollection.AddAuthorization`呼び出します。

この動作をカスタマイズするには、別の登録`IAuthorizationPolicyProvider`アプリの実装[依存関係の注入](xref:fundamentals/dependency-injection)コンテナー。 

`IAuthorizationPolicyProvider`インターフェイスには、2 つの Api が含まれています。

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)承認ポリシーを指定した名前を返します。
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)は既定の承認ポリシーを返します (に対して使用されるポリシー`[Authorize]`ポリシーが指定されていない属性)。 

これら 2 つの Api を実装すると、承認ポリシーの提供方法をカスタマイズできます。

## <a name="parameterized-authorize-attribute-example"></a>パラメーター化された属性の例の承認

1 つのシナリオ、`IAuthorizationPolicyProvider`役に立ちますカスタムを有効にする`[Authorize]`属性の要件は、パラメーターによって異なります。 たとえば、[ポリシー ベースの承認](xref:security/authorization/policies)年齢に基づいた ("AtLeast21") をサンプルとして使用されたポリシーのドキュメント。 アプリ内の別のコント ローラー アクションのユーザーに可能にするかどう*異なる*年齢で撮った多くの異なる有効期間に基づくポリシーを設定すると便利かもしれません。 異なる年齢に基づいたされるすべてのポリシーに必要なアプリケーションを登録するのではなく`AuthorizationOptions`、動的にカスタム ポリシーを生成する`IAuthorizationPolicyProvider`します。 ポリシーを簡単に使用するためは、ようにカスタム承認属性を持つアクションの注釈を付ける`[MinimumAgeAuthorize(20)]`します。

## <a name="custom-authorization-attributes"></a>カスタム承認属性

承認ポリシーは、名前によって識別されます。 カスタム`MinimumAgeAuthorizeAttribute`説明されている以前に対応する承認ポリシーの取得に使用できる文字列に引数をマップする必要があります。 派生することによってこれを行う`AuthorizeAttribute`を行うと、`Age`プロパティ ラップ、`AuthorizeAttribute.Policy`プロパティ。

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

この属性の型が、`Policy`ハード コーディングされたプレフィックスに基づいた文字列 (`"MinimumAge"`) コンス トラクターによって渡された整数とします。

同じように他のアクションに適用することができます`Authorize`属性の整数をパラメーターとして受け取る点を除いて。

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>カスタム IAuthorizationPolicyProvider

カスタム`MinimumAgeAuthorizeAttribute`必要な最小年齢の要求の承認ポリシーに簡単です。 次の問題を解決するためを行ってそれらのさまざまな年齢層のすべての承認ポリシーが使用できることを確認します。 これは、ような場合、`IAuthorizationPolicyProvider`役に立ちます。

使用する場合`MinimumAgeAuthorizationAttribute`、承認ポリシーの名前はパターン`"MinimumAge" + Age`ため、カスタム`IAuthorizationPolicyProvider`で承認ポリシーを生成する必要があります。

* ポリシーの名前からの経過時間を解析します。
* 使用して`AuthorizationPolicyBuilder`新たに作成するには `AuthorizationPolicy`
* 経過期間に基づいて、ポリシーに要件を追加する`AuthorizationPolicyBuilder.AddRequirements`します。 他のシナリオで使用する場合があります`RequireClaim`、 `RequireRole`、または`RequireUserName`代わりにします。

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

## <a name="multiple-authorization-policy-providers"></a>複数の承認ポリシー プロバイダー

カスタムの使用時に`IAuthorizationPolicyProvider`、実装は、ASP.NET Core は 1 つのインスタンスだけを使用することに留意してください`IAuthorizationPolicyProvider`します。 カスタム プロバイダーが使用されるすべてのポリシー名の承認ポリシーを指定することがない場合は、バックアップのプロバイダーにフォールバックする必要があります。 

たとえば、カスタムの経過期間ポリシーとは、従来のロール ベースのポリシーの取得の両方が必要なアプリケーションを検討してください。 このようなアプリは、カスタム承認ポリシー プロバイダーを使用できますです。

* ポリシー名を解析しようとします。 
* 別のポリシー プロバイダーを呼び出す (など`DefaultAuthorizationPolicyProvider`) ポリシーの名前には、年齢が含まれていない場合。

例では、`IAuthorizationPolicyProvider`に上記の実装を更新することができます、 `DefaultAuthorizationPolicyProvider` (ポリシー名が 'MinimumAge' + 年齢の想定されるパターンと一致しない場合に使用) をそのコンス トラクターでフォールバック ポリシー プロバイダーを作成しています。

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

次に、`GetPolicyAsync`を使用するメソッドを更新することができます、 `FallbackPolicyProvider` null を返す代わりにします。

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>既定のポリシー

カスタムの名前付きの承認ポリシーを提供するだけでなく`IAuthorizationPolicyProvider`を実装する必要がある`GetDefaultPolicyAsync`承認ポリシーを提供する`[Authorize]`ポリシー名が指定のない属性です。

多くの場合、この承認属性のみが必要です、認証されたユーザーへの呼び出しに必要なポリシーを活用するため`RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

カスタムのすべての側面と同様`IAuthorizationPolicyProvider`、必要に応じて、これをカスタマイズできます。 場合によっては、フォールバックから既定のポリシーを取得することが望ましい場合があります`IAuthorizationPolicyProvider`します。

## <a name="required-policy"></a>必要なポリシー

カスタム`IAuthorizationPolicyProvider`を実装する必要がある`GetRequiredPolicyAsync`に、必要に応じて、常に必要なポリシーを指定します。 場合`GetRequiredPolicyAsync`null 以外のポリシーを返します (既定または名前付き) で、他と組み合わせて、ポリシーが要求されたポリシー。

必要なポリシーが不要な場合、プロバイダーできますだけは null を返すまたはフォールバック プロバイダーに延期します。

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>カスタム IAuthorizationPolicyProvider を使用します。

カスタム ポリシーを使用する、 `IAuthorizationPolicyProvider`、する必要があります。

* 適切な登録`AuthorizationHandler`依存関係の挿入の種類 (で説明されている[ポリシー ベースの承認](xref:security/authorization/policies#authorization-handlers)) すべてのポリシー ベースの承認のシナリオと同様に、します。
* カスタム登録`IAuthorizationPolicyProvider`アプリの依存関係注入サービス コレクション内の型 (で`Startup.ConfigureServices`) を既定のポリシー プロバイダーを置き換えます。

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

完全なカスタム`IAuthorizationPolicyProvider`サンプルは、 [aspnet/AuthSamples GitHub リポジトリ](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)します。
