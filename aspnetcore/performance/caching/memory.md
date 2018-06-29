---
title: ASP.NET Core のメモリ内のキャッシュします。
author: rick-anderson
description: ASP.NET Core でのメモリ内のデータをキャッシュする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
uid: performance/caching/memory
ms.openlocfilehash: 5a7085269e255ae233a0e7eeb860a04b2c6bede3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077587"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="00a46-103">ASP.NET Core のメモリ内のキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="00a46-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="00a46-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [John Luo](https://github.com/JunTaoLuo)、および[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="00a46-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="00a46-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="00a46-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="00a46-106">キャッシュの基礎</span><span class="sxs-lookup"><span data-stu-id="00a46-106">Caching basics</span></span>

<span data-ttu-id="00a46-107">キャッシュと、パフォーマンスと、アプリのスケーラビリティが大幅に、コンテンツの生成に必要な作業を減らすことによって向上できます。</span><span class="sxs-lookup"><span data-stu-id="00a46-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="00a46-108">キャッシュの動作変更頻度が低いデータに最適です。</span><span class="sxs-lookup"><span data-stu-id="00a46-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="00a46-109">キャッシュで返すことができる多くのデータのコピー元のソースからよりも高速です。</span><span class="sxs-lookup"><span data-stu-id="00a46-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="00a46-110">キャッシュされたデータに依存しないアプリのテストを読み書きする必要があります。</span><span class="sxs-lookup"><span data-stu-id="00a46-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="00a46-111">ASP.NET Core では、いくつかの異なるキャッシュをサポートします。</span><span class="sxs-lookup"><span data-stu-id="00a46-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="00a46-112">最も単純なキャッシュがに基づいて、 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)、web サーバーのメモリ内キャッシュを表します。</span><span class="sxs-lookup"><span data-stu-id="00a46-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="00a46-113">複数のサーバーのサーバー ファーム上で実行されるアプリは、メモリ内キャッシュを使用するときにセッションが固定であることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00a46-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="00a46-114">スティッキー セッションでは、すべてのクライアントからの後続の要求が同じサーバーに送られることを確認します。</span><span class="sxs-lookup"><span data-stu-id="00a46-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="00a46-115">たとえば、Azure Web アプリで使用する[アプリケーション要求ルーティング処理](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) を同じサーバーにすべての後続の要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="00a46-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="00a46-116">Web ファーム内の非スティッキー セッションが必要な[分散キャッシュ](distributed.md)キャッシュ整合性の問題を回避します。</span><span class="sxs-lookup"><span data-stu-id="00a46-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="00a46-117">アプリによっては、分散キャッシュはメモリ内キャッシュよりも高いスケール アウトをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="00a46-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="00a46-118">分散キャッシュを使用して外部処理へのキャッシュ メモリの負荷を軽減します。</span><span class="sxs-lookup"><span data-stu-id="00a46-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="00a46-119">`IMemoryCache`キャッシュがない限りにメモリ不足のキャッシュ エントリを削除し、[優先度をキャッシュ](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)に設定されている`CacheItemPriority.NeverRemove`です。</span><span class="sxs-lookup"><span data-stu-id="00a46-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="00a46-120">設定することができます、`CacheItemPriority`キャッシュがメモリ負荷状態にある項目を削除すると、優先度を調整します。</span><span class="sxs-lookup"><span data-stu-id="00a46-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="00a46-121">メモリ内キャッシュは、すべてのオブジェクトを格納できます。分散キャッシュ インターフェイスに制限されて`byte[]`です。</span><span class="sxs-lookup"><span data-stu-id="00a46-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="00a46-122">IMemoryCache を使用します。</span><span class="sxs-lookup"><span data-stu-id="00a46-122">Using IMemoryCache</span></span>

<span data-ttu-id="00a46-123">メモリ内キャッシュは、*サービス*を使用して、アプリから参照される[依存性の注入](../../fundamentals/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="00a46-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="00a46-124">呼び出す`AddMemoryCache`で`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="00a46-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="00a46-125">要求、`IMemoryCache`コンス トラクターでインスタンス。</span><span class="sxs-lookup"><span data-stu-id="00a46-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="00a46-126">`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)です。</span><span class="sxs-lookup"><span data-stu-id="00a46-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="00a46-127">`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)、利用可能なは、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)です。</span><span class="sxs-lookup"><span data-stu-id="00a46-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="00a46-128">`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)、利用可能なは、 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)です。</span><span class="sxs-lookup"><span data-stu-id="00a46-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="00a46-129">次のコードでは[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)キャッシュ内で時間をチェックします。</span><span class="sxs-lookup"><span data-stu-id="00a46-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="00a46-130">新しいエントリが作成されとキャッシュに追加された場合は、時間がキャッシュされない、[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)です。</span><span class="sxs-lookup"><span data-stu-id="00a46-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="00a46-131">現在の時刻とキャッシュされた時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00a46-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="00a46-132">キャッシュされた`DateTime`内でタイムアウト期間 (メモリ不足のためのない削除) 要求があるときに、値がキャッシュに保持します。</span><span class="sxs-lookup"><span data-stu-id="00a46-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="00a46-133">次の図は、キャッシュから取得される前の時刻と現在の時刻を示します。</span><span class="sxs-lookup"><span data-stu-id="00a46-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![表示される 2 つの異なるタイミングでインデックス ビュー](memory/_static/time.png)

