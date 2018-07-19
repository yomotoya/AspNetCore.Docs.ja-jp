---
title: ASP.NET Core での分散キャッシュを使用します。
author: ardalis
description: アプリのパフォーマンスと特にクラウドまたはサーバー ファーム環境でのスケーラビリティを向上させるためにキャッシュされた分散 ASP.NET Core を使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123841"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="2b94e-103">ASP.NET Core での分散キャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="2b94e-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2b94e-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2b94e-105">分散キャッシュでは、クラウドまたはサーバー ファームでホストされている場合は特に、パフォーマンスと、ASP.NET Core アプリのスケーラビリティを向上できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="2b94e-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2b94e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="2b94e-107">分散キャッシュとは</span><span class="sxs-lookup"><span data-stu-id="2b94e-107">What is a distributed cache</span></span>

<span data-ttu-id="2b94e-108">分散キャッシュは、複数のアプリケーション サーバーで共有されます (を参照してください[Cache の基本](memory.md#caching-basics))。</span><span class="sxs-lookup"><span data-stu-id="2b94e-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="2b94e-109">キャッシュ内の情報は、個々 の web サーバーのメモリに格納されていないし、キャッシュされたデータはすべてのアプリのサーバーに使用できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="2b94e-110">これは、いくつかの利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-110">This provides several advantages:</span></span>

1. <span data-ttu-id="2b94e-111">キャッシュされたデータがすべての web サーバー上で生じます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="2b94e-112">ユーザーによっては、web サーバーは、要求を処理する別の結果に表示されません。</span><span class="sxs-lookup"><span data-stu-id="2b94e-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="2b94e-113">キャッシュされたデータは、web サーバーを再起動し、展開に存続します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="2b94e-114">個々 の web サーバーを削除またはキャッシュの影響を与えずに追加できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="2b94e-115">ソース データ ストアが、(より、複数のメモリ内キャッシュですべてのキャッシュ) に加えられた要求が減少します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="2b94e-116">場合は、SQL Server の分散キャッシュを使用して、これらの利点の一部は、アプリのソース データよりも、キャッシュの別のデータベース インスタンスを使用する場合のみ。</span><span class="sxs-lookup"><span data-stu-id="2b94e-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="2b94e-117">、任意のキャッシュのような分散キャッシュが飛躍的に向上、アプリの応答性、ため、通常、データはリレーショナル データベース (または web サービス) からよりも高速のキャッシュから取得できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="2b94e-118">キャッシュの構成は、特定の実装です。</span><span class="sxs-lookup"><span data-stu-id="2b94e-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="2b94e-119">この記事では、Redis 両方を構成して、SQL Server の分散キャッシュする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="2b94e-120">アプリが、一般的なを使用して、キャッシュと対話する実装の種類を選択するに関係なく`IDistributedCache`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2b94e-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="2b94e-121">IDistributedCache インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2b94e-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="2b94e-122">`IDistributedCache`インターフェイスには、同期および非同期のメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b94e-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="2b94e-123">インターフェイスは、追加、取得、および分散キャッシュの実装から削除する項目を使用します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="2b94e-124">`IDistributedCache`インターフェイスには、次のメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2b94e-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="2b94e-125">**Get、GetAsync**</span><span class="sxs-lookup"><span data-stu-id="2b94e-125">**Get, GetAsync**</span></span>

<span data-ttu-id="2b94e-126">文字列のキーを受け取り、としてキャッシュされた項目を取得、`byte[]`場合、キャッシュ内に存在します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="2b94e-127">**SetAsync セット**</span><span class="sxs-lookup"><span data-stu-id="2b94e-127">**Set, SetAsync**</span></span>

<span data-ttu-id="2b94e-128">項目を追加します (として`byte[]`) 文字列のキーを使用してキャッシュにします。</span><span class="sxs-lookup"><span data-stu-id="2b94e-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="2b94e-129">**RefreshAsync の更新**</span><span class="sxs-lookup"><span data-stu-id="2b94e-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="2b94e-130">そのスライド式有効期限のタイムアウトをリセットする (ある場合)、そのキーに基づいて、キャッシュ内の項目を更新します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="2b94e-131">**RemoveAsync を削除します。**</span><span class="sxs-lookup"><span data-stu-id="2b94e-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="2b94e-132">そのキーに基づくキャッシュ エントリを削除します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="2b94e-133">使用する、`IDistributedCache`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="2b94e-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="2b94e-134">必要な NuGet パッケージをプロジェクト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="2b94e-135">特定の実装を構成する`IDistributedCache`で、`Startup`クラスの`ConfigureServices`メソッドがコンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="2b94e-136">アプリの[ミドルウェア](xref:fundamentals/middleware/index)MVC コント ローラーのクラスのインスタンスを要求または`IDistributedCache`コンス トラクターから。</span><span class="sxs-lookup"><span data-stu-id="2b94e-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="2b94e-137">インスタンスがによって提供される[依存関係の注入](../../fundamentals/dependency-injection.md)(DI)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="2b94e-138">シングルトンまたはスコープ ベースの有効期間を使用する必要はありません`IDistributedCache`インスタンス (少なくとも組み込み実装のため)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="2b94e-139">いずれかが必要な場合がありますに、インスタンスを作成することもできます (使用する代わりに[依存関係の挿入](../../fundamentals/dependency-injection.md)) が、コードをテストするには、このことができますに違反して、 [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="2b94e-140">次の例は、のインスタンスを使用する方法を示しています。`IDistributedCache`では、単純なミドルウェア コンポーネント。</span><span class="sxs-lookup"><span data-stu-id="2b94e-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="2b94e-141">上記のコードでは、キャッシュされた値は、読み取りが書き込まれていません。</span><span class="sxs-lookup"><span data-stu-id="2b94e-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="2b94e-142">このサンプルで、値は、サーバーが起動し、変更されない場合にのみ設定されます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="2b94e-143">マルチ サーバーのシナリオで開始する直近のサーバーに他のサーバーで設定した前の値が上書きされます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="2b94e-144">`Get`と`Set`メソッドを使用して、`byte[]`型。</span><span class="sxs-lookup"><span data-stu-id="2b94e-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="2b94e-145">使用して文字列値を変換する必要がありますので、 `Encoding.UTF8.GetString` (の`Get`) と`Encoding.UTF8.GetBytes`(の`Set`)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="2b94e-146">次のコードから*Startup.cs*設定されている値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="2b94e-147">`IDistributedCache`で構成されている場合は、`ConfigureServices`メソッドが使用できる、`Configure`メソッドをパラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="2b94e-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="2b94e-148">パラメーターとして追加することによって、DI を通じて提供されるインスタンスを構成できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="2b94e-149">Redis の分散キャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="2b94e-150">[Redis](https://redis.io/)は分散キャッシュとしてよく使用されているオープン ソース、メモリ内データ ストアです。</span><span class="sxs-lookup"><span data-stu-id="2b94e-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="2b94e-151">ローカルで使用して、構成することができます、 [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure でホストされる ASP.NET Core アプリ。</span><span class="sxs-lookup"><span data-stu-id="2b94e-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="2b94e-152">キャッシュ実装を使用して、ASP.NET Core アプリの構成、`RedisDistributedCache`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2b94e-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="2b94e-153">Redis cache が必要です[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="2b94e-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="2b94e-154">Redis の実装を構成する`ConfigureServices`のインスタンスを要求することによって、アプリのコードでアクセス`IDistributedCache`(上記のコードを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="2b94e-155">サンプル コードで、`RedisCache`実装には、サーバーが構成されている場合、使用、`Staging`環境。</span><span class="sxs-lookup"><span data-stu-id="2b94e-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="2b94e-156">したがって、`ConfigureStagingServices`メソッドは、構成、 `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="2b94e-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="2b94e-157">ローカル コンピューターに Redis をインストールするには、chocolatey パッケージをインストール[ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/)実行`redis-server`コマンド プロンプトからです。</span><span class="sxs-lookup"><span data-stu-id="2b94e-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="2b94e-158">SQL Server を使用して分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2b94e-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="2b94e-159">SqlServerCache 実装では、バッキング ストアとして SQL Server データベースを使用する分散キャッシュを許可します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="2b94e-160">SQL サーバーを作成するには、ツール、sql キャッシュを使用するテーブルは指定した名前とスキーマ テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="2b94e-161">追加`SqlConfig.Tools`を`<ItemGroup>`要素は、プロジェクト ファイルと実行の`dotnet restore`します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="2b94e-162">次のコマンドを実行して SqlConfig.Tools をテストします。</span><span class="sxs-lookup"><span data-stu-id="2b94e-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="2b94e-163">SqlConfig.Tools では、使用状況、オプション、およびコマンドのヘルプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="2b94e-164">SQL server を実行してテーブルを作成、`sql-cache create`コマンド。</span><span class="sxs-lookup"><span data-stu-id="2b94e-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="2b94e-165">作成されたテーブルでは、次のスキーマがあります。</span><span class="sxs-lookup"><span data-stu-id="2b94e-165">The created table has the following schema:</span></span>

![SqlServer キャッシュ テーブル](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="2b94e-167">すべてのキャッシュ実装と同様に、アプリする必要がありますを取得および設定のインスタンスを使用してキャッシュ値`IDistributedCache`ではなく、`SqlServerCache`します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="2b94e-168">サンプルでは実装`SqlServerCache`実稼働環境で (で構成されているように`ConfigureProductionServices`)。</span><span class="sxs-lookup"><span data-stu-id="2b94e-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="2b94e-169">`ConnectionString` (および必要に応じて、`SchemaName`と`TableName`) 資格情報を含めることは、通常 (UserSecrets) などのソース管理の外部に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2b94e-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="2b94e-170">推奨事項</span><span class="sxs-lookup"><span data-stu-id="2b94e-170">Recommendations</span></span>

<span data-ttu-id="2b94e-171">実装を決定する際に`IDistributedCache`は Redis 間を選択して、アプリの右と、既存のインフラストラクチャと環境、パフォーマンス要件、およびチームの経験に基づいて、SQL Server。</span><span class="sxs-lookup"><span data-stu-id="2b94e-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="2b94e-172">チームがより快適な Redis の操作の場合は、優れた選択肢となります。</span><span class="sxs-lookup"><span data-stu-id="2b94e-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="2b94e-173">チームが SQL Server を希望する場合でも実装することを確信できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="2b94e-174">従来のキャッシュ ソリューションがデータをメモリ内データを迅速に取得できるを格納することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2b94e-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="2b94e-175">一般的に使用されるデータをキャッシュに格納し、SQL Server または Azure Storage などのバックエンドの永続的なストアにデータ全体を保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2b94e-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="2b94e-176">Redis Cache とは、SQL キャッシュと比較して、高スループットと低待機時間に提供するキャッシュ ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="2b94e-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b94e-177">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2b94e-177">Additional resources</span></span>

* [<span data-ttu-id="2b94e-178">Redis azure Cache</span><span class="sxs-lookup"><span data-stu-id="2b94e-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="2b94e-179">Azure 上の SQL データベース</span><span class="sxs-lookup"><span data-stu-id="2b94e-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
