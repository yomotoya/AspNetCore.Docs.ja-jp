---
title: ASP.NET Core で応答のキャッシュ
author: rick-anderson
description: 応答キャッシュを使用し、帯域幅要件を下げ、ASP.NET Core アプリのパフォーマンスを上げる方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/28/2019
uid: performance/caching/response
ms.openlocfilehash: 2e247dcff2cbaa3711a9206d7237a061ae351e1d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892469"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core で応答のキャッシュ

によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

応答のキャッシュ クライアントまたはプロキシは、web サーバーに要求の数が減少します。 応答のキャッシュ量が減少も、応答を生成する作業の web サーバーを実行します。 応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定するヘッダーによって制御されます。

[ResponseCache 属性](#responsecache-attribute)応答のキャッシュ クライアントが応答をキャッシュする場合に従うことがあります、ヘッダーの設定に参加します。 [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)サーバーの応答をキャッシュに使用できます。 ミドルウェアが使用できる<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>サーバー側のキャッシュ動作を決定するプロパティ。

## <a name="http-based-response-caching"></a>応答の HTTP ベースのキャッシュ

[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。 キャッシュに使用される主な HTTP ヘッダーが[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*します。 ディレクティブは、要求で利用できるようクライアントからサーバーへと応答に届けられますサーバーからクライアントに返送時のキャッシュ動作を制御します。 要求と応答は、プロキシ サーバーを介して移動し、プロキシ サーバーが HTTP 1.1 キャッシュの仕様に準拠する必要がありますもします。

一般的な`Cache-Control`ディレクティブは、次の表に表示されます。

| ディレクティブ                                                       | アクション |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | キャッシュは、応答を格納することができます。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 共有キャッシュで応答を格納する必要がありません。 プライベート キャッシュを格納および応答を再利用できます。 |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | クライアントでは、として、年齢が指定した秒数より大きい応答を受け入れません。 次に例を示します。 `max-age=60` (60 秒) `max-age=2592000` (1 か月) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **要求で**:キャッシュ要求を満たすためにストアド応答を使用する必要があります。 配信元サーバーがクライアントの場合、応答を再生成し、ミドルウェアがストアドのキャッシュに応答を更新します。<br><br>**応答に**:後続の要求、配信元サーバーの検証を行わずに、応答を使用しない必要があります。 |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **要求で**:キャッシュは、要求を格納する必要があります。<br><br>**応答に**:キャッシュは、応答の一部を格納する必要があります。 |

次の表は、キャッシュ内の役割を果たすその他のキャッシュ ヘッダーに表示されます。

| Header                                                     | 関数 |
| ---------------------------------------------------------- | -------- |
| [経過時間](https://tools.ietf.org/html/rfc7234#section-5.1)     | 生成された応答からの経過時間の推定値、または配信元サーバーに正常に検証します。 |
| [有効期限が切れます](https://tools.ietf.org/html/rfc7234#section-5.3) | 応答と見なされるまで時間が古い。 |
| [プラグマ](https://tools.ietf.org/html/rfc7234#section-5.4)  | Http/1.0 との互換性設定は、キャッシュ内を後方に向かって存在`no-cache`動作します。 場合、`Cache-Control`ヘッダーが存在する、`Pragma`ヘッダーは無視されます。 |
| [異なる](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドに、キャッシュされた応答の元の要求と、新しい要求の両方に一致します。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ

[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)を優先する有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。 クライアントがで要求を行うことができます、`no-cache`ヘッダーの値と force、サーバー要求のたびに新しい応答を生成します。

クライアントを常に受け入れること`Cache-Control`HTTP キャッシュの目的を検討する場合に要求ヘッダーを意味します。 公式の仕様では、キャッシュは、クライアント、プロキシ、およびサーバーのネットワーク要求を満たすの待機時間とネットワークのオーバーヘッドを削減するもの。 必ずしも、配信元サーバーの負荷を制御する方法はありません。

使用する場合、このキャッシュの動作に対する開発者のコントロールがない、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアは、公式のキャッシュの仕様に準拠しているためです。 [ミドルウェアの機能強化を計画](https://github.com/aspnet/AspNetCore/issues/2612)要求を無視するミドルウェアを構成する機会は`Cache-Control`ヘッダー キャッシュされた応答を処理するために決定する際にします。 計画的な機能強化では、管理サーバーの負荷を向上する機会を提供します。

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core でのキャッシュの他のテクノロジ

### <a name="in-memory-caching"></a>メモリ内キャッシュ

メモリ内キャッシュでは、サーバー メモリを使用して、キャッシュされたデータを格納します。 この種のキャッシュは 1 台のサーバーまたは複数のサーバーに適した*スティッキー セッション*します。 スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。

詳細については、「 <xref:performance/caching/memory> 」を参照してください。

### <a name="distributed-cache"></a>分散キャッシュ

分散キャッシュを使用して、アプリがクラウドまたはサーバー ファームでホストされている場合は、データをメモリに格納します。 キャッシュは、要求を処理するサーバー間で共有されます。 クライアントでは、クライアントのキャッシュされたデータが使用可能な場合は、グループ内のサーバーによって処理される要求を送信できます。 ASP.NET Core には、SQL Server 分散 Redis キャッシュが提供しています。

詳細については、「 <xref:performance/caching/distributed> 」を参照してください。

### <a name="cache-tag-helper"></a>キャッシュ タグ ヘルパー

キャッシュ タグ ヘルパーと MVC ビューまたは Razor ページからコンテンツをキャッシュします。 キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。

詳細については、「 <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper> 」を参照してください。

### <a name="distributed-cache-tag-helper"></a>分散キャッシュ タグ ヘルパー

分散キャッシュ タグ ヘルパーでは、分散クラウドまたは web ファームのシナリオでは、MVC ビューまたは Razor ページからのコンテンツをキャッシュします。 分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。

詳細については、「 <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper> 」を参照してください。

## <a name="responsecache-attribute"></a>ResponseCache 属性

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>応答のキャッシュで適切なヘッダーを設定するために必要なパラメーターを指定します。

> [!WARNING]
> 認証されたクライアントの情報を含むコンテンツのキャッシュを無効にします。 キャッシュは、ユーザーの id またはユーザーがサインインしているかどうかに基づいて変更されないコンテンツのみ有効にします。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> ストアドの応答は、クエリ キーの指定された一覧の値によって異なります。 1 つの値のときに`*`応答により、すべての要求クエリ文字列パラメーターが指定されて、ミドルウェアによって異なります。

[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)設定を有効にする必要があります、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>プロパティ。 それ以外の場合、ランタイム例外がスローされます。 対応する HTTP ヘッダーがない、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>プロパティ。 プロパティは、応答キャッシュ ミドルウェアによって処理される HTTP 機能です。 キャッシュされた応答を処理するためにミドルウェアのクエリ文字列とクエリ文字列の値は、前の要求に一致する必要があります。 たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。

| 要求                          | 結果                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | サーバーから返されます。 |
| `http://example.com?key1=value1` | ミドルウェアから返されます。 |
| `http://example.com?key1=value2` | サーバーから返されます。 |

最初の要求は、サーバーによって返され、ミドルウェアにキャッシュされています。 2 番目の要求は、クエリ文字列には、前の要求が一致するためにミドルウェアによって返されます。 3 番目の要求は、クエリ文字列の値は、前の要求と一致しないために、ミドルウェアのキャッシュではありません。

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>構成し、作成に使用されます (を使用して<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>)、<xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>します。 <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>適切な HTTP ヘッダーと応答の機能の更新の作業を実行します。 フィルター:

* 任意の既存のヘッダーを削除します。 `Vary`、 `Cache-Control`、および`Pragma`します。
* 設定されたプロパティに基づいて、適切なヘッダーを書き込みます、<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>します。
* 更新プログラムの応答の場合は、HTTP の機能をキャッシュ<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>設定されます。

### <a name="vary"></a>異なる

このヘッダーがのみ書き込まれるときに、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>プロパティを設定します。 プロパティに設定、`Vary`プロパティの値。 次のサンプルは、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>プロパティ。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

サンプル アプリを使用して、ブラウザーのネットワーク ツールを使用して、応答ヘッダーを表示します。 次の応答ヘッダーは、Cache1 ページ応答で送信されます。

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore と Location.None

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> ほとんどの他のプロパティをオーバーライドします。 このプロパティに設定しているときに`true`、`Cache-Control`にヘッダーが設定されている`no-store`します。 場合<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>に設定されている`None`:

* `Cache-Control` が `no-store,no-cache` に設定されます。
* `Pragma` が `no-cache` に設定されます。

場合<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>は`false`と<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>は`None`、 `Cache-Control`、および`Pragma`に設定されている`no-cache`します。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 設定されている通常`true`のエラー ページ。 サンプル アプリで Cache2 ページには、応答を格納しないようにクライアントに指示する応答ヘッダーが生成されます。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

サンプル アプリでは、次のヘッダーと共に Cache2 ページが返されます。

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>場所と期間

キャッシュを有効にする<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>正の値に設定する必要がありますと<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>いずれかである必要があります`Any`(既定値) または`Client`します。 ここで、`Cache-Control`ヘッダー後に場所の値に設定されて、`max-age`応答の。

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。 前述のように、設定<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>に`None`両方設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`します。

次の例では、サンプル アプリと設定されたヘッダーから Cache3 ページ モデル<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>し、既定のままにして<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>値。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

サンプル アプリでは、次のヘッダーを含む Cache3 ページが返されます。

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>キャッシュ プロファイル

多くのコント ローラー アクション属性の応答のキャッシュ設定ではなく、キャッシュ プロファイルとして構成できるオプションの MVC と Razor ページの設定時に`Startup.ConfigureServices`します。 値が参照されているキャッシュ プロファイルによって既定値として使用する、<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>は属性で指定された任意のプロパティでオーバーライドされるとします。

キャッシュ プロファイルを設定します。 次の例では、30 2 つ目のキャッシュ プロファイルを示しますが、サンプル アプリの`Startup.ConfigureServices`:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

サンプル アプリの Cache4 ページ モデルの参照、`Default30`プロファイルをキャッシュします。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>に適用できます。

* Razor ページ ハンドラー (クラス)&ndash;ハンドラー メソッドに属性を適用できません。
* MVC コント ローラー (クラス)。
* MVC アクション (メソッド)&ndash;メソッド レベルの属性がクラス レベルの属性で指定された設定をオーバーライドします。

結果として得られるヘッダーによって Cache4 ページの応答に適用される、`Default30`キャッシュ プロファイル。

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>その他の技術情報

* [応答をキャッシュに格納します。](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
