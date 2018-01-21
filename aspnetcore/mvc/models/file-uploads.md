---
title: "ASP.NET Core でのファイルのアップロード"
author: ardalis
description: "モデル バインディングおよびストリーミングを使用して ASP.NET Core MVC でのファイルをアップロードする方法です。"
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3c5abe84a5c7cc399e0586e680a414fab7a26c1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET Core でのファイルのアップロード

によって[Steve Smith](https://ardalis.com/)

ASP.NET MVC アクションは、単純なモデル サイズの小さいファイルのバインドまたはよりも大きなファイルのストリーミングを使用して 1 つまたは複数のファイルのアップロードをサポートします。

[GitHub から表示またはダウンロードのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>モデル バインディングで小さなファイルをアップロードします。

小さなファイルをアップロードするには、マルチパート HTML フォームを使用してまたは JavaScript を使用して、POST 要求を作成できます。 例の形式がアップロードされた複数のファイルをサポートするには、Razor を使用して、次に示します。

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

ファイルのアップロードをサポートするために HTML フォームを指定する必要があります、`enctype`の`multipart/form-data`します。 `files`入力要素の前に示した複数ファイルのアップロードをサポートしています。 省略、`multiple`アップロードする 1 つのファイルだけを許可するこの入力要素の属性です。 としてブラウザーで上記のマークアップを表示します。

![ファイルのアップロード フォーム](file-uploads/_static/upload-form.png)

サーバーにアップロードされた個別のファイルからアクセスできます[モデル バインド](xref:mvc/models/model-binding)を使用して、 [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile)インターフェイスです。 `IFormFile`次の構造があります。

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
> 依存しない、または信頼、`FileName`検証を伴わないプロパティです。 `FileName`プロパティは、表示目的でのみ使用する必要があります。

モデル バインディングを使用してファイルをアップロードするとき、`IFormFile`インターフェイス、アクション メソッドは、1 つを受け入れることができます`IFormFile`または`IEnumerable<IFormFile>`(または`List<IFormFile>`) を表すいくつかのファイルです。 次の例は、1 つまたは複数のアップロードされたファイルをループ処理、ローカル ファイル システムに保存し、アップロードされたファイルのサイズと合計数を返します。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

使用してアップロードされたファイル、`IFormFile`手法は、バッファー内のメモリまたは web サーバー上のディスクに処理される前にします。 アクション メソッドの内部、`IFormFile`内容はストリームとしてアクセスします。 ローカル ファイル システムだけでなくファイルにストリーミングできる[Azure Blob ストレージ](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)または[Entity Framework](https://docs.microsoft.com/ef/core/index)です。

Entity Framework を使用してデータベースにバイナリ ファイルのデータを保存して、型のプロパティを定義`byte[]`エンティティで。

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

型の viewmodel プロパティを指定して`IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`使用できますがアクション メソッド パラメーターとして直接または viewmodel プロパティとして、上記のようにします。

コピー、`IFormFile`ストリームをバイト配列に保存します。

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
> ように、パフォーマンスに悪影響は、リレーショナル データベースにバイナリ データを格納する場合の注意を使用します。

## <a name="uploading-large-files-with-streaming"></a>ストリーミングの大きいファイルのアップロード

サイズまたはファイルのアップロードの頻度には、アプリのリソースの問題が発生する場合は、完全にバッファリングするのではなく、ファイルのアップロードのストリーミングを上に示したモデル バインディングのアプローチは、検討します。 使用しているときに`IFormFile`モデル バインディングがより簡単に解決、およびストリーミングを正しく実装する手順の数が必要です。

> [!NOTE]
> 64 KB を超える単一バッファー内のファイルは、そのディスク上の一時ファイルをサーバーで RAM から移動されます。 ファイルのアップロードで使用したリソース (ディスク、RAM) は、同時実行ファイルのアップロードのサイズと数によって異なります。 ストリーミングはパフォーマンスについて多くはスケールに関する説明。 バッファーが多すぎますアップロードしようとする場合、メモリまたはディスク領域を使い果たした場合、サイトがクラッシュします。

次の例では、コント ローラーのアクションをストリーム配信の JavaScript/角の使用方法を示します。 カスタム フィルター属性を使用して、HTTP ヘッダーの代わりに、要求本文で渡されたファイルの antiforgery トークンが生成されます。 アクション メソッドがアップロードされたデータを直接処理されるため、別のフィルターによりモデル バインディングが無効です。 使用して、アクション内で、フォームの内容の読み取りは、 `MultipartReader`、各個人を読み取ります`MultipartSection`ファイルの処理、または適切な内容を保存します。 すべてのセクションが読み取られた後、アクションは独自のモデル バインドを実行します。

最初のアクションがフォームを読み込むし、antiforgery トークンを cookie に保存されます (を使用して、`GenerateAntiforgeryTokenCookieForAjax`属性)。

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

属性が ASP.NET Core の組み込みを使用して[Antiforgery](xref:security/anti-request-forgery)要求トークンを使用して cookie の設定をサポートします。

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

角が自動的に antiforgery トークンを渡すという名前の要求ヘッダーで`X-XSRF-TOKEN`です。 構成でこのヘッダーを参照する ASP.NET Core MVC アプリが構成されている*Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding`モデルのバインディングを無効にする、次に示す属性が使用される、`Upload`アクション メソッド。

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

モデル バインディングが無効になるので、`Upload`アクション メソッド パラメーターを受け取らないです。 直接連携して、`Request`プロパティ`ControllerBase`です。 A`MultipartReader`各セクションの読み取りに使用します。 GUID ファイル名を持つファイルが保存され、内のキー/値のデータが格納されている、`KeyValueAccumulator`です。 すべてのセクションが読み込まれた後の内容、`KeyValueAccumulator`フォーム データをモデルの種類にバインドするために使用します。

完全な`Upload`メソッドを次に示します。

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>トラブルシューティング

下には、ファイルとその考えられる解決策のアップロードを扱うときの一般的な問題を示します。

### <a name="unexpected-not-found-error-with-iis"></a>IIS での予期しないが見つかりません。 エラー

次のエラーを示します、ファイルのアップロードを超える、サーバーの構成済み`maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

既定の設定は`30000000`、約 28.6 MB であります。 値を編集してカスタマイズできる*web.config*:

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

この設定は、IIS にのみ適用されます。 動作は、Kestrel でホストしているときに、既定では発生しません。 詳細については、次を参照してください。[要求制限\<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)です。

### <a name="null-reference-exception-with-iformfile"></a>IFormFile を使用して null 参照例外

コント ローラーが受け入れる場合を使用してファイルをアップロード`IFormFile`値が常に null を検索、HTML フォームを指定することを確認するが、`enctype`の値`multipart/form-data`です。 この属性に設定されていない場合、`<form>`要素、ファイルのアップロードは行われませんと任意のバインド`IFormFile`引数は null になります。
