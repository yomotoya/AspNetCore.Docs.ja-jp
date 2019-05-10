# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core の応答キャッシュのサンプル

このサンプルは、ASP.NET Core の使用方法を示します[応答キャッシュ ミドルウェア](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)します。

そのインデックス ページのアプリの応答を含む、`Cache-Control`ヘッダー キャッシュの動作を構成します。 また、アプリ、設定、`Vary`場合にのみ応答を処理するためにキャッシュを構成するヘッダー、`Accept-Encoding`を指定する元の要求からの後続の要求のヘッダーと一致します。

サンプルを実行するときに、インデックス ページは保存され、最大で 10 秒間キャッシュにキャッシュから提供されます。
