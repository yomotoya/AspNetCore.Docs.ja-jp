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
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>ASP.NET Core での分散キャッシュを使用します。

作成者: [Steve Smith](https://ardalis.com/)

分散キャッシュでは、クラウドまたはサーバー ファームでホストされている場合は特に、パフォーマンスと、ASP.NET Core アプリのスケーラビリティを向上できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="what-is-a-distributed-cache"></a>分散キャッシュとは

分散キャッシュは、複数のアプリケーション サーバーで共有されます (を参照してください[Cache の基本](memory.md#caching-basics))。 キャッシュ内の情報は、個々 の web サーバーのメモリに格納されていないし、キャッシュされたデータはすべてのアプリのサーバーに使用できます。 これは、いくつかの利点を提供します。

1. キャッシュされたデータがすべての web サーバー上で生じます。 ユーザーによっては、web サーバーは、要求を処理する別の結果に表示されません。

2. キャッシュされたデータは、web サーバーを再起動し、展開に存続します。 個々 の web サーバーを削除またはキャッシュの影響を与えずに追加できます。

3. ソース データ ストアが、(より、複数のメモリ内キャッシュですべてのキャッシュ) に加えられた要求が減少します。

> [!NOTE]
> 場合は、SQL Server の分散キャッシュを使用して、これらの利点の一部は、アプリのソース データよりも、キャッシュの別のデータベース インスタンスを使用する場合のみ。

、任意のキャッシュのような分散キャッシュが飛躍的に向上、アプリの応答性、ため、通常、データはリレーショナル データベース (または web サービス) からよりも高速のキャッシュから取得できます。

キャッシュの構成は、特定の実装です。 この記事では、Redis 両方を構成して、SQL Server の分散キャッシュする方法について説明します。 アプリが、一般的なを使用して、キャッシュと対話する実装の種類を選択するに関係なく`IDistributedCache`インターフェイス。

## <a name="the-idistributedcache-interface"></a>IDistributedCache インターフェイス

`IDistributedCache`インターフェイスには、同期および非同期のメソッドが含まれています。 インターフェイスは、追加、取得、および分散キャッシュの実装から削除する項目を使用します。 `IDistributedCache`インターフェイスには、次のメソッドが含まれています。

**Get、GetAsync**

文字列のキーを受け取り、としてキャッシュされた項目を取得、`byte[]`場合、キャッシュ内に存在します。

**SetAsync セット**

項目を追加します (として`byte[]`) 文字列のキーを使用してキャッシュにします。

**RefreshAsync の更新**

そのスライド式有効期限のタイムアウトをリセットする (ある場合)、そのキーに基づいて、キャッシュ内の項目を更新します。

**RemoveAsync を削除します。**

そのキーに基づくキャッシュ エントリを削除します。

使用する、`IDistributedCache`インターフェイス。

   1. 必要な NuGet パッケージをプロジェクト ファイルに追加します。

   2. 特定の実装を構成する`IDistributedCache`で、`Startup`クラスの`ConfigureServices`メソッドがコンテナーに追加します。

   3. アプリの[ミドルウェア](xref:fundamentals/middleware/index)MVC コント ローラーのクラスのインスタンスを要求または`IDistributedCache`コンス トラクターから。 インスタンスがによって提供される[依存関係の注入](../../fundamentals/dependency-injection.md)(DI)。

> [!NOTE]
> シングルトンまたはスコープ ベースの有効期間を使用する必要はありません`IDistributedCache`インスタンス (少なくとも組み込み実装のため)。 いずれかが必要な場合がありますに、インスタンスを作成することもできます (使用する代わりに[依存関係の挿入](../../fundamentals/dependency-injection.md)) が、コードをテストするには、このことができますに違反して、 [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/)。

次の例は、のインスタンスを使用する方法を示しています。`IDistributedCache`では、単純なミドルウェア コンポーネント。

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

上記のコードでは、キャッシュされた値は、読み取りが書き込まれていません。 このサンプルで、値は、サーバーが起動し、変更されない場合にのみ設定されます。 マルチ サーバーのシナリオで開始する直近のサーバーに他のサーバーで設定した前の値が上書きされます。 `Get`と`Set`メソッドを使用して、`byte[]`型。 使用して文字列値を変換する必要がありますので、 `Encoding.UTF8.GetString` (の`Get`) と`Encoding.UTF8.GetBytes`(の`Set`)。

次のコードから*Startup.cs*設定されている値が表示されます。

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

`IDistributedCache`で構成されている場合は、`ConfigureServices`メソッドが使用できる、`Configure`メソッドをパラメーターとして。 パラメーターとして追加することによって、DI を通じて提供されるインスタンスを構成できます。

## <a name="using-a-redis-distributed-cache"></a>Redis の分散キャッシュを使用します。

[Redis](https://redis.io/)は分散キャッシュとしてよく使用されているオープン ソース、メモリ内データ ストアです。 ローカルで使用して、構成することができます、 [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure でホストされる ASP.NET Core アプリ。 キャッシュ実装を使用して、ASP.NET Core アプリの構成、`RedisDistributedCache`インスタンス。

Redis cache が必要です[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)

Redis の実装を構成する`ConfigureServices`のインスタンスを要求することによって、アプリのコードでアクセス`IDistributedCache`(上記のコードを参照してください)。

サンプル コードで、`RedisCache`実装には、サーバーが構成されている場合、使用、`Staging`環境。 したがって、`ConfigureStagingServices`メソッドは、構成、 `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

ローカル コンピューターに Redis をインストールするには、chocolatey パッケージをインストール[ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/)実行`redis-server`コマンド プロンプトからです。

## <a name="using-a-sql-server-distributed-cache"></a>SQL Server を使用して分散キャッシュ

SqlServerCache 実装では、バッキング ストアとして SQL Server データベースを使用する分散キャッシュを許可します。 SQL サーバーを作成するには、ツール、sql キャッシュを使用するテーブルは指定した名前とスキーマ テーブルを作成します。

::: moniker range="< aspnetcore-2.1"

追加`SqlConfig.Tools`を`<ItemGroup>`要素は、プロジェクト ファイルと実行の`dotnet restore`します。

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

SqlConfig.Tools では、使用状況、オプション、およびコマンドのヘルプが表示されます。

SQL server を実行してテーブルを作成、`sql-cache create`コマンド。

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

作成されたテーブルでは、次のスキーマがあります。

![SqlServer キャッシュ テーブル](distributed/_static/SqlServerCacheTable.png)

すべてのキャッシュ実装と同様に、アプリする必要がありますを取得および設定のインスタンスを使用してキャッシュ値`IDistributedCache`ではなく、`SqlServerCache`します。 サンプルでは実装`SqlServerCache`実稼働環境で (で構成されているように`ConfigureProductionServices`)。

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (および必要に応じて、`SchemaName`と`TableName`) 資格情報を含めることは、通常 (UserSecrets) などのソース管理の外部に格納する必要があります。

## <a name="recommendations"></a>推奨事項

実装を決定する際に`IDistributedCache`は Redis 間を選択して、アプリの右と、既存のインフラストラクチャと環境、パフォーマンス要件、およびチームの経験に基づいて、SQL Server。 チームがより快適な Redis の操作の場合は、優れた選択肢となります。 チームが SQL Server を希望する場合でも実装することを確信できます。 従来のキャッシュ ソリューションがデータをメモリ内データを迅速に取得できるを格納することに注意してください。 一般的に使用されるデータをキャッシュに格納し、SQL Server または Azure Storage などのバックエンドの永続的なストアにデータ全体を保存する必要があります。 Redis Cache とは、SQL キャッシュと比較して、高スループットと低待機時間に提供するキャッシュ ソリューションです。

## <a name="additional-resources"></a>その他の技術情報

* [Redis azure Cache](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure 上の SQL データベース](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
