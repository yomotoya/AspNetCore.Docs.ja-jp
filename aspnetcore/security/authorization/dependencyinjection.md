---
title: ASP.NET Core の要件のハンドラーで依存関係の挿入
author: rick-anderson
description: 依存関係の挿入を使用して ASP.NET Core アプリケーションに承認要求ハンドラーを挿入する方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273723"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core の要件のハンドラーで依存関係の挿入

<a name="security-authorization-di"></a>

[認証ハンドラーを登録する必要があります](xref:security/authorization/policies#handler-registration)構成中にサービスのコレクションで (を使用して[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection))。

認証ハンドラー内で評価を行うとルールのリポジトリがあるとし、そのリポジトリは、サービスのコレクションに登録されました。 承認を解決して、コンス トラクターを挿入します。

たとえば、次のように ASP を使用する場合です。挿入するインフラストラクチャのログ記録の NET`ILoggerFactory`ハンドラーにします。 このようなハンドラーは、ようになります。

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

ハンドラーを登録するよう`services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

アプリケーションを起動時に作成される、ハンドラーのインスタンスと、登録されている挿入 DI は`ILoggerFactory`コンス トラクターにします。

> [!NOTE]
> シングルトンとしては、Entity Framework を使用してハンドラーを登録するべきではありません。
