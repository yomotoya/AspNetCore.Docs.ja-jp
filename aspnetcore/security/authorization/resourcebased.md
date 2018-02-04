---
title: "ASP.NET Core でのリソース ベースの承認"
author: scottaddie
description: "Authorize attribute で十分しない場合に、ASP.NET Core アプリケーションのリソース ベースの承認を実装する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a><span data-ttu-id="f72e8-103">リソース ベースの承認</span><span class="sxs-lookup"><span data-stu-id="f72e8-103">Resource-based authorization</span></span>

<span data-ttu-id="f72e8-104">承認方法は、アクセス対象のリソースに依存します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="f72e8-105">Author プロパティを持つドキュメントを検討してください。</span><span class="sxs-lookup"><span data-stu-id="f72e8-105">Consider a document which has an author property.</span></span> <span data-ttu-id="f72e8-106">ドキュメントを更新するには、作成者のみが許可されます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="f72e8-107">その結果、承認の評価を行う前に、データ ストアから、ドキュメントを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f72e8-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="f72e8-108">属性の評価は、データ バインディングの前に、ページのハンドラーまたはドキュメントに読み込みアクションの実行前に発生します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="f72e8-109">これらの理由、による宣言型の承認から、`[Authorize]`属性が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="f72e8-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="f72e8-110">代わりに、カスタム承認メソッドを呼び出すことができます&mdash;命令型の承認と呼ばれるスタイル。</span><span class="sxs-lookup"><span data-stu-id="f72e8-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="f72e8-111">使用して、[アプリのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) をこのトピックで説明する機能を探索します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="f72e8-112">[認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成する](xref:security/authorization/secure-data)リソース ベースの承認を使用するサンプル アプリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f72e8-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="f72e8-113">命令型の承認を使用します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-113">Use imperative authorization</span></span>

<span data-ttu-id="f72e8-114">承認として実装、 [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)サービスおよびサービス内にコレクションに登録されて、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="f72e8-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="f72e8-115">サービスを使用可能で[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)ページ ハンドラーやアクションにします。</span><span class="sxs-lookup"><span data-stu-id="f72e8-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="f72e8-116">`IAuthorizationService`2 つの`AuthorizeAsync`メソッドのオーバー ロード: リソースと、ポリシー名およびその他のリソースおよび評価するための要件の一覧を受け入れるを 1 つ受け入れます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72e8-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72e8-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="f72e8-119">次の例では、セキュリティで保護するリソースに追加されるカスタム`Document`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f72e8-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="f72e8-120">`AuthorizeAsync`現在のユーザーが指定されたドキュメントを編集できるかどうかを決定するオーバー ロードが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="f72e8-121">カスタム"EditPolicy"承認ポリシーでは、意思決定を考慮します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="f72e8-122">参照してください[カスタム ポリシー ベースの承認](xref:security/authorization/policies)承認ポリシーを作成する方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="f72e8-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="f72e8-123">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="f72e8-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72e8-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72e8-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="f72e8-126">リソース ベースのハンドラーを記述します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-126">Write a resource-based handler</span></span>

<span data-ttu-id="f72e8-127">リソース ベースの承認されないとは大きく異なるため、ハンドラーの記述[plain 要件ハンドラーの記述](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)です。</span><span class="sxs-lookup"><span data-stu-id="f72e8-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="f72e8-128">カスタム要求のクラスを作成し、要件ハンドラー クラスを実装します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="f72e8-129">ハンドラー クラスには、要件およびリソースの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="f72e8-130">たとえば、ハンドラーを使用して、`SameAuthorRequirement`要件と`Document`リソースは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f72e8-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72e8-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72e8-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="f72e8-133">要件とのハンドラーを登録、`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f72e8-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="f72e8-134">運用上の要件</span><span class="sxs-lookup"><span data-stu-id="f72e8-134">Operational requirements</span></span>

<span data-ttu-id="f72e8-135">CRUD の結果に基づく決定を作るかどうか (**C**reate、 **R**いてぇ、 **U**更新する、 **D**削除 ())、操作を使用、 [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)ヘルパー クラス。</span><span class="sxs-lookup"><span data-stu-id="f72e8-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="f72e8-136">このクラスでは、操作の種類ごとの個別のクラスではなく 1 つのハンドラーを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="f72e8-137">これを使用するには、いくつかの操作名を提供します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="f72e8-138">ハンドラーを実装する次のように、`OperationAuthorizationRequirement`要件と`Document`リソース。</span><span class="sxs-lookup"><span data-stu-id="f72e8-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72e8-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72e8-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="f72e8-141">前のハンドラーは、リソース、ユーザーの id、および要件を使用して、操作を検証`Name`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="f72e8-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="f72e8-142">運用上のリソース ハンドラーを呼び出すには、操作を呼び出すときに指定`AuthorizeAsync`ページ ハンドラーまたはアクションにします。</span><span class="sxs-lookup"><span data-stu-id="f72e8-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="f72e8-143">次の例では、指定されたドキュメントを表示する、認証されたユーザーが許可されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="f72e8-144">次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="f72e8-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72e8-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="f72e8-146">承認に成功した場合、ドキュメントの表示 ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="f72e8-147">かどうかの承認が失敗したが、ユーザーが認証されると、返す`ForbidResult`承認に失敗したすべての認証ミドルウェアに通知します。</span><span class="sxs-lookup"><span data-stu-id="f72e8-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="f72e8-148">A`ChallengeResult`認証を実行する必要がありますが返されます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="f72e8-149">対話型ブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトする適切な場合があります。</span><span class="sxs-lookup"><span data-stu-id="f72e8-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72e8-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72e8-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="f72e8-151">承認に成功した場合、ドキュメントのビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="f72e8-152">承認に失敗した場合に返す`ChallengeResult`すべての認証ミドルウェアを通知承認に失敗しましたとミドルウェアは、適切な応答を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="f72e8-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="f72e8-153">適切な応答には、401 または 403 ステータス コードを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f72e8-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="f72e8-154">対話型ブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトする、可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f72e8-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
