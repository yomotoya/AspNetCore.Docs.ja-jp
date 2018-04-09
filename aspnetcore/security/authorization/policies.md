---
title: ASP.NET Core でのポリシー ベースの承認
author: rick-anderson
description: 作成および ASP.NET Core アプリケーションの承認要件を適用するための承認ポリシーのハンドラーを使用する方法を説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET Core でのポリシー ベースの承認

背後の[ロールベース承認](xref:security/authorization/roles)と[クレーム ベースの承認](xref:security/authorization/claims)要件、要件のハンドラー、および構成済みのポリシーを使用します。 これらのビルド ブロックでは、コードで、式の評価の承認をサポートします。 結果は、豊富な再利用可能なテスト可能な承認構造です。

承認ポリシーは、1 つまたは複数の要件で構成されます。 承認サービスの構成の一部として登録されている、`Startup.ConfigureServices`メソッド。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

前の例では、"AtLeast21"ポリシーが作成されます。 1 つの要求がある&mdash;の最小経過期間、要件にパラメーターとして指定します。

使用してポリシーが適用されます、`[Authorize]`ポリシーの名前を持つ属性。 例えば:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>要件

承認要件は、現在のユーザー プリンシパルを評価するポリシーで使用するデータのパラメーターのコレクションです。 当社"AtLeast21"ポリシーの要件が 1 つのパラメーター&mdash;最小経過期間。 要件を実装する[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)、これは、空のマーカーのインターフェイスです。 パラメーター化の最低年齢の要件は、次のように実装可能性があります。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> 要件は、データまたはプロパティを持つ必要はありません。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>認証ハンドラー

認証ハンドラーは、要件のプロパティの評価にします。 に対して、指定された要件を評価して、承認ハンドラー [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)をアクセスが許可されたかどうかを判断します。

要件が持てる[複数のハンドラー](#security-authorization-policies-based-multiple-handlers)です。 ハンドラーを継承できます[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)ここで、`TRequirement`を処理するための要件です。 ハンドラーを実装することが別の方法として、 [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)要件の 1 つ以上の型を処理します。

### <a name="use-a-handler-for-one-requirement"></a>要件の 1 つのハンドラーを使用します。

<a name="security-authorization-handler-example"></a>

最小経過期間のハンドラーが 1 つの要求を利用して、一対一リレーションシップの例を次に示します。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上記のコードは、現在のユーザー プリンシパルは、生年月日が既知で信頼できる発行者によって発行された要求の場合を判断します。 承認は、クレームが不足している場合に発生することはできません、その場合、完了したタスクが返されます。 クレームが存在する場合は、ユーザーの年齢が計算されます。 ユーザーが要求することにより定義されている最小経過期間を満たさない場合承認は成功と見なされます。 承認が正常に完了、`context.Succeed`が満たされた要件唯一のパラメーターで呼び出されます。

### <a name="use-a-handler-for-multiple-requirements"></a>複数の要件のハンドラーを使用します。

アクセス許可のハンドラーが 3 つの要件を利用して一対多リレーションシップの一例を次に示します。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上記のコードを通過する時間[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;いない要件が含まれているプロパティは成功としてマークします。 場合は、ユーザーに読み取りアクセス許可が、自分でなければなりません所有者またはスポンサー、要求されたリソースにアクセスします。 ユーザーが編集または削除アクセス許可場合、は、要求されたリソースにアクセスする所有者をする必要があります。 承認が正常に完了、`context.Succeed`が満たされた要件唯一のパラメーターで呼び出されます。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>ハンドラーの登録

ハンドラーは、構成中にサービスのコレクションに登録されます。 例えば:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

呼び出すことによって、サービスのコレクションに各ハンドラーが追加された`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`です。

## <a name="what-should-a-handler-return"></a>ハンドラーの取得と必要がありますか。

なお、`Handle`メソッドで、[ハンドラーの例](#security-authorization-handler-example)値を返しません。 成功または失敗が示されている状態ですか。

* ハンドラーは、呼び出すことで成功を示す`context.Succeed(IAuthorizationRequirement requirement)`要件を渡すことが正常に検証済みです。

* ハンドラーは、同じ要件に対する他のハンドラーが正常に終了すると、一般に、エラーを処理する必要はありません。

* 保証するための障害、他の要件のハンドラーが成功する場合でも、呼び出す`context.Fail`です。

設定すると`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)プロパティ (ASP.NET Core 1.1 で利用可能なおよびそれ以降) short-circuits ハンドラーの実行時に`context.Fail`と呼びます。 `InvokeHandlersAfterFailure` 既定値は`true`、すべてのハンドラーが呼び出される場合。 これにより、常に実行されるログ記録などの側効果を生成するための要件も`context.Fail`は別のハンドラーで呼び出されました。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>複数のハンドラーを必要条件の目的は

評価である必要がある場合に、**または**単位、1 つの要件の複数のハンドラーを実装します。 たとえば、マイクロソフトでは、キーのカードでのみ開くことがドアがあります。 キー カードを自宅に移動すると、受付は一時的なステッカーを出力し、ドアを開きます。 このシナリオでは 1 つの要件が、 *BuildingEntry*が複数のハンドラーが各いずれか 1 つの要件を確認します。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

両方のハンドラー[登録](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)です。 いずれかのハンドラーは、ときに、ポリシーが成功した場合は、評価、`BuildingEntryRequirement`ポリシーの評価が成功するとします。

## <a name="using-a-func-to-fulfill-a-policy"></a>ポリシーを満たすために、func を使用します。

どので満たすことで、ポリシーがコードで明示する単純な状況である可能性があります。 指定することは、`Func<AuthorizationHandlerContext, bool>`を使用してポリシーを構成するときに、`RequireAssertion`ポリシー ビルダー。

たとえば、前の`BadgeEntryHandler`次のように書き換えることができます。

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>ハンドラーで MVC 要求コンテキストにアクセスします。

`HandleRequirementAsync`承認ハンドラーで実装するメソッドが 2 つのパラメーター:`AuthorizationHandlerContext`と`TRequirement`を処理します。 MVC または Jabbr などのフレームワークを自由に任意のオブジェクトを追加、`Resource`プロパティを`AuthorizationHandlerContext`余分な情報を渡します。

たとえば、MVC のインスタンスを渡す[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)で、`Resource`プロパティです。 このプロパティへのアクセスを提供する`HttpContext`、 `RouteData`、MVC および Razor ページによって提供される他すべてのものとします。

使用、`Resource`プロパティは、フレームワークに固有です。 内の情報を使用して、`Resource`プロパティは特定のフレームワークを承認ポリシーを制限します。 キャストする必要があります、`Resource`プロパティを使用して、`as`キーワード、し確認コードがクラッシュしないことを確認するキャストが成功すると、`InvalidCastException`他のフレームワークで実行されたとき。

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
