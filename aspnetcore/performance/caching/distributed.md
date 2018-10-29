---
title: ASP.NET Core でのキャッシュを分散
author: guardrex
description: アプリのパフォーマンスと特にクラウドまたはサーバー ファーム環境でのスケーラビリティを向上させるために、ASP.NET Core の分散キャッシュを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/19/2018
uid: performance/caching/distributed
ms.openlocfilehash: 37806cc5c8da115f6a95fdad5ccc716d6375cb6e
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206250"
---
# <a name="distributed-caching-in-aspnet-core"></a>ASP.NET Core でのキャッシュを分散

作成者: [Steve Smith](https://ardalis.com/)、[Luke Latham](https://github.com/guardrex)

分散キャッシュは、通常、外部サービスにアクセスするアプリケーション サーバーとして保守管理されて複数のアプリ サーバーによって共有キャッシュです。 分散キャッシュでは、アプリがクラウド サービスまたはサーバー ファームでホストされている場合に特に、パフォーマンスと ASP.NET Core アプリのスケーラビリティを向上できます。

分散キャッシュは、個々 のアプリのサーバーでのキャッシュされたデータの格納の他のキャッシュ シナリオのいくつかのメリットです。

キャッシュされたデータを分散するときに、データ:

* *一貫性のある*複数のサーバーへの要求内では (一貫性)。
* サーバーを再起動し、アプリの展開は存続します。
* ローカル メモリを使用しません。

分散キャッシュの構成は、特定の実装です。 この記事では、SQL Server を構成し、Redis cache を分散する方法について説明します。 サード パーティ製の実装はなどに使用できるも[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github NCache](https://github.com/Alachisoft/NCache))。 どの実装を選択するに関係なく、アプリとキャッシュを使用して、対話、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイス。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

::: moniker range=">= aspnetcore-2.1"

SQL Server を使用する分散キャッシュを参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)パッケージ。

Redis を使用する分散キャッシュ、参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)にパッケージ参照を追加し、 [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)パッケージ。 Redis のパッケージに含まれていない、`Microsoft.AspNetCore.App`パッケージ化、Redis のパッケージの参照は、プロジェクト ファイル内とは別にする必要があります。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

SQL Server を使用する分散キャッシュ、参照、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)へのパッケージ参照を追加したり、 [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)パッケージ。

Redis を使用する分散キャッシュ、参照、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)へのパッケージ参照を追加したり、 [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)パッケージ。 Redis パッケージが含まれている`Microsoft.AspNetCore.All`パッケージ化、Redis のパッケージをプロジェクト ファイル内で個別に参照する必要はありません。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

SQL Server を使用する分散キャッシュ、パッケージ参照を追加、 [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)パッケージ。

分散キャッシュ、Redis を使用する、パッケージ参照を追加、 [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)パッケージ。

::: moniker-end

## <a name="idistributedcache-interface"></a>IDistributedCache インターフェイス

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイスには、分散キャッシュの実装でアイテムを操作する次のメソッドが用意されています。

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;文字列キーを受け取り、としてキャッシュされた項目を取得、`byte[]`配列の場合、キャッシュ内に存在します。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;項目を追加します (として`byte[]`配列) に文字列キーを使用してキャッシュします。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;そのスライド式有効期限のタイムアウトをリセットする (ある場合)、そのキーに基づいて、キャッシュ内の項目を更新します。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>、 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;文字列キーに基づいてキャッシュ アイテムを削除します。

## <a name="establish-distributed-caching-services"></a>分散キャッシュ サービスを確立します。

実装を登録<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>で`Startup.ConfigureServices`します。 このトピックで説明されているフレームワークが提供の実装は次のとおりです。

