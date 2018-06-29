---
title: ASP.NET Core での分散キャッシュを使用します。
author: ardalis
description: アプリのパフォーマンスと特にクラウドまたはサーバー ファーム環境でのスケーラビリティを向上させるためにキャッシュされた分散 ASP.NET Core を使用する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 5ddc3a6927652f773ab38f93db1e222c5a1900b3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077700"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>ASP.NET Core での分散キャッシュを使用します。

作成者: [Steve Smith](https://ardalis.com/)

分散キャッシュでは、クラウドまたはサーバー ファーム環境でホストされている場合に特に ASP.NET Core アプリケーションのスケーラビリティとパフォーマンスを向上できます。 この記事では、ASP.NET Core の組み込みの分散キャッシュの抽象化と実装を操作する方法について説明します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-a-distributed-cache"></a>分散キャッシュとは

分散キャッシュが複数のアプリケーション サーバーで共有 (を参照してください[キャッシュ基本](memory.md#caching-basics))。 キャッシュ内の情報が個々 の web サーバーのメモリに格納されていないし、キャッシュされたデータはすべてのアプリのサーバーに使用できます。 これは、いくつかの利点を提供します。

1. キャッシュされたデータがすべての web サーバーに生じます。 ユーザーによっては、web サーバーは、要求を処理、異なる結果が表示されません。

2. キャッシュされたデータが回収されなかったは、web サーバーを再起動し展開します。 個々 の web サーバーを削除またはキャッシュに影響を与えずに追加することができます。

3. ソースのデータ ストアでは、(より、複数のメモリ内キャッシュまたはなしですべてのキャッシュ) に加えられた以下の要求があります。

> [!NOTE]
> SQL Server 分散キャッシュを使用する場合は、別のデータベース インスタンスは、アプリのソース データのよりも、キャッシュの使用は、true を返します。 これらの利点の一部はのみです。

、任意のキャッシュのように、分散キャッシュ大幅にアプリの応答性が向上、ので、通常のリレーショナル データベース (または web サービス) よりも高速のキャッシュからデータを取得できます。

キャッシュの構成は、特定の実装です。 この記事では、Redis 両方を構成して、SQL Server の分散キャッシュ方法について説明します。 アプリが共通を使用して、キャッシュと対話する実装の種類を選択するに関係なく`IDistributedCache`インターフェイスです。

## <a name="the-idistributedcache-interface"></a>IDistributedCache インターフェイス

`IDistributedCache`インターフェイスには、同期および非同期のメソッドが含まれています。 インターフェイスは、追加、取得、および実装では、分散キャッシュから削除する項目を使用します。 `IDistributedCache`インターフェイスには、次のメソッドが含まれています。

**Get、されます。**

文字列のキーは、としてキャッシュされたアイテムを取得、`byte[]`場合、キャッシュ内に存在します。

**セット、SetAsync**

項目を追加 (として`byte[]`) 文字列キーを使用してキャッシュします。

**更新、RefreshAsync**

スライド式有効期限タイムアウトをリセットする (存在する場合)、そのキーに基づく、キャッシュ内の項目を更新します。

**に対して removeasync を実行を削除します。**

そのキーに基づくキャッシュ エントリを削除します。

使用する、`IDistributedCache`インターフェイス。

   1. 必要な NuGet パッケージをプロジェクト ファイルに追加します。

   2. 特定の実装を構成する`IDistributedCache`で、`Startup`クラスの`ConfigureServices`メソッドがコンテナーに追加します。

   3. アプリから[ミドルウェア](xref:fundamentals/middleware/index)MVC コント ローラー クラスでは、要求のインスタンスまたは`IDistributedCache`コンス トラクターからです。 によって、インスタンスが提供されます[依存性の注入](../../fundamentals/dependency-injection.md)(DI)。

> [!NOTE]
> シングルトンまたはスコープ ベースの有効期間を使用する必要はありません`IDistributedCache`インスタンス (少なくとも組み込みの実装のため)。 必要に応じていずれかの場所にインスタンスを作成することも (を使用せずに[依存性の注入](../../fundamentals/dependency-injection.md))、これが難しく、コードをテストするには、違反している、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。

次の例は、のインスタンスを使用する方法を示しています。`IDistributedCache`では、単純なミドルウェア コンポーネント。

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

上記のコードでキャッシュされた値が、読み取りが書き込まれていません。 このサンプルでは、値は、サーバーの起動、および変更されない場合にのみ設定されます。 マルチ サーバーのシナリオで開始する最も最近使用したサーバーに他のサーバーで設定した前の値が上書きされます。 `Get`と`Set`メソッドを使用して、`byte[]`型です。 使用して文字列値を変換する必要がありますので、 `Encoding.UTF8.GetString` (の`Get`) および`Encoding.UTF8.GetBytes`(の`Set`)。

次のコードから*Startup.cs*設定される値を示しています。

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> `IDistributedCache`で構成された、`ConfigureServices`メソッドが使用できる、`Configure`メソッドのパラメーターとして。 パラメーターとして追加すると、DI で指定する構成済みのインスタンスが許可されます。

## <a name="using-a-redis-distributed-cache"></a>分散 Redis キャッシュの使用

[Redis](https://redis.io/)分散キャッシュとして使用される多くの場合、オープン ソースのインメモリ データ ストアです。 ローカルで行うこともでき、構成することができます、 [Azure Redis Cache](https://azure.microsoft.com/services/cache/) ASP.NET Core の Azure でホストされるアプリにします。 キャッシュ実装を使用して、ASP.NET Core アプリケーションの構成、`RedisDistributedCache`インスタンス。

Redis の実装を構成する`ConfigureServices`のインスタンスを要求することによって、アプリ コードでアクセスおよび`IDistributedCache`(上記のコードを参照してください)。

サンプル コードで、`RedisCache`のサーバーが構成されている実装を使用して、`Staging`環境。 したがって、`ConfigureStagingServices`メソッドは、構成、 `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> ローカル コンピューターに Redis をインストールするには、chocolatey パッケージをインストール[ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/)実行と`redis-server`コマンド プロンプトからです。

## <a name="using-a-sql-server-distributed-cache"></a>SQL Server を使用してキャッシュを分散

SqlServerCache 実装では、バッキング ストアとして SQL Server データベースを使用する分散キャッシュを許可します。 SQL サーバーを作成するには、ツール、sql キャッシュを使用するテーブルは指定した名前とスキーマを持つテーブルを作成します。

::: moniker range="< aspnetcore-2.1"

追加`SqlConfig.Tools`を`<ItemGroup>`実行して、プロジェクト ファイルの要素`dotnet restore`です。

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

次のコマンドを実行して SqlConfig.Tools をテストします。

```console
dotnet sql-cache create --help
```

SqlConfig.Tools は、使用状況、オプション、およびコマンドのヘルプを表示します。

SQL server を実行してテーブルを作成、`sql-cache create`コマンド。

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

作成されたテーブルには、次のスキーマがあります。

![SqlServer キャッシュ テーブル](distributed/_static/SqlServerCacheTable.png)

すべてのキャッシュ実装と同様にアプリを取得および設定のインスタンスを使用してキャッシュ値`IDistributedCache`ではなく、`SqlServerCache`です。 このサンプルを実装`SqlServerCache`実稼働環境で (で構成されているように`ConfigureProductionServices`)。

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (および必要に応じて、`SchemaName`と`TableName`) 資格情報を含めることは、通常 (UserSecrets) などのソース管理の外部に格納する必要があります。

## <a name="recommendations"></a>推奨事項

のどの実装を決定する際に`IDistributedCache`Redis 間を選択して、アプリの権利は、既存のインフラストラクチャと環境、パフォーマンス要件、およびチームの経験に基づいて、SQL Server、します。 チームがより快適な Redis の操作の場合は、最適な選択肢であります。 チームが SQL Server を希望する場合で同様に実装することを確信できます。 従来のキャッシュ ソリューションがデータをメモリ内データの取得を高速を格納することに注意してください。 一般的に使用されるデータをキャッシュに格納し、SQL Server または Azure ストレージなどのバックエンドの永続的なストアでデータ全体を格納する必要があります。 Redis Cache は、SQL キャッシュと比較して、高いスループットと低待機時間ににより、キャッシュ ソリューションです。

## <a name="additional-resources"></a>その他の技術情報

* [Redis の Azure キャッシュ](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure SQL データベース](https://azure.microsoft.com/documentation/services/sql-database/)
* [メモリ内キャッシュ](xref:performance/caching/memory)
* [変更トークンを使用する変更の検出](xref:fundamentals/primitives/change-tokens)
* [応答キャッシュ](xref:performance/caching/response)
* [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
* [キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
