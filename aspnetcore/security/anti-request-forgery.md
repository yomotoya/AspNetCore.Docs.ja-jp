---
title: "ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃"
author: steve-smith
description: "悪意のある web サイトがクライアント ブラウザーとアプリ間の相互作用を与えることができますの web アプリに対する攻撃を防止する方法を検出します。"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃

[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>偽造はどのような攻撃を防止しますか。

クロスサイト リクエスト フォージェリ (とも呼ばれる XSRF または CSRF、発音*「surf*) は、web ホスト アプリケーションそれ悪意のある web サイト影響を与える、クライアント ブラウザーと web サイトを信頼する間の相互作用に対する攻撃そのブラウザー。 Web ブラウザーの web サイトにいくつかの種類の認証トークンに自動的にすべての要求を送信するため、このような攻撃が可能になります。 悪用のこのフォームのとも呼ばれる、 *1 回のクリック攻撃*または*乗るセッション*ユーザーの攻撃の活用セッションを認証していたため、します。

CSRF 攻撃の例:

1. ユーザーがログイン`www.example.com`、フォーム認証を使用します。
2. サーバーは、ユーザーを認証し、認証 cookie を含んでいる応答を発行します。
3. ユーザーは、悪意のあるサイトにアクセスします。

   悪意のあるサイトには、次のような HTML フォームが含まれています。

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

フォームのアクションが悪意のあるサイトではない、脆弱なサイトにポストすることに注意してください。 これは、CSRF の「クロスサイト」の一部です。

4. ユーザーは、[送信] ボタンをクリックします。 ブラウザーには、自動的に要求を使用して要求されたドメイン (この場合は脆弱なサイト) の認証クッキーが含まれています。
5. 要求は、ユーザーの認証コンテキストを使用してサーバーで実行しを行うには、認証されたユーザーが許可されているすべてのアクションを実行することができます。

この例では、ユーザーがフォームのボタンをクリックする必要があります。 悪意のあるページには次があります。

* フォームを自動的に送信するスクリプトを実行します。
* フォームの送信を AJAX 要求として送信します。 
* CSS を非表示のフォームを使用します。 

SSL を使用すると、CSRF 攻撃を防ぐことは、悪意のあるサイトが送信できる、`https://`要求します。 

一部の攻撃対象が応答するサイトのエンドポイント`GET`要求、(この種の攻撃は、一般的なフォーラム サイトでイメージを許可するが、JavaScript をブロックする) 操作を実行する場合、イメージ タグを使用できます。 使用して状態を変更するアプリケーション`GET`悪意のある攻撃からの要求を受けます。

CSRF 攻撃は、ブラウザーが web サイトに関連するすべての cookie を送信するために、認証の cookie を使用する web サイトに対して可能です。 ただし、CSRF 攻撃では、cookie の悪用に制限はありません。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 ユーザーは、基本認証またはダイジェスト認証を使用してログイン、ブラウザーは、セッションが終了するまで自動的に資格情報を送信します。

注: このコンテキストで*セッション*をユーザーが認証されたクライアント側のセッションを参照します。 サーバー側のセッションに関連付けられていないか、[セッション ミドルウェア](xref:fundamentals/app-state)です。

ユーザーは、によって CSRF 脆弱性を防ぐことができます。
* Web サイトのログ出力をそれらの使用が終了します。
* ブラウザーの cookie を定期的に削除します。

CSRF 脆弱性が根本的に、web アプリで、エンド ユーザーではない問題です。

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>ASP.NET Core MVC が CSRF に対処しますか。

> [!WARNING]
> Request 偽造対策を使用して ASP.NET Core を実装して、 [ASP.NET Core データ保護スタック](xref:security/data-protection/introduction)です。 ASP.NET Core データ保護は、サーバー ファームで動作するように構成する必要があります。 参照してください[データ保護を構成する](xref:security/data-protection/configuration/overview)詳細についてはします。

ASP.NET Core request-偽造防止対策の既定のデータ保護の構成 

ASP.NET Core MVC 2.0、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) HTML フォーム要素を偽造防止トークンを挿入します。 たとえば、Razor ファイルに次のマークアップでは、偽造防止トークンが自動的に生成します。

```html
<form method="post">
  <!-- form markup -->
</form>
```

HTML フォーム要素を偽造防止トークンの自動生成の発生時にします。

* `form`タグが含まれています、`method="post"`属性 AND

  * [アクション] 属性が空です。 ( `action=""`) または
  * Action 属性が指定されていません。 (`<form method="post">`)

