---
title: "キャッシュ タグ ヘルパーの分散 |Microsoft ドキュメント"
author: pkellner
description: "キャッシュ タグ ヘルパーを使用する方法を示しています。"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: 2b260624fb2d85ab1a2625511397bcb4a85b6e77
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2017
---
# <a name="distributed-cache-tag-helper"></a>分散キャッシュ タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 


分散キャッシュ タグ ヘルパーは、分散キャッシュ ソースにそのコンテンツをキャッシュすることによって、ASP.NET Core アプリケーションのパフォーマンスを大幅に改善する機能を提供します。

分散キャッシュ タグ ヘルパーは、キャッシュ タグ ヘルパーと同じ基本クラスから継承します。  キャッシュ タグ ヘルパーに関連付けられているすべての属性は、分散タグ ヘルパーでも機能します。


分散キャッシュ タグ ヘルパーの後、**明示的な依存関係の原則**と呼ばれる**コンス トラクター インジェクション**です。  具体的には、`IDistributedCache`インターフェイス コンテナーは、分散キャッシュ タグ ヘルパーのコンス トラクターに渡されます。  場合のない特定の具象実装`IDistributedCache`内で作成された`ConfigureServices`、通常、startup.cs の分散キャッシュ タグ ヘルパーは基本のキャッシュ タグ ヘルパーとしてキャッシュされたデータを格納するため同じメモリ内のプロバイダーを使用します。

## <a name="distributed-cache-tag-helper-attributes"></a>分散キャッシュ タグ ヘルパー属性

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>有効期限が切れるの有効期限が切れた後に有効期限が切れる-スライディング有効になっているヘッダーの異なるクエリによって異なりますルートによって異なります cookie の異なるユーザーによって異なる別の優先順位

定義については、タグ ヘルパーのキャッシュを参照してください。 分散キャッシュ タグ ヘルパーは、これらすべての属性はキャッシュ タグ ヘルパーから一般的なので、キャッシュ タグ ヘルパーと同じクラスから継承します。

- - -

### <a name="name-required"></a>名前 (必須)

| 属性の型    | 値の例     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

必要な`name`属性は、分散キャッシュ タグ ヘルパーのインスタンスごとに格納されているキャッシュへのキーとして使用します。  Razor ページで、タグ ヘルパーの場所と Razor ページ名に基づいて各タグ ヘルパーのキャッシュ インスタンスをキーを代入する基本的なキャッシュ タグ ヘルパーとは異なり、分散キャッシュ タグ ヘルパーのみに基づいて、キーの属性`name`

使用例:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>キャッシュ タグ ヘルパー IDistributedCache 実装の分散

2 つの実装がある`IDistributedCache`ASP.NET Core に組み込まれています。  いずれかに基づきます**Sql Server** 、もう一方に基づいて**Redis**です。 これらの実装の詳細については、名前付き「での作業を分散キャッシュ」の下で参照されているリソースで確認できます。 どちらの実装では、インスタンスの設定が関係`IDistributedCache`ASP.NET Core で**startup.cs**です。

具体的にはの特定の実装を使用してに関連付けられているタグの属性がない`IDistributedCache`です。



- - -



## <a name="additional-resources"></a>その他の技術情報

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
