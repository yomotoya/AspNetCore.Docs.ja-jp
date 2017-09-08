---
title: "応答の ASP.NET Core のミドルウェアのキャッシュ"
author: guardrex
description: "構成および ASP.NET Core アプリケーションでキャッシュ ミドルウェアの応答を使用します。"
keywords: "ASP.NET Core 応答のキャッシュ、キャッシュ、ResponseCache、ResponseCaching、Cache-control、VaryByQueryKeys、ミドルウェア"
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.assetid: f9267eab-2762-42ac-1638-4a25d2c9d67c
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: 7790f38dda61eabd3cbbc6088ad455c07289b739
ms.sourcegitcommit: 70089de5bfd8ecd161261aa95faf07a4e1534cf8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/23/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a>応答の ASP.NET Core のミドルウェアのキャッシュ

によって[Luke Latham](https://github.com/GuardRex)と[John Luo](https://github.com/JunTaoLuo)

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples)

このドキュメントでは、ASP.NET Core アプリケーションの応答のキャッシュ ミドルウェアを構成する方法の詳細を説明します。 ミドルウェアは、応答がキャッシュ可能な場合、ストア応答、およびキャッシュからの応答の機能を決定します。 HTTP キャッシュの概要について、`ResponseCache`属性は、「[応答のキャッシュ](response.md)です。

## <a name="package"></a>Package
プロジェクトに含める、ミドルウェアへの参照を追加、 [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)パッケージまたはを使用して、 [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)パッケージです。

## <a name="configuration"></a>構成
`ConfigureServices`ミドルウェアをサービスのコレクションに追加します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

ミドルウェアを使用するアプリの構成、`UseResponseCaching`要求処理パイプラインにミドルウェアが追加される拡張メソッド。 サンプル アプリを追加、 [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)を 10 秒間キャッシュ可能な応答をキャッシュする応答ヘッダー。 サンプルでは送信、 [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)をキャッシュされた応答の場合にのみを処理するミドルウェアを構成するヘッダー、 [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)後続の要求のヘッダーの元の要求と一致します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]

---

応答のキャッシュ ミドルウェアのみ 200 (OK) サーバーの応答をキャッシュします。 など、その他の応答[エラー ページ](xref:fundamentals/error-handling)、ミドルウェアによって無視されます。

