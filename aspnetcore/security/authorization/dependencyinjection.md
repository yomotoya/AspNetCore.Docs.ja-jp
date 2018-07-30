---
title: ASP.NET Core での要件ハンドラーで依存関係の挿入
author: rick-anderson
description: 依存関係の挿入を使用して ASP.NET Core アプリを承認要件ハンドラーを挿入する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342115"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core での要件ハンドラーで依存関係の挿入

<a name="security-authorization-di"></a>

[承認ハンドラーを登録する必要があります](xref:security/authorization/policies#handler-registration)構成中にサービスのコレクションで (を使用して[依存関係の注入](xref:fundamentals/dependency-injection))。

承認ハンドラーの内部評価を行うとルールのリポジトリがあるとし、そのリポジトリがサービス コレクションに登録します。 承認が解決され、コンス トラクターに挿入します。

たとえば、次のように ASP を使用する場合です。NET に挿入するインフラストラクチャのログ記録`ILoggerFactory`ハンドラーにします。 このようなハンドラーは、ようになります。

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

ハンドラーを登録すると`services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

アプリケーションの開始時に作成される、ハンドラーのインスタンスと、登録済み挿入 DI は`ILoggerFactory`コンス トラクターにします。

> [!NOTE]
> Entity Framework を使用して、ハンドラーは、シングルトンとして登録することはできません。
