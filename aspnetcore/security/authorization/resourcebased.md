---
title: ASP.NET Core でリソース ベースの承認
author: scottaddie
description: Authorize 属性が十分ではない場合に、ASP.NET Core アプリでリソース ベースの承認を実装する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342290"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="459fa-103">ASP.NET Core でリソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="459fa-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="459fa-104">承認方法は、アクセス対象のリソースに依存します。</span><span class="sxs-lookup"><span data-stu-id="459fa-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="459fa-105">作成者のプロパティを持つドキュメントを検討してください。</span><span class="sxs-lookup"><span data-stu-id="459fa-105">Consider a document which has an author property.</span></span> <span data-ttu-id="459fa-106">ドキュメントを更新するには、作成者のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="459fa-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="459fa-107">その結果、承認の評価を行う前に、ドキュメントをデータ ストアから取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="459fa-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="459fa-108">属性の評価では、データ バインディングの前に、ページ ハンドラーまたはドキュメントが読み込まれますアクションの実行前に発生します。</span><span class="sxs-lookup"><span data-stu-id="459fa-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="459fa-109">これらの理由、宣言型を使用した承認からを`[Authorize]`属性が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="459fa-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="459fa-110">代わりに、カスタム承認メソッドを呼び出すことができます&mdash;命令型の承認と呼ばれるスタイル。</span><span class="sxs-lookup"><span data-stu-id="459fa-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="459fa-111">使用して、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) 機能をこのトピックで説明します。</span><span class="sxs-lookup"><span data-stu-id="459fa-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="459fa-112">[承認によって保護されたユーザー データと ASP.NET Core アプリを作成する](xref:security/authorization/secure-data)リソース ベースの承認を使用するサンプル アプリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="459fa-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="459fa-113">命令型の承認を使用します。</span><span class="sxs-lookup"><span data-stu-id="459fa-113">Use imperative authorization</span></span>

<span data-ttu-id="459fa-114">承認の実装として、 [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)サービスし、は、サービスのコレクション内に登録されて、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="459fa-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="459fa-115">サービスを使用して利用可能になって[依存関係の注入](xref:fundamentals/dependency-injection)ページ ハンドラーやアクション。</span><span class="sxs-lookup"><span data-stu-id="459fa-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="459fa-116">`IAuthorizationService` 2 つの`AuthorizeAsync`メソッドのオーバー ロード: リソースと、ポリシー名とその他のリソースと、評価するための要件の一覧を受け入れる 1 つを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="459fa-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="459fa-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="459fa-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="459fa-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="459fa-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="459fa-119">次の例では、セキュリティで保護するリソースがカスタムに読み込まれる`Document`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="459fa-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="459fa-120">`AuthorizeAsync`現在のユーザーが指定されたドキュメントを編集できるかどうかを判断するオーバー ロードが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="459fa-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="459fa-121">カスタムの"EditPolicy"承認ポリシーは、意思決定にファクタリングされます。</span><span class="sxs-lookup"><span data-stu-id="459fa-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="459fa-122">参照してください[カスタム ポリシー ベースの承認](xref:security/authorization/policies)承認ポリシーの作成に関する詳細について。</span><span class="sxs-lookup"><span data-stu-id="459fa-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="459fa-123">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="459fa-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="459fa-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="459fa-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="459fa-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="459fa-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="459fa-126">リソース ベースのハンドラーを記述します。</span><span class="sxs-lookup"><span data-stu-id="459fa-126">Write a resource-based handler</span></span>

<span data-ttu-id="459fa-127">リソース ベースの承認がかなり異なるハンドラーの記述[プレーンな要件ハンドラーの記述](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)します。</span><span class="sxs-lookup"><span data-stu-id="459fa-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="459fa-128">カスタム要件クラスを作成し、要件のハンドラー クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="459fa-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="459fa-129">ハンドラー クラスには、要件とリソースの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="459fa-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="459fa-130">など、ハンドラー利用して、`SameAuthorRequirement`要件と`Document`リソースを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="459fa-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="459fa-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="459fa-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="459fa-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="459fa-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="459fa-133">要件とハンドラーの登録、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="459fa-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="459fa-134">運用上の要件</span><span class="sxs-lookup"><span data-stu-id="459fa-134">Operational requirements</span></span>

<span data-ttu-id="459fa-135">CRUD (作成、読み取り、更新、削除) 操作の結果に基づいた決定を作成する場合は、使用、 [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)ヘルパー クラス。</span><span class="sxs-lookup"><span data-stu-id="459fa-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="459fa-136">このクラスでは、操作の種類ごとの個々 のクラスではなく、単一のハンドラーを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="459fa-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="459fa-137">これを使用するには、いくつかの操作名を指定します。</span><span class="sxs-lookup"><span data-stu-id="459fa-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="459fa-138">ハンドラーが、次のように実装されるを使用して、`OperationAuthorizationRequirement`要件と`Document`リソース。</span><span class="sxs-lookup"><span data-stu-id="459fa-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="459fa-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="459fa-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="459fa-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="459fa-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="459fa-141">前のハンドラーは、リソース、ユーザーの id、および要件を使用して、操作を検証します。`Name`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="459fa-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="459fa-142">運用上のリソースのハンドラーを呼び出すには、操作を呼び出すときに指定`AuthorizeAsync`ページ ハンドラーまたはアクションにします。</span><span class="sxs-lookup"><span data-stu-id="459fa-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="459fa-143">次の例では、指定されたドキュメントを表示する、認証されたユーザーが許可されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="459fa-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="459fa-144">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="459fa-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="459fa-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="459fa-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="459fa-146">承認が成功すると、ドキュメントを表示するためのページが返されます。</span><span class="sxs-lookup"><span data-stu-id="459fa-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="459fa-147">かどうかの承認が失敗したが、ユーザーが認証されると、返す`ForbidResult`承認に失敗したすべての認証ミドルウェアに通知します。</span><span class="sxs-lookup"><span data-stu-id="459fa-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="459fa-148">A`ChallengeResult`認証を実行する必要がありますが返されます。</span><span class="sxs-lookup"><span data-stu-id="459fa-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="459fa-149">対話型のブラウザー クライアントの場合は、ユーザーをログイン ページにリダイレクトする適切な場合があります。</span><span class="sxs-lookup"><span data-stu-id="459fa-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="459fa-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="459fa-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="459fa-151">承認が成功すると、ドキュメントのビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="459fa-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="459fa-152">承認に失敗した場合に返す`ChallengeResult`承認に失敗しました、ミドルウェアは、適切な応答にかかるすべての認証ミドルウェアに通知します。</span><span class="sxs-lookup"><span data-stu-id="459fa-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="459fa-153">適切な応答には、401 または 403 状態コードを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="459fa-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="459fa-154">対話型のブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトすることも考えられます。</span><span class="sxs-lookup"><span data-stu-id="459fa-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
