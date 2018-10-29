---
title: ASP.NET Core で応答のキャッシュ
author: rick-anderson
description: 応答キャッシュを使用し、帯域幅要件を下げ、ASP.NET Core アプリのパフォーマンスを上げる方法について説明します。
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207369"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core で応答のキャッシュ

によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Razor ページの応答のキャッシュは、ASP.NET Core 2.1 で使用できる以降です。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

応答のキャッシュ クライアントまたはプロキシは、web サーバーに要求の数が減少します。 応答のキャッシュ量が減少も、応答を生成する作業の web サーバーを実行します。 応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定するヘッダーによって制御されます。

追加すると、web サーバーが応答をキャッシュできます[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)します。

## <a name="http-based-response-caching"></a>応答の HTTP ベースのキャッシュ

[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。 キャッシュに使用される主な HTTP ヘッダーが[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*します。 ディレクティブは、要求で利用できるようクライアントからサーバーへと応答に届けられますサーバーからクライアントに返送時のキャッシュ動作を制御します。 要求と応答は、プロキシ サーバーを介して移動し、プロキシ サーバーが HTTP 1.1 キャッシュの仕様に準拠する必要がありますもします。

一般的な`Cache-Control`ディレクティブは、次の表に表示されます。

| ディレクティブ                                                       | アクション |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | キャッシュは、応答を格納することができます。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 共有キャッシュで応答を格納する必要がありません。 プライベート キャッシュを格納および応答を再利用できます。 |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | クライアントは応答として、年齢が指定した秒数より大きいを受け付けません。 例: `max-age=60` (60 秒) `max-age=2592000` (1 か月) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **要求で**: キャッシュでは、要求を満たすためストアド応答を使用する必要があります。 注: は、配信元サーバーがクライアントの場合、応答を再生成し、ミドルウェアがストアドのキャッシュに応答を更新します。<br><br>**応答に**: 後続の要求、配信元サーバーの検証を行わず、応答を使用しない必要があります。 |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **要求で**: キャッシュが要求を格納する必要があります。<br><br>**応答に**: キャッシュでは、応答の一部を格納する必要があります。 |

次の表は、キャッシュ内の役割を果たすその他のキャッシュ ヘッダーに表示されます。

| Header                                                     | 関数 |
| ---------------------------------------------------------- | -------- |
| [経過時間](https://tools.ietf.org/html/rfc7234#section-5.1)     | 生成された応答からの経過時間の推定値、または配信元サーバーに正常に検証します。 |
| [有効期限が切れます](https://tools.ietf.org/html/rfc7234#section-5.3) | その後、応答が古いと見なされます日付/時刻。 |
| [プラグマ](https://tools.ietf.org/html/rfc7234#section-5.4)  | Http/1.0 との互換性設定は、キャッシュ内を後方に向かって存在`no-cache`動作します。 場合、`Cache-Control`ヘッダーが存在する、`Pragma`ヘッダーは無視されます。 |
| [異なる](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドに、キャッシュされた応答の元の要求と、新しい要求の両方に一致します。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ

[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)を優先する有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。 クライアントがで要求を行うことができます、`no-cache`ヘッダーの値と force、サーバー要求のたびに新しい応答を生成します。

クライアントを常に受け入れること`Cache-Control`HTTP キャッシュの目的を検討する場合に要求ヘッダーを意味します。 公式の仕様では、キャッシュは、クライアント、プロキシ、およびサーバーのネットワーク要求を満たすの待機時間とネットワークのオーバーヘッドを削減するもの。 必ずしも、配信元サーバーの負荷を制御する方法はありません。

使用する場合、このキャッシュの動作に対する現在の開発者コントロールがない、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアは、公式のキャッシュの仕様に準拠しているためです。 [ミドルウェアは将来拡張](https://github.com/aspnet/ResponseCaching/issues/96)要求を無視するミドルウェアを構成するを許可する`Cache-Control`ヘッダー キャッシュされた応答を処理するために決定する際にします。 これが提供されますが、サーバーの管理、負荷を改善する機会、ミドルウェアを使用する場合。

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core でのキャッシュの他のテクノロジ

### <a name="in-memory-caching"></a>メモリ内キャッシュ

メモリ内キャッシュでは、サーバー メモリを使用して、キャッシュされたデータを格納します。 この種のキャッシュは 1 台のサーバーまたは複数のサーバーに適した*スティッキー セッション*します。 スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。

詳細については、次を参照してください。[メモリ内キャッシュ](xref:performance/caching/memory)します。

### <a name="distributed-cache"></a>分散キャッシュ

分散キャッシュを使用して、アプリがクラウドまたはサーバー ファームでホストされている場合は、データをメモリに格納します。 キャッシュは、要求を処理するサーバー間で共有されます。 クライアントでは、クライアントのキャッシュされたデータが使用可能な場合は、グループ内のサーバーによって処理される要求を送信できます。 ASP.NET Core には、SQL Server 分散 Redis キャッシュが提供しています。

詳細については、「 <xref:performance/caching/distributed> 」を参照してください。

### <a name="cache-tag-helper"></a>キャッシュ タグ ヘルパー

キャッシュ タグ ヘルパーでは、MVC ビューまたは Razor ページからコンテンツをキャッシュできます。 キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。

詳細については、次を参照してください。 [ASP.NET Core MVC でのキャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)します。

### <a name="distributed-cache-tag-helper"></a>分散キャッシュ タグ ヘルパー

分散キャッシュ タグ ヘルパーでは、MVC ビューまたは分散クラウドまたは web ファームのシナリオでの Razor ページからコンテンツをキャッシュできます。 分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。

詳細については、次を参照してください。[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)します。

## <a name="responsecache-attribute"></a>ResponseCache 属性

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)応答のキャッシュで適切なヘッダーを設定するために必要なパラメーターを指定します。

> [!WARNING]
> 認証されたクライアントの情報を含むコンテンツのキャッシュを無効にします。 キャッシュは、ユーザーの id またはユーザーがサインインしているかどうかに基づいて変更されないコンテンツのみ有効にします。

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)ストアドの応答を指定した一連のクエリ キーの値によって異なります。 1 つの値のときに`*`応答により、すべての要求クエリ文字列パラメーターが指定されて、ミドルウェアによって異なります。 `VaryByQueryKeys` ASP.NET Core 1.1 以降が必要です。

応答キャッシュ ミドルウェアは、設定を有効にする必要があります、`VaryByQueryKeys`プロパティ。 それ以外の場合、ランタイム例外がスローされます。 対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティ。 プロパティは、応答キャッシュ ミドルウェアによって処理される HTTP 機能です。 キャッシュされた応答を処理するためにミドルウェアのクエリ文字列とクエリ文字列の値は、前の要求に一致する必要があります。 たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。

| 要求                          | 結果                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | サーバーから返される     |
| `http://example.com?key1=value1` | ミドルウェアから返される |
| `http://example.com?key1=value2` | サーバーから返される     |

最初の要求は、サーバーによって返され、ミドルウェアにキャッシュされています。 2 番目の要求は、クエリ文字列には、前の要求が一致するためにミドルウェアによって返されます。 3 番目の要求は、クエリ文字列の値は、前の要求と一致しないために、ミドルウェアのキャッシュではありません。 

`ResponseCacheAttribute`構成し、作成に使用されます (を使用して`IFilterFactory`)、 [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)します。 `ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の作業を実行します。 フィルター:

* 任意の既存のヘッダーを削除します。 `Vary`、 `Cache-Control`、および`Pragma`します。 
* 設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`します。 
* 更新プログラムの応答の場合は、HTTP の機能をキャッシュ`VaryByQueryKeys`設定されます。

### <a name="vary"></a>異なる

このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。 設定されている、`Vary`プロパティの値。 次のサンプルは、`VaryByHeader`プロパティ。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

ブラウザーのネットワーク ツールを使用して、応答ヘッダーを表示できます。 次の図は、出力で Edge の f12 キー、**ネットワーク**ときにタブ、`About2`アクション メソッドが更新されます。

![About2 アクション メソッドが呼び出されたときに、[ネットワーク] タブで F12 出力をエッジします。](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore と Location.None

`NoStore` ほとんどの他のプロパティをオーバーライドします。 このプロパティに設定しているときに`true`、`Cache-Control`にヘッダーが設定されている`no-store`します。 場合`Location`に設定されている`None`:

* `Cache-Control` が `no-store,no-cache` に設定されます。
* `Pragma` が `no-cache` に設定されます。

場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されている`no-cache`します。

通常は設定`NoStore`に`true`エラー ページ。 例えば:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

これは、次のヘッダーが得られます。

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>場所と期間

キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定値) または`Client`します。 ここで、`Cache-Control`ヘッダー後に場所の値に設定されて、`max-age`応答の。

> [!NOTE]
> `Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。 前述のように、設定`Location`に`None`両方設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`します。

以下は、ヘッダーを示す例によって生成された設定`Duration`し、既定のままにして`Location`値。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

これには、次のヘッダーが生成されます。

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>キャッシュ プロファイル

複製ではなく`ResponseCache`で MVC をセットアップするときに、オプションとしてコント ローラー アクションの多くの属性、キャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`します。 値が参照されているキャッシュ プロファイルによって既定値として使用する、`ResponseCache`属性し、属性に指定したプロパティによってオーバーライドされます。

キャッシュ プロファイルの設定。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

キャッシュ プロファイルを参照するには。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

`ResponseCache`属性は、アクション (メソッド) とコント ローラー (クラス) の両方に適用できます。 メソッド レベルの属性は、クラス レベルの属性で指定された設定をオーバーライドします。

上記の例では、メソッド レベルの属性は、継続時間が 60 秒に設定キャッシュ プロファイルを参照中に、クラス レベルの属性は 30 秒の期間を指定します。

結果として得られるヘッダー:

```
Cache-Control: public,max-age=60
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
