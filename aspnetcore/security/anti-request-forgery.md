---
title: ASP.NET Core で防ぐクロスサイト リクエスト フォージェリ (XSRF または CSRF) 攻撃
author: steve-smith
description: 悪意のある web サイトがクライアント ブラウザーとアプリ間のやり取りに影響する web アプリに対する攻撃を防ぐ方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6a30e1e2321ca3a81d6e1a320d1d87dddb3033c7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095789"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET Core で防ぐクロスサイト リクエスト フォージェリ (XSRF または CSRF) 攻撃

によって[Steve Smith](https://ardalis.com/)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

クロスサイト リクエスト フォージェリ (XSRF または CSRF の発音とも呼ばれます *「サーフ*) は、悪意ある web アプリ影響を与える、クライアント ブラウザーと web アプリを信頼している間の相互作用 web でホストされたアプリに対する攻撃ブラウザー。 Web ブラウザーの web サイトにすべての要求で自動的に認証トークンの一部の種類を送信するため、これらの攻撃は可能です。 悪用のこのフォームとも呼ばれますが、*ワンクリックによる攻撃*または*乗るセッション*セッション、ユーザーの認証に以前の攻撃を利用するためです。

CSRF 攻撃の例:

1. ユーザーにサインイン`www.good-banking-site.com`フォーム認証を使用します。 サーバーは、ユーザーを認証し、認証 cookie を含む応答を発行します。 サイトは、有効な認証 cookie を受信するすべての要求を信頼しているために攻撃に対して脆弱です。
1. ユーザーにアクセスする悪意のあるサイトでは、`www.bad-crook-site.com`します。

   悪意のあるサイト、`www.bad-crook-site.com`次のような HTML フォームが含まれています。

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   注意して、フォームの`action`悪意のあるサイトではなく、脆弱性のあるサイトに投稿します。 これは、CSRF の"cross-site"の一部です。

1. ユーザーは、[送信] ボタンを選択します。 ブラウザーが、要求を行うし、要求されたドメインの認証クッキーを自動的に含まれます`www.good-banking-site.com`します。
1. 要求の実行、`www.good-banking-site.com`ユーザーの認証コンテキストを持つサーバー、認証されたユーザーの実行が許可されている任意のアクションを実行できます。

ユーザーがフォームを送信するボタンを選択する場合、だけでなく、悪意のあるサイトでは次のことができます。

* フォームを自動的に送信するスクリプトを実行します。
* フォームの送信は、AJAX 要求として送信します。
* CSS を使用してフォームを非表示にします。

これらの代替シナリオは、すべての操作や悪意のあるサイトに最初にアクセスする以外のユーザーからの入力を必要としません。

HTTPS を使用して CSRF 攻撃を防ぐことができます。 悪意のあるサイトに送信できる、`https://www.good-banking-site.com/`簡単、安全でない要求を送信できるように要求します。

一部の攻撃は、イメージ タグを使用して、操作を実行する場合、GET 要求に応答するエンドポイントを対象します。 この種の攻撃は、イメージを許可しますが、JavaScript をブロックするフォーラム サイトが一般的です。 変数またはリソースの変更で、GET 要求の状態を変更するアプリは、悪意のある攻撃に対して脆弱です。 **状態を変更する GET 要求では、安全です。GET 要求の状態が変更されないことをお勧めします。**

CSRF 攻撃は cookie 認証を使用するため、web アプリに対して使用できます。

* ブラウザーでは、web アプリによって発行されたクッキーを格納します。
* ストアドの cookie には、認証されたユーザーのセッションの cookie が含まれます。
* ブラウザーでは、すべての cookie に関連付けられているドメイン、web アプリをブラウザー内でアプリに要求が生成する方法に関係なくすべての要求を送信します。

ただし、CSRF 攻撃に制限されない cookie を利用します。 たとえば、基本認証とダイジェスト認証では、また脆弱です。 ブラウザーが、セッションまで、資格情報を自動的に送信するユーザーが基本認証またはダイジェスト認証を使ってサインインした後&dagger;が終了します。

&dagger;このコンテキストで*セッション*ユーザーを認証するクライアント側のセッションを指します。 これは、サーバー側のセッションに関連しないまたは[ASP.NET Core のセッション ミドルウェア](xref:fundamentals/app-state)します。

ユーザーは、CSRF の脆弱性に対する対策を講じることで防ぐことができます。

* 使用してそれらを完了すると、web アプリからサインインします。
* クリア ブラウザー cookie 定期的にします。

ただし、CSRF の脆弱性、基本的に web アプリをエンドユーザーではない問題です。

## <a name="authentication-fundamentals"></a>認証の基礎

Cookie ベースの認証は、認証の一般的な形式です。 トークン ベースの認証システムがシングル ページ アプリケーション (Spa) の特に普及しつつ、成長しています。

### <a name="cookie-based-authentication"></a>Cookie ベースの認証

ユーザー認証、ユーザー名とパスワードを使用すると、それらがトークンを認証と承認のために使用できる認証チケットを格納しているに発行しています。 トークンは、すべての要求、クライアントに付属している cookie として格納されます。 生成して、この cookie を検証していますが、Cookie 認証ミドルウェアによって実行されます。 [ミドルウェア](xref:fundamentals/middleware/index)暗号化された cookie にユーザー プリンシパルをシリアル化します。 以降の要求で、ミドルウェア、cookie を検証、プリンシパルが再作成や、プリンシパルに割り当てます、[ユーザー](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)プロパティの[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)します。

### <a name="token-based-authentication"></a>トークン ベースの認証

ユーザーが認証されるときに、発行されたトークン (偽造防止トークンされません) がいます。 トークンには形式でユーザー情報が含まれています[クレーム](/dotnet/framework/security/claims-based-identity-model)またはアプリで管理ユーザーの状態をアプリを指す参照トークンです。 ユーザーが認証を必要とするリソースにアクセスしようとした場合、トークンはベアラー トークンの形式で追加の承認ヘッダーを使ってアプリに送信されます。 これにより、アプリはステートレスです。 後続の要求ごとに、サーバー側の検証のための要求でトークンが渡されます。 このトークンが*暗号化*; が*エンコード*します。 サーバーで、その情報にアクセス トークンがデコードされます。 後続の要求でトークンを送信するには、ブラウザーのローカル ストレージに格納します。 トークンがブラウザーのローカル ストレージに格納されている場合、CSRF の脆弱性について心配いけません。 CSRF 問題では、トークンがクッキーに格納されている場合です。

### <a name="multiple-apps-hosted-at-one-domain"></a>1 つのドメインでホストされている複数のアプリ

共有ホスティング環境は、セッション ハイジャック、ログイン CSRF、およびその他の攻撃に対して脆弱です。

`example1.contoso.net`と`example2.contoso.net`は別のホストのホスト間で暗黙的な信頼リレーションシップがある、`*.contoso.net`ドメイン。 この暗黙的な信頼関係は、(HTTP クッキーには、AJAX 要求を管理する同一オリジン ポリシーは適用しないとは限りません)、互いの cookie に影響を与える可能性のある信頼されていないホストを使用します。

ドメインが共有されない、同じドメインでホストされているアプリ間の信頼された cookie を悪用した攻撃を防ぐことができます。 各アプリは、独自のドメインでホストされている、ときに、悪用する cookie の暗黙的な信頼関係はありません。

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core の偽造防止構成

> [!WARNING]
> ASP.NET Core では、偽造防止を使用して実装する[ASP.NET Core データ保護](xref:security/data-protection/introduction)します。 データ保護スタックは、サーバー ファームで動作するように構成する必要があります。 参照してください[データ保護の構成](xref:security/data-protection/configuration/overview)詳細についてはします。

ASP.NET Core 2.0 以降では、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)は HTML フォーム要素に偽造防止トークンを挿入します。 Razor ファイルで次のマークアップでは、偽造防止トークンが自動的に生成されます。

