# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>ASP.NET Core 応答のキャッシュのサンプル (ASP.NET のコア 1.x)

このサンプルは、ASP.NET Core の使用方法を示しています。[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ASP.NET Core と 1.x です。 ASP.NET Core 2.x サンプルでは、次を参照してください。 [ASP.NET Core 応答キャッシュのサンプル (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x)です。

アプリケーションは、送信、`Hello World!`メッセージと現在の時刻と組み合わせて、`Cache-Control`キャッシュ動作を構成するヘッダー。 アプリケーションはまた、送信、`Vary`機能の場合にのみ応答するようにキャッシュを構成するヘッダー、`Accept-Encoding`後続の要求のヘッダーと一致する元の要求。

サンプルを実行するときに、応答がキャッシュから提供可能および格納されているときに 10 秒間です。
