---
title: ASP.NET Core でリソース ベースの承認
author: scottaddie
description: Authorize 属性が十分ではない場合に、ASP.NET Core アプリでリソース ベースの承認を実装する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: afc152ea677cab42d57bd642b4821159f125117e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891739"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="44a7f-103">ASP.NET Core でリソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="44a7f-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="44a7f-104">承認方法は、アクセス対象のリソースに依存します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="44a7f-105">作成者のプロパティを持つドキュメントを検討してください。</span><span class="sxs-lookup"><span data-stu-id="44a7f-105">Consider a document that has an author property.</span></span> <span data-ttu-id="44a7f-106">ドキュメントを更新するには、作成者のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="44a7f-107">その結果、承認の評価を行う前に、ドキュメントをデータ ストアから取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="44a7f-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="44a7f-108">属性の評価では、データ バインディングの前に、ページ ハンドラーまたはドキュメントを読み込みますアクションの実行前に発生します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="44a7f-109">これらの理由、宣言型を使用した承認からを`[Authorize]`属性が十分でないです。</span><span class="sxs-lookup"><span data-stu-id="44a7f-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="44a7f-110">代わりに、カスタム承認メソッドを呼び出すことができます&mdash;と呼ばれるスタイル*命令型の承認*します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="44a7f-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="44a7f-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="44a7f-112">[承認によって保護されたユーザー データと ASP.NET Core アプリを作成する](xref:security/authorization/secure-data)リソース ベースの承認を使用するサンプル アプリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="44a7f-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="44a7f-113">命令型の承認を使用します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-113">Use imperative authorization</span></span>

<span data-ttu-id="44a7f-114">承認の実装として、 [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)サービスし、は、サービスのコレクション内に登録されて、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="44a7f-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="44a7f-115">サービスを使用して利用可能になって[依存関係の注入](xref:fundamentals/dependency-injection)ページ ハンドラーやアクション。</span><span class="sxs-lookup"><span data-stu-id="44a7f-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="44a7f-116">`IAuthorizationService` 2 つの`AuthorizeAsync`メソッドのオーバー ロード: リソースと、ポリシー名とその他のリソースと、評価するための要件の一覧を受け入れる 1 つを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="44a7f-117">次の例では、セキュリティで保護するリソースがカスタムに読み込まれる`Document`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="44a7f-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="44a7f-118">`AuthorizeAsync`現在のユーザーが指定されたドキュメントを編集できるかどうかを判断するオーバー ロードが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="44a7f-119">カスタムの"EditPolicy"承認ポリシーは、意思決定にファクタリングされます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="44a7f-120">参照してください[カスタム ポリシー ベースの承認](xref:security/authorization/policies)承認ポリシーの作成に関する詳細について。</span><span class="sxs-lookup"><span data-stu-id="44a7f-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="44a7f-121">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="44a7f-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="44a7f-122">リソース ベースのハンドラーを記述します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-122">Write a resource-based handler</span></span>

<span data-ttu-id="44a7f-123">リソース ベースの承認がかなり異なるハンドラーの記述[プレーンな要件ハンドラーの記述](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="44a7f-124">カスタム要件クラスを作成し、要件のハンドラー クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="44a7f-125">要件のクラスを作成する方法の詳細については、次を参照してください。[要件](xref:security/authorization/policies#requirements)します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="44a7f-126">ハンドラー クラスには、要件とリソースの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="44a7f-127">など、ハンドラー利用して、`SameAuthorRequirement`と`Document`リソース次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="44a7f-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="44a7f-128">前の例では、imagine を`SameAuthorRequirement`特殊なケースより汎用的なは、`SpecificAuthorRequirement`クラス。</span><span class="sxs-lookup"><span data-stu-id="44a7f-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="44a7f-129">`SpecificAuthorRequirement` (非表示) クラスが含まれています、`Name`作成者の名前を表すプロパティ。</span><span class="sxs-lookup"><span data-stu-id="44a7f-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="44a7f-130">`Name`プロパティは、現在のユーザーに設定する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="44a7f-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="44a7f-131">要件とハンドラーを登録`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="44a7f-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="44a7f-132">運用上の要件</span><span class="sxs-lookup"><span data-stu-id="44a7f-132">Operational requirements</span></span>

<span data-ttu-id="44a7f-133">CRUD (作成、読み取り、更新、削除) 操作の結果に基づいた決定を作成する場合は、使用、 [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)ヘルパー クラス。</span><span class="sxs-lookup"><span data-stu-id="44a7f-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="44a7f-134">このクラスでは、操作の種類ごとの個々 のクラスではなく、単一のハンドラーを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="44a7f-135">これを使用するには、いくつかの操作名を指定します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="44a7f-136">ハンドラーが、次のように実装されるを使用して、`OperationAuthorizationRequirement`要件と`Document`リソース。</span><span class="sxs-lookup"><span data-stu-id="44a7f-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="44a7f-137">前のハンドラーは、リソース、ユーザーの id、および要件を使用して、操作を検証します。`Name`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="44a7f-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="44a7f-138">運用上のリソースのハンドラーを呼び出すには、操作を呼び出すときに指定`AuthorizeAsync`ページ ハンドラーまたはアクションにします。</span><span class="sxs-lookup"><span data-stu-id="44a7f-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="44a7f-139">次の例では、指定されたドキュメントを表示する、認証されたユーザーが許可されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="44a7f-140">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="44a7f-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="44a7f-141">承認が成功すると、ドキュメントを表示するためのページが返されます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="44a7f-142">かどうかの承認が失敗したが、ユーザーが認証されると、返す`ForbidResult`承認に失敗したすべての認証ミドルウェアに通知します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="44a7f-143">A`ChallengeResult`認証を実行する必要がありますが返されます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="44a7f-144">対話型のブラウザー クライアントの場合は、ユーザーをログイン ページにリダイレクトする適切な場合があります。</span><span class="sxs-lookup"><span data-stu-id="44a7f-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="44a7f-145">承認が成功すると、ドキュメントのビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="44a7f-146">承認に失敗した場合に返す`ChallengeResult`承認に失敗しました、ミドルウェアは、適切な応答にかかるすべての認証ミドルウェアに通知します。</span><span class="sxs-lookup"><span data-stu-id="44a7f-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="44a7f-147">適切な応答には、401 または 403 状態コードを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="44a7f-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="44a7f-148">対話型のブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトすることも考えられます。</span><span class="sxs-lookup"><span data-stu-id="44a7f-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
