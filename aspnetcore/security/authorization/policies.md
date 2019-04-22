---
title: ASP.NET Core でのポリシー ベースの承認
author: rick-anderson
description: 作成し、ASP.NET Core アプリでの承認要件を適用するための承認ポリシーのハンドラーを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068171"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET Core でのポリシー ベースの承認

実際には、下にある[ロールベースの承認](xref:security/authorization/roles)と[クレームに基づく承認](xref:security/authorization/claims)要件、要件ハンドラー、および事前構成済みポリシーを使用します。 これらのビルディング ブロックでは、コードでの評価の承認の式をサポートします。 結果より豊富な再利用可能なテスト可能な承認構造です。

承認ポリシーは、1 つまたは複数の要件で構成されます。 承認サービスの構成の一部としてに登録されている、`Startup.ConfigureServices`メソッド。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

上記の例では、"AtLeast21"ポリシーが作成されます。 1 つの要件がある&mdash;の最小経過期間が、要件にパラメーターとして指定されています。

## <a name="applying-policies-to-mvc-controllers"></a>MVC コント ローラーにポリシーを適用します。

Razor ページを使用している場合は、次を参照してください。 [Razor ページにポリシーを適用する](#applying-policies-to-razor-pages)このドキュメントで。

使用して、コント ローラーに適用されるポリシー、`[Authorize]`ポリシーの名前を持つ属性です。 例えば:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Razor ページにポリシーを適用します。

使用して Razor ページにポリシーが適用されます、`[Authorize]`ポリシーの名前を持つ属性です。 例えば:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

使用して Razor ページにポリシーが適用もできます、[承認規則](xref:security/authorization/razor-pages-authorization)します。

## <a name="requirements"></a>必要条件

承認要件を現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。 要件が 1 つのパラメーターを"AtLeast21"ポリシーで&mdash;最小経過期間。 要件を実装して[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)、空のマーカー インターフェイスであります。 パラメーター化の最小経過期間の要件は、次のように実装できます。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

承認ポリシーに複数の承認要件が含まれている場合、すべての要件は、ポリシー評価を成功させるために渡す必要があります。 つまり、複数の承認要件を 1 つの承認ポリシーに追加がで扱われます、 **AND**ごと。

> [!NOTE]
> 要件にデータまたはプロパティを持つ必要はありません。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>承認ハンドラー

承認ハンドラーは、要件のプロパティの評価を担当します。 に対して、指定された要件を評価して、承認ハンドラー [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)へのアクセスが許可されているかどうかを判断します。

要件を持つことができます[複数のハンドラー](#security-authorization-policies-based-multiple-handlers)します。 ハンドラーを継承できます[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)ここで、`TRequirement`が処理に必要です。 ハンドラーを実装または、 [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)要件の 1 つ以上の型を処理します。

### <a name="use-a-handler-for-one-requirement"></a>要件の 1 つのハンドラーを使用します。

<a name="security-authorization-handler-example"></a>

最小経過期間のハンドラーが 1 つの要求を利用して一対一のリレーションシップの例を次に示します。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上記のコードでは、現在のユーザー プリンシパル生年月日が既知または信頼された発行者によって発行される要求がある場合を決定します。 承認は、クレームが見つからない場合に発生することはできません、その場合、完了したタスクが返されます。 クレームが存在する場合は、ユーザーの年齢が計算されます。 ユーザーが要求することにより定義されている最小経過期間を満たしている場合は、承認が成功したと見なされます。 承認が成功すると、`context.Succeed`を唯一のパラメーターとして満足要件とが呼び出されます。

### <a name="use-a-handler-for-multiple-requirements"></a>複数の要件のハンドラーを使用します。

アクセス許可のハンドラーが 3 つの異なるタイプの要件を処理できる一対多リレーションシップの例を次に示します。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上記のコードを走査[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;いない要件を含むプロパティは成功としてマークします。 `ReadPermission`要件、ユーザーは、要求されたリソースにアクセスする所有者またはスポンサーにある必要があります。 場合、`EditPermission`または`DeletePermission`要件、そのユーザーが要求されたリソースにアクセスする所有者にする必要があります。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>ハンドラーの登録

ハンドラーは、構成中にサービスのコレクションに登録されます。 例:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

上記のコードを登録`MinimumAgeHandler`を呼び出すことによって、シングルトンとして`services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`します。 組み込みのいずれかを使用してハンドラーを登録できる[サービスの有効期間](xref:fundamentals/dependency-injection#service-lifetimes)します。

## <a name="what-should-a-handler-return"></a>ハンドラーを返すどのような必要があります。

なお、`Handle`メソッドで、[ハンドラーの例](#security-authorization-handler-example)値を返しません。 成功または失敗が示される状態ですか。

* ハンドラーを呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証されました。

* ハンドラーと同じ要件の他のハンドラーが成功する可能性が一般的には、エラーを処理する必要はありません。

* 場合でも、その他の要件ハンドラーで成功をエラーを確実に呼び出す`context.Fail`します。

ハンドラーを呼び出す場合`context.Succeed`または`context.Fail`、他のすべてのハンドラーが呼び出されます。 これにより、別のハンドラーが正常に検証または要件の失敗した場合でも行われるログ記録などの副作用を生成するための要件です。 設定すると`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)プロパティ (ASP.NET Core 1.1 で利用可能な以降) を実行せずにハンドラーの実行時に`context.Fail`が呼び出されます。 `InvokeHandlersAfterFailure` 既定値は`true`、すべてのハンドラーが呼び出される場合。

> [!NOTE]
> 認証が失敗した場合でも、承認ハンドラーは呼び出されます。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>複数のハンドラーを要件の対象は

評価上に存在する必要がある場合に、**または**ごと、1 つの要件の複数のハンドラーを実装します。 たとえば、Microsoft では、キーのカードでのみ開くことがドアがあります。 キー カードを自宅でままにした場合、受付は一時ステッカーを出力し、ドアが開きます。 このシナリオでは唯一の要件がある*BuildingEntry*が、複数のハンドラーがあり、それぞれ 1 つの要件を確認します。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

両方のハンドラーを[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)します。 いずれかのハンドラーは、ときに、ポリシーが成功した場合は、評価、`BuildingEntryRequirement`ポリシーの評価が成功するとします。

## <a name="using-a-func-to-fulfill-a-policy"></a>ポリシーを満たす func を使用します。

どのを満たすことで、ポリシーがコードで表現する単純な状況である可能性があります。 指定することは、`Func<AuthorizationHandlerContext, bool>`でポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。

たとえば、以前`BadgeEntryHandler`次のように書き換えることができます。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>ハンドラーで MVC 要求のコンテキストへのアクセス

`HandleRequirementAsync`承認ハンドラーで実装するメソッドが 2 つのパラメーター: `AuthorizationHandlerContext` 、`TRequirement`開梱を行う。 MVC または Jabbr などのフレームワークは、任意のオブジェクトを追加する自由、`Resource`プロパティを`AuthorizationHandlerContext`余分な情報を渡します。

たとえば、MVC のインスタンスを渡します[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)で、`Resource`プロパティ。 このプロパティにアクセスできます`HttpContext`、 `RouteData`、MVC および Razor ページによって提供される他すべてのものとします。

使用、`Resource`プロパティは、特定のフレームワークです。 情報を使用して、`Resource`プロパティは、特定のフレームワークに独自の承認ポリシーを制限します。 キャストする必要があります、`Resource`プロパティを使用して、`is`キーワード、コードがクラッシュしないことを確認して、キャストが成功したことを確認およびで、`InvalidCastException`他のフレームワークで実行した場合。

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
