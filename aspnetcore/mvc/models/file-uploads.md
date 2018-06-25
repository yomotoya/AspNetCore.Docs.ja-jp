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
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET Core でのファイルのアップロード

作成者: [Steve Smith](https://ardalis.com/)

ASP.NET MVC アクションでは、より小さいファイルの単純なモデル バインドまたはより大きいファイルのストリーミングを使用する 1 つ以上のファイルのアップロードがサポートされます。

[GitHub のサンプルを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>モデル バインドによる小さいファイルのアップロード

小さいファイルをアップロードする場合は、マルチパートの HTML フォームを使用できます。または、JavaScript を使用して POST 要求を構築することができます。 複数のアップロード済みファイルをサポートする、Razor を使用するフォーム例を以下に示します。

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

ファイルのアップロードをサポートするには、HTML フォームで `multipart/form-data` の `enctype` を指定する必要があります。 上記の `files` 入力要素では、複数ファイルのアップロードがサポートされます。 この入力要素の `multiple` 属性を省略すると、単一のファイルのみをアップロードできます。 上記のマークアップは次のようにブラウザーに表示されます。

![ファイルのアップロード フォーム](file-uploads/_static/upload-form.png)

サーバーにアップロードされた個々のファイルには、[モデル バインド](xref:mvc/models/model-binding)を介して、[IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) インターフェイスを使用してアクセスできます。 `IFormFile` の構造は次のとおりです。

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
> 検証を行わずに `FileName` プロパティに依存したり、信頼したりしないでください。 `FileName` プロパティは表示目的でのみ使用する必要があります。

モデル バインドと `IFormFile` インターフェイスを使用してファイルをアップロードする場合、アクション メソッドでは単一の `IFormFile`、または複数のファイルを表す `IEnumerable<IFormFile>` (または `List<IFormFile>`) を受け入れることができます。 次の例では 1 つ以上のアップロードされたファイルをループ処理し、それをローカル ファイル システムに保存し、アップロードされたファイルの合計数とサイズを返します。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

`IFormFile` 手法を使用してアップロードされたファイルは、Web サーバー上のディスクまたはメモリ内にバッファーされてから処理されます。 アクション メソッド内では、`IFormFile` コンテンツにはストリームとしてアクセスできます。 ローカル ファイル システムだけでなく、[Azure BLOB ストレージ](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)または [Entity Framework](https://docs.microsoft.com/ef/core/index) にもファイルをストリーミングすることができます。

Entity Framework を使用してデータベースにバイナリ ファイル データを格納するには、次のようにエンティティで `byte[]` 型のプロパティを定義します。

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

次のように `IFormFile` 型の viewmodel プロパティを指定します。

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` は直接アクション メソッド パラメーターとして、または上記のように viewmodel プロパティとして使用することができます。

次のように、`IFormFile` をストリームにコピーして、バイト配列に保存します。

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
> パフォーマンスに悪影響を与える可能性があるため、リレーショナル データベースにバイナリ データを格納する場合は注意してください。

## <a name="uploading-large-files-with-streaming"></a>ストリーミングによる大きいファイルのアップロード

ファイルのアップロードのサイズまたは頻度が原因でアプリのリソース問題が発生する場合は、ファイル全体をバッファーするのではなく、上記のモデル バインド方法で行ったようにファイルのアップロードのストリーミングを検討してください。 `IFormFile` とモデル バインドの使用ははるかに単純なソリューションですが、ストリーミングでは適切に実装するための多くの手順が必要になります。

> [!NOTE]
> 64 KB を超える単一のバッファー済みファイルは、RAM からサーバー上のディスクの一時ファイルに移動されます。 ファイルのアップロードで使用されるリソース (ディスク、RAM) は、同時ファイル アップロードの数とサイズによって異なります。 ストリーミングではパフォーマンスについてそれほど気にする必要はありませんが、サイズには注意が必要です。 あまり多くのアップロードをバッファーしようとすると、メモリまたはディスク領域が不足したときにサイトがクラッシュします。

JavaScript/Angular を使用して、コントローラーのアクションにストリーミングする例を以下に示します。 ファイルの偽造防止トークンは、カスタム フィルター属性を使用して生成され、要求本文ではなく HTTP ヘッダーで渡されます。 アクション メソッドではアップロードされたデータが直接処理されるため、モデル バインドは別のフィルターで無効になります。 アクション内では、フォームのコンテンツが `MultipartReader` を使用して読み取られます。その場合、各 `MultipartSection` が読み取られ、必要に応じて、ファイルが処理されるかコンテンツが格納されます。 すべてのセクションが読み取られた後、アクションで独自のモデル バインドが実行されます。

最初のアクションではフォームが読み込まれ、cookie に偽造防止トークンが保存されます (`GenerateAntiforgeryTokenCookieForAjax` 属性を使用)。

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

この属性では ASP.NET Core の組み込みの[偽造防止](xref:security/anti-request-forgery)サポートを使用して、要求トークンで cookie を設定します。

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular は、自動的に `X-XSRF-TOKEN` という名前の要求ヘッダーで偽造防止トークンを渡します。 ASP.NET Core MVC アプリは、次のように、*Startup.cs* の構成でこのヘッダーを参照するように構成されます。

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

以下に示す `DisableFormValueModelBinding` 属性は、`Upload` アクション メソッドでモデル バインドを無効にするために使用されます。

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

モデル バインドが無効であるため、`Upload` アクション メソッドではパラメーターを受け入れません。 `ControllerBase` の `Request` プロパティを直接操作します。 `MultipartReader` は各セクションを読み取るために使用されます。 ファイルは GUID ファイル名で保存され、キー/値データは `KeyValueAccumulator` に格納されます。 すべてのセクションが読み込まれた後、`KeyValueAccumulator` のコンテンツはフォーム データをモデル型にバインドするために使用されます。

完全な `Upload` メソッドを以下に示します。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>トラブルシューティング

ファイルのアップロード時に発生する一般的ないくつかの問題と、考えられる解決策を以下に示します。

### <a name="unexpected-not-found-error-with-iis"></a>IIS での予期しない "見つかりません" エラー

次のエラーは、ファイルのアップロードがサーバーの構成済みの `maxAllowedContentLength` を超えていることを示しています。

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

既定の設定は `30000000` で、約 28.6 MB になります。 *web.config* を編集して、値をカスタマイズすることができます。

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

この設定は IIS にのみ適用されます。 Kestrel でホストする場合、既定ではこの動作は発生しません。 詳細については、「[Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)」 (要求制限 <requestLimits>) を参照してください。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile での null 参照例外

コントローラーが `IFormFile` を使用してアップロードされたファイルを受け入れていても、値が常に null であることがわかった場合は、HTML フォームで `multipart/form-data` の `enctype` を指定していることを確認してください。 この属性が `<form>` 要素で設定されていない場合、ファイルはアップロードされず、バインドされた `IFormFile` 引数は null になります。
