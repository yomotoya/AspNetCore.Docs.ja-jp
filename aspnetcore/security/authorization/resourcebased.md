---
title: ASP.NET Core でリソース ベースの承認
author: scottaddie
description: Authorize 属性が十分ではない場合に、ASP.NET Core アプリでリソース ベースの承認を実装する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206697"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET Core でリソース ベースの承認

承認方法は、アクセス対象のリソースに依存します。 作成者のプロパティを持つドキュメントを検討してください。 ドキュメントを更新するには、作成者のみが許可されます。 その結果、承認の評価を行う前に、ドキュメントをデータ ストアから取得する必要があります。

属性の評価では、データ バインディングの前に、ページ ハンドラーまたはドキュメントが読み込まれますアクションの実行前に発生します。 これらの理由、宣言型を使用した承認からを`[Authorize]`属性が十分ではありません。 代わりに、カスタム承認メソッドを呼び出すことができます&mdash;命令型の承認と呼ばれるスタイル。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

[承認によって保護されたユーザー データと ASP.NET Core アプリを作成する](xref:security/authorization/secure-data)リソース ベースの承認を使用するサンプル アプリが含まれています。

## <a name="use-imperative-authorization"></a>命令型の承認を使用します。

承認の実装として、 [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)サービスし、は、サービスのコレクション内に登録されて、`Startup`クラス。 サービスを使用して利用可能になって[依存関係の注入](xref:fundamentals/dependency-injection)ページ ハンドラーやアクション。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` 2 つの`AuthorizeAsync`メソッドのオーバー ロード: リソースと、ポリシー名とその他のリソースと、評価するための要件の一覧を受け入れる 1 つを受け入れます。

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

次の例では、セキュリティで保護するリソースがカスタムに読み込まれる`Document`オブジェクト。 `AuthorizeAsync`現在のユーザーが指定されたドキュメントを編集できるかどうかを判断するオーバー ロードが呼び出されます。 カスタムの"EditPolicy"承認ポリシーは、意思決定にファクタリングされます。 参照してください[カスタム ポリシー ベースの承認](xref:security/authorization/policies)承認ポリシーの作成に関する詳細について。

> [!NOTE]
> 次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>リソース ベースのハンドラーを記述します。

リソース ベースの承認がかなり異なるハンドラーの記述[プレーンな要件ハンドラーの記述](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)します。 カスタム要件クラスを作成し、要件のハンドラー クラスを実装します。 ハンドラー クラスには、要件とリソースの種類を指定します。 など、ハンドラー利用して、`SameAuthorRequirement`要件と`Document`リソースを以下に示します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

要件とハンドラーの登録、`Startup.ConfigureServices`メソッド。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>運用上の要件

CRUD (作成、読み取り、更新、削除) 操作の結果に基づいた決定を作成する場合は、使用、 [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)ヘルパー クラス。 このクラスでは、操作の種類ごとの個々 のクラスではなく、単一のハンドラーを記述することができます。 これを使用するには、いくつかの操作名を指定します。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

ハンドラーが、次のように実装されるを使用して、`OperationAuthorizationRequirement`要件と`Document`リソース。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

前のハンドラーは、リソース、ユーザーの id、および要件を使用して、操作を検証します。`Name`プロパティ。

運用上のリソースのハンドラーを呼び出すには、操作を呼び出すときに指定`AuthorizeAsync`ページ ハンドラーまたはアクションにします。 次の例では、指定されたドキュメントを表示する、認証されたユーザーが許可されているかどうかを判断します。

> [!NOTE]
> 次のコード サンプルは、認証が実行を想定し、セット、`User`プロパティ。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

承認が成功すると、ドキュメントを表示するためのページが返されます。 かどうかの承認が失敗したが、ユーザーが認証されると、返す`ForbidResult`承認に失敗したすべての認証ミドルウェアに通知します。 A`ChallengeResult`認証を実行する必要がありますが返されます。 対話型のブラウザー クライアントの場合は、ユーザーをログイン ページにリダイレクトする適切な場合があります。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

承認が成功すると、ドキュメントのビューが返されます。 承認に失敗した場合に返す`ChallengeResult`承認に失敗しました、ミドルウェアは、適切な応答にかかるすべての認証ミドルウェアに通知します。 適切な応答には、401 または 403 状態コードを返す可能性があります。 対話型のブラウザー クライアントでは、ユーザーをログイン ページにリダイレクトすることも考えられます。

---
