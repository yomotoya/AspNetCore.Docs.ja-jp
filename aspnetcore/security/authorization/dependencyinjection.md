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
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="eeda8-103">ASP.NET Core での要件ハンドラーで依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="eeda8-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="eeda8-104">[承認ハンドラーを登録する必要があります](xref:security/authorization/policies#handler-registration)構成中にサービスのコレクションで (を使用して[依存関係の注入](xref:fundamentals/dependency-injection))。</span><span class="sxs-lookup"><span data-stu-id="eeda8-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="eeda8-105">承認ハンドラーの内部評価を行うとルールのリポジトリがあるとし、そのリポジトリがサービス コレクションに登録します。</span><span class="sxs-lookup"><span data-stu-id="eeda8-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="eeda8-106">承認が解決され、コンス トラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="eeda8-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="eeda8-107">たとえば、次のように ASP を使用する場合です。NET に挿入するインフラストラクチャのログ記録`ILoggerFactory`ハンドラーにします。</span><span class="sxs-lookup"><span data-stu-id="eeda8-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="eeda8-108">このようなハンドラーは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="eeda8-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="eeda8-109">ハンドラーを登録すると`services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="eeda8-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="eeda8-110">アプリケーションの開始時に作成される、ハンドラーのインスタンスと、登録済み挿入 DI は`ILoggerFactory`コンス トラクターにします。</span><span class="sxs-lookup"><span data-stu-id="eeda8-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="eeda8-111">Entity Framework を使用して、ハンドラーは、シングルトンとして登録することはできません。</span><span class="sxs-lookup"><span data-stu-id="eeda8-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
