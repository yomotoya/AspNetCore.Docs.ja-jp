---
title: ASP.NET Core での要件ハンドラーで依存関係の挿入
author: rick-anderson
description: 依存関係の挿入を使用して ASP.NET Core アプリを承認要件ハンドラーを挿入する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896369"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core での要件ハンドラーで依存関係の挿入

<a name="security-authorization-di"></a>

構成の中にあるサービスコレクションで、[承認ハンドラーを登録する必要があります](xref:security/authorization/policies#handler-registration) (これには[依存関係の注入](xref:fundamentals/dependency-injection)を使用します)。

承認ハンドラーの内部で評価を行たいルールのリポジトリがあり、それがサービスコレクションに登録されてると仮定します。 承認が解決されコンストラクターに挿入します。

例えば、ASP.NET のロギングの委付らストラクチャを使いたい場合は、ハンドラーに `ILoggerFactory` を挿入します。そのときは、ハンドラーを次のようにします。

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

`services.AddSingleton()` を使ってハンドラーを登録します:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

アプリケーションの起動時にハンドラーのインスタンスが生成され、DIでハンドラーのコンストラクターに登録した `ILoggerFactory` を挿入されます。

> [!NOTE]
> Entity Framework を使用したハンドラーは、シングルトンとして登録することはできません。
