---
title: ASP.NET Core でのクレーム ベースの承認
author: rick-anderson
description: ASP.NET Core アプリで要求承認チェックを追加する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086150"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>ASP.NET Core でのクレーム ベースの承認

<a name="security-authorization-claims-based"></a>

Id が作成されたときに、信頼されたパーティによって発行された 1 つまたは複数の要求が割り当てられます。 要求とは、どのような件名を表す名前値のペアが、どのようなサブジェクトではありませんが行うことができます。 たとえば、運転免許証、ローカルの運転ライセンス機関によって発行された場合もあります。 生年月日、運転免許証が。 この場合、要求の名前になります`DateOfBirth`、要求の値は、生年月日を例になります`8th June 1970`発行者を運転するライセンス権限となります。 クレーム ベースの承認、簡単に言うでは、クレームの値をチェックし、その値に基づいてリソースへのアクセスを許可します。 夜クラブ活動用に承認プロセスにアクセスする場合の例は次のようになります。

ドア セキュリティ責任者は、生年月日要求へのアクセスを許可する前に (運転ライセンス機関) の発行者を信頼するかどうかを日付の値に評価されます。

Id は、複数の値を持つ複数の信頼性情報を含めることができ、同じ型の複数の要求を含めることができます。

## <a name="adding-claims-checks"></a>チェックの要求を追加します。

要求ベースの承認チェックは、宣言型 - 開発者それらを埋め込みます、コント ローラーまたはコント ローラー内のアクションに対して、コード内で、現在のユーザーが所有する必要がありますとにアクセスするクレームの値を保持する必要があります必要に応じて信頼性情報を指定する、要求されたリソース。 要求要件は次のポリシー ベース、開発者する必要がありますを構築およびクレーム要件を表現するポリシーを登録します。

単純な種類の要求のポリシーは、クレームが存在し、値をチェックしません。

最初にビルドし、ポリシーを登録する必要があります。 これは、処理が正常に含まれる承認サービスの構成の一部として行わ`ConfigureServices()`で、 *Startup.cs*ファイル。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

この場合、`EmployeeOnly`ポリシーの存在を確認します、`EmployeeNumber`の現在の id を要求します。

ポリシーを使用して、適用する、`Policy`プロパティを`AuthorizeAttribute`; ポリシー名を指定する属性

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`コント ローラーで、ポリシーに一致するユーザーが任意のアクションへのアクセスを許可するだけでは、このインスタンスで、コント ローラー全体に属性を適用できます。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

によって保護されているコント ローラーがある場合、`AuthorizeAttribute`属性しますが、適用する特定のアクションへの匿名アクセスを許可する、`AllowAnonymousAttribute`属性。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

ほとんどの要求値が付属します。 ポリシーを作成するときに使用できる値の一覧を指定できます。 次の例はのみ、ある従業員数が 1、2、3、4 または 5 従業員に対して成功します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a>汎用の要求チェックを追加します。

要求の値が 1 つの値はありません、変換が必要です。 または、を使用して、 [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)します。 詳細については、次を参照してください。 [func を使用してポリシーを満たすために](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)します。

## <a name="multiple-policy-evaluation"></a>複数のポリシーの評価

コント ローラーまたはアクションに複数のポリシーを適用する場合は、アクセスを許可する前に、すべてのポリシーは渡す必要があります。 例:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

上記の例では満たされています、id、`EmployeeOnly`ポリシーがアクセスできる、`Payslip`コント ローラーにそのポリシーの処理が適用されて。 ただしを呼び出すために、 `UpdateSalary` id が満たす必要がありますアクション*両方*、`EmployeeOnly`ポリシーと`HumanResources`ポリシー。

複雑なポリシーを設定する場合などの生年月日要求の日付を取得するには、そこから年齢を計算し、年齢を確認 21 歳以上を記述する必要がある[カスタム ポリシー ハンドラー](xref:security/authorization/policies)します。
