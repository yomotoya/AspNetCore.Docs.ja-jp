# <a name="response-compression-sample-application-aspnet-core-1x"></a>応答の圧縮サンプル アプリケーション (ASP.NET Core 1.x)

このサンプルは、ASP.NET Core の使用方法を示します 1.x の HTTP 応答を圧縮する応答の圧縮のミドルウェア。 このサンプルでは、Gzip とテキストおよびイメージ応答の圧縮のカスタム プロバイダーを示していて、圧縮の MIME の種類を追加する方法を示します。 ASP.NET Core 2.x サンプルでは、次を参照してください。[応答の圧縮サンプル アプリケーション (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x)です。

## <a name="examples-in-this-sample"></a>このサンプルの例
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-927 バイトに圧縮する 2,044 バイトで Lorem Ipsum テキスト ファイルの応答
    * **/testfile1kb.txt** -47 バイトに圧縮する 1,033 バイトでテキスト ファイルの応答
    * **トリクル/** -1 秒間隔で 1 つの文字として発行される応答 
  * `image/svg+xml`
    * **/banner.svg** -4,459 バイトに圧縮する 9,707 バイトで A スケーラブル ベクター グラフィックス (SVG) のイメージの応答
* `CustomCompressionProvider`<br>ミドルウェアで使用するためのカスタム圧縮プロバイダーを実装する方法を示します

要求が含まれています、`Accept-Encoding`ヘッダー、サンプルは、追加、`Vary: Accept-Encoding`応答ヘッダー。 `Vary`ヘッダーの代替値に基づいて、応答の複数のコピーを維持するためにキャッシュするよう指示`Accept-Encoding`かシステムで、圧縮を受け入れるためのキャッシュに保存する (gzip) の圧縮と圧縮されていないバージョンの両方、または圧縮されていない応答です。

## <a name="using-the-sample"></a>サンプルの使用
1. 要求を使用して作成[Fiddler](http://www.telerik.com/fiddler)、 [Firebug](http://getfirebug.com/)、または[Postman](https://www.getpostman.com/)なしで、アプリケーションに、`Accept-Encoding`ヘッダーと応答のペイロード応答のサイズと応答ヘッダー。
2. 追加、`Accept-Encoding: gzip`ヘッダーおよび応答ヘッダーと圧縮された応答のサイズに注意してください。 削除するには、応答のサイズを参照してください、`Content-Encoding: gzip`サンプル アプリで応答ヘッダーが含まれます。 Lorem Ipsum の応答本文の検索または**testfile1kb.txt**応答、表示のテキストが圧縮、または読み取り不可能です。
3. 追加、`Accept-Encoding: mycustomcompression`ヘッダーおよび応答ヘッダーに注意してください。 `CustomCompressionProvider` 、応答は実際には圧縮されず、空の実装のラッパーでカスタム圧縮ストリームを作成することができますが、`CreateStream()`メソッドです。
