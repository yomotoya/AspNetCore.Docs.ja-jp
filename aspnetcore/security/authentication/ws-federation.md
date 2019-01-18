---
title: ASP.NET Core では、Ws-federation でユーザーを認証します。
author: chlowell
description: このチュートリアルでは、ASP.NET Core アプリで Ws-federation を使用する方法を示します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 6b568928aaf6c958d66279af9fef80ac0c968c8b
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396156"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>ASP.NET Core では、Ws-federation でユーザーを認証します。

このチュートリアルは、Active Directory フェデレーション サービス (ADFS) のように WS フェデレーション認証プロバイダーでユーザーがサインインを有効にする方法を示しますまたは[Azure Active Directory](/azure/active-directory/) (AAD)。 説明されている ASP.NET Core 2.0 のサンプル アプリを使用して[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)します。

ASP.NET Core 2.0 アプリでは、Ws-federation のサポートがによって提供される[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)します。 このコンポーネントはから移植[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)とそのコンポーネントの機構を数多く共有します。 ただし、コンポーネントは、いくつかの重要な点で異なります。

既定では、新しいミドルウェア。

* 要請されていないログインは許可されません。 Ws-federation プロトコルのこの機能は、XSRF 攻撃に対して脆弱です。 ただしで有効にする、`AllowUnsolicitedLogins`オプション。
* サインイン メッセージのすべてのフォーム ポストをチェックしません。 要求のみ、`CallbackPath`サインオン再起動のチェックは`CallbackPath`の既定値は`/signin-wsfed`変更することが、継承を使用して[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions)クラス。 このパスは、有効にすると他の認証プロバイダーと共有できる、 [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests)オプション。

## <a name="register-the-app-with-active-directory"></a>Active Directory にアプリを登録します。

### <a name="active-directory-federation-services"></a>Active Directory フェデレーション サービス

* サーバーの開く**追加証明書利用者信頼のウィザード**ADFS 管理コンソールから。

![証明書利用者の信頼ウィザードを追加します。ようこそ](ws-federation/_static/AdfsAddTrust.png)

* データを手動で入力を選択します。

![証明書利用者の信頼ウィザードを追加します。データ ソースの選択](ws-federation/_static/AdfsSelectDataSource.png)

* 証明書利用者の表示名を入力します。 名前は、ASP.NET Core アプリに重要ではないです。

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)トークン暗号化証明書を構成していないため、トークンの暗号化のサポートされていません。

![証明書利用者の信頼ウィザードを追加します。証明書を構成します。](ws-federation/_static/AdfsConfigureCert.png)

* アプリの URL を使用して、Ws-federation パッシブ プロトコルのサポートを有効にします。 アプリの適切なポートを確認します。

![証明書利用者の信頼ウィザードを追加します。URL を構成します。](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> これは、HTTPS URL でなければなりません。 IIS Express と、開発中に、アプリをホストする場合に自己署名証明書を提供できます。 Kestrel では、証明書の手動構成が必要です。 参照してください、 [Kestrel ドキュメント](xref:fundamentals/servers/kestrel)の詳細。

* をクリックして **[次へ]** ウィザードの残りと**閉じる**最後にします。

* ASP.NET Core Identity が必要です、**名前 ID**要求。 1 つ追加、**要求規則の編集**ダイアログ。

![要求規則を編集します。](ws-federation/_static/EditClaimRules.png)

* **変換要求規則追加ウィザード**、既定値のままに**要求として LDAP 属性を送信**選択すると、テンプレートをクリックします**次**します。 ルールのマッピングを追加、 **SAM アカウント名**LDAP 属性を**名前 ID**出力方向の要求。

![変換要求規則のウィザードを追加します。要求規則を構成します。](ws-federation/_static/AddTransformClaimRule.png)

* をクリックして**完了** > **OK**で、**要求規則の編集**ウィンドウ。

### <a name="azure-active-directory"></a>Azure Active Directory

* AAD テナントのアプリの登録 ブレードに移動します。 クリックして**新しいアプリケーションの登録**:

![Azure Active Directory:アプリの登録](ws-federation/_static/AadNewAppRegistration.png)

* アプリの登録の名前を入力します。 そうでは、ASP.NET Core アプリに重要です。
* として、アプリがリッスンする URL を入力、**サインオン URL**:

![Azure Active Directory:アプリの登録を作成します。](ws-federation/_static/AadCreateAppRegistration.png)

* クリックして**エンドポイント**とに注意してください、**フェデレーション メタデータ ドキュメント**URL。 これは、Ws-federation ミドルウェアの`MetadataAddress`:

![Azure Active Directory:エンドポイント](ws-federation/_static/AadFederationMetadataDocument.png)

* 新しいアプリの登録に移動します。 をクリックして**設定** > **プロパティ**メモ、**アプリ ID URI**します。 これは、Ws-federation ミドルウェアの`Wtrealm`:

![Azure Active Directory:アプリの登録プロパティ](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>ASP.NET Core Identity の外部ログイン プロバイダーとして WS フェデレーションを追加します。

* 依存関係を追加[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)をプロジェクトにします。
* 追加するには、Ws-federation `Startup.ConfigureServices`:

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

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>WS フェデレーション ログイン

参照 をクリックし、アプリ、**ログイン**nav ヘッダーにリンクします。 WsFederation でログインするためのオプションがあります。![ログイン ページ](ws-federation/_static/WsFederationButton.png)

プロバイダーとして adfs を使用するには、ボタンは、ad FS サインイン ページにリダイレクトします。![Ad FS サインイン ページ](ws-federation/_static/AdfsLoginPage.png)

プロバイダーとして Azure Active directory、ボタンは、AAD のサインイン ページにリダイレクトします。![AAD のサインイン ページ](ws-federation/_static/AadSignIn.png)

新しいユーザーの成功したサインインには、アプリのユーザー登録ページにリダイレクトします。![登録 ページ](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>ASP.NET Core Identity なしには、Ws-federation を使用します。

Identity なし Ws-federation ミドルウェアを使用できます。 例:

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
