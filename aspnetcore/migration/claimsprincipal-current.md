---
title: ClaimsPrincipal.Current からの移行します。
author: mjrousos
description: 現在の認証されたユーザーの id および ASP.NET Core 内のクレームを取得する ClaimsPrincipal.Current から離れた場所に移行する方法を説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851534"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="a1e70-103">ClaimsPrincipal.Current からの移行します。</span><span class="sxs-lookup"><span data-stu-id="a1e70-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="a1e70-104">ASP.NET プロジェクトでは使用する一般的な[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)ユーザーの id と要求を認証するには、現在を取得します。</span><span class="sxs-lookup"><span data-stu-id="a1e70-104">In ASP.NET projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="a1e70-105">ASP.NET Core では、このプロパティは設定不要になった。</span><span class="sxs-lookup"><span data-stu-id="a1e70-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="a1e70-106">コードは、別の手段を使用して、現在認証済みユーザーの id を取得するように更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1e70-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="a1e70-107">静的データではなく、コンテキスト固有のデータ</span><span class="sxs-lookup"><span data-stu-id="a1e70-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="a1e70-108">ASP.NET Core、両方の値を使用するときに`ClaimsPrincipal.Current`と`Thread.CurrentPrincipal`よう設定されていません。</span><span class="sxs-lookup"><span data-stu-id="a1e70-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="a1e70-109">これらのプロパティは両方を表す静的状態は、ASP.NET Core を一般的に回避できます。</span><span class="sxs-lookup"><span data-stu-id="a1e70-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="a1e70-110">ASP.NET Core のアーキテクチャは、コンテキスト固有のサービスのコレクションから (現在のユーザーの id) のような依存関係を取得する代わりに、(を使用してその[依存性の注入](xref:fundamentals/dependency-injection)(DI) モデル)。</span><span class="sxs-lookup"><span data-stu-id="a1e70-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="a1e70-111">さらに、`Thread.CurrentPrincipal`スレッドが静的では、一部の非同期のシナリオでの変更を保持可能性がありますされないように (および`ClaimsPrincipal.Current`呼び出すだけ`Thread.CurrentPrincipal`既定)。</span><span class="sxs-lookup"><span data-stu-id="a1e70-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="a1e70-112">問題のスレッドの種類を理解するには、静的メンバー発生する可能性が非同期のシナリオでは、次のコード スニペットを検討してください。</span><span class="sxs-lookup"><span data-stu-id="a1e70-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

<span data-ttu-id="a1e70-113">上記のサンプル コード セット`Thread.CurrentPrincipal`前に、と後の非同期呼び出しを待機してその値を確認します。</span><span class="sxs-lookup"><span data-stu-id="a1e70-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="a1e70-114">`Thread.CurrentPrincipal` 固有の*スレッド*が設定されているし、メソッドは await の後ろで別のスレッドで実行を再開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1e70-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="a1e70-115">その結果、`Thread.CurrentPrincipal`が最初にチェックが null で呼び出しの後に、ある`await Task.Yield()`です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="a1e70-116">アプリの DI サービスのコレクションから現在のユーザーの id を取得がよりテストが容易なすぎます、テスト ユーザーが簡単に挿入されるためです。</span><span class="sxs-lookup"><span data-stu-id="a1e70-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="a1e70-117">ASP.NET Core アプリケーションの現在のユーザーを取得します</span><span class="sxs-lookup"><span data-stu-id="a1e70-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="a1e70-118">現在の認証済みユーザーを取得するためのいくつかのオプション`ClaimsPrincipal`ASP.NET Core の代わりに`ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="a1e70-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="a1e70-119">**ControllerBase.User**です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-119">**ControllerBase.User**.</span></span> <span data-ttu-id="a1e70-120">MVC コント ローラーで、現在の認証済みユーザーにアクセスできる、[ユーザー](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a1e70-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="a1e70-121">**HttpContext.User**です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-121">**HttpContext.User**.</span></span> <span data-ttu-id="a1e70-122">現在のアクセス権を持つコンポーネント`HttpContext`(ミドルウェアの例) は、現在のユーザーを取得できます`ClaimsPrincipal`から[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="a1e70-123">**呼び出し元から渡された**です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-123">**Passed in from caller**.</span></span> <span data-ttu-id="a1e70-124">現在のアクセス権のないライブラリ`HttpContext`コント ローラーまたはミドルウェア コンポーネントから多くの場合、呼び出され、引数として渡された現在のユーザーの id を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="a1e70-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="a1e70-125">**IHttpContextAccessor**です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="a1e70-126">ASP.NET Core に移行して、ASP.NET プロジェクトは、すべての必要な場所に、現在のユーザーの id を簡単に渡すには大きすぎる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1e70-126">The ASP.NET project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="a1e70-127">このような場合、 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)回避策として使用できます。</span><span class="sxs-lookup"><span data-stu-id="a1e70-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="a1e70-128">`IHttpContextAccessor` 現在アクセス可能な`HttpContext`(が存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="a1e70-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="a1e70-129">短期的なソリューション内の ASP.NET Core DI ドリブン アーキテクチャと作業をまだ更新されていないコードの現在のユーザーの id を取得するのには次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a1e70-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="a1e70-130">ください`IHttpContextAccessor`を呼び出して DI コンテナーで使用できる[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)で`Startup.ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="a1e70-131">インスタンスを取得`IHttpContextAccessor`の起動時に静的変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="a1e70-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="a1e70-132">インスタンスが行われますが、現在のユーザーが静的プロパティから取得することは以前のコードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a1e70-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="a1e70-133">現在のユーザーの取得`ClaimsPrincipal`を使用して`HttpContextAccessor.HttpContext?.User`です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="a1e70-134">このコードは、HTTP 要求のコンテキスト外で使用する場合、`HttpContext`が null です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="a1e70-135">最終的なオプションを使用して`IHttpContextAccessor`は、(静的な依存関係を挿入された依存関係を使い続ける理由) ASP.NET Core の基本原則とは対照的です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-135">The final option, using `IHttpContextAccessor`, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="a1e70-136">最終的に、静的な依存関係を削除しようとしています。`IHttpContextAccessor`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="a1e70-136">Plan to eventually remove the dependency on the static `IHttpContextAccessor` helper.</span></span> <span data-ttu-id="a1e70-137">できますに便利です、ブリッジは、以前使用していた既存の ASP.NET アプリケーションが大きなを移行するときに`ClaimsPrincipal.Current`です。</span><span class="sxs-lookup"><span data-stu-id="a1e70-137">It can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
