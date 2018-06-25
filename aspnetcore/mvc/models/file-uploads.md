---
title: ASP.NET Core でのファイルのアップロード
author: ardalis
description: モデル バインドとストリーミングを使用して、ASP.NET Core MVC でファイルをアップロードする方法。
ms.author: riande
ms.date: 07/05/2017
uid: mvc/models/file-uploads
ms.openlocfilehash: 771e22ca01c67f2b6bbee780324d9d08759b3279
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272544"
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="137cf-103">ASP.NET Core でのファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="137cf-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="137cf-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="137cf-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="137cf-105">ASP.NET MVC アクションでは、より小さいファイルの単純なモデル バインドまたはより大きいファイルのストリーミングを使用する 1 つ以上のファイルのアップロードがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="137cf-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="137cf-106">GitHub のサンプルを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="137cf-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="137cf-107">モデル バインドによる小さいファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="137cf-107">Uploading small files with model binding</span></span>

<span data-ttu-id="137cf-108">小さいファイルをアップロードする場合は、マルチパートの HTML フォームを使用できます。または、JavaScript を使用して POST 要求を構築することができます。</span><span class="sxs-lookup"><span data-stu-id="137cf-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="137cf-109">複数のアップロード済みファイルをサポートする、Razor を使用するフォーム例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="137cf-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="137cf-110">ファイルのアップロードをサポートするには、HTML フォームで `multipart/form-data` の `enctype` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="137cf-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="137cf-111">上記の `files` 入力要素では、複数ファイルのアップロードがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="137cf-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="137cf-112">この入力要素の `multiple` 属性を省略すると、単一のファイルのみをアップロードできます。</span><span class="sxs-lookup"><span data-stu-id="137cf-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="137cf-113">上記のマークアップは次のようにブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-113">The above markup renders in a browser as:</span></span>

![ファイルのアップロード フォーム](file-uploads/_static/upload-form.png)

<span data-ttu-id="137cf-115">サーバーにアップロードされた個々のファイルには、[モデル バインド](xref:mvc/models/model-binding)を介して、[IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) インターフェイスを使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="137cf-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="137cf-116">`IFormFile` の構造は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="137cf-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="137cf-117">検証を行わずに `FileName` プロパティに依存したり、信頼したりしないでください。</span><span class="sxs-lookup"><span data-stu-id="137cf-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="137cf-118">`FileName` プロパティは表示目的でのみ使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="137cf-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="137cf-119">モデル バインドと `IFormFile` インターフェイスを使用してファイルをアップロードする場合、アクション メソッドでは単一の `IFormFile`、または複数のファイルを表す `IEnumerable<IFormFile>` (または `List<IFormFile>`) を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="137cf-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="137cf-120">次の例では 1 つ以上のアップロードされたファイルをループ処理し、それをローカル ファイル システムに保存し、アップロードされたファイルの合計数とサイズを返します。</span><span class="sxs-lookup"><span data-stu-id="137cf-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="137cf-121">`IFormFile` 手法を使用してアップロードされたファイルは、Web サーバー上のディスクまたはメモリ内にバッファーされてから処理されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="137cf-122">アクション メソッド内では、`IFormFile` コンテンツにはストリームとしてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="137cf-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="137cf-123">ローカル ファイル システムだけでなく、[Azure BLOB ストレージ](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)または [Entity Framework](https://docs.microsoft.com/ef/core/index) にもファイルをストリーミングすることができます。</span><span class="sxs-lookup"><span data-stu-id="137cf-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="137cf-124">Entity Framework を使用してデータベースにバイナリ ファイル データを格納するには、次のようにエンティティで `byte[]` 型のプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="137cf-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="137cf-125">次のように `IFormFile` 型の viewmodel プロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="137cf-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="137cf-126">`IFormFile` は直接アクション メソッド パラメーターとして、または上記のように viewmodel プロパティとして使用することができます。</span><span class="sxs-lookup"><span data-stu-id="137cf-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="137cf-127">次のように、`IFormFile` をストリームにコピーして、バイト配列に保存します。</span><span class="sxs-lookup"><span data-stu-id="137cf-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="137cf-128">パフォーマンスに悪影響を与える可能性があるため、リレーショナル データベースにバイナリ データを格納する場合は注意してください。</span><span class="sxs-lookup"><span data-stu-id="137cf-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="137cf-129">ストリーミングによる大きいファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="137cf-129">Uploading large files with streaming</span></span>

