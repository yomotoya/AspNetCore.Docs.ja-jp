---
title: "カスタムのポリシー ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: dd7187f67887bb39a5ff425dcbae0927c7565cb8
ms.sourcegitcommit: 41e3e007512c175a42910bc69678f3f0403cab04
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/01/2017
---
# <a name="custom-policy-based-authorization"></a>カスタムのポリシー ベースの承認

<a name=security-authorization-policies-based></a>

背後の[ロールの承認](roles.md#security-authorization-role-based)と[承認を要求](claims.md#security-authorization-claims-based)要件の使用、要件とポリシーを事前構成済みのハンドラーを作成します。 これらの構成要素を使用すると、承認の評価を許可する、豊富なため、再利用可能なコードと簡単にテストが容易な承認の構造を高速にできます。

承認ポリシーを 1 つまたは複数の要件で構成およびに登録されているアプリケーションの起動時に認証サービスの構成の一部として`ConfigureServices`で、 *Startup.cs*ファイル。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

ここで、最小経過期間のこれが渡されるパラメーターとして、要件、1 つの要求で、"Over21"ポリシーが作成されたかを確認できます。

使用してポリシーが適用される、`Authorize`属性名を指定して、ポリシー、たとえば;

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

## <a name="requirements"></a>要件

承認要件は、現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。 最小経過期間ポリシーで、要件があるが 1 つのパラメーター、最小経過期間です。 要件を実装する必要があります`IAuthorizationRequirement`です。 これは、空、マーカーのインターフェイスです。 パラメーター化の最低年齢の要件が実装されているとおりです。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

要件は、データまたはプロパティを持つ必要はありません。

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a>認証ハンドラー

認証ハンドラーは、要件のプロパティの評価にします。 認証ハンドラーが、指定されたに対して評価する必要があります`AuthorizationHandlerContext`承認が許可されたかどうかを決定します。 要件が持てる[複数のハンドラー](policies.md#security-authorization-policies-based-multiple-handlers)です。 ハンドラーを継承する必要があります`AuthorizationHandler<T>`T は、要件を処理します。

<a name=security-authorization-handler-example></a>

最小経過期間ハンドラーは、次のようになります。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

前のコードで最初におる場合、現在のユーザー プリンシパルは、生年月日が発行したことが分かって発行者との信頼の要求を参照してください。 クレームが存在しない場合は、私たちを返すようお承認できません。 どれだけ古いユーザーが調べる、要求した場合と、要件ごとに渡された最小経過期間を満たしている場合、承認が成功しました。 承認が成功した後と呼んで`context.Succeed()`をパラメーターとして正常に実行されている要件に渡すことです。

<a name=security-authorization-policies-based-handler-registration></a>

ハンドラーを構成中に、サービスのコレクションにたとえば; 登録する必要があります。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

使用して各ハンドラーがサービスのコレクションに追加された`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`ハンドラー クラスを渡します。

## <a name="what-should-a-handler-return"></a>ハンドラーの取得と必要がありますか。

表示、[ハンドラーの例](policies.md#security-authorization-handler-example)を`Handle()`メソッドを持たない戻り値は、どのように成功または失敗を示すことですか?

* ハンドラーは、呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証済みです。

* ハンドラーは、同じ要件に対する他のハンドラーが正常に終了すると、一般に、エラーを処理する必要はありません。

* 要件の他のハンドラーが成功する場合でも、エラーを保証、するために呼び出す`context.Fail`です。

内部ハンドラーと呼ばれる要素に関係なく、ポリシー要件が必要な場合に、要件のすべてのハンドラーが呼び出されます。 これにより、要件を常に行われるログ記録など、副作用がある場合でも`context.Fail()`は別のハンドラーで呼び出されました。

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>複数のハンドラーを必要条件の目的は

評価である必要がある場合に、**または**ごとに 1 つの要件の複数のハンドラーを実装します。 たとえば、マイクロソフトでは、キーのカードでのみ開くことがドアがあります。 キー カードを自宅に移動すると、受付は一時的なステッカーを出力し、ドアを開きます。 このシナリオでは 1 つの要求は*EnterBuilding*が複数のハンドラーが各いずれか 1 つの要件を確認します。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

ここが両方のハンドラーを想定して[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)ポリシーと評価される場合、`EnterBuildingRequirement`ポリシーの評価が成功かハンドラーが成功した場合。

## <a name="using-a-func-to-fulfill-a-policy"></a>ポリシーを満たすために、func を使用します。

可能性がありますの機会でポリシーを満たすことがコードで明示する単純です。 だけを指定することは、`Func<AuthorizationHandlerContext, bool>`を使用してポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。

たとえば、前`BadgeEntryHandler`; 次のように書き換えることができます

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

## <a name="accessing-mvc-request-context-in-handlers"></a>ハンドラーで MVC 要求コンテキストにアクセスします。

`Handle`承認ハンドラーで実装する必要がありますのメソッドが 2 つのパラメーター、`AuthorizationContext`と`Requirement`を処理します。 MVC または Jabbr などのフレームワークを自由に任意のオブジェクトを追加、`Resource`プロパティを`AuthorizationContext`余分な情報を通過します。

たとえば MVC がのインスタンスを渡す`Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext`HttpContext、RouteData およびそのすべてのアクセスに使用されるリソースのプロパティでは他の MVC を提供します。

使用、`Resource`プロパティは、フレームワークに固有です。 内の情報を使用して、`Resource`プロパティが特定のフレームワークを承認ポリシーを制限します。 キャストする必要があります、`Resource`プロパティを使用して、`as`コードがクラッシュしないことを確認するキーワード、およびチェックインのキャストが成功で`InvalidCastExceptions`; 他のフレームワークで実行されたとき

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
