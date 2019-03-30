---
title: ASP.NET Core でのキャッシュを分散
author: guardrex
description: アプリのパフォーマンスと特にクラウドまたはサーバー ファーム環境でのスケーラビリティを向上させるために、ASP.NET Core の分散キャッシュを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: performance/caching/distributed
ms.openlocfilehash: c3774c26116a4cb70386d0060f2244d224fec8e1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750988"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="83ff0-103">ASP.NET Core でのキャッシュを分散</span><span class="sxs-lookup"><span data-stu-id="83ff0-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="83ff0-104">によって[Luke Latham](https://github.com/guardrex)と[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="83ff0-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="83ff0-105">分散キャッシュは、通常、外部サービスにアクセスするアプリケーション サーバーとして保守管理されて複数のアプリ サーバーによって共有キャッシュです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="83ff0-106">分散キャッシュでは、アプリがクラウド サービスまたはサーバー ファームでホストされている場合に特に、パフォーマンスと ASP.NET Core アプリのスケーラビリティを向上できます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="83ff0-107">分散キャッシュは、個々 のアプリのサーバーでのキャッシュされたデータの格納の他のキャッシュ シナリオのいくつかのメリットです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="83ff0-108">キャッシュされたデータを分散するときに、データ:</span><span class="sxs-lookup"><span data-stu-id="83ff0-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="83ff0-109">*一貫性のある*複数のサーバーへの要求内では (一貫性)。</span><span class="sxs-lookup"><span data-stu-id="83ff0-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="83ff0-110">サーバーを再起動し、アプリの展開は存続します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="83ff0-111">ローカル メモリを使用しません。</span><span class="sxs-lookup"><span data-stu-id="83ff0-111">Doesn't use local memory.</span></span>

<span data-ttu-id="83ff0-112">分散キャッシュの構成は、特定の実装です。</span><span class="sxs-lookup"><span data-stu-id="83ff0-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="83ff0-113">この記事では、SQL Server を構成し、Redis cache を分散する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="83ff0-114">サード パーティ製の実装はなどに使用できるも[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github NCache](https://github.com/Alachisoft/NCache))。</span><span class="sxs-lookup"><span data-stu-id="83ff0-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="83ff0-115">どの実装を選択するに関係なく、アプリとキャッシュを使用して、対話、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="83ff0-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="83ff0-116">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="83ff0-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83ff0-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="83ff0-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="83ff0-118">SQL Server を使用する分散キャッシュを参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="83ff0-119">Redis を使用する分散キャッシュ、参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)にパッケージ参照を追加し、 [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="83ff0-120">Redis のパッケージに含まれていない、`Microsoft.AspNetCore.App`パッケージ化、Redis のパッケージの参照は、プロジェクト ファイル内とは別にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="83ff0-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="83ff0-121">SQL Server を使用する分散キャッシュを参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="83ff0-122">Redis を使用する分散キャッシュ、参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)にパッケージ参照を追加し、 [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="83ff0-123">Redis のパッケージに含まれていない、`Microsoft.AspNetCore.App`パッケージ化、Redis のパッケージの参照は、プロジェクト ファイル内とは別にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="83ff0-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="83ff0-124">IDistributedCache インターフェイス</span><span class="sxs-lookup"><span data-stu-id="83ff0-124">IDistributedCache interface</span></span>

<span data-ttu-id="83ff0-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイスには、分散キャッシュの実装でアイテムを操作する次のメソッドが用意されています。</span><span class="sxs-lookup"><span data-stu-id="83ff0-125">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="83ff0-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;文字列キーを受け取り、としてキャッシュされた項目を取得、`byte[]`配列の場合、キャッシュ内に存在します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="83ff0-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;項目を追加します (として`byte[]`配列) に文字列キーを使用してキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="83ff0-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="83ff0-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;そのスライド式有効期限のタイムアウトをリセットする (ある場合)、そのキーに基づいて、キャッシュ内の項目を更新します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="83ff0-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;文字列キーに基づいてキャッシュ アイテムを削除します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="83ff0-130">分散キャッシュ サービスを確立します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-130">Establish distributed caching services</span></span>

<span data-ttu-id="83ff0-131">実装を登録<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>で`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-131">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="83ff0-132">このトピックで説明されているフレームワークが提供の実装は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-132">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="83ff0-133">分散メモリ キャッシュ</span><span class="sxs-lookup"><span data-stu-id="83ff0-133">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="83ff0-134">SQL Server の分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="83ff0-134">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="83ff0-135">Redis cache の分散</span><span class="sxs-lookup"><span data-stu-id="83ff0-135">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="83ff0-136">分散メモリ キャッシュ</span><span class="sxs-lookup"><span data-stu-id="83ff0-136">Distributed Memory Cache</span></span>

<span data-ttu-id="83ff0-137">メモリの分散キャッシュ (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) framework 提供の実装は、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>メモリ内の項目を格納します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-137">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="83ff0-138">分散メモリ キャッシュでは、実際の分散キャッシュはありません。</span><span class="sxs-lookup"><span data-stu-id="83ff0-138">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="83ff0-139">アプリが実行されているサーバーで、アプリのインスタンスによってキャッシュされた項目が格納されます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-139">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="83ff0-140">メモリの分散キャッシュでは、便利な実装です。</span><span class="sxs-lookup"><span data-stu-id="83ff0-140">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="83ff0-141">で開発およびテスト シナリオ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-141">In development and testing scenarios.</span></span>
* <span data-ttu-id="83ff0-142">運用環境とメモリの消費量の 1 つのサーバーを使用する場合は問題になりません。</span><span class="sxs-lookup"><span data-stu-id="83ff0-142">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="83ff0-143">データ ストレージをキャッシュする分散メモリ キャッシュの抽象化を実装します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-143">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="83ff0-144">実装するためにより複数のノードまたはフォールト トレランスが必要になる場合、真の分散キャッシュ ソリューションの将来の場合。</span><span class="sxs-lookup"><span data-stu-id="83ff0-144">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="83ff0-145">サンプル アプリで開発環境でアプリを実行するときに、分散メモリ キャッシュの使用により、 `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="83ff0-145">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="83ff0-146">分散型の SQL Server のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="83ff0-146">Distributed SQL Server Cache</span></span>

<span data-ttu-id="83ff0-147">分散型の SQL Server キャッシュの実装 (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) バッキング ストアとして SQL Server データベースを使用する分散キャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-147">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="83ff0-148">SQL Server インスタンスで SQL Server のキャッシュされた項目のテーブルを作成するに使用することができます、`sql-cache`ツール。</span><span class="sxs-lookup"><span data-stu-id="83ff0-148">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="83ff0-149">ツールでは、指定したスキーマと名前でテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-149">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="83ff0-150">SQL server を実行してテーブルを作成、`sql-cache create`コマンド。</span><span class="sxs-lookup"><span data-stu-id="83ff0-150">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="83ff0-151">SQL Server インスタンスを提供 (`Data Source`)、データベース (`Initial Catalog`)、スキーマ (たとえば、 `dbo`) とテーブル名 (たとえば、 `TestCache`)。</span><span class="sxs-lookup"><span data-stu-id="83ff0-151">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="83ff0-152">ツールが成功したことを示すメッセージが記録されます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-152">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="83ff0-153">によって作成されたテーブル、`sql-cache`ツールには、次のスキーマ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-153">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer キャッシュ テーブル](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="83ff0-155">アプリのインスタンスを使用してキャッシュ値を操作する必要があります<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>ではなく、<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-155">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="83ff0-156">サンプル アプリの実装<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>非開発環境で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="83ff0-156">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="83ff0-157">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (および必要に応じて、<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>と<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) は、通常ソース管理の外部に格納 (によって格納されるなど、 [Secret Manager](xref:security/app-secrets)または*appsettings.json* /*appsettings します。{ENVIRONMENT} .json*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="83ff0-157">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="83ff0-158">接続文字列は、ソース管理システムから守る必要のある資格情報を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-158">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="83ff0-159">分散の Redis Cache</span><span class="sxs-lookup"><span data-stu-id="83ff0-159">Distributed Redis Cache</span></span>

<span data-ttu-id="83ff0-160">[Redis](https://redis.io/)は分散キャッシュとしてよく使用されているオープン ソース、メモリ内データ ストアです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-160">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="83ff0-161">Redis をローカルで使用することができ、構成することができます、 [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure でホストされる ASP.NET Core アプリ。</span><span class="sxs-lookup"><span data-stu-id="83ff0-161">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="83ff0-162">キャッシュ実装を使用して、アプリの構成、<xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache>インスタンス (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) 以外の開発環境で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="83ff0-162">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="83ff0-163">キャッシュ実装を使用して、アプリの構成、<xref:Microsoft.Extensions.Caching.Redis.RedisCache>インスタンス (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>)。</span><span class="sxs-lookup"><span data-stu-id="83ff0-163">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="83ff0-164">Redis をローカル コンピューターにインストールするには</span><span class="sxs-lookup"><span data-stu-id="83ff0-164">To install Redis on your local machine:</span></span>

* <span data-ttu-id="83ff0-165">インストール、 [Chocolatey Redis パッケージ](https://chocolatey.org/packages/redis-64/)します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-165">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="83ff0-166">実行`redis-server`コマンド プロンプトからです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-166">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="83ff0-167">分散キャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-167">Use the distributed cache</span></span>

<span data-ttu-id="83ff0-168">使用する、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイスのインスタンスを要求<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>、アプリで任意のコンス トラクターから。</span><span class="sxs-lookup"><span data-stu-id="83ff0-168">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="83ff0-169">によって、インスタンスが提供される[依存関係の注入 (DI)](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-169">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="83ff0-170">アプリの起動時、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>には挿入`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-170">When the app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="83ff0-171">使用して、現在の時刻にキャッシュされている<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>(詳細については、次を参照してください。 [Web ホスト。IApplicationLifetime インターフェイス](xref:fundamentals/host/web-host#iapplicationlifetime-interface))。</span><span class="sxs-lookup"><span data-stu-id="83ff0-171">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="83ff0-172">サンプル アプリは挿入<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>に、`IndexModel`インデックス ページで使用するためです。</span><span class="sxs-lookup"><span data-stu-id="83ff0-172">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="83ff0-173">インデックス ページが読み込まれるたびにキャッシュされる時間のキャッシュがチェック`OnGetAsync`します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-173">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="83ff0-174">キャッシュされる時間が期限切れで、時間が表示されます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-174">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="83ff0-175">前回のキャッシュ時間 (このページが読み込まれた最後の時刻) にアクセスした 20 秒が経過している場合、ページが表示されます*キャッシュされた期限切れ*します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-175">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="83ff0-176">選択して、現在の時刻にキャッシュされる時間をすぐに更新、**キャッシュされる時間のリセット**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="83ff0-176">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="83ff0-177">ボタン トリガー、`OnPostResetCachedTime`ハンドラー メソッド。</span><span class="sxs-lookup"><span data-stu-id="83ff0-177">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="83ff0-178">シングルトンまたはスコープ ベースの有効期間を使用する必要はありません<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インスタンス (少なくとも組み込み実装のため)。</span><span class="sxs-lookup"><span data-stu-id="83ff0-178">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="83ff0-179">作成することも、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 、DI を使用する代わりに 1 つにする必要がありますが、インスタンスを作成するコードでは、コードは厄介なをテストする任意の場所のインスタンスに違反して、 [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-179">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="83ff0-180">推奨事項</span><span class="sxs-lookup"><span data-stu-id="83ff0-180">Recommendations</span></span>

<span data-ttu-id="83ff0-181">実装を決定する際に<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>アプリに最適な次を検討してください。</span><span class="sxs-lookup"><span data-stu-id="83ff0-181">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="83ff0-182">既存のインフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="83ff0-182">Existing infrastructure</span></span>
* <span data-ttu-id="83ff0-183">パフォーマンスの要件</span><span class="sxs-lookup"><span data-stu-id="83ff0-183">Performance requirements</span></span>
* <span data-ttu-id="83ff0-184">コスト</span><span class="sxs-lookup"><span data-stu-id="83ff0-184">Cost</span></span>
* <span data-ttu-id="83ff0-185">チーム エクスペリエンス</span><span class="sxs-lookup"><span data-stu-id="83ff0-185">Team experience</span></span>

<span data-ttu-id="83ff0-186">キャッシュ ソリューションは通常、キャッシュされたデータの高速検索を提供する、インメモリ ストレージに依存して、メモリが制限されているリソースが、展開コストがかかります。</span><span class="sxs-lookup"><span data-stu-id="83ff0-186">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="83ff0-187">唯一のストアは、通常、データをキャッシュに使用されます。</span><span class="sxs-lookup"><span data-stu-id="83ff0-187">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="83ff0-188">一般に、Redis cache より高いスループットと SQL Server キャッシュよりも低い待機時間を提供します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-188">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="83ff0-189">ただし、ベンチマークは、通常はキャッシュ戦略のパフォーマンス特性を決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="83ff0-189">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="83ff0-190">分散キャッシュのバッキング ストアとして SQL Server を使用すると、キャッシュと、アプリの通常のデータ ストレージを使用して、同じデータベースの両方のパフォーマンスに悪影響を取得します。</span><span class="sxs-lookup"><span data-stu-id="83ff0-190">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="83ff0-191">バッキング ストアの分散キャッシュを専用の SQL Server インスタンスを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="83ff0-191">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83ff0-192">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="83ff0-192">Additional resources</span></span>

* [<span data-ttu-id="83ff0-193">Redis azure Cache</span><span class="sxs-lookup"><span data-stu-id="83ff0-193">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="83ff0-194">Azure 上の SQL データベース</span><span class="sxs-lookup"><span data-stu-id="83ff0-194">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="83ff0-195">[ASP.NET Core の Web ファームで NCache IDistributedCache プロバイダー](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github NCache](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="83ff0-195">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