<span data-ttu-id="137cf-130">ファイルのアップロードのサイズまたは頻度が原因でアプリのリソース問題が発生する場合は、ファイル全体をバッファーするのではなく、上記のモデル バインド方法で行ったようにファイルのアップロードのストリーミングを検討してください。</span><span class="sxs-lookup"><span data-stu-id="137cf-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="137cf-131">`IFormFile` とモデル バインドの使用ははるかに単純なソリューションですが、ストリーミングでは適切に実装するための多くの手順が必要になります。</span><span class="sxs-lookup"><span data-stu-id="137cf-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="137cf-132">64 KB を超える単一のバッファー済みファイルは、RAM からサーバー上のディスクの一時ファイルに移動されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="137cf-133">ファイルのアップロードで使用されるリソース (ディスク、RAM) は、同時ファイル アップロードの数とサイズによって異なります。</span><span class="sxs-lookup"><span data-stu-id="137cf-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="137cf-134">ストリーミングではパフォーマンスについてそれほど気にする必要はありませんが、サイズには注意が必要です。</span><span class="sxs-lookup"><span data-stu-id="137cf-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="137cf-135">あまり多くのアップロードをバッファーしようとすると、メモリまたはディスク領域が不足したときにサイトがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="137cf-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="137cf-136">JavaScript/Angular を使用して、コントローラーのアクションにストリーミングする例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="137cf-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="137cf-137">ファイルの偽造防止トークンは、カスタム フィルター属性を使用して生成され、要求本文ではなく HTTP ヘッダーで渡されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="137cf-138">アクション メソッドではアップロードされたデータが直接処理されるため、モデル バインドは別のフィルターで無効になります。</span><span class="sxs-lookup"><span data-stu-id="137cf-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="137cf-139">アクション内では、フォームのコンテンツが `MultipartReader` を使用して読み取られます。その場合、各 `MultipartSection` が読み取られ、必要に応じて、ファイルが処理されるかコンテンツが格納されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="137cf-140">すべてのセクションが読み取られた後、アクションで独自のモデル バインドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="137cf-141">最初のアクションではフォームが読み込まれ、cookie に偽造防止トークンが保存されます (`GenerateAntiforgeryTokenCookieForAjax` 属性を使用)。</span><span class="sxs-lookup"><span data-stu-id="137cf-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="137cf-142">この属性では ASP.NET Core の組み込みの[偽造防止](xref:security/anti-request-forgery)サポートを使用して、要求トークンで cookie を設定します。</span><span class="sxs-lookup"><span data-stu-id="137cf-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="137cf-143">Angular は、自動的に `X-XSRF-TOKEN` という名前の要求ヘッダーで偽造防止トークンを渡します。</span><span class="sxs-lookup"><span data-stu-id="137cf-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="137cf-144">ASP.NET Core MVC アプリは、次のように、*Startup.cs* の構成でこのヘッダーを参照するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="137cf-145">以下に示す `DisableFormValueModelBinding` 属性は、`Upload` アクション メソッドでモデル バインドを無効にするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="137cf-146">モデル バインドが無効であるため、`Upload` アクション メソッドではパラメーターを受け入れません。</span><span class="sxs-lookup"><span data-stu-id="137cf-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="137cf-147">`ControllerBase` の `Request` プロパティを直接操作します。</span><span class="sxs-lookup"><span data-stu-id="137cf-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="137cf-148">`MultipartReader` は各セクションを読み取るために使用されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="137cf-149">ファイルは GUID ファイル名で保存され、キー/値データは `KeyValueAccumulator` に格納されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="137cf-150">すべてのセクションが読み込まれた後、`KeyValueAccumulator` のコンテンツはフォーム データをモデル型にバインドするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="137cf-151">完全な `Upload` メソッドを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="137cf-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="137cf-152">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="137cf-152">Troubleshooting</span></span>

<span data-ttu-id="137cf-153">ファイルのアップロード時に発生する一般的ないくつかの問題と、考えられる解決策を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="137cf-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="137cf-154">IIS での予期しない "見つかりません" エラー</span><span class="sxs-lookup"><span data-stu-id="137cf-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="137cf-155">次のエラーは、ファイルのアップロードがサーバーの構成済みの `maxAllowedContentLength` を超えていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="137cf-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="137cf-156">既定の設定は `30000000` で、約 28.6 MB になります。</span><span class="sxs-lookup"><span data-stu-id="137cf-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="137cf-157">*web.config* を編集して、値をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="137cf-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="137cf-158">この設定は IIS にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="137cf-158">This setting only applies to IIS.</span></span> <span data-ttu-id="137cf-159">Kestrel でホストする場合、既定ではこの動作は発生しません。</span><span class="sxs-lookup"><span data-stu-id="137cf-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="137cf-160">詳細については、「[Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)」 (要求制限 <requestLimits>) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="137cf-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="137cf-161">IFormFile での null 参照例外</span><span class="sxs-lookup"><span data-stu-id="137cf-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="137cf-162">コントローラーが `IFormFile` を使用してアップロードされたファイルを受け入れていても、値が常に null であることがわかった場合は、HTML フォームで `multipart/form-data` の `enctype` を指定していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="137cf-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="137cf-163">この属性が `<form>` 要素で設定されていない場合、ファイルはアップロードされず、バインドされた `IFormFile` 引数は null になります。</span><span class="sxs-lookup"><span data-stu-id="137cf-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
