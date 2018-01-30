# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core 応答キャッシュのサンプル

このサンプルは、ASP.NET Core の使用方法を示しています。[応答キャッシュ ミドルウェア](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)です。

アプリは、インデックス ページで応答を含む、`Cache-Control`キャッシュ動作を構成するヘッダー。 また、アプリを設定、`Vary`機能の場合にのみ応答するようにキャッシュを構成するヘッダー、`Accept-Encoding`後続の要求のヘッダーと一致する元の要求。

サンプルを実行するときに、インデックス ページが格納され、最大 10 秒間にキャッシュ時にキャッシュから提供されます。
