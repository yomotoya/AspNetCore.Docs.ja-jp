# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>ASP.NET Core の URL リライト サンプル (ASP.NET Core 1.x)

このサンプルでは、ASP.NET Core 1.x URL リライト ミドルウェアの使用法を示します。 このアプリケーションは、URL リダイレクトと URL リライト オプションを示しています。 ASP.NET Core 2.x サンプルについては、「[ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x)」(ASP.NET Core の URL リライト サンプル (ASP.NET Core 2.x)) を参照してください。

このサンプルを実行すると、いずれかのルールが要求 URL に適用されるときに書き換えられる URL またはリダイレクトされる URL を示す応答が提供されます。

## <a name="examples-in-this-sample"></a>このサンプルの例

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - 成功状態コード: 302 (検出)
  - 例 (リダイレクト): **/redirect-rule/{capture_group}** から **/redirected/{capture_group}** へ
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - 成功状態コード: 200 (OK)
  - 例 (リライト): **/rewrite-rule/{capture_group_1}/{capture_group_2}** から **/rewritten?var1={capture_group_1}&var2={capture_group_2}** へ
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - 成功状態コード: 302 (検出)
  - 例 (リダイレクト): **/apache-mod-rules-redirect/{capture_group}** から **/redirected?id={capture_group}** へ
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - 成功状態コード: 200 (OK)
  - 例 (リライト): **/iis-rules-rewrite/{capture_group}** から **/rewritten?id={capture_group}** へ
* `Add(RedirectXMLRequests)`
  - 成功状態コード: 301 (完全な移動)
  - 例 (リダイレクト): **/file.xml** から **/xmlfiles/file.xml** へ
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - 成功状態コード: 301 (完全な移動)
  - 例 (リダイレクト): **/image.png** から **/png-images/image.png** へ
  - 例 (リダイレクト): **/image.jpg** から **/jpg-images/image.jpg** へ

## <a name="using-a-physicalfileprovider"></a>`PhysicalFileProvider` の使用
`AddApacheModRewrite()` メソッドと `AddIISUrlRewrite()` メソッドに渡す `PhysicalFileProvider` を作成して、`IFileProvider` を取得することもできます。
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>セキュリティで保護されたリダイレクトの拡張機能
このサンプルには、URL (**https://localhost:5001**、**https://localhost**) とテスト証明書 (**testCert.pfx**) を使用してこれらのリダイレクト メソッドを調べるための `WebHostBuilder` 構成が含まれています。 それらの動作を調べるには、そのいずれかを **Startup.cs** の `RewriteOptions()` に追加します。

メソッド | 状態コード | ポート
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
