# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>ASP.NET Core URL のサンプルを書き換え (ASP.NET 2.x のコア)

このサンプルは、ASP.NET Core の使用法を示しています。 2.x URL 書き換えミドルウェア。 アプリケーションは、URL リダイレクトと URL の書き換えのオプションを示します。 ASP.NET Core 1.x サンプルでは、次を参照してください。 [ASP.NET Core URL 書き換えサンプル (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x)です。

サンプルを実行するときに、規則のいずれかが適用されたときに要求 URL 書き換えられたまたはリダイレクト URL が表示、応答が提供されます。

## <a name="examples-in-this-sample"></a>このサンプルの例

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - 成功状態コード: 302 (検出)
  - 例 (リダイレクト): **/redirect-rule/{capture_group}**に**/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - 成功状態コード: 200 (OK)
  - 例 (書き換え): **/rewrite-rule/{capture_group_1}/{capture_group_2}**に**書き直す/? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - 成功状態コード: 302 (検出)
  - 例 (リダイレクト): **/apache-mod-rules-redirect/{capture_group}**に**リダイレクト/? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - 成功状態コード: 200 (OK)
  - 例 (書き換え): **/iis-rules-rewrite/{capture_group}**に**書き直す/? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - 成功状態コード: 301 (完全な移動)
  - 例 (リダイレクト): **/file.xml**に**/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - 成功状態コード: 301 (完全な移動)
  - 例 (リダイレクト): **/image.png**に**/png-images/image.png**
  - 例 (リダイレクト): **/image.jpg**に**/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>使用して、`PhysicalFileProvider`
取得することも、`IFileProvider`作成することで、`PhysicalFileProvider`に渡す、`AddApacheModRewrite()`と`AddIISUrlRewrite()`メソッド。
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>セキュリティで保護されたリダイレクトの拡張機能
このサンプルが含まれます`WebHostBuilder`Url を使用してアプリの構成 (**https://localhost:5001**、 **https://localhost**) および証明書のテスト (**testCert.pfx**) を支援するメソッドをリダイレクトするこれらの表示にします。 追加するように、`RewriteOptions()`で**Startup.cs**にその動作を調査します。

メソッド | 状態コード | ポート
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
