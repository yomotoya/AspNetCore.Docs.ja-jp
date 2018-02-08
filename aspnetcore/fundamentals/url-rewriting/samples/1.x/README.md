# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a><span data-ttu-id="52758-101">ASP.NET Core の URL リライト サンプル (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="52758-101">ASP.NET Core URL Rewriting Sample (ASP.NET Core 1.x)</span></span>

<span data-ttu-id="52758-102">このサンプルでは、ASP.NET Core 1.x URL リライト ミドルウェアの使用法を示します。</span><span class="sxs-lookup"><span data-stu-id="52758-102">This sample illustrates usage of ASP.NET Core 1.x URL Rewriting Middleware.</span></span> <span data-ttu-id="52758-103">このアプリケーションは、URL リダイレクトと URL リライト オプションを示しています。</span><span class="sxs-lookup"><span data-stu-id="52758-103">The application demonstrates URL redirect and URL rewriting options.</span></span> <span data-ttu-id="52758-104">ASP.NET Core 2.x サンプルについては、「[ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x)」(ASP.NET Core の URL リライト サンプル (ASP.NET Core 2.x)) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="52758-104">For the ASP.NET Core 2.x sample, see [ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).</span></span>

<span data-ttu-id="52758-105">このサンプルを実行すると、いずれかのルールが要求 URL に適用されるときに書き換えられる URL またはリダイレクトされる URL を示す応答が提供されます。</span><span class="sxs-lookup"><span data-stu-id="52758-105">When running the sample, a response will be served that shows the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="52758-106">このサンプルの例</span><span class="sxs-lookup"><span data-stu-id="52758-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - <span data-ttu-id="52758-107">成功状態コード: 302 (検出)</span><span class="sxs-lookup"><span data-stu-id="52758-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="52758-108">例 (リダイレクト): **/redirect-rule/{capture_group}** から **/redirected/{capture_group}** へ</span><span class="sxs-lookup"><span data-stu-id="52758-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="52758-109">成功状態コード: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="52758-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="52758-110">例 (リライト): **/rewrite-rule/{capture_group_1}/{capture_group_2}** から **/rewritten?var1={capture_group_1}&var2={capture_group_2}** へ</span><span class="sxs-lookup"><span data-stu-id="52758-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="52758-111">成功状態コード: 302 (検出)</span><span class="sxs-lookup"><span data-stu-id="52758-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="52758-112">例 (リダイレクト): **/apache-mod-rules-redirect/{capture_group}** から **/redirected?id={capture_group}** へ</span><span class="sxs-lookup"><span data-stu-id="52758-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="52758-113">成功状態コード: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="52758-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="52758-114">例 (リライト): **/iis-rules-rewrite/{capture_group}** から **/rewritten?id={capture_group}** へ</span><span class="sxs-lookup"><span data-stu-id="52758-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXMLRequests)`
  - <span data-ttu-id="52758-115">成功状態コード: 301 (完全な移動)</span><span class="sxs-lookup"><span data-stu-id="52758-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="52758-116">例 (リダイレクト): **/file.xml** から **/xmlfiles/file.xml** へ</span><span class="sxs-lookup"><span data-stu-id="52758-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="52758-117">成功状態コード: 301 (完全な移動)</span><span class="sxs-lookup"><span data-stu-id="52758-117">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="52758-118">例 (リダイレクト): **/image.png** から **/png-images/image.png** へ</span><span class="sxs-lookup"><span data-stu-id="52758-118">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="52758-119">例 (リダイレクト): **/image.jpg** から **/jpg-images/image.jpg** へ</span><span class="sxs-lookup"><span data-stu-id="52758-119">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="using-a-physicalfileprovider"></a><span data-ttu-id="52758-120">`PhysicalFileProvider` の使用</span><span class="sxs-lookup"><span data-stu-id="52758-120">Using a `PhysicalFileProvider`</span></span>
<span data-ttu-id="52758-121">`AddApacheModRewrite()` メソッドと `AddIISUrlRewrite()` メソッドに渡す `PhysicalFileProvider` を作成して、`IFileProvider` を取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="52758-121">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a><span data-ttu-id="52758-122">セキュリティで保護されたリダイレクトの拡張機能</span><span class="sxs-lookup"><span data-stu-id="52758-122">Secure redirection extensions</span></span>
<span data-ttu-id="52758-123">このサンプルには、URL (**https://localhost:5001**、**https://localhost**) とテスト証明書 (**testCert.pfx**) を使用してこれらのリダイレクト メソッドを調べるための `WebHostBuilder` 構成が含まれています。</span><span class="sxs-lookup"><span data-stu-id="52758-123">This sample includes `WebHostBuilder` configuration for the app to use URLs (**https://localhost:5001**, **https://localhost**) and a test certificate (**testCert.pfx**) to assist you in exploring these redirect methods.</span></span> <span data-ttu-id="52758-124">それらの動作を調べるには、そのいずれかを **Startup.cs** の `RewriteOptions()` に追加します。</span><span class="sxs-lookup"><span data-stu-id="52758-124">Add any of them to the `RewriteOptions()` in **Startup.cs** to study their behavior.</span></span>

<span data-ttu-id="52758-125">メソッド</span><span class="sxs-lookup"><span data-stu-id="52758-125">Method</span></span> | <span data-ttu-id="52758-126">状態コード</span><span class="sxs-lookup"><span data-stu-id="52758-126">Status Code</span></span> | <span data-ttu-id="52758-127">ポート</span><span class="sxs-lookup"><span data-stu-id="52758-127">Port</span></span>
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | <span data-ttu-id="52758-128">301</span><span class="sxs-lookup"><span data-stu-id="52758-128">301</span></span> | <span data-ttu-id="52758-129">null (465)</span><span class="sxs-lookup"><span data-stu-id="52758-129">null (465)</span></span>
`.AddRedirectToHttps()` | <span data-ttu-id="52758-130">302</span><span class="sxs-lookup"><span data-stu-id="52758-130">302</span></span> | <span data-ttu-id="52758-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="52758-131">null (465)</span></span>
`.AddRedirectToHttps(301)` | <span data-ttu-id="52758-132">301</span><span class="sxs-lookup"><span data-stu-id="52758-132">301</span></span> | <span data-ttu-id="52758-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="52758-133">null (465)</span></span>
`.AddRedirectToHttps(301, 5001)` | <span data-ttu-id="52758-134">301</span><span class="sxs-lookup"><span data-stu-id="52758-134">301</span></span> | <span data-ttu-id="52758-135">5001</span><span class="sxs-lookup"><span data-stu-id="52758-135">5001</span></span>
