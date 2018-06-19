---
title: ASP.NET Core では、Ws-federation でユーザーを認証します。
author: chlowell
description: このチュートリアルでは、ASP.NET Core アプリケーションで Ws-federation を使用する方法を示します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898805"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="d2746-103">ASP.NET Core では、Ws-federation でユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="d2746-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="d2746-104">このチュートリアルは、Active Directory フェデレーション サービス (ADFS) のような Ws-federation 認証プロバイダーを使用してサインインするユーザーを有効にする方法を示しますまたは[Azure Active Directory](/azure/active-directory/) (AAD)。</span><span class="sxs-lookup"><span data-stu-id="d2746-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="d2746-105">説明されている ASP.NET Core 2.0 のサンプル アプリを使用して[Facebook、Google、および外部プロバイダー認証](xref:security/authentication/social/index)です。</span><span class="sxs-lookup"><span data-stu-id="d2746-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d2746-106">ASP.NET Core 2.0 アプリの場合、Ws-federation サポートがによって提供される[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)です。</span><span class="sxs-lookup"><span data-stu-id="d2746-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="d2746-107">このコンポーネントがから移植された[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)とそのコンポーネントのしくみの多くを共有します。</span><span class="sxs-lookup"><span data-stu-id="d2746-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="d2746-108">ただし、コンポーネントは、いくつかの重要な点で異なります。</span><span class="sxs-lookup"><span data-stu-id="d2746-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="d2746-109">既定では、新しいミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="d2746-109">By default, the new middleware:</span></span>

* <span data-ttu-id="d2746-110">要請されていないログインを許可されていません。</span><span class="sxs-lookup"><span data-stu-id="d2746-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="d2746-111">Ws-federation プロトコルのこの機能は、XSRF 攻撃に対して脆弱です。</span><span class="sxs-lookup"><span data-stu-id="d2746-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="d2746-112">ただし、これを有効にする、`AllowUnsolicitedLogins`オプション。</span><span class="sxs-lookup"><span data-stu-id="d2746-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="d2746-113">サインイン メッセージをすべてフォーム ポストをチェックしません。</span><span class="sxs-lookup"><span data-stu-id="d2746-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="d2746-114">要求のみ、`CallbackPath`チェックされます。 次の記号`CallbackPath`の既定値は`/signin-wsfed`が変更できます。</span><span class="sxs-lookup"><span data-stu-id="d2746-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="d2746-115">このパスは、有効にすると他の認証プロバイダーと共有できる、`SkipUnrecognizedRequests`オプション。</span><span class="sxs-lookup"><span data-stu-id="d2746-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="d2746-116">Active Directory にアプリを登録します。</span><span class="sxs-lookup"><span data-stu-id="d2746-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="d2746-117">Active Directory フェデレーション サービス</span><span class="sxs-lookup"><span data-stu-id="d2746-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="d2746-118">サーバーの開く**追加証明書利用者信頼のウィザード**ADFS 管理コンソールから。</span><span class="sxs-lookup"><span data-stu-id="d2746-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![証明書利用者の信頼を追加ウィザード: へようこそ](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="d2746-120">データを手動で入力することを選択します。</span><span class="sxs-lookup"><span data-stu-id="d2746-120">Choose to enter data manually:</span></span>

