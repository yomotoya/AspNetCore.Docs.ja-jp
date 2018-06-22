---
title: ASP.NET Core での SMS で 2 要素認証
author: rick-anderson
description: ASP.NET Core のアプリに 2 要素認証 (2 fa) を設定する方法を説明します。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272310"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="13188-103">ASP.NET Core での SMS で 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="13188-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="13188-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[スイス開発者](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="13188-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="13188-105">参照してください[ASP.NET Core でのアプリの認証子の QR コードを有効にする生成](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 以降。</span><span class="sxs-lookup"><span data-stu-id="13188-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="13188-106">このチュートリアルでは、SMS を使用した 2 要素認証 (2 fa) を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="13188-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="13188-107">手順が示されて[twilio](https://www.twilio.com/)と[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="13188-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="13188-108">完了したことをお勧め[アカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)このチュートリアルを開始する前にします。</span><span class="sxs-lookup"><span data-stu-id="13188-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="13188-109">ビュー、[完成したサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)です。</span><span class="sxs-lookup"><span data-stu-id="13188-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="13188-110">[ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)です。</span><span class="sxs-lookup"><span data-stu-id="13188-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="13188-111">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="13188-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="13188-112">という名前の新しい ASP.NET Core web アプリを作成する`Web2FA`個々 のユーザー アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="13188-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="13188-113">指示に従って、 [ASP.NET Core アプリケーションでは SSL を強制](xref:security/enforcing-ssl)を設定し、SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="13188-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="13188-114">SMS アカウントを作成します</span><span class="sxs-lookup"><span data-stu-id="13188-114">Create an SMS account</span></span>

<span data-ttu-id="13188-115">例については、SMS アカウントの作成[twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)です。</span><span class="sxs-lookup"><span data-stu-id="13188-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="13188-116">認証資格情報を記録 (twilio の: accountSid と ASPSMS 用の authToken: ユーザー キーとパスワード)。</span><span class="sxs-lookup"><span data-stu-id="13188-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="13188-117">SMS プロバイダーの資格情報を見つけ出し</span><span class="sxs-lookup"><span data-stu-id="13188-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="13188-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="13188-118">**Twilio:**</span></span>  
<span data-ttu-id="13188-119">Twilio アカウントの [ダッシュ ボード] タブから、コピー、**アカウント SID**と**認証トークン**です。</span><span class="sxs-lookup"><span data-stu-id="13188-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="13188-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="13188-120">**ASPSMS:**</span></span>  
<span data-ttu-id="13188-121">アカウントの設定に移動**ユーザー キー**コピーと共に使用して、**パスワード**です。</span><span class="sxs-lookup"><span data-stu-id="13188-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="13188-122">キー内のシークレット マネージャー ツールを使用してこれらの値は後で格納`SMSAccountIdentification`と`SMSAccountPassword`です。</span><span class="sxs-lookup"><span data-stu-id="13188-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="13188-123">SenderID を指定する/発信元</span><span class="sxs-lookup"><span data-stu-id="13188-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="13188-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="13188-124">**Twilio:**</span></span>  
<span data-ttu-id="13188-125">数値 タブで、コピー、Twilio**電話番号**です。</span><span class="sxs-lookup"><span data-stu-id="13188-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="13188-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="13188-126">**ASPSMS:**</span></span>  
<span data-ttu-id="13188-127">ロックを解除発信者メニュー内で 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="13188-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="13188-128">キー内のシークレット マネージャー ツールを使用してこの値は後で格納`SMSAccountFrom`です。</span><span class="sxs-lookup"><span data-stu-id="13188-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="13188-129">SMS サービスの資格を情報します。</span><span class="sxs-lookup"><span data-stu-id="13188-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="13188-130">使用して、[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="13188-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="13188-131">セキュリティで保護された SMS キーを取得するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="13188-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="13188-132">このサンプルで、`SMSoptions`でクラスを作成、 *Services/SMSoptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="13188-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="13188-133">設定、 `SMSAccountIdentification`、`SMSAccountPassword`と`SMSAccountFrom`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="13188-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="13188-134">例えば:</span><span class="sxs-lookup"><span data-stu-id="13188-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="13188-135">SMS プロバイダーの NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="13188-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="13188-136">パッケージ マネージャー コンソール (PMC) を実行します。</span><span class="sxs-lookup"><span data-stu-id="13188-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="13188-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="13188-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="13188-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="13188-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="13188-139">コードを追加、 *Services/MessageServices.cs* SMS を有効にするファイル。</span><span class="sxs-lookup"><span data-stu-id="13188-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="13188-140">Twilio または ASPSMS セクションのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="13188-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="13188-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="13188-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="13188-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="13188-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="13188-143">使用するスタートアップを構成します。 `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="13188-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="13188-144">追加`SMSoptions`のサービス コンテナーに、`ConfigureServices`メソッドで、 *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="13188-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="13188-145">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="13188-145">Enable two-factor authentication</span></span>

<span data-ttu-id="13188-146">開く、 *Views/Manage/Index.cshtml* Razor ファイルの表示と削除 で、コメント文字 (つまり、マークアップには、除外したりはありません)。</span><span class="sxs-lookup"><span data-stu-id="13188-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="13188-147">2 要素認証を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="13188-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="13188-148">アプリを実行して、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="13188-148">Run the app and register a new user</span></span>

![Web アプリケーション登録の Microsoft Edge でビューを開く](2fa/_static/login2fa1.png)

* <span data-ttu-id="13188-150">アクティブ化するユーザー名をタップして、`Index`管理コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="13188-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="13188-151">電話番号の順にタップ**追加**リンクします。</span><span class="sxs-lookup"><span data-stu-id="13188-151">Then tap the phone number **Add** link.</span></span>

![ビューを管理します。](2fa/_static/login2fa2.png)

* <span data-ttu-id="13188-153">確認コードを受信してタップ、する電話番号を追加する**確認コードを送信**です。</span><span class="sxs-lookup"><span data-stu-id="13188-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![ページの 電話番号を追加します。](2fa/_static/login2fa3.png)

* <span data-ttu-id="13188-155">確認コードと共にテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="13188-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="13188-156">タップし、それを入力**送信**</span><span class="sxs-lookup"><span data-stu-id="13188-156">Enter it and tap **Submit**</span></span>

![ページの 電話番号を確認してください。](2fa/_static/login2fa4.png)

<span data-ttu-id="13188-158">テキスト メッセージが表示されない場合は、twilio ログ ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="13188-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="13188-159">電話番号が正常に追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="13188-159">The Manage view shows your phone number was added successfully.</span></span>

![ビューを管理します。](2fa/_static/login2fa5.png)

* <span data-ttu-id="13188-161">タップ**を有効にする**2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="13188-161">Tap **Enable** to enable two-factor authentication.</span></span>

![ビューを管理します。](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="13188-163">2 要素認証のテスト</span><span class="sxs-lookup"><span data-stu-id="13188-163">Test two-factor authentication</span></span>

* <span data-ttu-id="13188-164">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="13188-164">Log off.</span></span>

* <span data-ttu-id="13188-165">ログイン。</span><span class="sxs-lookup"><span data-stu-id="13188-165">Log in.</span></span>

* <span data-ttu-id="13188-166">認証の 2 番目の要素を提供する必要があるために、ユーザー アカウントは、2 要素認証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="13188-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="13188-167">このチュートリアルでは、電話番号の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="13188-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="13188-168">組み込みのテンプレートを使用して、2 つ目の要素として電子メールで送信を設定することもします。</span><span class="sxs-lookup"><span data-stu-id="13188-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="13188-169">QR コードなど、認証の他の 2 番目の要素を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="13188-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="13188-170">タップ**送信**です。</span><span class="sxs-lookup"><span data-stu-id="13188-170">Tap **Submit**.</span></span>

![確認コード ビューを送信します。](2fa/_static/login2fa7.png)

* <span data-ttu-id="13188-172">SMS メッセージで取得するコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="13188-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="13188-173">クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、同じデバイスおよびブラウザーを使用するときにログオンする必要があるからです。</span><span class="sxs-lookup"><span data-stu-id="13188-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="13188-174">2 fa を有効にしてをクリックすると**このブラウザーに覚えて**が提供されます 2 fa の強力な保護で悪意のあるユーザーがデバイスへのアクセスがあるない限り、アカウントにアクセスしようとしているからです。</span><span class="sxs-lookup"><span data-stu-id="13188-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="13188-175">これは、定期的に使用する任意のプライベート デバイスで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="13188-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="13188-176">設定して**このブラウザーに覚えて**を定期的に使用しないデバイスから 2 fa の追加のセキュリティを取得する、および独自のデバイスで 2 fa を通過する必要がないで利便性を取得します。</span><span class="sxs-lookup"><span data-stu-id="13188-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![ビューを確認してください。](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="13188-178">ブルート フォース攻撃から保護するためのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="13188-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="13188-179">2 fa では、アカウントのロックアウトをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="13188-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="13188-180">ユーザーがローカル アカウントやソーシャル アカウントでサインイン 2 fa に失敗した場合はそれぞれが格納されます。</span><span class="sxs-lookup"><span data-stu-id="13188-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="13188-181">ユーザーをロックアウトする最大の失敗したアクセス試行回数に達した場合 (既定: 5 アクセス試行の失敗後 5 分間ロックアウト)。</span><span class="sxs-lookup"><span data-stu-id="13188-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="13188-182">成功した認証は、失敗したアクセス試行数をリセットし、時計をリセットします。</span><span class="sxs-lookup"><span data-stu-id="13188-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="13188-183">失敗したアクセス試行の最大とでのロックアウト時間を設定できる[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)と[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)です。</span><span class="sxs-lookup"><span data-stu-id="13188-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="13188-184">次は、アクセスの試行が 10 回失敗後 10 分のアカウントのロックアウトを構成します。</span><span class="sxs-lookup"><span data-stu-id="13188-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="13188-185">いることを確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)設定`lockoutOnFailure`に`true`:</span><span class="sxs-lookup"><span data-stu-id="13188-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
