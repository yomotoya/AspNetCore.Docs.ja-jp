---
title: ClaimsPrincipal.Current からの移行します。
author: mjrousos
description: 現在の認証済みユーザーの id と ASP.NET Core でのクレームを取得する ClaimsPrincipal.Current から移行する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894789"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="4e92d-103">ClaimsPrincipal.Current からの移行します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="4e92d-104">ASP.NET 4.x プロジェクトでは使用する一般的な[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)現在を取得するユーザーの id と要求を認証します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-104">In ASP.NET 4.x projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="4e92d-105">ASP.NET Core では、このプロパティは設定不要になった。</span><span class="sxs-lookup"><span data-stu-id="4e92d-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="4e92d-106">それに依存していたコードは、さまざまな手段を通じて現在の認証済みユーザーの id を取得するように更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e92d-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="4e92d-107">静的データではなく、コンテキスト固有のデータ</span><span class="sxs-lookup"><span data-stu-id="4e92d-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="4e92d-108">ASP.NET Core では、両方の値を使用する場合`ClaimsPrincipal.Current`と`Thread.CurrentPrincipal`が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="4e92d-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="4e92d-109">これらのプロパティは両方を表す静的状態は、ASP.NET Core は一般的に回避できます。</span><span class="sxs-lookup"><span data-stu-id="4e92d-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="4e92d-110">ASP.NET Core のアーキテクチャは、コンテキスト固有のサービスのコレクションから (現在のユーザーの id) などの依存関係を取得する代わりに、(を使用してその[依存関係の注入](xref:fundamentals/dependency-injection)(DI) モデル)。</span><span class="sxs-lookup"><span data-stu-id="4e92d-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="4e92d-111">さらに、`Thread.CurrentPrincipal`はいくつかの非同期のシナリオでの変更を無効になること可能性がありますので、スレッドの静的な (と`ClaimsPrincipal.Current`呼び出すだけです`Thread.CurrentPrincipal`既定で)。</span><span class="sxs-lookup"><span data-stu-id="4e92d-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="4e92d-112">問題のスレッドの種類を理解するには、静的メンバーを非同期では、次のコード スニペットを検討してください。 発生する可能性します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

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

<span data-ttu-id="4e92d-113">上記のサンプル コード セット`Thread.CurrentPrincipal`前に、と後の非同期呼び出しを待機しています。 その値を確認します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="4e92d-114">`Thread.CurrentPrincipal` 固有では、*スレッド*を設定すると、およびメソッドが await の後に別のスレッドで実行を再開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e92d-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="4e92d-115">その結果、`Thread.CurrentPrincipal`が最初にチェックが null で呼び出しの後に、ある`await Task.Yield()`します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="4e92d-116">アプリの DI サービスのコレクションから現在のユーザーの id の取得がしやすい、すぎます、テスト ユーザーを簡単に挿入されるためです。</span><span class="sxs-lookup"><span data-stu-id="4e92d-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="4e92d-117">ASP.NET Core アプリの現在のユーザーを取得します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="4e92d-118">現在の認証済みユーザーを取得するためのいくつかのオプションがある`ClaimsPrincipal`の代わりに ASP.NET Core で`ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="4e92d-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="4e92d-119">**ControllerBase.User**します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-119">**ControllerBase.User**.</span></span> <span data-ttu-id="4e92d-120">MVC コント ローラーを現在の認証済みユーザーにアクセスできる、[ユーザー](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4e92d-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="4e92d-121">**HttpContext.User**します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-121">**HttpContext.User**.</span></span> <span data-ttu-id="4e92d-122">現在のアクセス権を持つコンポーネント`HttpContext`(ミドルウェアの例) は、現在のユーザーを取得できます`ClaimsPrincipal`から[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="4e92d-123">**呼び出し元から渡された**します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-123">**Passed in from caller**.</span></span> <span data-ttu-id="4e92d-124">現在のアクセス権を持たないライブラリ`HttpContext`コント ローラーまたはミドルウェア コンポーネントから多くの場合、呼び出され、引数として渡された現在のユーザーの id を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="4e92d-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="4e92d-125">**IHttpContextAccessor**します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="4e92d-126">ASP.NET Core に移行しているプロジェクトは、簡単に、現在のユーザーの id を渡すために必要なすべての場所には大きすぎる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4e92d-126">The project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="4e92d-127">このような場合、 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)回避策として使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e92d-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="4e92d-128">`IHttpContextAccessor` 現在アクセスできる`HttpContext`(存在する場合。</span><span class="sxs-lookup"><span data-stu-id="4e92d-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="4e92d-129">ASP.NET Core の DI 駆動アーキテクチャで作業をまだ更新されていないコードで、現在のユーザーの id を取得する短期的なソリューションは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4e92d-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="4e92d-130">ように`IHttpContextAccessor`呼び出して DI コンテナーで使用できる[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)で`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="4e92d-131">インスタンスを取得`IHttpContextAccessor`スタートアップ中に静的変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="4e92d-132">インスタンスが行われたが、現在のユーザーが静的プロパティから取得することは以前のコードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e92d-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="4e92d-133">現在のユーザーの取得`ClaimsPrincipal`を使用して`HttpContextAccessor.HttpContext?.User`します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="4e92d-134">このコードは、HTTP 要求のコンテキスト外で使用されている場合、`HttpContext`が null です。</span><span class="sxs-lookup"><span data-stu-id="4e92d-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="4e92d-135">最終的なオプションを使用して、`IHttpContextAccessor`インスタンスの静的変数に格納されている、(静的な依存関係に挿入された依存関係を優先) ASP.NET Core の原則に反するです。</span><span class="sxs-lookup"><span data-stu-id="4e92d-135">The final option, using an `IHttpContextAccessor` instance stored in a static variable, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="4e92d-136">最終的に取得する`IHttpContextAccessor`インスタンス依存関係の挿入からの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4e92d-136">Plan to eventually retrieve `IHttpContextAccessor` instances from dependency injection instead.</span></span> <span data-ttu-id="4e92d-137">静的ヘルパーできる便利なブリッジでは、ただし、以前使用していた既存の ASP.NET アプリが大きなを移行するときに`ClaimsPrincipal.Current`します。</span><span class="sxs-lookup"><span data-stu-id="4e92d-137">A static helper can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
