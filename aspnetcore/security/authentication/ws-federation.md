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
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="5f59d-103">ASP.NET Core では、Ws-federation でユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="5f59d-104">このチュートリアルは、Active Directory フェデレーション サービス (ADFS) のように WS フェデレーション認証プロバイダーでユーザーがサインインを有効にする方法を示しますまたは[Azure Active Directory](/azure/active-directory/) (AAD)。</span><span class="sxs-lookup"><span data-stu-id="5f59d-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="5f59d-105">説明されている ASP.NET Core 2.0 のサンプル アプリを使用して[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="5f59d-106">ASP.NET Core 2.0 アプリでは、Ws-federation のサポートがによって提供される[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="5f59d-107">このコンポーネントはから移植[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)とそのコンポーネントの機構を数多く共有します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="5f59d-108">ただし、コンポーネントは、いくつかの重要な点で異なります。</span><span class="sxs-lookup"><span data-stu-id="5f59d-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="5f59d-109">既定では、新しいミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="5f59d-109">By default, the new middleware:</span></span>

* <span data-ttu-id="5f59d-110">要請されていないログインは許可されません。</span><span class="sxs-lookup"><span data-stu-id="5f59d-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="5f59d-111">Ws-federation プロトコルのこの機能は、XSRF 攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="5f59d-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="5f59d-112">ただしで有効にする、`AllowUnsolicitedLogins`オプション。</span><span class="sxs-lookup"><span data-stu-id="5f59d-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="5f59d-113">サインイン メッセージのすべてのフォーム ポストをチェックしません。</span><span class="sxs-lookup"><span data-stu-id="5f59d-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="5f59d-114">要求のみ、`CallbackPath`サインオン再起動のチェックは`CallbackPath`の既定値は`/signin-wsfed`変更することが、継承を使用して[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)のプロパティ、 [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions)クラス。</span><span class="sxs-lookup"><span data-stu-id="5f59d-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="5f59d-115">このパスは、有効にすると他の認証プロバイダーと共有できる、 [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests)オプション。</span><span class="sxs-lookup"><span data-stu-id="5f59d-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="5f59d-116">Active Directory にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="5f59d-117">Active Directory フェデレーション サービス</span><span class="sxs-lookup"><span data-stu-id="5f59d-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="5f59d-118">サーバーの開く**追加証明書利用者信頼のウィザード**ADFS 管理コンソールから。</span><span class="sxs-lookup"><span data-stu-id="5f59d-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![証明書利用者の信頼ウィザードを追加します。ようこそ](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="5f59d-120">データを手動で入力を選択します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-120">Choose to enter data manually:</span></span>

![証明書利用者の信頼ウィザードを追加します。データ ソースの選択](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="5f59d-122">証明書利用者の表示名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="5f59d-123">名前は、ASP.NET Core アプリに重要ではないです。</span><span class="sxs-lookup"><span data-stu-id="5f59d-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="5f59d-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)トークン暗号化証明書を構成していないため、トークンの暗号化のサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="5f59d-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![証明書利用者の信頼ウィザードを追加します。証明書を構成します。](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="5f59d-126">アプリの URL を使用して、Ws-federation パッシブ プロトコルのサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5f59d-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="5f59d-127">アプリの適切なポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-127">Verify the port is correct for the app:</span></span>

![証明書利用者の信頼ウィザードを追加します。URL を構成します。](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="5f59d-129">これは、HTTPS URL でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="5f59d-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="5f59d-130">IIS Express と、開発中に、アプリをホストする場合に自己署名証明書を提供できます。</span><span class="sxs-lookup"><span data-stu-id="5f59d-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="5f59d-131">Kestrel では、証明書の手動構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="5f59d-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="5f59d-132">参照してください、 [Kestrel ドキュメント](xref:fundamentals/servers/kestrel)の詳細。</span><span class="sxs-lookup"><span data-stu-id="5f59d-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="5f59d-133">をクリックして **[次へ]** ウィザードの残りと**閉じる**最後にします。</span><span class="sxs-lookup"><span data-stu-id="5f59d-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="5f59d-134">ASP.NET Core Identity が必要です、**名前 ID**要求。</span><span class="sxs-lookup"><span data-stu-id="5f59d-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="5f59d-135">1 つ追加、**要求規則の編集**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="5f59d-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![要求規則を編集します。](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="5f59d-137">**変換要求規則追加ウィザード**、既定値のままに**要求として LDAP 属性を送信**選択すると、テンプレートをクリックします**次**します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="5f59d-138">ルールのマッピングを追加、 **SAM アカウント名**LDAP 属性を**名前 ID**出力方向の要求。</span><span class="sxs-lookup"><span data-stu-id="5f59d-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![変換要求規則のウィザードを追加します。要求規則を構成します。](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="5f59d-140">をクリックして**完了** > **OK**で、**要求規則の編集**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="5f59d-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="5f59d-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f59d-141">Azure Active Directory</span></span>

* <span data-ttu-id="5f59d-142">AAD テナントのアプリの登録 ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="5f59d-143">クリックして**新しいアプリケーションの登録**:</span><span class="sxs-lookup"><span data-stu-id="5f59d-143">Click **New application registration**:</span></span>

![Azure Active Directory:アプリの登録](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="5f59d-145">アプリの登録の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-145">Enter a name for the app registration.</span></span> <span data-ttu-id="5f59d-146">そうでは、ASP.NET Core アプリに重要です。</span><span class="sxs-lookup"><span data-stu-id="5f59d-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="5f59d-147">として、アプリがリッスンする URL を入力、**サインオン URL**:</span><span class="sxs-lookup"><span data-stu-id="5f59d-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory:アプリの登録を作成します。](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="5f59d-149">クリックして**エンドポイント**とに注意してください、**フェデレーション メタデータ ドキュメント**URL。</span><span class="sxs-lookup"><span data-stu-id="5f59d-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="5f59d-150">これは、Ws-federation ミドルウェアの`MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="5f59d-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory:エンドポイント](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="5f59d-152">新しいアプリの登録に移動します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-152">Navigate to the new app registration.</span></span> <span data-ttu-id="5f59d-153">をクリックして**設定** > **プロパティ**メモ、**アプリ ID URI**します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="5f59d-154">これは、Ws-federation ミドルウェアの`Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="5f59d-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory:アプリの登録プロパティ](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="5f59d-156">ASP.NET Core Identity の外部ログイン プロバイダーとして WS フェデレーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="5f59d-157">依存関係を追加[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="5f59d-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="5f59d-158">追加するには、Ws-federation `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5f59d-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="5f59d-159">WS フェデレーション ログイン</span><span class="sxs-lookup"><span data-stu-id="5f59d-159">Log in with WS-Federation</span></span>

<span data-ttu-id="5f59d-160">参照 をクリックし、アプリ、**ログイン**nav ヘッダーにリンクします。</span><span class="sxs-lookup"><span data-stu-id="5f59d-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="5f59d-161">WsFederation でログインするためのオプションがあります。![ログイン ページ](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="5f59d-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="5f59d-162">プロバイダーとして adfs を使用するには、ボタンは、ad FS サインイン ページにリダイレクトします。![Ad FS サインイン ページ](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="5f59d-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="5f59d-163">プロバイダーとして Azure Active directory、ボタンは、AAD のサインイン ページにリダイレクトします。![AAD のサインイン ページ](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="5f59d-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="5f59d-164">新しいユーザーの成功したサインインには、アプリのユーザー登録ページにリダイレクトします。![登録 ページ](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="5f59d-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="5f59d-165">ASP.NET Core Identity なしには、Ws-federation を使用します。</span><span class="sxs-lookup"><span data-stu-id="5f59d-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="5f59d-166">Identity なし Ws-federation ミドルウェアを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5f59d-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="5f59d-167">例:</span><span class="sxs-lookup"><span data-stu-id="5f59d-167">For example:</span></span>

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
