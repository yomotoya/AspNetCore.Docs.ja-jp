---
title: "ASP.NET Core では、Ws-federation でユーザーを認証します。"
author: chlowell
description: "このチュートリアルでは、ASP.NET Core アプリケーションで Ws-federation を使用する方法を示します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>ASP.NET Core では、Ws-federation でユーザーを認証します。

このチュートリアルは、Active Directory フェデレーション サービス (ADFS) のような Ws-federation 認証プロバイダーを使用してサインインするユーザーを有効にする方法を示しますまたは[Azure Active Directory](/azure/active-directory/) (AAD)。 説明されている ASP.NET Core 2.0 のサンプル アプリを使用して[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)です。

ASP.NET Core 2.0 アプリの場合、Ws-federation サポートがによって提供される[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)です。 このコンポーネントがから移植された[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)とそのコンポーネントのしくみの多くを共有します。 ただし、コンポーネントは、いくつかの重要な点で異なります。

既定では、新しいミドルウェア。

* 要請されていないログインを許可されていません。 Ws-federation プロトコルのこの機能は、XSRF 攻撃に対して脆弱です。 ただし、これを有効にする、`AllowUnsolicitedLogins`オプション。
* サインイン メッセージをすべてフォーム ポストをチェックしません。 要求のみ、`CallbackPath`チェックされます。 次の記号`CallbackPath`の既定値は`/signin-wsfed`が変更できます。 このパスは、有効にすると他の認証プロバイダーと共有できる、`SkipUnrecognizedRequests`オプション。

## <a name="register-the-app-with-active-directory"></a>Active Directory にアプリを登録します。

### <a name="active-directory-federation-services"></a>Active Directory フェデレーション サービス

* サーバーの開く**追加証明書利用者信頼のウィザード**ADFS 管理コンソールから。

![証明書利用者の信頼を追加ウィザード: へようこそ](ws-federation/_static/AdfsAddTrust.png)

* データを手動で入力することを選択します。

![データ ソースを選択する証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsSelectDataSource.png)

* 証明書利用者の表示名を入力します。 名前は、ASP.NET Core アプリケーションに重要です。

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:

![証明書を構成する証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsConfigureCert.png)

* アプリの URL を使用して、Ws-federation パッシブ プロトコルのサポートを有効にします。 アプリの正しいポートを確認します。

![URL の構成証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> これは、HTTPS URL でなければなりません。 IIS Express と、開発時にアプリをホストする場合に自己署名証明書を提供できます。 Kestrel には、証明書の手動構成が必要です。 参照してください、 [Kestrel ドキュメント](xref:fundamentals/servers/kestrel)詳細についてはします。

* をクリックして**[次へ]**ウィザードの残りの手順と**閉じる**最後にします。

* ASP.NET Core Id が必要です、**名前 ID**要求します。 1 つ追加、**要求規則の編集** ダイアログ。

![要求規則を編集します。](ws-federation/_static/EditClaimRules.png)

* **変換要求規則ウィザードの追加**、既定値のままにして**要求として LDAP 属性を送信**、選択したテンプレートとクリック**[次へ]**です。 ルールのマッピングを追加、 **SAM アカウント名**LDAP 属性を**名前 ID**出力方向の要求。

![要求規則の構成変換要求規則のウィザードを追加します。](ws-federation/_static/AddTransformClaimRule.png)

* をクリックして**完了** > **OK**で、**要求規則の編集**ウィンドウ。

### <a name="azure-active-directory"></a>Azure Active Directory

* AAD テナントのアプリの登録 ブレードに移動します。 をクリックして**新しいアプリケーションの登録**:

![Azure Active Directory: アプリの登録](ws-federation/_static/AadNewAppRegistration.png)

* アプリの登録の名前を入力します。 これは、ASP.NET Core アプリケーションに重要です。
* として、アプリがリッスンする URL を入力、**サインオン URL**:

![Azure Active Directory: アプリの登録を作成します。](ws-federation/_static/AadCreateAppRegistration.png)

* をクリックして**エンドポイント**し、確認、**フェデレーション メタデータ ドキュメント**URL。 これは、Ws-federation ミドルウェアの`MetadataAddress`:

![Azure Active Directory: エンドポイント](ws-federation/_static/AadFederationMetadataDocument.png)

* 新しいアプリの登録に移動します。 をクリックして**設定** > **プロパティ**しメモ、**アプリ ID URI**です。 これは、Ws-federation ミドルウェアの`Wtrealm`:

![Azure Active Directory: アプリの登録プロパティ](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Ws-federation を ASP.NET Core Id の外部ログイン プロバイダーとして追加します。

* 依存関係を追加[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)をプロジェクトにします。
* 追加するには、Ws-federation、`Configure`メソッド*Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Ws-federation を使用してログイン

参照し、アプリをクリックして、**ログイン**nav ヘッダーにリンクします。 WsFederation でログインするためのオプションがある:![ログイン ページ](ws-federation/_static/WsFederationButton.png)

プロバイダーとして adfs を使用するには、ボタンは、ad FS サインイン ページにリダイレクト: ![ad FS サインイン ページ](ws-federation/_static/AdfsLoginPage.png)

プロバイダーとして Azure Active Directory で、ボタンは AAD のサインイン ページにリダイレクト: ![AAD サインイン ページ](ws-federation/_static/AadSignIn.png)

新しいユーザーの成功したサインインの [アプリのユーザーの登録] ページにリダイレクト:![登録ページ](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>ASP.NET Core Identity せずには、Ws-federation を使用します。

Ws-federation ミドルウェアは、Identity ことがなく使用できます。 例:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
