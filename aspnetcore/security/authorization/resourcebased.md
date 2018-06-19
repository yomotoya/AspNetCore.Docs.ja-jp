---
title: ASP.NET Core でのリソース ベースの承認
author: scottaddie
description: Authorize attribute で十分しない場合に、ASP.NET Core アプリケーションのリソース ベースの承認を実装する方法を説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094283"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET Core でのリソース ベースの承認

承認方法は、アクセス対象のリソースに依存します。 Author プロパティを持つドキュメントを検討してください。 ドキュメントを更新するには、作成者のみが許可されます。 その結果、承認の評価を行う前に、データ ストアから、ドキュメントを取得する必要があります。

属性の評価は、データ バインディングの前に、ページのハンドラーまたはドキュメントに読み込みアクションの実行前に発生します。 これらの理由、による宣言型の承認から、`[Authorize]`属性が十分ではありません。 代わりに、カスタム承認メソッドを呼び出すことができます&mdash;命令型の承認と呼ばれるスタイル。

使用して、[アプリのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) をこのトピックで説明する機能を探索します。

[認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成する](xref:security/authorization/secure-data)リソース ベースの承認を使用するサンプル アプリが含まれています。

## <a name="use-imperative-authorization"></a>命令型の承認を使用します。

承認として実装、 [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)サービスおよびサービス内にコレクションに登録されて、`Startup`クラスです。 サービスを使用可能で[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)ページ ハンドラーやアクションにします。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` 2 つの`AuthorizeAsync`メソッドのオーバー ロード: リソースと、ポリシー名およびその他のリソースおよび評価するための要件の一覧を受け入れるを 1 つ受け入れます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

次の例では、セキュリティで保護するリソースに追加されるカスタム`Document`オブジェクト。 `AuthorizeAsync`現在のユーザーが指定されたドキュメントを編集できるかどうかを決定するオーバー ロードが呼び出されます。 カスタム"EditPolicy"承認ポリシーでは、意思決定を考慮します。 参照してください[カスタム ポリシー ベースの承認](xref:security/authorization/policies)承認ポリシーを作成する方法の詳細。

> [!NOTE]
> 次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>リソース ベースのハンドラーを記述します。

リソース ベースの承認されないとは大きく異なるため、ハンドラーの記述[plain 要件ハンドラーの記述](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)です。 カスタム要求のクラスを作成し、要件ハンドラー クラスを実装します。 ハンドラー クラスには、要件およびリソースの種類を指定します。 たとえば、ハンドラーを使用して、`SameAuthorRequirement`要件と`Document`リソースは次のようになります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

要件とのハンドラーを登録、`Startup.ConfigureServices`メソッド。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>運用上の要件

CRUD (作成、読み取り、更新、削除) 操作の結果を基に決定している場合を使用して、 [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)ヘルパー クラス。 このクラスでは、操作の種類ごとの個別のクラスではなく 1 つのハンドラーを記述することができます。 これを使用するには、いくつかの操作名を提供します。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

ハンドラーを実装する次のように、`OperationAuthorizationRequirement`要件と`Document`リソース。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

前のハンドラーは、リソース、ユーザーの id、および要件を使用して、操作を検証`Name`プロパティです。

運用上のリソース ハンドラーを呼び出すには、操作を呼び出すときに指定`AuthorizeAsync`ページ ハンドラーまたはアクションにします。 次の例では、指定されたドキュメントを表示する、認証されたユーザーが許可されているかどうかを判断します。

> [!NOTE]
> 次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティです。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

承認に成功した場合、ドキュメントの表示 ページが返されます。 かどうかの承認が失敗したが、ユーザーが認証されると、返す`ForbidResult`承認に失敗したすべての認証ミドルウェアに通知します。 A`ChallengeResult`認証を実行する必要がありますが返されます。 対話型ブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトする適切な場合があります。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

承認に成功した場合、ドキュメントのビューが返されます。 承認に失敗した場合に返す`ChallengeResult`すべての認証ミドルウェアを通知承認に失敗しましたとミドルウェアは、適切な応答を受け取ることができます。 適切な応答には、401 または 403 ステータス コードを返す可能性があります。 対話型ブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトする、可能性があります。

---
