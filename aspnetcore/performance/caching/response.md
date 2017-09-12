---
title: "応答のキャッシュ"
author: rick-anderson
description: "応答の帯域幅を削減し、パフォーマンスを向上させるキャッシュの使用方法について説明します。"
keywords: "ASP.NET Core、応答の HTTP ヘッダーをキャッシュします。"
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 8b20ac70f257213ae3830749e729156ee5ab6447
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="response-caching"></a>応答のキャッシュ

によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Steve Smith](https://ardalis.com/)

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>応答のキャッシュとは

*応答のキャッシュ*応答をキャッシュに関連するヘッダーを追加します。 これらのヘッダーは、クライアント、プロキシとミドルウェアが応答をキャッシュする方法を指定します。 応答のキャッシュ クライアントまたはプロキシ web サーバーに、要求の数を減らすことができます。 応答のキャッシュ量を減らすことができますも作業の web サーバーを実行して応答を生成します。 

キャッシュに使用するプライマリ HTTP ヘッダーは`Cache-Control`します。 参照してください、 [HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234#section-5.2)詳細についてはします。 一般的なキャッシュのディレクティブ。

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [キャッシュなし](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [プラグマ](https://tools.ietf.org/html/rfc7234#section-5.4)
* [異なる](https://tools.ietf.org/html/rfc7231#section-7.1.4)

Web サーバーは、応答のミドルウェアのキャッシュを追加することで応答をキャッシュできます。 参照してください[応答のミドルウェアをキャッシュ](middleware.md)詳細についてはします。

## <a name="distributed-cache-tag-helper"></a>分散キャッシュ タグ ヘルパー

[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)有効分散キャッシュします。


## <a name="responsecache-attribute"></a>ResponseCache 属性

`ResponseCacheAttribute`応答のキャッシュでは、適切なヘッダーを設定するために必要なパラメーターを指定します。 参照してください[ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)パラメーターの詳細についてはします。

>[!WARNING]
> 認証されたクライアントに関する情報を含むコンテンツのキャッシュを無効にします。 ユーザーの id に基づいて、コンテンツが変化しない、または、ユーザーがログインしているかどうか、キャッシュを有効のみ必要があります。

`VaryByQueryKeys string[]`(ASP.NET Core 1.1.0 を必要と以降): 設定すると、応答のキャッシュ ミドルウェアによって異なります。 ストアド応答クエリ キーの指定された一覧の値。 設定するには、応答のミドルウェアのキャッシュを有効にする必要があります、`VaryByQueryKeys`プロパティ、それ以外の場合、ランタイム例外がスローされます。 対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティです。 このプロパティは、応答のミドルウェアのキャッシュによって処理される HTTP 機能です。 キャッシュされた応答を提供するミドルウェアをクエリ文字列とクエリ文字列の値必要がありますと一致前の要求。 たとえば、次のシーケンスがあるとします。

| 要求          | 結果 |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | サーバーから返される |
| `http://example.com?key1=value1` | ミドルウェアから返される |
| `http://example.com?key1=value2` | サーバーから返される |

最初の要求がサーバーによって返され、ミドルウェア内でキャッシュします。 2 番目の要求は、クエリ文字列が、前回の要求と一致するので、ミドルウェアによって返されます。 3 番目の要求ではありません、ミドルウェア キャッシュにクエリ文字列の値は、前の要求と一致しません。 

`ResponseCacheAttribute`構成を作成するために使用 (を介して`IFilterFactory`)、`ResponseCacheFilter`です。 `ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の処理を実行します。 フィルター:

* 既存のすべてのヘッダーを削除`Vary`、 `Cache-Control`、および`Pragma`です。 
* 設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`です。 
* 更新プログラムの応答の場合は、HTTP 機能をキャッシュ`VaryByQueryKeys`が設定されています。

### <a name="vary"></a>異なる

このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。 設定されている、`Vary`プロパティの値。 次のサンプルは、`VaryByHeader`プロパティです。

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

応答ヘッダーは、ブラウザー、ネットワーク ツールを使用して表示できます。 次の図は出力エッジ F12、**ネットワーク** タブのときに、`About2`アクション メソッドが更新されます。 

![F12 出力をエッジ、* * 'About2' アクション メソッドが呼び出されたときに * * ネットワーク タブ](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore と Location.None

`NoStore`ほとんどの他のプロパティをオーバーライドします。 このプロパティに設定するときに`true`、`Cache-Control`ヘッダーは"no store"に設定されます。 場合`Location`に設定されている`None`:

* `Cache-Control` が `"no-store, no-cache"` に設定されます。 
* `Pragma` が `no-cache` に設定されます。 

場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されます`no-cache`です。

通常は設定`NoStore`に`true`エラー ページにします。 例:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

これは、次のヘッダーで決定されます。

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置と長さ

キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定) または`Client`です。 ここで、`Cache-Control`ヘッダーは、場所の値の後の応答の"最大 age"に設定されます。

> [!NOTE]
> `Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。 前述のように、設定`Location`に`None`両方を設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`です。

次のヘッダーを示す例によって生成される設定`Duration`し、既定のままにして`Location`値。

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

次のヘッダーを生成します。

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>キャッシュ プロファイル

複製ではなく`ResponseCache`で MVC を設定するとき、オプションとしてコント ローラー アクションの多くの属性でキャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`です。 参照されているキャッシュ プロファイル内の値によって既定値として使用される、`ResponseCache`属性があり、この属性で指定したプロパティによってオーバーライドされます。

キャッシュ プロファイルを設定します。

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

キャッシュ プロファイルを参照するには。

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache` (メソッド) のアクションとコント ローラー (クラス) の両方に属性を適用できます。 メソッド レベルの属性は、クラス レベルの属性で指定された設定で上書きされます。

上記の例では、クラス レベルの属性は、メソッド レベルの属性は、継続時間が 60 秒に設定するキャッシュ プロファイルを参照中に 30 秒の長さを指定します。

結果として得られるヘッダー。

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>その他のリソース

* [指定から HTTP でのキャッシュ](https://tools.ietf.org/html/rfc7234#section-3)
* [キャッシュ制御](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