> [!WARNING]
> 認証されたクライアント用コンテンツを含む応答を格納して、その応答の処理からミドルウェアを防ぐためにキャッシュ不可としてマークする必要があります。 参照してください[キャッシュ用の条件](#conditions-for-caching)ミドルウェアが応答がキャッシュ可能なかどうかを決定する方法の詳細。

## <a name="options"></a>オプション
ミドルウェアは、応答のキャッシュを制御する 3 つのオプションを提供します。

| オプション                | 既定値 |
| --------------------- | ------------- |
| UseCaseSensitivePaths | 大文字小文字を区別パスで応答がキャッシュされるかどうかを判断します。</p><p>既定値は `false` です。 |
| MaximumBodySize       | (バイト単位)、応答本文の最大キャッシュ サイズ。</p>既定値は`64 * 1024 * 1024`64 MB です。 |
| SizeLimit             | 応答キャッシュ ミドルウェア (バイト単位) のサイズの制限。 既定値は`100 * 1024 * 1024`100 MB です。 |

次の例よりも小さいか等しい 1,024 のバイトへの応答を格納する、大文字小文字を区別のパスを使用して応答をキャッシュするミドルウェアを構成する`/page1`と`/Page1`とは別にします。

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys
MVC を使用する場合、`ResponseCache`属性は、応答のキャッシュの適切なヘッダーを設定するために必要なパラメーターを指定します。 唯一のパラメーター、`ResponseCache`ミドルウェアを厳密に必要な属性が`VaryByQueryKeys`、実際の HTTP ヘッダーに対応するしません。 詳細については、次を参照してください。 [ResponseCache 属性](response.md#responsecache-attribute)です。

MVC を使用していないときに変えることができますと応答のキャッシュ、`VaryByQueryKeys`機能します。 使用して、`ResponseCachingFeature`から直接、`IFeatureCollection`の`HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>応答のキャッシュ ミドルウェアで使用される HTTP ヘッダー
応答のミドルウェアによってキャッシュは、HTTP ヘッダーを使用して構成されます。 関連するヘッダーは、どのように影響キャッシュに関する注意事項を以下に示します。

| Header | 詳細 |
| ------ | ------- |
| 承認 | ヘッダーが存在する場合、応答がキャッシュされません。 |
| キャッシュ制御 | マークされた応答をキャッシュにのみ考慮ミドルウェア、`public`キャッシュ ディレクティブです。 次のパラメーターでキャッシュを制御できます。<ul><li>最大継続期間</li><li>最大値、古くなって & #8224 です。</li><li>min 新規</li><li>必須の再検証</li><li>キャッシュなし</li><li>no ストア</li><li>専用の場合のキャッシュ</li><li>private</li><li>public</li><li>s maxage</li><li>プロキシの再検証 & #8225 です。</li></ul>&#8224; に制限が指定されていない場合`max-stale`ミドルウェアの動作は行いません。<br>& #8225 です。`proxy-revalidate`と同じ効果を持つ`must-revalidate`します。<br><br>詳細については、次を参照してください。 [RFC 7231: 要求のキャッシュ制御ディレクティブ](https://tools.ietf.org/html/rfc7234#section-5.2.1)です。 |
| プラグマ | A`Pragma: no-cache`要求のヘッダーと同じ効果を生成する`Cache-Control: no-cache`です。 このヘッダーは、関連するディレクティブによってオーバーライド、`Cache-Control`ヘッダーが存在する場合。 Http/1.0 との下位互換性と見なされます。 |
| Set-cookie | ヘッダーが存在する場合、応答がキャッシュされません。 |
| 異なる | `Vary`ヘッダーがキャッシュされた応答を変更する別のヘッダーで使用されます。 含めることによってエンコードすることによって応答をキャッシュするなど、`Vary: Accept-Encoding`ヘッダーで要求に対する応答がキャッシュされているヘッダー`Accept-Encoding: gzip`と`Accept-Encoding: text/plain`とは別にします。 ヘッダーの値を含む応答`*`は保存されません。 |
| 有効期限が切れる | 格納されていない、または他のオーバーライドされない限りを取得このヘッダーで古いと見なされる応答`Cache-Control`ヘッダー。 |
| None-If-match | 値がない場合、完全な応答がキャッシュから提供された`*`と`ETag`応答の一致しない指定された値のいずれか。 それ以外の場合、304 (変更なし) の応答を処理します。 |
| 場合の変更-以降 | 場合、`If-None-Match`ヘッダーが存在しない、キャッシュされた応答の日付が指定された値よりも新しい場合、完全な応答がキャッシュから提供されます。 それ以外の場合、304 (変更なし) の応答を処理します。 |
| 日付 | キャッシュから提供するときに、`Date`元の応答で指定されていない場合、ミドルウェアによってヘッダーが設定されています。 |
| コンテンツの長さ | キャッシュから提供するときに、`Content-Length`元の応答で指定されていない場合、ミドルウェアによってヘッダーが設定されています。 |
| 経過時間 | `Age`元の応答で送信されるヘッダーは無視されます。 ミドルウェアは、キャッシュされた応答を提供するときに、新しい値を計算します。 |

## <a name="troubleshooting"></a>トラブルシューティング
キャッシュの動作が期待どおりでない場合は、応答がキャッシュを備え、確認するには、要求の受信ヘッダーおよび応答の送信ヘッダーがキャッシュから提供されていることを確認します。 有効にする[ログ](xref:fundamentals/logging)をデバッグするときに役立つことができます。 ミドルウェアは、キャッシュの動作および応答がキャッシュから取得されたときを記録します。

キャッシュの動作のトラブルシューティングと、テスト時に、ブラウザーは、望ましくない方法でキャッシュに影響する要求ヘッダーを設定可能性があります。 たとえば、ブラウザーを設定、`Cache-Control`ヘッダーを`no-cache`ページを更新するときにします。 次のツールは、要求ヘッダーを明示的に設定でき、キャッシュのテストに適しています。

* [Fiddler](http://www.telerik.com/fiddler)
* [また、firebug](http://getfirebug.com/)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>キャッシュの条件
* 要求と、サーバーから 200 (OK) 応答する必要があります。
* 要求メソッドは、GET または HEAD でなければなりません。
* 静的ファイル ミドルウェアなどの端末ミドルウェアは、応答のキャッシュ ミドルウェアの前に、応答を処理できません必要があります。
* `Authorization`ヘッダーを表示することはできません。
* `Cache-Control`ヘッダーのパラメーターが有効である必要があり、応答をマークする必要があります`public`マークされていないと`private`です。
* `Pragma: no-cache`ヘッダーと値を表示することはできない場合、`Cache-Control`ヘッダーは、として、存在しない、`Cache-Control`ヘッダーよりも優先、`Pragma`ヘッダーが存在する場合。
* `Set-Cookie`ヘッダーを表示することはできません。
* `Vary`ヘッダーのパラメーターが有効であり、等しくないにする必要があります`*`です。
* `Content-Length`ヘッダーの値 (場合に設定)、応答本文のサイズに一致する必要があります。
* `HttpSendFileFeature`は使用されません。
* 応答で指定された古いすることはできません、`Expires`ヘッダーと`max-age`と`s-maxage`ディレクティブをキャッシュします。
* 応答バッファー処理が完了すると、応答のサイズが、構成されたより小さくなるか既定の`SizeLimit`します。
* 応答がキャッシュによるとする必要があります、 [RFC 7234](https://tools.ietf.org/html/rfc7234)仕様です。 たとえば、`no-store`ディレクティブが要求または応答のヘッダー フィールドに存在する必要があります。 参照してください*セクション 3: キャッシュに格納する応答*の[RFC 7234](https://tools.ietf.org/html/rfc7234)詳細についてはします。

> [!NOTE]
> クロスサイト リクエスト フォージェリ (CSRF) を防ぐためにセキュリティで保護されたトークンを生成するために Antiforgery システム セットの攻撃、`Cache-Control`と`Pragma`ヘッダーを`no-cache`応答がキャッシュされないようにします。

## <a name="additional-resources"></a>その他の技術情報

* [アプリケーションの起動](xref:fundamentals/startup)
* [ミドルウェア](xref:fundamentals/middleware)
