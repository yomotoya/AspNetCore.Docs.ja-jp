---
title: "SMS と 2 要素認証"
author: rick-anderson
description: "ASP.NET Core での 2 要素認証 (2 fa) を設定する方法を示しています。"
keywords: "ASP.NET Core、SMS、認証、2 fa、2 要素認証、2 要素認証"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 15620d89c4db2e74dbcec4707bb2ebc6df916e03
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="65614-104">SMS と 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="65614-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="65614-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[スイス開発者](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="65614-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="65614-106">このチュートリアルの ASP.NET Core 対象 1.x のみです。</span><span class="sxs-lookup"><span data-stu-id="65614-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="65614-107">参照してください[ASP.NET Core でのアプリの認証子の QR コードを有効にすると生成](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 以降。</span><span class="sxs-lookup"><span data-stu-id="65614-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="65614-108">このチュートリアルでは、SMS を使用した 2 要素認証 (2 fa) を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="65614-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="65614-109">手順が示されて[twilio](https://www.twilio.com/)と[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="65614-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="65614-110">完了したことをお勧め[アカウントの確認とパスワードの回復](accconfirm.md)このチュートリアルを開始する前にします。</span><span class="sxs-lookup"><span data-stu-id="65614-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="65614-111">ビュー、[完成したサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)です。</span><span class="sxs-lookup"><span data-stu-id="65614-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="65614-112">[ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)です。</span><span class="sxs-lookup"><span data-stu-id="65614-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="65614-113">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="65614-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="65614-114">という名前の新しい ASP.NET Core web アプリを作成する`Web2FA`個々 のユーザー アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="65614-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="65614-115">指示に従って、 [ASP.NET Core アプリケーションでは SSL を適用する](xref:security/enforcing-ssl)を設定し、SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="65614-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="65614-116">SMS アカウントを作成します</span><span class="sxs-lookup"><span data-stu-id="65614-116">Create an SMS account</span></span>

<span data-ttu-id="65614-117">例については、SMS アカウントの作成[twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)です。</span><span class="sxs-lookup"><span data-stu-id="65614-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="65614-118">認証資格情報を記録 (twilio の: accountSid と ASPSMS 用の authToken: ユーザー キーとパスワード)。</span><span class="sxs-lookup"><span data-stu-id="65614-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="65614-119">SMS プロバイダーの資格情報を見つけ出し</span><span class="sxs-lookup"><span data-stu-id="65614-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="65614-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="65614-120">**Twilio:**</span></span>  
<span data-ttu-id="65614-121">Twilio アカウントの [ダッシュ ボード] タブから、コピー、**アカウント SID**と**認証トークン**です。</span><span class="sxs-lookup"><span data-stu-id="65614-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="65614-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="65614-122">**ASPSMS:**</span></span>  
<span data-ttu-id="65614-123">アカウントの設定に移動**ユーザー キー**コピーと共に使用して、**パスワード**です。</span><span class="sxs-lookup"><span data-stu-id="65614-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="65614-124">キー内のシークレット マネージャー ツールを使用してこれらの値は後で格納`SMSAccountIdentification`と`SMSAccountPassword`です。</span><span class="sxs-lookup"><span data-stu-id="65614-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="65614-125">SenderID を指定する/発信元</span><span class="sxs-lookup"><span data-stu-id="65614-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="65614-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="65614-126">**Twilio:**</span></span>  
<span data-ttu-id="65614-127">数値 タブで、コピー、Twilio**電話番号**です。</span><span class="sxs-lookup"><span data-stu-id="65614-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="65614-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="65614-128">**ASPSMS:**</span></span>  
<span data-ttu-id="65614-129">ロックを解除発信者メニュー内で 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="65614-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="65614-130">キー内のシークレット マネージャー ツールを使用してこの値は後で格納`SMSAccountFrom`です。</span><span class="sxs-lookup"><span data-stu-id="65614-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="65614-131">SMS サービスの資格を情報します。</span><span class="sxs-lookup"><span data-stu-id="65614-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="65614-132">使用して、[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="65614-132">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="65614-133">セキュリティで保護された SMS キーを取得するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="65614-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="65614-134">このサンプルで、`SMSoptions`でクラスを作成、 *Services/SMSoptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="65614-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="65614-135">設定、 `SMSAccountIdentification`、`SMSAccountPassword`と`SMSAccountFrom`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="65614-135">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="65614-136">例:</span><span class="sxs-lookup"><span data-stu-id="65614-136">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="65614-137">SMS プロバイダーの NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="65614-137">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="65614-138">パッケージ マネージャー コンソール (PMC) を実行します。</span><span class="sxs-lookup"><span data-stu-id="65614-138">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="65614-139">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="65614-139">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="65614-140">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="65614-140">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="65614-141">コードを追加、 *Services/MessageServices.cs* SMS を有効にするファイル。</span><span class="sxs-lookup"><span data-stu-id="65614-141">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="65614-142">Twilio または ASPSMS セクションのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="65614-142">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="65614-143">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="65614-143">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="65614-144">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="65614-144">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="65614-145">使用するスタートアップを構成します。`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="65614-145">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="65614-146">追加`SMSoptions`のサービス コンテナーに、`ConfigureServices`メソッドで、 *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="65614-146">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="65614-147">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="65614-147">Enable two-factor authentication</span></span>

<span data-ttu-id="65614-148">開く、 *Views/Manage/Index.cshtml* Razor ファイルの表示と削除 で、コメント文字 (つまり、マークアップには、除外したりはありません)。</span><span class="sxs-lookup"><span data-stu-id="65614-148">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="65614-149">2 要素認証を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="65614-149">Log in with two-factor authentication</span></span>

* <span data-ttu-id="65614-150">アプリを実行して、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="65614-150">Run the app and register a new user</span></span>

![Web アプリケーション登録の Microsoft Edge でビューを開く](2fa/_static/login2fa1.png)

* <span data-ttu-id="65614-152">アクティブ化するユーザー名をタップして、`Index`管理コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="65614-152">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="65614-153">電話番号の順にタップ**追加**リンクします。</span><span class="sxs-lookup"><span data-stu-id="65614-153">Then tap the phone number **Add** link.</span></span>

![ビューを管理します。](2fa/_static/login2fa2.png)

* <span data-ttu-id="65614-155">確認コードを受信してタップ、する電話番号を追加する**確認コードを送信**です。</span><span class="sxs-lookup"><span data-stu-id="65614-155">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![ページの 電話番号を追加します。](2fa/_static/login2fa3.png)

* <span data-ttu-id="65614-157">確認コードと共にテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="65614-157">You will get a text message with the verification code.</span></span> <span data-ttu-id="65614-158">タップし、それを入力**送信**</span><span class="sxs-lookup"><span data-stu-id="65614-158">Enter it and tap **Submit**</span></span>

![ページの 電話番号を確認してください。](2fa/_static/login2fa4.png)

<span data-ttu-id="65614-160">テキスト メッセージが表示されない場合は、twilio ログ ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="65614-160">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="65614-161">電話番号が正常に追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="65614-161">The Manage view shows your phone number was added successfully.</span></span>

![ビューを管理します。](2fa/_static/login2fa5.png)

* <span data-ttu-id="65614-163">タップ**を有効にする**2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="65614-163">Tap **Enable** to enable two-factor authentication.</span></span>

![ビューを管理します。](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="65614-165">2 要素認証のテスト</span><span class="sxs-lookup"><span data-stu-id="65614-165">Test two-factor authentication</span></span>

* <span data-ttu-id="65614-166">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="65614-166">Log off.</span></span>

* <span data-ttu-id="65614-167">ログイン。</span><span class="sxs-lookup"><span data-stu-id="65614-167">Log in.</span></span>

* <span data-ttu-id="65614-168">認証の 2 番目の要素を提供する必要があるために、ユーザー アカウントは、2 要素認証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="65614-168">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="65614-169">このチュートリアルでは、電話番号の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="65614-169">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="65614-170">組み込みのテンプレートを使用して、2 つ目の要素として電子メールで送信を設定することもします。</span><span class="sxs-lookup"><span data-stu-id="65614-170">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="65614-171">QR コードなど、認証の他の 2 番目の要素を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="65614-171">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="65614-172">タップ**送信**です。</span><span class="sxs-lookup"><span data-stu-id="65614-172">Tap **Submit**.</span></span>

![確認コード ビューを送信します。](2fa/_static/login2fa7.png)

* <span data-ttu-id="65614-174">SMS メッセージで取得するコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="65614-174">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="65614-175">クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、同じデバイスおよびブラウザーを使用するときにログオンする必要があるからです。</span><span class="sxs-lookup"><span data-stu-id="65614-175">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="65614-176">2 fa を有効にしてをクリックすると**このブラウザーに覚えて**が提供されます 2 fa の強力な保護で悪意のあるユーザーがデバイスへのアクセスがあるない限り、アカウントにアクセスしようとしているからです。</span><span class="sxs-lookup"><span data-stu-id="65614-176">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="65614-177">これは、定期的に使用する任意のプライベート デバイスで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="65614-177">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="65614-178">設定して**このブラウザーに覚えて**を定期的に使用しないデバイスから 2 fa の追加のセキュリティを取得する、および独自のデバイスで 2 fa を通過する必要がないで利便性を取得します。</span><span class="sxs-lookup"><span data-stu-id="65614-178">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![ビューを確認してください。](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="65614-180">ブルート フォース攻撃から保護するためのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="65614-180">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="65614-181">2 fa でアカウントのロックアウトを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="65614-181">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="65614-182">(ローカル アカウントまたはアカウントのソーシャル) 経由ユーザーがログオン後に 2 fa に失敗した場合はそれぞれが格納されている場合 (既定値は 5) の最大試行回数に達すると、ユーザーはロックアウトを 5 分間 (で時間のロックアウトを設定することができます`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="65614-182">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="65614-183">次に、10 の試行が失敗した後 10 分間ロックアウトにアカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="65614-183">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