```cshtml
<form method="post">
    ...
</form>
```

同様に、 [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)フォームのメソッドが GET でない場合は、既定で偽造防止トークンを生成します。

偽造防止トークンの自動生成の HTML フォーム要素の発生時に、`<form>`タグが含まれています、`method="post"`属性と、次のいずれかに該当します。

  * Action 属性が空 (`action=""`)。
  * Action 属性が指定されていない (`<form method="post">`)。

HTML フォーム要素を偽造防止トークンの自動生成を無効にすることができます。

* 偽造防止トークンを明示的に無効化、`asp-antiforgery`属性。

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* フォーム要素はオプトアウトのタグ ヘルパーのタグ ヘルパーを使用して[! オプトアウト シンボル](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* 削除、`FormTagHelper`ビューから。 `FormTagHelper` Razor ビューに、次のディレクティブを追加することで、ビューから削除できます。

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor ページ](xref:razor-pages/index)XSRF/CSRF から自動的に保護されます。 詳細については、次を参照してください。 [XSRF/CSRF と Razor ページ](xref:razor-pages/index#xsrf)します。

CSRF 攻撃から保護する最も一般的なアプローチは、使用する、*シンクロナイザー トークン パターン*(STP)。 ユーザーがフォーム データを含むページを要求したときに、STP が使用されます。

1. サーバーは、クライアントの現在のユーザーの id に関連付けられたトークンを送信します。
1. クライアントは、検証のためのサーバーにトークンで送信バックアップします。
1. サーバーは、認証されたユーザーの id とも一致しないトークンを受信する場合は、要求は拒否されます。

トークンは、一意かつ予測不可能です。 トークンが一連の要求が適切な順序を確認することもできます (たとえば、要求シーケンスのことを確認: 1 ページ&ndash;2 ページ目&ndash;ページ 3)。 ASP.NET Core MVC と Razor ページ テンプレート内のフォームのすべての偽造防止トークンを生成します。 次の 2 つのビューの例では、偽造防止トークンを生成します。

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

偽造防止トークンを明示的に追加、 `<form>` HTML ヘルパーとタグ ヘルパーを使用することがなく要素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

前のケースの各では、ASP.NET Core は、次のような隠しフォーム フィールドを追加します。

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core には、3 つが含まれます[フィルター](xref:mvc/controllers/filters)の偽造防止トークンを使用します。

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>偽造防止オプション

カスタマイズ[偽造防止オプション](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)で`Startup.ConfigureServices`:

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
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | 偽造防止 cookie を作成するために使用する設定を決定します。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Cookie のドメイン。 既定値は `null` です。 このプロパティは廃止され、今後のバージョンで削除される予定です。 Cookie.Domain お勧めします。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | クッキーの名前。 設定されていないシステム生成一意の名前の先頭に、 [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("。AspNetCore.Antiforgery。")。 このプロパティは廃止され、今後のバージョンで削除される予定です。 Cookie.Name お勧めします。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Cookie の設定のパス。 このプロパティは廃止され、今後のバージョンで削除される予定です。 Cookie.Path お勧めします。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | ビューで偽造防止トークンを表示するために、偽造防止システムによって使用される隠しフォーム フィールドの名前。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | 偽造防止システムによって使用されるヘッダーの名前。 場合`null`フォームのデータのみが考慮されます。 |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | 偽造防止システムでは SSL が必要かどうかを指定します。 場合`true`、非 SSL 要求は失敗します。 既定値は `false` です。 このプロパティは廃止され、今後のバージョンで削除される予定です。 Cookie.SecurePolicy を設定することをお勧めします。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 生成を抑制するかどうかを指定します、`X-Frame-Options`ヘッダー。 既定では、"SAMEORIGIN"の値を持つヘッダーが生成されます。 既定値は `false` です。 |

詳細については、次を参照してください。 [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)します。

## <a name="configure-antiforgery-features-with-iantiforgery"></a>IAntiforgery 偽造防止機能を構成します。

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)偽造防止機能を構成する API を提供します。 `IAntiforgery` 要求することができます、`Configure`のメソッド、`Startup`クラス。 次の例では、偽造防止トークンを生成し、(このトピックの後半で説明、既定の Angular の名前付け規則を使用して) クッキーとしての応答で送信するアプリのホーム ページからミドルウェアを使用します。

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

### <a name="require-antiforgery-validation"></a>偽造防止検証が必要

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)個々 のアクション、コント ローラーに適用できるアクション フィルターは、グローバルにします。 要求に有効な偽造防止トークンは、しない限り、このフィルターを適用する操作に対して行われた要求はブロックされます。

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

