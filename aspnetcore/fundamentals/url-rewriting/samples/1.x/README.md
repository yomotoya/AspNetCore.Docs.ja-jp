# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a><span data-ttu-id="569b7-101">ASP.NET Core URL のサンプルを書き換え (ASP.NET のコア 1.x)</span><span class="sxs-lookup"><span data-stu-id="569b7-101">ASP.NET Core URL Rewriting Sample (ASP.NET Core 1.x)</span></span>

<span data-ttu-id="569b7-102">このサンプルは、ASP.NET Core の使用法を示しています。 1.x URL 書き換えミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="569b7-102">This sample illustrates usage of ASP.NET Core 1.x URL Rewriting Middleware.</span></span> <span data-ttu-id="569b7-103">アプリケーションは、URL リダイレクトと URL の書き換えのオプションを示します。</span><span class="sxs-lookup"><span data-stu-id="569b7-103">The application demonstrates URL redirect and URL rewriting options.</span></span> <span data-ttu-id="569b7-104">ASP.NET Core 2.x サンプルでは、次を参照してください。 [ASP.NET Core URL 書き換えサンプル (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x)です。</span><span class="sxs-lookup"><span data-stu-id="569b7-104">For the ASP.NET Core 2.x sample, see [ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).</span></span>

<span data-ttu-id="569b7-105">サンプルを実行するときに、規則のいずれかが適用されたときに要求 URL 書き換えられたまたはリダイレクト URL が表示、応答が提供されます。</span><span class="sxs-lookup"><span data-stu-id="569b7-105">When running the sample, a response will be served that shows the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="569b7-106">このサンプルの例</span><span class="sxs-lookup"><span data-stu-id="569b7-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - <span data-ttu-id="569b7-107">成功状態コード: 302 (検出)</span><span class="sxs-lookup"><span data-stu-id="569b7-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="569b7-108">例 (リダイレクト): **/redirect-rule/{capture_group}**に**/redirected/{capture_group}**</span><span class="sxs-lookup"><span data-stu-id="569b7-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="569b7-109">成功状態コード: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="569b7-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="569b7-110">例 (書き換え): **/rewrite-rule/{capture_group_1}/{capture_group_2}**に**書き直す/? var1 = {capture_group_1} & var2 = {capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="569b7-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="569b7-111">成功状態コード: 302 (検出)</span><span class="sxs-lookup"><span data-stu-id="569b7-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="569b7-112">例 (リダイレクト): **/apache-mod-rules-redirect/{capture_group}**に**リダイレクト/? id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="569b7-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="569b7-113">成功状態コード: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="569b7-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="569b7-114">例 (書き換え): **/iis-rules-rewrite/{capture_group}**に**書き直す/? id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="569b7-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXMLRequests)`
  - <span data-ttu-id="569b7-115">成功状態コード: 301 (完全な移動)</span><span class="sxs-lookup"><span data-stu-id="569b7-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="569b7-116">例 (リダイレクト): **/file.xml**に**/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="569b7-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="569b7-117">成功状態コード: 301 (完全な移動)</span><span class="sxs-lookup"><span data-stu-id="569b7-117">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="569b7-118">例 (リダイレクト): **/image.png**に**/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="569b7-118">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="569b7-119">例 (リダイレクト): **/image.jpg**に**/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="569b7-119">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="using-a-physicalfileprovider"></a><span data-ttu-id="569b7-120">使用して、`PhysicalFileProvider`</span><span class="sxs-lookup"><span data-stu-id="569b7-120">Using a `PhysicalFileProvider`</span></span>
<span data-ttu-id="569b7-121">取得することも、`IFileProvider`作成することで、`PhysicalFileProvider`に渡す、`AddApacheModRewrite()`と`AddIISUrlRewrite()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="569b7-121">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a><span data-ttu-id="569b7-122">セキュリティで保護されたリダイレクトの拡張機能</span><span class="sxs-lookup"><span data-stu-id="569b7-122">Secure redirection extensions</span></span>
<span data-ttu-id="569b7-123">このサンプルが含まれます`WebHostBuilder`Url を使用してアプリの構成 (**https://localhost:5001**、 **https://localhost**) および証明書のテスト (**testCert.pfx**) を支援するメソッドをリダイレクトするこれらの表示にします。</span><span class="sxs-lookup"><span data-stu-id="569b7-123">This sample includes `WebHostBuilder` configuration for the app to use URLs (**https://localhost:5001**, **https://localhost**) and a test certificate (**testCert.pfx**) to assist you in exploring these redirect methods.</span></span> <span data-ttu-id="569b7-124">追加するように、`RewriteOptions()`で**Startup.cs**にその動作を調査します。</span><span class="sxs-lookup"><span data-stu-id="569b7-124">Add any of them to the `RewriteOptions()` in **Startup.cs** to study their behavior.</span></span>

<span data-ttu-id="569b7-125">メソッド</span><span class="sxs-lookup"><span data-stu-id="569b7-125">Method</span></span> | <span data-ttu-id="569b7-126">状態コード</span><span class="sxs-lookup"><span data-stu-id="569b7-126">Status Code</span></span> | <span data-ttu-id="569b7-127">ポート</span><span class="sxs-lookup"><span data-stu-id="569b7-127">Port</span></span>
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | <span data-ttu-id="569b7-128">301</span><span class="sxs-lookup"><span data-stu-id="569b7-128">301</span></span> | <span data-ttu-id="569b7-129">null (465)</span><span class="sxs-lookup"><span data-stu-id="569b7-129">null (465)</span></span>
`.AddRedirectToHttps()` | <span data-ttu-id="569b7-130">302</span><span class="sxs-lookup"><span data-stu-id="569b7-130">302</span></span> | <span data-ttu-id="569b7-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="569b7-131">null (465)</span></span>
`.AddRedirectToHttps(301)` | <span data-ttu-id="569b7-132">301</span><span class="sxs-lookup"><span data-stu-id="569b7-132">301</span></span> | <span data-ttu-id="569b7-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="569b7-133">null (465)</span></span>
`.AddRedirectToHttps(301, 5001)` | <span data-ttu-id="569b7-134">301</span><span class="sxs-lookup"><span data-stu-id="569b7-134">301</span></span> | <span data-ttu-id="569b7-135">5001</span><span class="sxs-lookup"><span data-stu-id="569b7-135">5001</span></span>
