---
title: "ASP.NET Core でのメモリ内キャッシュ"
author: rick-anderson
description: "ASP.NET Core でのメモリ内のデータをキャッシュする方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 7c6d629ea94dd7c79a2f4e24fd4d0ff797f7e516
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>ASP.NET Core でのメモリ内キャッシュ

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [John Luo](https://github.com/JunTaoLuo)、および[Steve Smith](https://ardalis.com/)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="caching-basics"></a>キャッシュの基礎

キャッシュと、パフォーマンスと、アプリのスケーラビリティが大幅に、コンテンツの生成に必要な作業を減らすことによって向上できます。 キャッシュの動作変更頻度が低いデータに最適です。 キャッシュで返すことができる多くのデータのコピー元のソースからよりも高速です。 キャッシュされたデータに依存しないアプリのテストを読み書きする必要があります。

ASP.NET Core では、いくつかの異なるキャッシュをサポートします。 最も単純なキャッシュがに基づいて、 [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)、web サーバーのメモリ内キャッシュを表します。 複数のサーバーのサーバー ファーム上で実行されるアプリは、メモリ内キャッシュを使用するときにセッションが固定であることを確認する必要があります。 スティッキー セッションでは、すべてのクライアントからの後続の要求が同じサーバーに送られることを確認します。 たとえば、Azure Web アプリで使用する[アプリケーション要求ルーティング処理](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) を同じサーバーにすべての後続の要求をルーティングします。

Web ファーム内の非スティッキー セッションが必要な[分散キャッシュ](distributed.md)キャッシュ整合性の問題を回避します。 アプリによっては、分散キャッシュはメモリ内キャッシュよりも高いスケール アウトをサポートできます。 分散キャッシュを使用して外部処理へのキャッシュ メモリの負荷を軽減します。 

`IMemoryCache`キャッシュがない限りにメモリ不足のキャッシュ エントリを削除し、[優先度をキャッシュ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority)に設定されている`CacheItemPriority.NeverRemove`です。 設定することができます、`CacheItemPriority`キャッシュがメモリ負荷状態にある項目を削除すると、優先度を調整します。

メモリ内キャッシュは、すべてのオブジェクトを格納できます。分散キャッシュ インターフェイスに制限されて`byte[]`です。

## <a name="using-imemorycache"></a>IMemoryCache を使用します。

メモリ内キャッシュは、*サービス*を使用して、アプリから参照される[依存性の注入](../../fundamentals/dependency-injection.md)です。 呼び出す`AddMemoryCache`で`ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

要求、`IMemoryCache`コンス トラクターでインスタンス。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

`IMemoryCache`NuGet パッケージ"Microsoft.Extensions.Caching.Memory"が必要です。

次のコードでは[TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)キャッシュに現在の時刻をチェックします。 新しいエントリが作成されとキャッシュに追加された場合は、項目がキャッシュされない、[設定](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_)です。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

現在の時刻とキャッシュされた時刻が表示されます。

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

キャッシュされた`DateTime`内でタイムアウト期間 (メモリ不足のためのない削除) 要求があるときに、値をキャッシュに残ります。 次の図は、キャッシュから取得した前の時刻と現在の時刻を示します。

![表示される 2 つの異なるタイミングでインデックス ビュー](memory/_static/time.png)

次のコードでは[GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)と[GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)データをキャッシュします。 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

次のコード呼び出し[取得](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)キャッシュの時間をフェッチします。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

参照してください[IMemoryCache メソッド](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)と[CacheExtensions メソッド](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions)キャッシュ方法の詳細についてはします。

## <a name="using-memorycacheentryoptions"></a>MemoryCacheEntryOptions を使用します。

次の例では:

- 絶対有効期限を設定します。 これは、エントリをキャッシュできる最大時間であり、アイテムがスライド式有効期限が継続的に更新されるときに古くなることを防ぎます。
- スライディング期限を設定します。 このキャッシュされたアイテムにアクセスするための要求は、スライド式有効期限の時間にリセットされます。
- キャッシュ優先度を設定`CacheItemPriority.NeverRemove`です。 
- セット、 [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate)するたびに呼び出されます、エントリがキャッシュから削除します。 キャッシュから項目を削除するコードから別のスレッドでコールバックが実行されます。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>キャッシュの依存関係

次の例では、依存するエントリの有効期限が切れた場合は、キャッシュ エントリを期限切れする方法を示します。 A`CancellationChangeToken`はキャッシュされたアイテムに追加します。 ときに`Cancel`で呼び出されると、 `CancellationTokenSource`、両方のキャッシュ エントリが削除されます。 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

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