`ValidateAntiForgeryToken`属性には、HTTP GET 要求を含む、修飾アクション メソッドへの要求のトークンが必要です。 場合、`ValidateAntiForgeryToken`アプリのコント ローラー間で属性が適用されるで上書きされること、`IgnoreAntiforgeryToken`属性。

> [!NOTE]
> ASP.NET Core では、GET 要求を偽造防止トークンを自動的に追加することをサポートしていません。

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>自動的に安全でない HTTP メソッドのみの偽造防止トークンを検証します。

ASP.NET Core アプリは、安全な HTTP メソッド (GET、HEAD、オプション、およびトレース) の偽造防止トークンを生成しません。 幅広く適用する代わりに、`ValidateAntiForgeryToken`属性とそれをオーバーライドする`IgnoreAntiforgeryToken`、属性、 [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)属性を使用できます。 この属性の動作と同様に、`ValidateAntiForgeryToken`属性は、次の HTTP メソッドを使用して行われる要求のトークンをする必要がない点が異なります。

* GET
* HEAD、
* オプション
* TRACE

使用をお勧め`AutoValidateAntiforgeryToken`API 以外のシナリオの広範です。 これにより、後の操作が既定では保護されます。 しない限り、既定では、偽造防止トークンを無視する代替手段です`ValidateAntiForgeryToken`個々 のアクション メソッドに適用されます。 これは、多くの場合のままにする POST アクション メソッドには、このシナリオで保護されていないを誤って、アプリの CSRF 攻撃に対して脆弱なままです。 すべての投稿では、偽造防止トークンを送信する必要があります。