<span data-ttu-id="00a46-135">次のコードでは[GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)と[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)データをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="00a46-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="00a46-136">次のコード呼び出し[取得](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)キャッシュの時間をフェッチします。</span><span class="sxs-lookup"><span data-stu-id="00a46-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="00a46-137">参照してください[IMemoryCache メソッド](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)と[CacheExtensions メソッド](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)キャッシュ方法の詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="00a46-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="00a46-138">MemoryCacheEntryOptions を使用します。</span><span class="sxs-lookup"><span data-stu-id="00a46-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="00a46-139">次の例では:</span><span class="sxs-lookup"><span data-stu-id="00a46-139">The following sample:</span></span>

- <span data-ttu-id="00a46-140">絶対有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="00a46-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="00a46-141">これは、エントリをキャッシュできる最大時間であり、アイテムがスライド式有効期限が継続的に更新されるときに古くなることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="00a46-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="00a46-142">スライディング期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="00a46-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="00a46-143">このキャッシュされたアイテムにアクセスするための要求は、スライド式有効期限の時間にリセットされます。</span><span class="sxs-lookup"><span data-stu-id="00a46-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="00a46-144">キャッシュ優先度を設定`CacheItemPriority.NeverRemove`です。</span><span class="sxs-lookup"><span data-stu-id="00a46-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="00a46-145">セット、 [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate)するたびに呼び出されます、エントリがキャッシュから削除します。</span><span class="sxs-lookup"><span data-stu-id="00a46-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="00a46-146">キャッシュから項目を削除するコードから別のスレッドでコールバックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="00a46-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="00a46-147">キャッシュの依存関係</span><span class="sxs-lookup"><span data-stu-id="00a46-147">Cache dependencies</span></span>

<span data-ttu-id="00a46-148">次の例では、依存するエントリの有効期限が切れた場合は、キャッシュ エントリを期限切れする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="00a46-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="00a46-149">A`CancellationChangeToken`はキャッシュされたアイテムに追加します。</span><span class="sxs-lookup"><span data-stu-id="00a46-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="00a46-150">ときに`Cancel`で呼び出されると、 `CancellationTokenSource`、両方のキャッシュ エントリが削除されます。</span><span class="sxs-lookup"><span data-stu-id="00a46-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="00a46-151">使用して、`CancellationTokenSource`グループとして削除する複数のキャッシュ エントリを許可します。</span><span class="sxs-lookup"><span data-stu-id="00a46-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="00a46-152">`using`内で作成された上記のコードでのパターン、キャッシュ エントリ、`using`ブロックはトリガーと有効期限の設定を継承します。</span><span class="sxs-lookup"><span data-stu-id="00a46-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="00a46-153">補足メモ</span><span class="sxs-lookup"><span data-stu-id="00a46-153">Additional notes</span></span>

- <span data-ttu-id="00a46-154">コールバックを使用して、キャッシュ項目を再作成します。</span><span class="sxs-lookup"><span data-stu-id="00a46-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="00a46-155">コールバックが完了していないために、複数の要求には、キャッシュされたキーの値が空ことができます検索します。</span><span class="sxs-lookup"><span data-stu-id="00a46-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="00a46-156">これは、結果、複数のスレッドが、キャッシュされた項目を再作成します。</span><span class="sxs-lookup"><span data-stu-id="00a46-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="00a46-157">1 つのキャッシュ エントリを使用して、別に作成した場合、子は親エントリの有効期限切れのトークンと時間ベースの有効期限の設定をコピーします。</span><span class="sxs-lookup"><span data-stu-id="00a46-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="00a46-158">子は、手動で削除して期限切れまたは親エントリの更新はありません。</span><span class="sxs-lookup"><span data-stu-id="00a46-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00a46-159">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="00a46-159">Additional resources</span></span>

* [<span data-ttu-id="00a46-160">分散キャッシュの使用</span><span class="sxs-lookup"><span data-stu-id="00a46-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="00a46-161">変更トークンを使用する変更の検出</span><span class="sxs-lookup"><span data-stu-id="00a46-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="00a46-162">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="00a46-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="00a46-163">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="00a46-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="00a46-164">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="00a46-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="00a46-165">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="00a46-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
