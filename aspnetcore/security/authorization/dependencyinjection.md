---
title: "要件のハンドラーで依存関係の挿入"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 308951a45ee6576f096e1cdc792208b89e476e61
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>要件のハンドラーで依存関係の挿入

<a name="security-authorization-di"></a>

[認証ハンドラーを登録する必要があります](policies.md#security-authorization-policies-based-handler-registration)構成中にサービスのコレクションで (を使用して[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。

認証ハンドラー内で評価を行うとルールのリポジトリがあるとし、そのリポジトリは、サービスのコレクションに登録されました。  承認を解決して、コンス トラクターを挿入します。

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
