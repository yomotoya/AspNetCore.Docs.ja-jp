---
title: "リソース ベースの承認"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a>リソース ベースの承認

<a name=security-authorization-resource-based></a>

多くの場合、承認は、アクセス対象のリソースに依存します。 たとえば、ドキュメントには、author プロパティがあります。 ドキュメントの作成者だけには許可されたリソースは、承認の評価を行う前に、ドキュメント リポジトリから読み込む必要があるため、更新します。 属性の評価はアクション内で、独自コード リソースの読み込みを実行する前に、データ バインディングの前に、承認属性を持つことはできません。 宣言型の承認、属性の方法ではなくを使用しなければならない命令型の承認では、開発者が独自のコード内で承認関数を呼び出す場所。

## <a name="authorizing-within-your-code"></a>コード内で承認します。

承認は、サービスとして実装`IAuthorizationService`サービス コレクションに登録されている、および経由で入手できます[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)コント ローラーにアクセスするためです。

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

`IAuthorizationService`いずれかの場所を渡すリソースと、ポリシー名およびその他のリソースおよび評価するための要件の一覧を渡す 2 つのメソッドがあります。

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

サービスの負荷を操作内でリソースを呼び出すを呼び出す、`AuthorizeAsync`オーバー ロードする必要があります。 次に例を示します。

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

## <a name="writing-a-resource-based-handler"></a>リソース ベースのハンドラーの記述

リソース ベースの承認のハンドラーの記述に大きな違いはなく[plain 要件ハンドラーの記述](policies.md#security-authorization-policies-based-authorization-handler)です。 要件を作成して前に、要件とも、リソースの種類を指定する、要件のハンドラーを実装します。 たとえば、ドキュメント リソースを受け入れることがハンドラーを示しています。

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

忘れずに、ハンドラーを登録する必要があります、`ConfigureServices`メソッドです。

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>運用上の要件

読み取り、書き込み、更新、削除などの操作に基づく決定を行う場合を使って、`OperationAuthorizationRequirement`クラス内で、`Microsoft.AspNetCore.Authorization.Infrastructure`名前空間。 この要件をあらかじめ作成されているクラスを使用すると、操作ごとに個別のクラスを作成する代わりに、パラメーター化された操作名を持つ 1 つのハンドラーを記述できます。 使用する、いくつかの操作名を指定します。

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

ハンドラーでしたして実装する次のように、仮定を使用して`Document`リソースとしてクラス

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

ハンドラーが動作を確認できます`OperationAuthorizationRequirement`です。 ハンドラー内のコードは、その評価を行うときに、アカウントに指定された要件の Name プロパティを受け取る必要があります。

呼び出すときに、操作を指定する必要があります。 運用リソース ハンドラーを呼び出して`AuthorizeAsync`アクションにします。 次に例を示します。

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

この例では、ユーザーが現在の読み取り操作を実行できないかどうか`document`インスタンス。 承認が成功した場合は、ドキュメントのビューが返されます。 返す承認に失敗したかどうかは`ChallengeResult`ミドルウェアの認証が失敗したため、ミドルウェアは 401 または 403 ステータス コードを返すなどのログイン ページにユーザーをリダイレクトする、適切な応答を受け取ることができます、任意の認証を通知します。対話型ブラウザー クライアント。
