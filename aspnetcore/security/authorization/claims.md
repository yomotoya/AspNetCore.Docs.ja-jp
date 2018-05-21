---
title: ASP.NET Core でクレーム ベースの承認
author: rick-anderson
description: ASP.NET Core アプリケーションの承認の要求の確認を追加する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2018
---
# <a name="claims-based-authorization-in-aspnet-core"></a>ASP.NET Core でクレーム ベースの承認

<a name="security-authorization-claims-based"></a>

Id を作成、割り当てることができます、信頼された相手によって発行された 1 つまたは複数のクレーム。 クレームはどのような件名を表す名前値のペアが、どのようなサブジェクトではなく操作を実行できます。 たとえば、ローカルの推進ライセンス機関によって発行された、運転免許証があります。 免許証生年月日になっています。 ここでは、要求の名前になります`DateOfBirth`、要求の値になります、生年月日たとえば`8th June 1970`発行者を推進するライセンス権限となります。 クレーム ベースの承認、簡単に言うは、要求の値をチェックし、その値に基づいてリソースへのアクセスを許可します。 夜間クラブ承認プロセスにアクセスする場合の例可能性があります。

ドア セキュリティ責任者は、生年月日要求とにアクセスを許可する前に、発行者 (駆動ライセンス機関) を信頼するかどうかの日付の値に評価されます。

Id は、複数の値を持つ複数の信頼性情報を含めることができ、同じ種類の複数の要求を含めることができます。

## <a name="adding-claims-checks"></a>チェックの要求を追加します。

要求ベースの承認チェックが宣言型 - 開発者は、そのファイルを埋め込みますコント ローラーや、コント ローラー内のアクションに対して、コード内で、現在のユーザーが所有する必要があります、および必要に応じて、クレームの値がアクセスするを保持する信頼性情報を指定する、要求されたリソース。 要件は、ポリシー ベースの信頼性情報、開発者は、する必要がありますビルドして、クレーム要件を表現するポリシーを登録します。

最も単純な種類のクレームが存在する可能性がポリシーを要求しない値を確認します。

最初のビルドし、ポリシーを登録する必要があります。 通常は、一部を承認サービスの構成の一部として行われるこの`ConfigureServices()`で、 *Startup.cs*ファイル。

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

ここでは、`EmployeeOnly`ポリシーの存在を確認、`EmployeeNumber`現在の id を要求します。

ポリシーを使用して、適用する、`Policy`プロパティを`AuthorizeAttribute`ポリシーの名前を指定する属性

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute`コント ローラーのポリシーに一致するユーザーが任意のアクションへのアクセスを許可するだけでは、このインスタンスで、全体のコント ローラーに属性を適用できます。

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

によって保護されているコント ローラーがある場合、`AuthorizeAttribute`属性しますが、適用する特定の操作への匿名アクセスを許可する、`AllowAnonymousAttribute`属性。

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

ほとんどの要求は、値が付属します。 ポリシーを作成するときに、有効な値の一覧を指定できます。 次の例のみに成功の従業員の従業員数が 1、2、3、4 または 5。

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

### <a name="add-a-generic-claim-check"></a>汎用の要求のチェックを追加します。

要求の値 1 つの値ではない、または変換が必要を使用して[RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)です。 詳細については、次を参照してください。 [func を使用してポリシーを満たすために](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)です。

## <a name="multiple-policy-evaluation"></a>複数のポリシーの評価

コント ローラーまたはアクションに複数のポリシーを適用する場合は、アクセスが許可される前に、すべてのポリシーは渡す必要があります。 例えば:

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

上記の例では満たされている id、`EmployeeOnly`ポリシーがアクセスできる、`Payslip`そのポリシーの処理は、コント ローラーに適用されます。 ただし呼び出すために、 `UpdateSalary` id が満たす必要がありますアクション*両方*、`EmployeeOnly`ポリシーおよび`HumanResources`ポリシー。

などの生年月日要求の日付を取得するには、そこから年齢を計算する場合、経過期間のチェックは 21 以上経過し、記述する必要がより複雑なポリシーを実行する場合に、[カスタム ポリシー ハンドラー](xref:security/authorization/policies)です。
