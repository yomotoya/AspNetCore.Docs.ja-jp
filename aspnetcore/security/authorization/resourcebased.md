---
title: "リソース ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 7f7df52bf51a81558818836450997281a21b5839
ms.sourcegitcommit: f303a457644ed034a49aa89edecb4e79d9028cb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="f505b-103">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="f505b-103">Resource Based Authorization</span></span>

<a name=security-authorization-resource-based></a>

<span data-ttu-id="f505b-104">多くの場合、承認は、アクセス対象のリソースに依存します。</span><span class="sxs-lookup"><span data-stu-id="f505b-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="f505b-105">たとえば、ドキュメントには、author プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="f505b-105">For example, a document may have an author property.</span></span> <span data-ttu-id="f505b-106">ドキュメントの作成者だけには許可されたリソースは、承認の評価を行う前に、ドキュメント リポジトリから読み込む必要があるため、更新します。</span><span class="sxs-lookup"><span data-stu-id="f505b-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="f505b-107">属性の評価はアクション内で、独自コード リソースの読み込みを実行する前に、データ バインディングの前に、承認属性を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="f505b-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="f505b-108">宣言型の承認、属性の方法ではなくを使用しなければならない命令型の承認では、開発者が独自のコード内で承認関数を呼び出す場所。</span><span class="sxs-lookup"><span data-stu-id="f505b-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="f505b-109">コード内で承認します。</span><span class="sxs-lookup"><span data-stu-id="f505b-109">Authorizing within your code</span></span>

<span data-ttu-id="f505b-110">承認は、サービスとして実装`IAuthorizationService`サービス コレクションに登録されている、および経由で入手できます[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)コント ローラーにアクセスするためです。</span><span class="sxs-lookup"><span data-stu-id="f505b-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="f505b-111">`IAuthorizationService`いずれかの場所を渡すリソースと、ポリシー名およびその他のリソースおよび評価するための要件の一覧を渡す 2 つのメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="f505b-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

<span data-ttu-id="f505b-112">サービスを呼び出すには、読み込む、アクション内でリソースを呼び出す、`AuthorizeAsync`オーバー ロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f505b-112">To call the service, load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="f505b-113">例:</span><span class="sxs-lookup"><span data-stu-id="f505b-113">For example:</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="f505b-114">リソース ベースのハンドラーの記述</span><span class="sxs-lookup"><span data-stu-id="f505b-114">Writing a resource based handler</span></span>

<span data-ttu-id="f505b-115">リソース ベースの承認のハンドラーの記述に大きな違いはなく[plain 要件ハンドラーの記述](policies.md#security-authorization-policies-based-authorization-handler)です。</span><span class="sxs-lookup"><span data-stu-id="f505b-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="f505b-116">要件を作成して前に、要件とも、リソースの種類を指定する、要件のハンドラーを実装します。</span><span class="sxs-lookup"><span data-stu-id="f505b-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="f505b-117">たとえば、ドキュメント リソースを受け入れることがハンドラーは示しています。</span><span class="sxs-lookup"><span data-stu-id="f505b-117">For example, a handler which might accept a Document resource would look as follows:</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="f505b-118">忘れずに、ハンドラーを登録する必要があります、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f505b-118">Don't forget you also need to register your handler in the `ConfigureServices` method:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="f505b-119">運用上の要件</span><span class="sxs-lookup"><span data-stu-id="f505b-119">Operational Requirements</span></span>

<span data-ttu-id="f505b-120">読み取り、書き込み、更新、削除などの操作に基づく決定を行う場合を使って、`OperationAuthorizationRequirement`クラス内で、`Microsoft.AspNetCore.Authorization.Infrastructure`名前空間。</span><span class="sxs-lookup"><span data-stu-id="f505b-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="f505b-121">この要件をあらかじめ作成されているクラスを使用すると、操作ごとに個別のクラスを作成する代わりに、パラメーター化された操作名を持つ 1 つのハンドラーを記述できます。</span><span class="sxs-lookup"><span data-stu-id="f505b-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="f505b-122">これを使用するには、いくつかの操作名を提供します。</span><span class="sxs-lookup"><span data-stu-id="f505b-122">To use it, provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="f505b-123">ハンドラーでしたして実装する次のように、仮定を使用して`Document`クラス リソースとして。</span><span class="sxs-lookup"><span data-stu-id="f505b-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource:</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="f505b-124">ハンドラーが動作を確認できます`OperationAuthorizationRequirement`です。</span><span class="sxs-lookup"><span data-stu-id="f505b-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="f505b-125">ハンドラー内のコードは、その評価を行うときに、アカウントに指定された要件の Name プロパティを受け取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="f505b-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="f505b-126">呼び出すときに、操作を指定する必要があります。 運用リソース ハンドラーを呼び出して`AuthorizeAsync`アクションにします。</span><span class="sxs-lookup"><span data-stu-id="f505b-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="f505b-127">例:</span><span class="sxs-lookup"><span data-stu-id="f505b-127">For example:</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="f505b-128">この例では、ユーザーが現在の読み取り操作を実行できないかどうか`document`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f505b-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="f505b-129">承認が成功した場合は、ドキュメントのビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="f505b-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="f505b-130">返す承認に失敗したかどうかは`ChallengeResult`ミドルウェアの認証が失敗したため、ミドルウェアは 401 または 403 ステータス コードを返すなどのログイン ページにユーザーをリダイレクトする、適切な応答を受け取ることができます、任意の認証を通知します。対話型ブラウザー クライアント。</span><span class="sxs-lookup"><span data-stu-id="f505b-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