HTML フォーム要素を偽造防止トークンの自動生成を無効にすることができます。

* 明示的に無効にすると`asp-antiforgery`です。 次に例を示します。

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* タグ ヘルパーの使用を form 要素タグ ヘルパー外で選択[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out)です。

  ```html
  <!form method="post">
  </!form>
  ```

* 削除、`FormTagHelper`ビューからです。 削除することができます、 `FormTagHelper` Razor ビューに次のディレクティブを追加することによってビューから。

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor ページ](xref:mvc/razor-pages/index)XSRF/CSRF から自動的に保護されます。 追加のコードを記述する必要はありません。 参照してください[XSRF/CSRF および Razor ページ](xref:mvc/razor-pages/index#xsrf)詳細についてはします。

CSRF 攻撃から保護する最も一般的な方法は、シンクロナイザー トークン パターン (STP) です。 STP は、ユーザーがフォームのデータを含むページを要求したときに使用される手法です。 サーバーは、クライアントに現在のユーザーの id に関連付けられたトークンを送信します。 クライアントは、検証用のサーバーに、トークンでから返送されます。 受信した場合、サーバーが認証されたユーザーの id と一致しないトークン、要求は拒否されます。 トークンは、一意で予期しないです。 トークンは、一連の要求 (確保ページ 1 の前にページ 3 の前にページ 2) が適切な順序を確実にも使用できます。 ASP.NET Core MVC テンプレート内のすべてのフォームは、antiforgery トークンを生成します。 ビュー ロジックの次の 2 つの例では、antiforgery トークンを生成します。

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Antiforgery トークンを明示的に追加したことができます、 `<form>` HTML ヘルパーとタグ ヘルパーを使用せず要素`@Html.AntiForgeryToken`:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

各ケースの前のでは、ASP.NET Core は、次のような隠しフォーム フィールドを追加します。
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

ASP.NET Core を含む 3 つ[フィルター](xref:mvc/controllers/filters) antiforgery トークンを使用するため: `ValidateAntiForgeryToken`、 `AutoValidateAntiforgeryToken`、および`IgnoreAntiforgeryToken`です。

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

`ValidateAntiForgeryToken`個々 のアクション、コント ローラーに適用できるアクション フィルターは、またはグローバルにします。 要求が有効な antiforgery トークンを含まない限り、このフィルターが適用されているアクションに対する要求がブロックされます。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken`属性トークンを必要とするための要求のアクション メソッドを含む、修飾`HTTP GET`要求します。 広範に適用する場合にできるメソッドをオーバーライドすると、`IgnoreAntiforgeryToken`属性。

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

通常、ASP.NET Core アプリケーションは、HTTP セーフ メソッド (GET、HEAD、オプション、およびトレース) に対する antiforgery トークンを生成しません。 広範に適用する代わりに、`ValidateAntiForgeryToken`属性とし、オーバーライドすることで`IgnoreAntiforgeryToken`使用することができます、属性、``AutoValidateAntiforgeryToken``属性。 この属性の動作と同じように、`ValidateAntiForgeryToken`次の HTTP メソッドを使用して行われる要求のトークンを必要としないためする点を除いて、属性します。

* GET
* HEAD、
* オプション
* TRACE

使用することをお勧め`AutoValidateAntiforgeryToken`非 API のシナリオを広範にします。 これにより、既定では、後の操作が保護されています。 代替手段は、しない限り、既定では、antiforgery トークンを無視するのには`ValidateAntiForgeryToken`個々 のアクション メソッドに適用します。 より多くする POST アクション メソッドのこのシナリオでは左、保護されていないアプリの CSRF 攻撃に対して脆弱なままです。 匿名投稿は、antiforgery トークンを送信する必要があります。

注: Api はトークン; の非 cookie の一部を送信するための自動メカニズムがありません。実装は、クライアント コードの実装に左右されます可能性があります。 いくつかの例は、以下に示します。

例 (クラス レベル):

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

例 (グローバル):

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

`IgnoreAntiforgeryToken`フィルターを使用して、指定したアクションまたはコント ローラー) に存在する antiforgery トークンの必要性を排除します。 適用すると、このフィルターはオーバーライドされます`ValidateAntiForgeryToken`や`AutoValidateAntiforgeryToken`(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a>JavaScript、AJAX、および SPAs

従来の HTML ベースのアプリケーションで antiforgery トークンは、隠しフォーム フィールドを使用してサーバーに渡されます。 JavaScript ベースの最新のアプリと単一ページ アプリケーション (SPAs) で多数の要求は、プログラムにより行われます。 これらの AJAX 要求は、要求ヘッダーまたはクッキー) などその他の手法を使用して、トークンを送信する可能性があります。 認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合 CSRF は潜在的な問題にします。 ただし場合は、トークンを格納するローカル ストレージを使用すると、CSRF 脆弱性軽減することが、すべての新しい要求を使用してサーバーをローカル ストレージからの値が自動的に送信しないためです。 したがって、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨される方法として、トークンを送信する antiforgery トークンを格納します。

### <a name="angularjs"></a>AngularJS

AngularJS は、アドレス CSRF に規則を使用します。 サーバーが名前を持つ cookie を送信する場合`XSRF-TOKEN`、角速度`$http`サービスに追加されます値この cookie からヘッダーをこのサーバーに要求を送信するとき。 このプロセスは自動です。ヘッダーを明示的に設定する必要はありません。 ヘッダー名は`X-XSRF-TOKEN`します。 サーバーは、このヘッダーを検出し、その内容を検証する必要があります。

ASP.NET Core API のこの規則を使用します。

* 呼ばれる cookie のトークンを提供するアプリを構成します。 `XSRF-TOKEN`
* Antiforgery という名前のヘッダーを検索するサービスを構成します。 `X-XSRF-TOKEN`

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[サンプルを表示](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)です。

### <a name="javascript"></a>JavaScript

でビューを JavaScript を使用して、ビュー内からサービスを使用してトークンを作成できます。 挿入するためには、`Microsoft.AspNetCore.Antiforgery.IAntiforgery`サービス ビューと呼び出しを`GetAndStoreTokens`ように。

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

この方法では、サーバーから cookie の設定や、クライアントからの読み取りを直接処理する必要があります。

前の例では、AJAX の POST ヘッダーを非表示フィールドの値を読み取り、jQuery を使用します。 JavaScript を使用して、トークンの値を取得するを使用して`document.getElementById('RequestVerificationToken').value`です。

JavaScript では、また、cookie で提供されるトークンにアクセスでき、次に示すように、トークンの値を持つヘッダーを作成する cookie の内容を使用することができます。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

というヘッダーにトークンを送信する要求、スクリプトを構築すると仮定した場合、`X-CSRF-TOKEN`を探して antiforgery サービスの構成、`X-CSRF-TOKEN`ヘッダー。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

次の例では、jQuery を使用して、適切なヘッダーを持つ AJAX 要求を行います。

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Antiforgery を構成します。

`IAntiforgery` antiforgery システムを構成する API を提供します。 要求することができます、`Configure`のメソッド、`Startup`クラスです。 次の例では、antiforgery トークンを生成し、(上記の既定値の角度の名前付け規則を使用) がクッキーとしての応答で送信するアプリのホーム ページからのミドルウェアを使用します。


```csharp
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>オプション

カスタマイズできる[antiforgery オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary)で`ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "mydomain.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|オプション        | 説明 |
|------------- | ----------- |
|CookieDomain  | Cookie のドメイン。 既定値は `null` です。 |
|CookieName    | Cookie の名前。 設定されていない、システムは一意の名前の先頭を生成、 `DefaultCookiePrefix` ("です。AspNetCore.Antiforgery。") です。 |
|CookiePath    | Cookie の設定のパス。 |
|FormFieldName | ビューで antiforgery トークンを表示するために、antiforgery システムで使用される隠しフォーム フィールドの名前。 |
|HeaderName    | Antiforgery システムによって使用されるヘッダーの名前。 場合`null`システムには、フォームのデータのみが検討してください。 |
|RequireSsl    | Antiforgery システムでは SSL が必要かどうかを指定します。 既定値は `false` です。 場合`true`、SSL 以外の要求は失敗します。 |
|SuppressXFrameOptionsHeader | 生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。 既定では、ヘッダーには"SAMEORIGIN"の値が生成されます。 既定値は `false` です。 |

詳細については https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions を参照してください。

### <a name="extending-antiforgery"></a>Antiforgery を拡張します。

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、各トークン内の追加データのラウンド トリップで ANTI-XSRF システムの動作を拡張する開発者です。 [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_)メソッドが呼び出された各フィールド トークンが生成され、戻り値が生成されたトークンに含まれています。 実装者が、タイムスタンプ、nonce、またはその他の値を返すし、呼び出すでした[ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_)トークンが検証されると、このデータを検証します。 生成されたトークンに埋め込まれているクライアントのユーザー名が既にするためにこの情報を含める必要はありません。 トークンは、補足データですが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足データが検証されません。

## <a name="fundamentals"></a>Fundamentals

CSRF 攻撃は、ドメインのドメインをそのドメインに加えられたすべての要求に関連付けられた cookie を送信する既定のブラウザーの動作に依存します。 これらの cookie は、ブラウザー内で格納されます。 頻繁に認証されたユーザーのセッションの cookie が含まれます。 Cookie ベースの認証は、認証の一般的な形式です。 トークン ベースの認証システムがされて普及しつつ、特に、SPAs やその他の「スマート クライアント」シナリオです。

### <a name="cookie-based-authentication"></a>Cookie ベースの認証

ユーザー名とパスワードを使用して、ユーザーが認証されたらを識別および認証されていることを検証するために使用するトークンが発行するしています。 クライアントのすべての要求に付属している cookie が行うと、トークンが格納されます。 生成して、この cookie を検証していますが、cookie 認証ミドルウェアによって実行されます。 ASP.NET Core 提供 cookie[ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化し、次に、後続の要求の cookie を検証するプリンシパルを再作成し、それを`User`プロパティ`HttpContext`.

Cookie を使用すると、認証 cookie、フォーム認証チケットのコンテナーだけです。 チケットは、各要求と一緒にフォーム認証 cookie の値として渡され、サーバー上のフォーム認証で認証されたユーザーを識別するために使用します。

ユーザーがシステムにログインした場合、ユーザー セッションはサーバー側でが作成され、データベースまたはその他の永続的なストアに格納されてです。 システムがデータ ストアの実際のセッションを指すセッション キーを生成し、クライアント側の cookie として送信されます。 Web サーバーでは、ユーザーの要求が承認を必要なリソース、いつでもこのセッション キーがチェックされます。 システムでは、関連付けられたユーザー セッションが要求されたリソースにアクセスするための権限を持つかどうかを確認します。 場合は、要求が続行されます。 それ以外の場合、要求は、権限がありませんとして返されます。 この方法で cookie を使用して、ステートフルなように見えるアプリケーションを作成する、ユーザーがサーバーで以前に認証できない「ことを忘れないでください」であるためです。

### <a name="user-tokens"></a>ユーザー トークン

トークン ベースの認証は、サーバーにセッションを格納しません。 ユーザーがログイン時に、発行されるトークン (antiforgery トークンされません) しています。 このトークンは、トークンを検証するために必要なデータを保持します。 形式でユーザー情報も含まれています。[クレーム](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model)です。 ユーザーは、認証を必要とするサーバーのリソースにアクセスする場合、トークンはベアラー {トークン} の形式で追加の承認ヘッダーを持つサーバーに送信されます。 これにより、アプリケーションを後続の要求では、要求でトークンを渡さサーバー側の検証のためステートレスです。 このトークンがない*暗号化*; むしろ*エンコード*です。 サーバー側では、トークン内の未加工の情報にアクセスするトークンをデコードすることができます。 トークンを以降の要求を送信するには、か、保存、ブラウザーのローカル記憶域またはで cookie。 トークンが、ローカル ストレージに格納されているが、トークンが、cookie に格納されている場合、問題がある場合は、XSRF の脆弱性について心配しないでください。

### <a name="multiple-applications-are-hosted-in-one-domain"></a>複数のアプリケーションが 1 つのドメインでホストされています。

`example1.cloudapp.net`と`example2.cloudapp.net`別のホストは、下にあるホスト間で暗黙的な信頼関係がある、`*.cloudapp.net`ドメイン。 この暗黙的な信頼関係は、それぞれの他のクッキー (HTTP クッキーには、同じオリジンのポリシーを AJAX 要求は該当しないとは限りません) に影響を与える可能性のある信頼されていないホストできます。 ASP.NET Core ランタイムは、フィールドのトークンにユーザー名が埋め込まれている点で、いくつかの対策を提供します。 悪意のあるサブドメインがセッション トークンを上書きできない場合でも、ユーザーの有効なフィールド トークンを生成できません。 このような環境でホストされている、ときに、組み込みの ANTI-XSRF ルーチンまだことはできません攻撃を防ぐセッションの乗っ取りまたはログイン CSRF です。 共有ホスティング環境とは、セッションの乗っ取り、ログイン CSRF、およびその他の攻撃に vunerable です。

### <a name="additional-resources"></a>その他の技術情報

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web アプリケーション プロジェクトを開きセキュリティ](https://www.owasp.org/index.php/Main_Page)(OWASP)。
