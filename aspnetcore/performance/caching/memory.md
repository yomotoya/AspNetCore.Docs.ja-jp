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
# <a name="cache-in-memory-in-aspnet-core"></a>ASP.NET Core のメモリ内のキャッシュします。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [John Luo](https://github.com/JunTaoLuo)、および[Steve Smith](https://ardalis.com/)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="caching-basics"></a>キャッシュの基礎

キャッシュと、パフォーマンスと、アプリのスケーラビリティが大幅に、コンテンツの生成に必要な作業を減らすことによって向上できます。 キャッシュの動作変更頻度が低いデータに最適です。 キャッシュで返すことができる多くのデータのコピー元のソースからよりも高速です。 キャッシュされたデータに依存しないアプリのテストを読み書きする必要があります。

ASP.NET Core では、いくつかの異なるキャッシュをサポートします。 最も単純なキャッシュがに基づいて、 [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)、web サーバーのメモリ内キャッシュを表します。 複数のサーバーのサーバー ファーム上で実行されるアプリは、メモリ内キャッシュを使用するときにセッションが固定であることを確認する必要があります。 スティッキー セッションでは、すべてのクライアントからの後続の要求が同じサーバーに送られることを確認します。 たとえば、Azure Web アプリで使用する[アプリケーション要求ルーティング処理](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) を同じサーバーにすべての後続の要求をルーティングします。

Web ファーム内の非スティッキー セッションが必要な[分散キャッシュ](distributed.md)キャッシュ整合性の問題を回避します。 アプリによっては、分散キャッシュはメモリ内キャッシュよりも高いスケール アウトをサポートできます。 分散キャッシュを使用して外部処理へのキャッシュ メモリの負荷を軽減します。

`IMemoryCache`キャッシュがない限りにメモリ不足のキャッシュ エントリを削除し、[優先度をキャッシュ](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)に設定されている`CacheItemPriority.NeverRemove`です。 設定することができます、`CacheItemPriority`キャッシュがメモリ負荷状態にある項目を削除すると、優先度を調整します。

メモリ内キャッシュは、すべてのオブジェクトを格納できます。分散キャッシュ インターフェイスに制限されて`byte[]`です。

## <a name="using-imemorycache"></a>IMemoryCache を使用します。

メモリ内キャッシュは、*サービス*を使用して、アプリから参照される[依存性の注入](../../fundamentals/dependency-injection.md)です。 呼び出す`AddMemoryCache`で`ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

要求、`IMemoryCache`コンス トラクターでインスタンス。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)です。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)、利用可能なは、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)です。

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` NuGet パッケージを必要と[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)、利用可能なは、 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)です。

::: moniker-end

次のコードでは[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)キャッシュ内で時間をチェックします。 新しいエントリが作成されとキャッシュに追加された場合は、時間がキャッシュされない、[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)です。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

現在の時刻とキャッシュされた時刻が表示されます。

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

キャッシュされた`DateTime`内でタイムアウト期間 (メモリ不足のためのない削除) 要求があるときに、値がキャッシュに保持します。 次の図は、キャッシュから取得される前の時刻と現在の時刻を示します。

![表示される 2 つの異なるタイミングでインデックス ビュー](memory/_static/time.png)

次のコードでは[GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)と[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)データをキャッシュします。 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

次のコード呼び出し[取得](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)キャッシュの時間をフェッチします。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

参照してください[IMemoryCache メソッド](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)と[CacheExtensions メソッド](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)キャッシュ方法の詳細についてはします。

## <a name="using-memorycacheentryoptions"></a>MemoryCacheEntryOptions を使用します。

次の例では:

- 絶対有効期限を設定します。 これは、エントリをキャッシュできる最大時間であり、アイテムがスライド式有効期限が継続的に更新されるときに古くなることを防ぎます。
- スライディング期限を設定します。 このキャッシュされたアイテムにアクセスするための要求は、スライド式有効期限の時間にリセットされます。
- キャッシュ優先度を設定`CacheItemPriority.NeverRemove`です。
- セット、 [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate)するたびに呼び出されます、エントリがキャッシュから削除します。 キャッシュから項目を削除するコードから別のスレッドでコールバックが実行されます。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a>キャッシュの依存関係

次の例では、依存するエントリの有効期限が切れた場合は、キャッシュ エントリを期限切れする方法を示します。 A`CancellationChangeToken`はキャッシュされたアイテムに追加します。 ときに`Cancel`で呼び出されると、 `CancellationTokenSource`、両方のキャッシュ エントリが削除されます。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

使用して、`CancellationTokenSource`グループとして削除する複数のキャッシュ エントリを許可します。 `using`内で作成された上記のコードでのパターン、キャッシュ エントリ、`using`ブロックはトリガーと有効期限の設定を継承します。

## <a name="additional-notes"></a>補足メモ

- コールバックを使用して、キャッシュ項目を再作成します。

  - コールバックが完了していないために、複数の要求には、キャッシュされたキーの値が空ことができます検索します。
  - これは、結果、複数のスレッドが、キャッシュされた項目を再作成します。

- 1 つのキャッシュ エントリを使用して、別に作成した場合、子は親エントリの有効期限切れのトークンと時間ベースの有効期限の設定をコピーします。 子は、手動で削除して期限切れまたは親エントリの更新はありません。

## <a name="additional-resources"></a>その他の技術情報

* [分散キャッシュの使用](xref:performance/caching/distributed)
* [変更トークンを使用する変更の検出](xref:fundamentals/primitives/change-tokens)
* [応答キャッシュ](xref:performance/caching/response)
* [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
* [キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