Api は、トークンの非 cookie の一部を送信するための自動メカニズムはありません。 おそらく、実装は、クライアント コードの実装に依存します。 いくつかの例を次に示します。

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>上書きのグローバルまたはコント ローラーの偽造防止属性

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)フィルターを使用して、特定のアクション (またはコント ローラー) の偽造防止トークンの必要性を排除します。 このフィルターを適用すると、オーバーライド`ValidateAntiForgeryToken`と`AutoValidateAntiforgeryToken`(グローバルまたはコント ローラー上) より高いレベルで指定されたフィルター。

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

ビューまたは Razor ページにユーザーをリダイレクトすることにより、ユーザーが認証された後、トークンを更新する必要があります。

## <a name="javascript-ajax-and-spas"></a>JavaScript、AJAX、および Spa

従来の HTML ベースのアプリで偽造防止トークンは、隠しフォーム フィールドを使用して、サーバーに渡されます。 最新の JavaScript ベースのアプリと Spa では、多数の要求が行われるプログラムを使用します。 これらの AJAX 要求は、(要求ヘッダー、cookie など) には、その他の手法を使用して、トークンを送信することがあります。

認証トークンを格納して、サーバー上の API 要求の認証に cookie を使用する場合は、潜在的な問題は CSRF です。 トークンを格納するローカル ストレージを使用する場合は、すべての要求でサーバーにローカル ストレージからの値が自動的に送信されないため、CSRF の脆弱性を軽減する可能性があります。 そのため、ローカル ストレージを使用して、クライアントと要求ヘッダーが推奨されるアプローチとしては、トークンを送信するで偽造防止トークンを格納します。

### <a name="javascript"></a>JavaScript

JavaScript は、ビューを使用して、トークンを作成できますからビュー内のサービスを使用します。 挿入、 [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)サービスを確認して呼び出したり[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

このアプローチでは、サーバーから cookie を設定またはクライアントからの読み取りを直接処理する必要があります。

上記の例では、JavaScript を使用して、AJAX の POST ヘッダーの非表示フィールドの値を読み取る。

JavaScript は、アクセス トークンを cookie にも、cookie の内容を使用して、トークンの値を持つヘッダーを作成します。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

という名前のヘッダーでトークンを送信する要求と仮定すると、スクリプト`X-CSRF-TOKEN`、偽造防止を探してサービスを構成、`X-CSRF-TOKEN`ヘッダー。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

次の例では、適切なヘッダーでの AJAX 要求を作成するのに JavaScript を使用します。

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

AngularJS は、アドレス CSRF に規則を使用します。 サーバーが名前を持つクッキーを送信する場合`XSRF-TOKEN`、AngularJS`$http`サービスがサーバーに要求を送信するときに、ヘッダーに cookie の値を追加します。 このプロセスは自動です。 ヘッダーを明示的に設定する必要はありません。 ヘッダー名は`X-XSRF-TOKEN`します。 サーバーは、このヘッダーを検出し、その内容を検証する必要があります。

ASP.NET Core API のこの規則を使用します。

* Cookie のトークンを提供するアプリの構成`XSRF-TOKEN`します。
* 偽造防止という名前のヘッダーを検索するサービスを構成`X-XSRF-TOKEN`します。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="extend-antiforgery"></a>偽造防止を拡張します。

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)型により、開発者は各トークン内の追加データのラウンド トリップで CSRF 対策システムの動作を拡張します。 [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)メソッドが呼び出されるフィールドのトークンが生成され、戻り値が生成されたトークン内に埋め込まれています。 実装するときのタイムスタンプ、nonce、またはその他の値を返すを呼び出してでした[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)トークンが検証されると、このデータを検証します。 この情報を含める必要はありませんので、クライアントのユーザー名は、生成されたトークンに埋め込まれています。 補足データ。 ただし、トークンが含まれている場合`IAntiForgeryAdditionalDataProvider`が構成されている、補足的なデータが検証されません。

## <a name="additional-resources"></a>その他の技術情報

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))で[Web Application Security Project を開いて](https://www.owasp.org/index.php/Main_Page)(OWASP)。
* <xref:host-and-deploy/web-farm>