* [分散メモリ キャッシュ](#distributed-memory-cache)
* [SQL Server の分散キャッシュ](#distributed-sql-server-cache)
* [Redis cache の分散](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>分散メモリ キャッシュ

メモリの分散キャッシュ (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) framework 提供の実装は、`IDistributedCache`メモリ内の項目を格納します。 分散メモリ キャッシュでは、実際の分散キャッシュはありません。 アプリが実行されているサーバーで、アプリのインスタンスによってキャッシュされた項目が格納されます。

メモリの分散キャッシュでは、便利な実装です。

* で開発およびテスト シナリオ。
* 運用環境とメモリの消費量の 1 つのサーバーを使用する場合は問題になりません。 データ ストレージをキャッシュする分散メモリ キャッシュの抽象化を実装します。 実装するためにより複数のノードまたはフォールト トレランスが必要になる場合、真の分散キャッシュ ソリューションの将来の場合。

サンプル アプリは、アプリの開発環境で実行するときに、分散メモリ キャッシュの使用します。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>分散型の SQL Server のキャッシュ

分散型の SQL Server キャッシュの実装 (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) バッキング ストアとして SQL Server データベースを使用する分散キャッシュを使用します。 SQL Server インスタンスで SQL Server のキャッシュされた項目のテーブルを作成するに使用することができます、`sql-cache`ツール。 ツールでは、指定したスキーマと名前でテーブルを作成します。

::: moniker range="< aspnetcore-2.1"

追加`SqlConfig.Tools`を`<ItemGroup>`要素は、プロジェクト ファイルと実行の`dotnet restore`します。

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

SQL server を実行してテーブルを作成、`sql-cache create`コマンド。 SQL Server インスタンスを提供 (`Data Source`)、データベース (`Initial Catalog`)、スキーマ (たとえば、 `dbo`) とテーブル名 (たとえば、 `TestCache`)。

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

ツールが成功したことを示すメッセージが記録されます。

```console
Table and index were created successfully.
```

によって作成されたテーブル、`sql-cache`ツールには、次のスキーマ。

![SqlServer キャッシュ テーブル](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> アプリのインスタンスを使用してキャッシュ値を操作する必要があります<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>ではなく、<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>します。

サンプル アプリの実装<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>非開発環境で。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (および必要に応じて、<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>と<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) は、通常ソース管理の外部に格納 (によって格納されるなど、 [Secret Manager](xref:security/app-secrets)または*appsettings.json* /*appsettings します。{Environment} .json*ファイル)。 接続文字列は、ソース管理システムから守る必要のある資格情報を含めることができます。

### <a name="distributed-redis-cache"></a>分散の Redis Cache

[Redis](https://redis.io/)は分散キャッシュとしてよく使用されているオープン ソース、メモリ内データ ストアです。 Redis をローカルで使用することができ、構成することができます、 [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure でホストされる ASP.NET Core アプリ。 キャッシュ実装を使用して、アプリの構成、<xref:Microsoft.Extensions.Caching.Redis.RedisCache>インスタンス (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>)。

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

Redis をローカル コンピューターにインストールするには

* インストール、 [Chocolatey Redis パッケージ](https://chocolatey.org/packages/redis-64/)します。
* 実行`redis-server`コマンド プロンプトからです。

## <a name="use-the-distributed-cache"></a>分散キャッシュを使用します。

使用する、<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>インターフェイスのインスタンスを要求`IDistributedCache`、アプリで任意のコンス トラクターから。 によって、インスタンスが提供される[依存関係の注入 (DI)](xref:fundamentals/dependency-injection)します。

アプリの起動時、`IDistributedCache`には挿入`Startup.Configure`します。 使用して、現在の時刻にキャッシュされている<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>(詳細については、次を参照してください。 [Web ホスト: IApplicationLifetime インターフェイス](xref:fundamentals/host/web-host#iapplicationlifetime-interface))。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

サンプル アプリは挿入`IDistributedCache`に、`IndexModel`インデックス ページで使用するためです。

インデックス ページが読み込まれるたびにキャッシュされる時間のキャッシュがチェック`OnGetAsync`します。 キャッシュされる時間が期限切れで、時間が表示されます。 前回のキャッシュ時間 (このページが読み込まれた最後の時刻) にアクセスした 20 秒が経過している場合、ページが表示されます*キャッシュされた期限切れ*します。

選択して、現在の時刻にキャッシュされる時間をすぐに更新、**キャッシュされる時間のリセット**ボタンをクリックします。 ボタン トリガー、`OnPostResetCachedTime`ハンドラー メソッド。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> シングルトンまたはスコープ ベースの有効期間を使用する必要はありません`IDistributedCache`インスタンス (少なくとも組み込み実装のため)。
>
> 作成することも、 `IDistributedCache` 、DI を使用する代わりに 1 つにする必要がありますが、インスタンスを作成するコードでは、コードは厄介なをテストする任意の場所のインスタンスに違反して、 [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)します。

## <a name="recommendations"></a>推奨事項

実装を決定する際に`IDistributedCache`アプリに最適な次を検討してください。

* 既存のインフラストラクチャ
* パフォーマンスの要件
* コスト
* チーム エクスペリエンス

キャッシュ ソリューションは通常、キャッシュされたデータの高速検索を提供する、インメモリ ストレージに依存して、メモリが制限されているリソースが、展開コストがかかります。 唯一のストアは、通常、データをキャッシュに使用されます。

一般に、Redis cache より高いスループットと SQL Server キャッシュよりも低い待機時間を提供します。 ただし、ベンチマークは、通常はキャッシュ戦略のパフォーマンス特性を決定する必要があります。

分散キャッシュのバッキング ストアとして SQL Server を使用すると、キャッシュと、アプリの通常のデータ ストレージを使用して、同じデータベースの両方のパフォーマンスに悪影響を取得します。 バッキング ストアの分散キャッシュを専用の SQL Server インスタンスを使用することをお勧めします。

## <a name="additional-resources"></a>その他の技術情報

* [Redis azure Cache](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure 上の SQL データベース](https://azure.microsoft.com/documentation/services/sql-database/)
* [ASP.NET Core の Web ファームで NCache IDistributedCache プロバイダー](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github NCache](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
