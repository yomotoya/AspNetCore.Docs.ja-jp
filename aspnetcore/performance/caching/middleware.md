---
title: "応答の ASP.NET Core のミドルウェアのキャッシュ"
author: guardrex
description: "構成および ASP.NET Core でのキャッシュ ミドルウェアの応答を使用する方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/middleware
ms.openlocfilehash: 78fa8fbe70eb7d6461b6e7340c6d57e330157911
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>応答の ASP.NET Core のミドルウェアのキャッシュ

によって[Luke Latham](https://github.com/guardrex)と[John Luo](https://github.com/JunTaoLuo)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

この記事では、ASP.NET Core アプリケーションの応答のキャッシュのミドルウェアを構成する方法について説明します。 ミドルウェアは、応答がキャッシュ可能な場合、ストア応答、およびキャッシュからの応答の機能を決定します。 HTTP キャッシュの概要について、`ResponseCache`属性は、「[応答のキャッシュ](xref:performance/caching/response)です。

## <a name="package"></a>Package

プロジェクトに含める、ミドルウェアへの参照を追加、 [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)パッケージまたはを使用して、 [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)パッケージ (ASP.NET Core 2.0 または .NET Core をターゲットとするときに後で)。

## <a name="configuration"></a>構成

`ConfigureServices`ミドルウェアをサービスのコレクションに追加します。

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

ミドルウェアを使用するアプリの構成、`UseResponseCaching`要求処理パイプラインにミドルウェアが追加される拡張メソッド。 サンプル アプリを追加、 [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)を 10 秒間キャッシュ可能な応答をキャッシュする応答ヘッダー。 サンプルでは送信、 [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)をキャッシュされた応答の場合にのみを処理するミドルウェアを構成するヘッダー、 [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)後続の要求のヘッダーの元の要求と一致します。

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

キャッシュ ミドルウェアの応答のみ 200 (OK) ステータス コードと、サーバーの応答をキャッシュします。 など、その他の応答[エラー ページ](xref:fundamentals/error-handling)、ミドルウェアによって無視されます。

> [!WARNING]
> 認証されたクライアント用コンテンツを含む応答を格納して、その応答の処理からミドルウェアを防ぐためにキャッシュ不可としてマークする必要があります。 参照してください[キャッシュ用の条件](#conditions-for-caching)ミドルウェアが応答がキャッシュ可能なかどうかを決定する方法の詳細。

## <a name="options"></a>オプション

ミドルウェアは、応答のキャッシュを制御する 3 つのオプションを提供します。

| オプション                | 既定値 |
| --------------------- | ------------- |
| UseCaseSensitivePaths | 大文字小文字を区別パスで応答がキャッシュされるかどうかを判断します。</p><p>既定値は `false` です。 |
| MaximumBodySize       | (バイト単位)、応答本文の最大キャッシュ サイズ。</p>既定値は`64 * 1024 * 1024`64 MB です。 |
| SizeLimit             | 応答キャッシュ ミドルウェア (バイト単位) のサイズの制限。 既定値は`100 * 1024 * 1024`100 MB です。 |

次の例では、ミドルウェアを構成します。

* 1,024 バイト以下の応答をキャッシュします。
* 大文字小文字を区別パスによって、応答を格納 (たとえば、`/page1`と`/Page1`別に保存されます)。

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

MVC または Web API コント ローラーまたは Razor ページのページのモデルを使用して、`ResponseCache`属性は、応答のキャッシュの適切なヘッダーを設定するために必要なパラメーターを指定します。 唯一のパラメーター、`ResponseCache`ミドルウェアを厳密に必要な属性が`VaryByQueryKeys`、実際の HTTP ヘッダーに対応するしません。 詳細については、次を参照してください。 [ResponseCache 属性](xref:performance/caching/response#responsecache-attribute)です。

使用していないときに、`ResponseCache`属性と変化する応答のキャッシュ、`VaryByQueryKeys`機能します。 使用して、`ResponseCachingFeature`から直接、`IFeatureCollection`の`HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>応答のキャッシュ ミドルウェアで使用される HTTP ヘッダー

ミドルウェアによって応答のキャッシュが構成されている HTTP ヘッダーを使用します。

| Header | 説明 |
| ------ | ------- |
| 承認 | ヘッダーが存在する場合、応答がキャッシュされません。 |
| Cache-Control | マークされた応答をキャッシュにのみ考慮ミドルウェア、`public`キャッシュ ディレクティブです。 次のパラメーターでキャッシュを制御します。<ul><li>max-age</li><li>max-stale&#8224;</li><li>min 新規</li><li>must-revalidate</li><li>no-cache</li><li>no ストア</li><li>専用の場合のキャッシュ</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; に制限が指定されていない場合`max-stale`ミドルウェアの動作は行いません。<br>&#8225;です。`proxy-revalidate`と同じ効果を持つ`must-revalidate`します。<br><br>詳細については、次を参照してください。 [RFC 7231: 要求のキャッシュ制御ディレクティブ](https://tools.ietf.org/html/rfc7234#section-5.2.1)です。 |
| プラグマ | A`Pragma: no-cache`要求のヘッダーと同じ効果を生成する`Cache-Control: no-cache`です。 このヘッダーは、関連するディレクティブによってオーバーライド、`Cache-Control`ヘッダーが存在する場合。 Http/1.0 との下位互換性と見なされます。 |
| Set-Cookie | ヘッダーが存在する場合、応答がキャッシュされません。 |
| 異なる | `Vary`ヘッダーがキャッシュされた応答を変更する別のヘッダーで使用されます。 などを含めることによってエンコードすることによって応答がキャッシュ、`Vary: Accept-Encoding`ヘッダーで要求に対する応答がキャッシュされているヘッダー`Accept-Encoding: gzip`と`Accept-Encoding: text/plain`とは別にします。 ヘッダーの値を含む応答`*`は保存されません。 |
| 有効期限が切れる | 格納されていない、または他のオーバーライドされない限りを取得このヘッダーで古いと見なされる応答`Cache-Control`ヘッダー。 |
| None-If-match | 値がない場合、完全な応答がキャッシュから提供された`*`と`ETag`応答の一致しない指定された値のいずれか。 それ以外の場合、304 (変更なし) の応答を処理します。 |
| If-Modified-Since | 場合、`If-None-Match`ヘッダーが存在しない、キャッシュされた応答の日付が指定された値よりも新しい場合、完全な応答がキャッシュから提供されます。 それ以外の場合、304 (変更なし) の応答を処理します。 |
| 日付 | キャッシュから提供するときに、`Date`元の応答で指定されていない場合、ミドルウェアによってヘッダーが設定されています。 |
| Content-Length | キャッシュから提供するときに、`Content-Length`元の応答で指定されていない場合、ミドルウェアによってヘッダーが設定されています。 |
| 経過時間 | `Age`元の応答で送信されるヘッダーは無視されます。 ミドルウェアは、キャッシュされた応答を提供するときに、新しい値を計算します。 |

## <a name="caching-respects-request-cache-control-directives"></a>要求キャッシュ制御ディレクティブを尊重キャッシュ

ミドルウェアがの規則を尊重、[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234#section-5.2)です。 規則に従う、有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。 仕様では、下にあるクライアントが要求を行うことができます、`no-cache`ヘッダーの値と force 要求ごとに新しい応答を生成するサーバー。 現時点ではありませんこのキャッシュ動作制御の開発者ミドルウェアが、公式のキャッシュの仕様に準拠しているため、ミドルウェアを使用する場合。

キャッシュの動作をより細かく制御は、ASP.NET Core のキャッシュの他の機能について説明します。 次のトピックを参照してください。

* [メモリ内キャッシュ](xref:performance/caching/memory)
* [分散キャッシュの使用](xref:performance/caching/distributed)
* [ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>トラブルシューティング

キャッシュ動作を期待どおりでない場合は、応答がキャッシュとキャッシュから提供されているに対応したことを確認します。 要求の受信ヘッダーおよび応答の送信ヘッダーを調べます。 有効にする[ログ](xref:fundamentals/logging/index)デバッグを支援します。

キャッシュの動作のトラブルシューティングと、テスト時に、ブラウザーは、望ましくない方法でキャッシュに影響する要求ヘッダーを設定可能性があります。 たとえば、ブラウザーを設定、`Cache-Control`ヘッダーを`no-cache`または`max-age=0`ページを更新するときにします。 次のツールは、要求ヘッダーを明示的に設定でき、キャッシュのテストに適しています。

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>キャッシュの条件

* 要求と、200 (OK) 状態コード、サーバーで応答する必要があります。
* 要求メソッドは、GET または HEAD でなければなりません。
* ターミナルのミドルウェアなど[静的ファイル ミドルウェア](xref:fundamentals/static-files)応答のキャッシュ ミドルウェアの前に、応答を処理する必要があります。
* `Authorization`ヘッダーを表示することはできません。
* `Cache-Control`ヘッダーのパラメーターが有効である必要があり、応答をマークする必要があります`public`マークされていないと`private`です。
* `Pragma: no-cache`ヘッダーを表示することはできない場合、`Cache-Control`ヘッダーは、として、存在しない、`Cache-Control`ヘッダーよりも優先、`Pragma`ヘッダーが存在する場合。
* `Set-Cookie`ヘッダーを表示することはできません。
* `Vary`ヘッダーのパラメーターが有効であり、等しくないにする必要があります`*`です。
* `Content-Length`ヘッダーの値 (場合に設定)、応答本文のサイズに一致する必要があります。
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)は使用されません。
* 応答で指定された古いすることはできません、`Expires`ヘッダーと`max-age`と`s-maxage`ディレクティブをキャッシュします。
* 成功すると、応答バッファー処理があり、応答のサイズが構成されているよりも小さくする必要がありますまたは既定`SizeLimit`です。
* 応答がキャッシュによるとする必要があります、 [RFC 7234](https://tools.ietf.org/html/rfc7234)仕様です。 たとえば、`no-store`ディレクティブが要求または応答のヘッダー フィールドに存在する必要があります。 参照してください*セクション 3: キャッシュに格納する応答*の[RFC 7234](https://tools.ietf.org/html/rfc7234)詳細についてはします。

> [!NOTE]
> クロスサイト リクエスト フォージェリ (CSRF) を防ぐためにセキュリティで保護されたトークンを生成するために Antiforgery システム セットの攻撃、`Cache-Control`と`Pragma`ヘッダーを`no-cache`応答がキャッシュされないようにします。

## <a name="additional-resources"></a>その他の技術情報

* [アプリケーションの起動](xref:fundamentals/startup)
* [ミドルウェア](xref:fundamentals/middleware/index)
* [メモリ内キャッシュ](xref:performance/caching/memory)
* [分散キャッシュの使用](xref:performance/caching/distributed)
* [変更トークンを使用する変更の検出](xref:fundamentals/primitives/change-tokens)
* [応答キャッシュ](xref:performance/caching/response)
* [キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
