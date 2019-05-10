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
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="5d5a2-103">ASP.NET Core でのクレーム ベースの承認</span><span class="sxs-lookup"><span data-stu-id="5d5a2-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="5d5a2-104">Id が作成されたときに、信頼されたパーティによって発行された 1 つまたは複数の要求が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="5d5a2-105">要求とは、どのような件名を表す名前値のペアが、どのようなサブジェクトではありませんが行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="5d5a2-106">たとえば、運転免許証、ローカルの運転ライセンス機関によって発行された場合もあります。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="5d5a2-107">生年月日、運転免許証が。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="5d5a2-108">この場合、要求の名前になります`DateOfBirth`、要求の値は、生年月日を例になります`8th June 1970`発行者を運転するライセンス権限となります。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="5d5a2-109">クレーム ベースの承認、簡単に言うでは、クレームの値をチェックし、その値に基づいてリソースへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="5d5a2-110">夜クラブ活動用に承認プロセスにアクセスする場合の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="5d5a2-111">ドア セキュリティ責任者は、生年月日要求へのアクセスを許可する前に (運転ライセンス機関) の発行者を信頼するかどうかを日付の値に評価されます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="5d5a2-112">Id は、複数の値を持つ複数の信頼性情報を含めることができ、同じ型の複数の要求を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="5d5a2-113">チェックの要求を追加します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-113">Adding claims checks</span></span>

<span data-ttu-id="5d5a2-114">要求ベースの承認チェックは、宣言型 - 開発者それらを埋め込みます、コント ローラーまたはコント ローラー内のアクションに対して、コード内で、現在のユーザーが所有する必要がありますとにアクセスするクレームの値を保持する必要があります必要に応じて信頼性情報を指定する、要求されたリソース。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="5d5a2-115">要求要件は次のポリシー ベース、開発者する必要がありますを構築およびクレーム要件を表現するポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="5d5a2-116">単純な種類の要求のポリシーは、クレームが存在し、値をチェックしません。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="5d5a2-117">最初にビルドし、ポリシーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-117">First you need to build and register the policy.</span></span> <span data-ttu-id="5d5a2-118">これは、処理が正常に含まれる承認サービスの構成の一部として行わ`ConfigureServices()`で、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="5d5a2-119">この場合、`EmployeeOnly`ポリシーの存在を確認します、`EmployeeNumber`の現在の id を要求します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="5d5a2-120">ポリシーを使用して、適用する、`Policy`プロパティを`AuthorizeAttribute`; ポリシー名を指定する属性</span><span class="sxs-lookup"><span data-stu-id="5d5a2-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="5d5a2-121">`AuthorizeAttribute`コント ローラーで、ポリシーに一致するユーザーが任意のアクションへのアクセスを許可するだけでは、このインスタンスで、コント ローラー全体に属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="5d5a2-122">によって保護されているコント ローラーがある場合、`AuthorizeAttribute`属性しますが、適用する特定のアクションへの匿名アクセスを許可する、`AllowAnonymousAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="5d5a2-123">ほとんどの要求値が付属します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-123">Most claims come with a value.</span></span> <span data-ttu-id="5d5a2-124">ポリシーを作成するときに使用できる値の一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="5d5a2-125">次の例はのみ、ある従業員数が 1、2、3、4 または 5 従業員に対して成功します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="5d5a2-126">汎用の要求チェックを追加します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-126">Add a generic claim check</span></span>

<span data-ttu-id="5d5a2-127">要求の値が 1 つの値はありません、変換が必要です。 または、を使用して、 [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion)します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="5d5a2-128">詳細については、次を参照してください。 [func を使用してポリシーを満たすために](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy)します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="5d5a2-129">複数のポリシーの評価</span><span class="sxs-lookup"><span data-stu-id="5d5a2-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="5d5a2-130">コント ローラーまたはアクションに複数のポリシーを適用する場合は、アクセスを許可する前に、すべてのポリシーは渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="5d5a2-131">例:</span><span class="sxs-lookup"><span data-stu-id="5d5a2-131">For example:</span></span>

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

<span data-ttu-id="5d5a2-132">上記の例では満たされています、id、`EmployeeOnly`ポリシーがアクセスできる、`Payslip`コント ローラーにそのポリシーの処理が適用されて。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="5d5a2-133">ただしを呼び出すために、 `UpdateSalary` id が満たす必要がありますアクション*両方*、`EmployeeOnly`ポリシーと`HumanResources`ポリシー。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="5d5a2-134">複雑なポリシーを設定する場合などの生年月日要求の日付を取得するには、そこから年齢を計算し、年齢を確認 21 歳以上を記述する必要がある[カスタム ポリシー ハンドラー](xref:security/authorization/policies)します。</span><span class="sxs-lookup"><span data-stu-id="5d5a2-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