![データ ソースを選択する証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="d2746-122">証明書利用者の表示名を入力します。</span><span class="sxs-lookup"><span data-stu-id="d2746-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="d2746-123">名前は、ASP.NET Core アプリケーションに重要です。</span><span class="sxs-lookup"><span data-stu-id="d2746-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="d2746-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span><span class="sxs-lookup"><span data-stu-id="d2746-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![証明書を構成する証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="d2746-126">アプリの URL を使用して、Ws-federation パッシブ プロトコルのサポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d2746-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="d2746-127">アプリの正しいポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2746-127">Verify the port is correct for the app:</span></span>

![URL の構成証明書利用者の信頼ウィザードを追加します。](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="d2746-129">これは、HTTPS URL でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="d2746-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="d2746-130">IIS Express と、開発時にアプリをホストする場合に自己署名証明書を提供できます。</span><span class="sxs-lookup"><span data-stu-id="d2746-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="d2746-131">Kestrel には、証明書の手動構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="d2746-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="d2746-132">参照してください、 [Kestrel ドキュメント](xref:fundamentals/servers/kestrel)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d2746-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="d2746-133">をクリックして **[次へ]** ウィザードの残りの手順と**閉じる**最後にします。</span><span class="sxs-lookup"><span data-stu-id="d2746-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="d2746-134">ASP.NET Core Id が必要です、**名前 ID**要求します。</span><span class="sxs-lookup"><span data-stu-id="d2746-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="d2746-135">1 つ追加、**要求規則の編集** ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="d2746-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![要求規則を編集します。](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="d2746-137">**変換要求規則ウィザードの追加**、既定値のままにして**要求として LDAP 属性を送信**、選択したテンプレートとクリック **[次へ]** です。</span><span class="sxs-lookup"><span data-stu-id="d2746-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="d2746-138">ルールのマッピングを追加、 **SAM アカウント名**LDAP 属性を**名前 ID**出力方向の要求。</span><span class="sxs-lookup"><span data-stu-id="d2746-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![要求規則の構成変換要求規則のウィザードを追加します。](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="d2746-140">をクリックして**完了** > **OK**で、**要求規則の編集**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="d2746-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="d2746-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2746-141">Azure Active Directory</span></span>

* <span data-ttu-id="d2746-142">AAD テナントのアプリの登録 ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="d2746-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="d2746-143">をクリックして**新しいアプリケーションの登録**:</span><span class="sxs-lookup"><span data-stu-id="d2746-143">Click **New application registration**:</span></span>

![Azure Active Directory: アプリの登録](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="d2746-145">アプリの登録の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="d2746-145">Enter a name for the app registration.</span></span> <span data-ttu-id="d2746-146">これは、ASP.NET Core アプリケーションに重要です。</span><span class="sxs-lookup"><span data-stu-id="d2746-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="d2746-147">として、アプリがリッスンする URL を入力、**サインオン URL**:</span><span class="sxs-lookup"><span data-stu-id="d2746-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: アプリの登録を作成します。](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="d2746-149">をクリックして**エンドポイント**し、確認、**フェデレーション メタデータ ドキュメント**URL。</span><span class="sxs-lookup"><span data-stu-id="d2746-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="d2746-150">これは、Ws-federation ミドルウェアの`MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="d2746-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: エンドポイント](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="d2746-152">新しいアプリの登録に移動します。</span><span class="sxs-lookup"><span data-stu-id="d2746-152">Navigate to the new app registration.</span></span> <span data-ttu-id="d2746-153">をクリックして**設定** > **プロパティ**しメモ、**アプリ ID URI**です。</span><span class="sxs-lookup"><span data-stu-id="d2746-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="d2746-154">これは、Ws-federation ミドルウェアの`Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="d2746-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: アプリの登録プロパティ](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="d2746-156">Ws-federation を ASP.NET Core Id の外部ログイン プロバイダーとして追加します。</span><span class="sxs-lookup"><span data-stu-id="d2746-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="d2746-157">依存関係を追加[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)をプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="d2746-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="d2746-158">追加するには、Ws-federation、`Configure`メソッド*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2746-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="d2746-159">Ws-federation を使用してログイン</span><span class="sxs-lookup"><span data-stu-id="d2746-159">Log in with WS-Federation</span></span>

<span data-ttu-id="d2746-160">参照し、アプリをクリックして、**ログイン**nav ヘッダーにリンクします。</span><span class="sxs-lookup"><span data-stu-id="d2746-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="d2746-161">WsFederation でログインするためのオプションがある:![ログイン ページ](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="d2746-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="d2746-162">プロバイダーとして adfs を使用するには、ボタンは、ad FS サインイン ページにリダイレクト: ![ad FS サインイン ページ](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="d2746-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="d2746-163">プロバイダーとして Azure Active Directory で、ボタンは AAD のサインイン ページにリダイレクト: ![AAD サインイン ページ](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="d2746-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="d2746-164">新しいユーザーの成功したサインインの [アプリのユーザーの登録] ページにリダイレクト:![登録ページ](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="d2746-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="d2746-165">ASP.NET Core Identity せずには、Ws-federation を使用します。</span><span class="sxs-lookup"><span data-stu-id="d2746-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="d2746-166">Ws-federation ミドルウェアは、Identity ことがなく使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2746-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="d2746-167">例えば:</span><span class="sxs-lookup"><span data-stu-id="d2746-167">For example:</span></span>

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
