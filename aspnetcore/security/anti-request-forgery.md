---
title: ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃
author: steve-smith
description: 悪意のある web サイトがクライアント ブラウザーとアプリ間の相互作用を与えることができますの web アプリに対する攻撃を防止する方法を検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET Core を防ぐクロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃

によって[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

クロスサイト リクエスト フォージェリ (とも呼ばれる XSRF または CSRF、発音*「surf*) は、これにより、悪意のある web アプリに影響を与える、クライアント ブラウザーと web アプリを信頼している間の相互作用 web ホスト型アプリへの攻撃ブラウザー。 Web ブラウザーの web サイトにいくつかの種類の認証トークンに自動的にすべての要求を送信するため、このような攻撃が可能です。 この形式の脆弱性攻撃とも呼ばれます、 *1 回のクリック攻撃*または*乗るセッション*セッション、ユーザーの認証に以前の攻撃を活用するためです。

CSRF 攻撃の例:

1. ユーザーにサインイン`www.good-banking-site.com`フォーム認証を使用します。 サーバーは、ユーザーを認証し、認証 cookie を含んでいる応答を発行します。 サイトは、有効な認証 cookie で受信したすべての要求を信頼しているために攻撃に対して脆弱です。
1. ユーザーが、悪意のあるサイトを訪問`www.bad-crook-site.com`です。

   悪意のあるサイト`www.bad-crook-site.com`次のような HTML フォームが含まれています。

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   注意して、フォームの`action`悪意のあるサイトではなく、脆弱なサイトに投稿します。 これは、CSRF の「クロスサイト」の一部です。

1. ユーザーは、[送信] ボタンを選択します。 ブラウザーが、要求を行うし、要求されたドメインの認証クッキーを自動的に含まれます`www.good-banking-site.com`です。
1. 要求が実行されて、`www.good-banking-site.com`ユーザーの認証コンテキストを持つサーバーと、認証されたユーザーの実行が許可されているすべてのアクションを実行することができます。

ユーザーがフォームを送信するボタンを選択すると、悪意のあるサイトことができます。

* フォームを自動的に送信するスクリプトを実行します。
* フォームの送信を AJAX 要求として送信します。 
* CSS を非表示のフォームを使用します。 

HTTPS を使用して CSRF 攻撃を阻止しません。 悪意のあるサイトが送信できる、`https://www.good-banking-site.com/`同じくらい簡単に保護されていない要求を送信できるように要求します。

一部の攻撃は、イメージ タグを使用して、操作を実行する場合、GET 要求に応答するエンドポイントを対象します。 この種の攻撃は、イメージの許可は、JavaScript をブロックするフォーラム サイトに共通です。 ここで変数またはリソースが変更、GET 要求に状態を変更するアプリは、悪意のある攻撃に対して脆弱です。 **GET 要求の状態を変更するには安全ではありません。GET 要求の状態を決して変更しないことをお勧めします。**

CSRF 攻撃があるために、認証の cookie を使用する web アプリに対する考えられます。

* ブラウザーでは、web アプリによって発行された cookie を保存します。
* ストアドの cookie には、認証されたユーザーのセッションの cookie が含まれます。
* ブラウザーでは、すべての cookie ドメインに関連付けられた、web アプリに、ブラウザー内アプリケーションへの要求を生成する方法に関係なくすべての要求を送信します。

ただし、CSRF 攻撃は制限に cookie を利用します。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 ブラウザーが、セッションまで、資格情報を自動的に送信するユーザーは、基本認証またはダイジェスト認証を使用してサインイン後、&dagger;を終了します。

&dagger;このコンテキストで*セッション*をユーザーが認証されたクライアント側のセッションを参照します。 サーバー側のセッションに関連付けられていないか、 [ASP.NET Core セッション ミドルウェア](xref:fundamentals/app-state)です。

ユーザーは、対策を講じること、CSRF 脆弱性を防ぐことができます。

* 使用してそれらを完了すると、web アプリからサインインします。
* オフのブラウザーの cookie 定期的にします。

CSRF 脆弱性が根本的に、web アプリで、エンド ユーザーではない問題です。

## <a name="authentication-fundamentals"></a>認証の基本事項

Cookie ベースの認証は、認証の一般的な形式です。 トークン ベースの認証システム普及しつつ、特に単一ページ アプリケーション (SPAs) の場合。

### <a name="cookie-based-authentication"></a>Cookie ベースの認証

ユーザーを認証ユーザー名とパスワードを使用して、ときに認証と承認に使用できる認証チケットを含む、トークンが発行するしています。 クライアントのすべての要求に付属している cookie が行うと、トークンが格納されます。 生成して、この cookie を検証していますが、Cookie 認証ミドルウェアによって実行されます。 [ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化します。 以降の要求でミドルウェア cookie を検証、プリンシパルが再作成およびするプリンシパルが割り当てられます、[ユーザー](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)プロパティ[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)です。

### <a name="token-based-authentication"></a>トークン ベースの認証

ユーザーが認証されると、トークン (antiforgery トークンされません) が発行するしています。 トークンには形式でユーザー情報が含まれます[クレーム](/dotnet/framework/security/claims-based-identity-model)またはアプリで管理ユーザーの状態をアプリを指す参照トークンです。 ユーザーが認証を必要とするリソースにアクセスしようとした場合、トークンはベアラー トークンの形式で、追加の承認ヘッダーを使用してアプリに送信されます。 これにより、アプリはステートレスです。 後続の要求、トークンは、サーバー側の検証の要求で渡されます。 このトークンがない*暗号化*; が*エンコード*です。 サーバーで、その情報へのアクセス トークンがデコードされます。 トークンに後続の要求を送信するには、ブラウザーのローカル ストレージにトークンを格納します。 トークンが、ブラウザーのローカル ストレージに格納されている場合、CSRF 脆弱性を懸念いけません。 CSRF は問題にトークンが、cookie に格納されている場合です。

### <a name="multiple-apps-hosted-at-one-domain"></a>1 つのドメインでホストされている複数のアプリ

共有ホスティング環境は、セッションの乗っ取り、ログイン CSRF、およびその他の攻撃に対して脆弱です。

`example1.contoso.net`と`example2.contoso.net`別のホストは、下にあるホスト間で暗黙的な信頼関係がある、`*.contoso.net`ドメイン。 この暗黙的な信頼関係は、それぞれの他のクッキー (HTTP クッキーには、同じオリジンのポリシーを AJAX 要求は該当しないとは限りません) に影響を与える可能性のある信頼されていないホストできます。

ドメインを共有していないでは、同じドメインでホストされているアプリ間で信頼されている cookie を利用する攻撃を防止できます。 各アプリケーションが独自のドメインでホストされている場合を悪用する cookie の暗黙的な信頼関係はありません。

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery 構成

> [!WARNING]
> ASP.NET Core を実装を使用して antiforgery [ASP.NET Core データ保護](xref:security/data-protection/introduction)です。 データ保護スタックは、サーバー ファームで動作するように構成する必要があります。 参照してください[データ保護を構成する](xref:security/data-protection/configuration/overview)詳細についてはします。

ASP.NET Core 2.0 以降では、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery トークンでは HTML フォーム要素に挿入します。 Razor ファイルに次のマークアップでは、antiforgery トークンが自動的に生成されます。

```cshtml
<form method="post">
    ...
</form>
```

同様に、 [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)フォームのメソッドが GET でない場合、既定では antiforgery トークンを生成します。

HTML フォーム要素 antiforgery トークンの自動生成の発生時に、`<form>`タグが含まれています、`method="post"`属性と、次のいずれかに該当します。

  * アクションの属性が空白 (`action=""`)。
  * Action 属性が指定されていない (`<form method="post">`)。

HTML フォーム要素の antiforgery トークンの自動生成を無効にすることができます。

* Antiforgery トークンを明示的に無効化、`asp-antiforgery`属性。

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form 要素はオプトアウト タグ ヘルパーのタグ ヘルパーの使用によって[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* 削除、`FormTagHelper`ビューからです。 `FormTagHelper` Razor ビューに次のディレクティブを追加することによってビューから削除することができます。

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor ページ](xref:mvc/razor-pages/index)XSRF/CSRF から自動的に保護されます。 詳細については、次を参照してください。 [XSRF/CSRF および Razor ページ](xref:mvc/razor-pages/index#xsrf)です。

CSRF 攻撃から保護する最も一般的な方法は、使用する、*シンクロナイザー トークン パターン*(STP)。 STP は、ユーザーがフォームのデータを含むページを要求するときに使用されます。

1. サーバーは、クライアントに現在のユーザーの id に関連付けられたトークンを送信します。
1. クライアントは、検証用のサーバーに、トークンでから返送されます。
1. 受信した場合、サーバーが認証されたユーザーの id と一致しないトークン、要求は拒否されます。

トークンは、一意で予期しないです。 一連の要求が適切な順序を確認トークンを使用することができますも (たとえば、要求シーケンスのことを確認: 1 ページ&ndash;ページ 2 &ndash; 3 ページ)。 ASP.NET Core MVC および Razor ページ テンプレートではフォームのすべての antiforgery トークンを生成します。 次の 2 つのビューの例では、antiforgery トークンを生成します。

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Antiforgery トークンを明示的に追加、 `<form>` HTML ヘルパーとタグ ヘルパーを使用せず要素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

各ケースの前のでは、ASP.NET Core には、次のような隠しフォーム フィールドが追加されます。

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core を含む 3 つ[フィルター](xref:mvc/controllers/filters) antiforgery トークンを使用するため。

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery オプション

カスタマイズ[antiforgery オプション](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)で`Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| オプション | 説明 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Antiforgery クッキーを作成するために使用する設定を決定します。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Cookie のドメイン。 既定値は `null` です。 このプロパティは廃止されており、今後のバージョンで削除される予定です。 Cookie.Domain お勧めします。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Cookie の名前。 設定されていないシステム生成一意な名前の先頭に、 [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("です。AspNetCore.Antiforgery。") です。 このプロパティは廃止されており、今後のバージョンで削除される予定です。 Cookie.Name お勧めします。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Cookie の設定のパス。 このプロパティは廃止されており、今後のバージョンで削除される予定です。 Cookie.Path お勧めします。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | ビューで antiforgery トークンを表示するために、antiforgery システムで使用される隠しフォーム フィールドの名前。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery システムによって使用されるヘッダーの名前。 場合`null`フォームのデータのみが考慮されます。 |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Antiforgery システムでは SSL が必要かどうかを指定します。 場合`true`、SSL 以外の要求は失敗します。 既定値は `false` です。 このプロパティは廃止されており、今後のバージョンで削除される予定です。 Cookie.SecurePolicy を設定することをお勧めします。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。 既定では、ヘッダーには"SAMEORIGIN"の値が生成されます。 既定値は `false` です。 |

詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)です。

## <a name="configure-antiforgery-features-with-iantiforgery"></a>IAntiforgery で antiforgery 機能を構成します。

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery 機能を構成する API を提供します。 `IAntiforgery` 要求されることができます、`Configure`のメソッド、`Startup`クラスです。 次の例では、antiforgery トークンを生成し、(このトピックの後半で説明、既定値の角度の名前付け規則を使用) がクッキーとしての応答で送信するアプリのホーム ページからのミドルウェアを使用します。

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Antiforgery 検証を要求します。

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)個々 のアクション、コント ローラーに適用できるアクション フィルターは、またはグローバルにします。 要求が有効な antiforgery トークンを含まない限り、このフィルターが適用されているアクションに対する要求がブロックされます。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken`属性には、HTTP GET 要求を含む、修飾アクション メソッドへの要求に対してトークンが必要です。 場合、`ValidateAntiForgeryToken`アプリのコント ローラー間で属性が適用されるでオーバーライドできる、`IgnoreAntiforgeryToken`属性。

> [!NOTE]
> ASP.NET Core では、GET 要求を antiforgery トークンを自動的に追加することをサポートしていません。

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>自動的に安全でない HTTP メソッドにのみの antiforgery のトークンを検証します。

ASP.NET Core アプリケーションは、安全な HTTP メソッド (GET、HEAD、オプション、およびトレース) に対する antiforgery トークンを生成しません。 広範に適用する代わりに、`ValidateAntiForgeryToken`属性とし、オーバーライドすることで`IgnoreAntiforgeryToken`、属性、 [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)属性を使用できます。 この属性の動作と同じように、`ValidateAntiForgeryToken`次の HTTP メソッドを使用して行われる要求のトークンを必要としないためする点を除いて、属性します。

* GET
* HEAD、
* オプション
* TRACE

使用をお勧め`AutoValidateAntiforgeryToken`非 API のシナリオを広範にします。 これにより、後の操作が既定では保護されています。 代替手段は、しない限り、既定では、antiforgery トークンを無視するのには`ValidateAntiForgeryToken`は個別のアクション メソッドに適用します。 可能性が高い POST アクション メソッドのままにするのには、このシナリオでは、保護されていない、誤って、アプリの CSRF 攻撃に対して脆弱なままです。 すべての投稿では、antiforgery トークンを送信する必要があります。

Api は、トークンのクッキー以外の一部を送信するための自動メカニズムはありません。 実装は、おそらくクライアント コードの実装に依存します。 いくつかの例を次に示します。

クラス レベルの例:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

グローバルの例:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>上書きのグローバルまたはコント ローラー antiforgery 属性

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)フィルターを使用して、特定のアクション (またはコント ローラー) の antiforgery トークンの必要性を排除します。 適用すると、このフィルターをオーバーライド`ValidateAntiForgeryToken`と`AutoValidateAntiforgeryToken`(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。

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

## <a name="refresh-tokens-after-authentication"></a>更新トークンを認証した後

ビューまたは Razor ページのページにユーザーをリダイレクトすることで、ユーザーが認証されると、トークンを更新する必要があります。

## <a name="javascript-ajax-and-spas"></a>JavaScript、AJAX、および SPAs

従来の HTML ベースのアプリでは、antiforgery トークンは隠しフォーム フィールドを使用してサーバーに渡されます。 JavaScript ベースの最新のアプリと SPAs で多数の要求は、プログラムにより行われます。 これらの AJAX 要求は、要求ヘッダーまたはクッキー) などその他の手法を使用して、トークンを送信する可能性があります。

認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合は、潜在的な問題は CSRF です。 トークンを格納するローカル ストレージを使用すると、すべての要求でサーバーにローカル ストレージからの値が自動的に送信されないために、CSRF 脆弱性を軽減する可能性があります。 したがって、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨される方法として、トークンを送信する antiforgery トークンを格納します。

### <a name="javascript"></a>JavaScript

JavaScript を使用して、ビューで、トークンを作成できますビュー内からサービスを使用します。 挿入、 [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)サービス ビューと呼び出しを[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

この方法では、サーバーから cookie の設定や、クライアントからの読み取りを直接処理する必要があります。

前の例では、AJAX の POST ヘッダーを非表示フィールドの値を読み取り、JavaScript を使用します。

JavaScript のアクセス cookie トークンと cookie の内容を使用して、トークンの値を持つヘッダーを作成することができますも。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

というヘッダーにトークンを送信する要求、スクリプトを想定して`X-CSRF-TOKEN`を探して antiforgery サービスの構成、`X-CSRF-TOKEN`ヘッダー。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

次の例では、JavaScript を使用して適切なヘッダーを持つ AJAX 要求を作成します。

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS は、アドレス CSRF に規則を使用します。 サーバーが名前を持つ cookie を送信する場合`XSRF-TOKEN`、AngularJS`$http`サービスがサーバーに要求を送信するときに、ヘッダーに cookie の値を追加します。 このプロセスは自動です。 ヘッダーは、明示的に設定する必要はありません。 ヘッダー名は`X-XSRF-TOKEN`します。 サーバーは、このヘッダーを検出し、その内容を検証する必要があります。

ASP.NET Core API のこの規則を使用します。

* 呼ばれる cookie のトークンを提供するアプリの構成`XSRF-TOKEN`です。
* サービスを構成します antiforgery という名前のヘッダーを検索する`X-XSRF-TOKEN`です。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="extend-antiforgery"></a>Antiforgery を拡張します。

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、各トークンの追加データのラウンド トリップしてアンチ CSRF システムの動作を拡張する開発者です。 [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)メソッドが呼び出された各フィールド トークンが生成され、戻り値が生成されたトークンに含まれています。 実装者が、タイムスタンプ、nonce、またはその他の値を返すし、呼び出すでした[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)トークンが検証されると、このデータを検証します。 生成されたトークンに埋め込まれているクライアントのユーザー名が既にするためにこの情報を含める必要はありません。 トークンは、補足データですが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足データが検証されません。

## <a name="additional-resources"></a>その他の技術情報

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web アプリケーション プロジェクトを開きセキュリティ](https://www.owasp.org/index.php/Main_Page)(OWASP)。
