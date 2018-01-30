---
title: "SMS と 2 要素認証"
author: rick-anderson
description: "ASP.NET Core での 2 要素認証 (2 fa) を設定する方法を示しています。"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 7bca1c6249bebe84b532b652ab736186f35c50ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="685fd-103">SMS と 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="685fd-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="685fd-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[スイス開発者](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="685fd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="685fd-105">このチュートリアルの ASP.NET Core 対象 1.x のみです。</span><span class="sxs-lookup"><span data-stu-id="685fd-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="685fd-106">参照してください[ASP.NET Core でのアプリの認証子の QR コードを有効にすると生成](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 以降。</span><span class="sxs-lookup"><span data-stu-id="685fd-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="685fd-107">このチュートリアルでは、SMS を使用した 2 要素認証 (2 fa) を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="685fd-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="685fd-108">手順が示されて[twilio](https://www.twilio.com/)と[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="685fd-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="685fd-109">完了したことをお勧め[アカウントの確認とパスワードの回復](accconfirm.md)このチュートリアルを開始する前にします。</span><span class="sxs-lookup"><span data-stu-id="685fd-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="685fd-110">ビュー、[完成したサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)です。</span><span class="sxs-lookup"><span data-stu-id="685fd-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="685fd-111">[ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)です。</span><span class="sxs-lookup"><span data-stu-id="685fd-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="685fd-112">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="685fd-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="685fd-113">という名前の新しい ASP.NET Core web アプリを作成する`Web2FA`個々 のユーザー アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="685fd-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="685fd-114">指示に従って、 [ASP.NET Core アプリケーションでは SSL を適用する](xref:security/enforcing-ssl)を設定し、SSL を要求します。</span><span class="sxs-lookup"><span data-stu-id="685fd-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="685fd-115">SMS アカウントを作成します</span><span class="sxs-lookup"><span data-stu-id="685fd-115">Create an SMS account</span></span>

<span data-ttu-id="685fd-116">例については、SMS アカウントの作成[twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)です。</span><span class="sxs-lookup"><span data-stu-id="685fd-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="685fd-117">認証資格情報を記録 (twilio の: accountSid と ASPSMS 用の authToken: ユーザー キーとパスワード)。</span><span class="sxs-lookup"><span data-stu-id="685fd-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="685fd-118">SMS プロバイダーの資格情報を見つけ出し</span><span class="sxs-lookup"><span data-stu-id="685fd-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="685fd-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="685fd-119">**Twilio:**</span></span>  
<span data-ttu-id="685fd-120">Twilio アカウントの [ダッシュ ボード] タブから、コピー、**アカウント SID**と**認証トークン**です。</span><span class="sxs-lookup"><span data-stu-id="685fd-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="685fd-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="685fd-121">**ASPSMS:**</span></span>  
<span data-ttu-id="685fd-122">アカウントの設定に移動**ユーザー キー**コピーと共に使用して、**パスワード**です。</span><span class="sxs-lookup"><span data-stu-id="685fd-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="685fd-123">キー内のシークレット マネージャー ツールを使用してこれらの値は後で格納`SMSAccountIdentification`と`SMSAccountPassword`です。</span><span class="sxs-lookup"><span data-stu-id="685fd-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="685fd-124">SenderID を指定する/発信元</span><span class="sxs-lookup"><span data-stu-id="685fd-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="685fd-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="685fd-125">**Twilio:**</span></span>  
<span data-ttu-id="685fd-126">数値 タブで、コピー、Twilio**電話番号**です。</span><span class="sxs-lookup"><span data-stu-id="685fd-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="685fd-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="685fd-127">**ASPSMS:**</span></span>  
<span data-ttu-id="685fd-128">ロックを解除発信者メニュー内で 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="685fd-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="685fd-129">キー内のシークレット マネージャー ツールを使用してこの値は後で格納`SMSAccountFrom`です。</span><span class="sxs-lookup"><span data-stu-id="685fd-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="685fd-130">SMS サービスの資格を情報します。</span><span class="sxs-lookup"><span data-stu-id="685fd-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="685fd-131">使用して、[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="685fd-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="685fd-132">セキュリティで保護された SMS キーを取得するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="685fd-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="685fd-133">このサンプルで、`SMSoptions`でクラスを作成、 *Services/SMSoptions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="685fd-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="685fd-134">設定、 `SMSAccountIdentification`、`SMSAccountPassword`と`SMSAccountFrom`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="685fd-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="685fd-135">例:</span><span class="sxs-lookup"><span data-stu-id="685fd-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="685fd-136">SMS プロバイダーの NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="685fd-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="685fd-137">パッケージ マネージャー コンソール (PMC) を実行します。</span><span class="sxs-lookup"><span data-stu-id="685fd-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="685fd-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="685fd-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="685fd-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="685fd-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="685fd-140">コードを追加、 *Services/MessageServices.cs* SMS を有効にするファイル。</span><span class="sxs-lookup"><span data-stu-id="685fd-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="685fd-141">Twilio または ASPSMS セクションのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="685fd-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="685fd-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="685fd-142">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="685fd-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="685fd-143">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="685fd-144">使用するスタートアップを構成します。`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="685fd-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="685fd-145">追加`SMSoptions`のサービス コンテナーに、`ConfigureServices`メソッドで、 *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="685fd-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="685fd-146">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="685fd-146">Enable two-factor authentication</span></span>

<span data-ttu-id="685fd-147">開く、 *Views/Manage/Index.cshtml* Razor ファイルの表示と削除 で、コメント文字 (つまり、マークアップには、除外したりはありません)。</span><span class="sxs-lookup"><span data-stu-id="685fd-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="685fd-148">2 要素認証を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="685fd-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="685fd-149">アプリを実行して、新しいユーザーの登録</span><span class="sxs-lookup"><span data-stu-id="685fd-149">Run the app and register a new user</span></span>

![Web アプリケーション登録の Microsoft Edge でビューを開く](2fa/_static/login2fa1.png)

* <span data-ttu-id="685fd-151">アクティブ化するユーザー名をタップして、`Index`管理コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="685fd-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="685fd-152">電話番号の順にタップ**追加**リンクします。</span><span class="sxs-lookup"><span data-stu-id="685fd-152">Then tap the phone number **Add** link.</span></span>

![ビューを管理します。](2fa/_static/login2fa2.png)

* <span data-ttu-id="685fd-154">確認コードを受信してタップ、する電話番号を追加する**確認コードを送信**です。</span><span class="sxs-lookup"><span data-stu-id="685fd-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![ページの 電話番号を追加します。](2fa/_static/login2fa3.png)

* <span data-ttu-id="685fd-156">確認コードと共にテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="685fd-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="685fd-157">タップし、それを入力**送信**</span><span class="sxs-lookup"><span data-stu-id="685fd-157">Enter it and tap **Submit**</span></span>

![ページの 電話番号を確認してください。](2fa/_static/login2fa4.png)

<span data-ttu-id="685fd-159">テキスト メッセージが表示されない場合は、twilio ログ ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="685fd-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="685fd-160">電話番号が正常に追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="685fd-160">The Manage view shows your phone number was added successfully.</span></span>

![ビューを管理します。](2fa/_static/login2fa5.png)

* <span data-ttu-id="685fd-162">タップ**を有効にする**2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="685fd-162">Tap **Enable** to enable two-factor authentication.</span></span>

![ビューを管理します。](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="685fd-164">2 要素認証のテスト</span><span class="sxs-lookup"><span data-stu-id="685fd-164">Test two-factor authentication</span></span>

* <span data-ttu-id="685fd-165">ログオフします。</span><span class="sxs-lookup"><span data-stu-id="685fd-165">Log off.</span></span>

* <span data-ttu-id="685fd-166">ログイン。</span><span class="sxs-lookup"><span data-stu-id="685fd-166">Log in.</span></span>

* <span data-ttu-id="685fd-167">認証の 2 番目の要素を提供する必要があるために、ユーザー アカウントは、2 要素認証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="685fd-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="685fd-168">このチュートリアルでは、電話番号の検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="685fd-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="685fd-169">組み込みのテンプレートを使用して、2 つ目の要素として電子メールで送信を設定することもします。</span><span class="sxs-lookup"><span data-stu-id="685fd-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="685fd-170">QR コードなど、認証の他の 2 番目の要素を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="685fd-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="685fd-171">タップ**送信**です。</span><span class="sxs-lookup"><span data-stu-id="685fd-171">Tap **Submit**.</span></span>

![確認コード ビューを送信します。](2fa/_static/login2fa7.png)

* <span data-ttu-id="685fd-173">SMS メッセージで取得するコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="685fd-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="685fd-174">クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、同じデバイスおよびブラウザーを使用するときにログオンする必要があるからです。</span><span class="sxs-lookup"><span data-stu-id="685fd-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="685fd-175">2 fa を有効にしてをクリックすると**このブラウザーに覚えて**が提供されます 2 fa の強力な保護で悪意のあるユーザーがデバイスへのアクセスがあるない限り、アカウントにアクセスしようとしているからです。</span><span class="sxs-lookup"><span data-stu-id="685fd-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="685fd-176">これは、定期的に使用する任意のプライベート デバイスで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="685fd-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="685fd-177">設定して**このブラウザーに覚えて**を定期的に使用しないデバイスから 2 fa の追加のセキュリティを取得する、および独自のデバイスで 2 fa を通過する必要がないで利便性を取得します。</span><span class="sxs-lookup"><span data-stu-id="685fd-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![ビューを確認してください。](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="685fd-179">ブルート フォース攻撃から保護するためのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="685fd-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="685fd-180">2 fa でアカウントのロックアウトを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="685fd-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="685fd-181">(ローカル アカウントまたはアカウントのソーシャル) 経由ユーザーがログオン後に 2 fa に失敗した場合はそれぞれが格納されている場合 (既定値は 5) の最大試行回数に達すると、ユーザーはロックアウトを 5 分間 (で時間のロックアウトを設定することができます`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="685fd-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="685fd-182">次に、10 の試行が失敗した後 10 分間ロックアウトにアカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="685fd-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
