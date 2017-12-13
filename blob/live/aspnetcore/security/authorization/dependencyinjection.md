---
title: "要件のハンドラーで依存関係の挿入"
author: rick-anderson
description: "このドキュメントでは、依存関係の挿入を使用して ASP.NET Core アプリケーションに承認要求ハンドラーを挿入する方法について説明します。"
keywords: "ASP.NET Core、依存関係の挿入、承認ハンドラー"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="b9e06-104">要件のハンドラーで依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="b9e06-104">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="b9e06-105">[認証ハンドラーを登録する必要があります](policies.md#handler-registration)構成中にサービスのコレクションで (を使用して[依存性の注入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。</span><span class="sxs-lookup"><span data-stu-id="b9e06-105">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="b9e06-106">認証ハンドラー内で評価を行うとルールのリポジトリがあるとし、そのリポジトリは、サービスのコレクションに登録されました。</span><span class="sxs-lookup"><span data-stu-id="b9e06-106">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="b9e06-107">承認を解決して、コンス トラクターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="b9e06-107">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="b9e06-108">たとえば、次のように ASP を使用する場合です。挿入するインフラストラクチャのログ記録の NET`ILoggerFactory`ハンドラーにします。</span><span class="sxs-lookup"><span data-stu-id="b9e06-108">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="b9e06-109">このようなハンドラーは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="b9e06-109">Such a handler might look like:</span></span>

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

<span data-ttu-id="b9e06-110">ハンドラーを登録するよう`services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="b9e06-110">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="b9e06-111">アプリケーションを起動時に作成される、ハンドラーのインスタンスと、登録されている挿入 DI は`ILoggerFactory`コンス トラクターにします。</span><span class="sxs-lookup"><span data-stu-id="b9e06-111">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e06-112">シングルトンとしては、Entity Framework を使用してハンドラーを登録するべきではありません。</span><span class="sxs-lookup"><span data-stu-id="b9e06-112">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
