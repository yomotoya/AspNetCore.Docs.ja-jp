---
title: "要件のハンドラーで依存関係の挿入"
author: rick-anderson
description: "このドキュメントでは、依存関係の挿入を使用して ASP.NET Core アプリケーションに承認要求ハンドラーを挿入する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="21e5f-103">要件のハンドラーで依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="21e5f-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="21e5f-104">[認証ハンドラーを登録する必要があります](policies.md#handler-registration)構成中にサービスのコレクションで (を使用して[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。</span><span class="sxs-lookup"><span data-stu-id="21e5f-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="21e5f-105">認証ハンドラー内で評価を行うとルールのリポジトリがあるとし、そのリポジトリは、サービスのコレクションに登録されました。</span><span class="sxs-lookup"><span data-stu-id="21e5f-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="21e5f-106">承認を解決して、コンス トラクターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="21e5f-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="21e5f-107">たとえば、次のように ASP を使用する場合です。挿入するインフラストラクチャのログ記録の NET`ILoggerFactory`ハンドラーにします。</span><span class="sxs-lookup"><span data-stu-id="21e5f-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="21e5f-108">このようなハンドラーは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="21e5f-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="21e5f-109">ハンドラーを登録するよう`services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="21e5f-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="21e5f-110">アプリケーションを起動時に作成される、ハンドラーのインスタンスと、登録されている挿入 DI は`ILoggerFactory`コンス トラクターにします。</span><span class="sxs-lookup"><span data-stu-id="21e5f-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="21e5f-111">シングルトンとしては、Entity Framework を使用してハンドラーを登録するべきではありません。</span><span class="sxs-lookup"><span data-stu-id="21e5f-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
