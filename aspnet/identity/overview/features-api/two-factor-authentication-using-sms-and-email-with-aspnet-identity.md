---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: SMS と電子メール ASP.NET の Id を使用した 2 要素認証 |Microsoft ドキュメント
author: HaoK
description: このチュートリアルでは、SMS や電子メールを使用した 2 要素認証 (2 fa) を設定する方法を示します。 この記事は、Rick Anderson によって書き込まれました ( @RickAndMSFT ) あたり、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="3bf50-104">SMS と電子メール ASP.NET の Id を使用した 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="3bf50-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="3bf50-105">によって[ハオ最後にトリム](https://github.com/HaoK)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="3bf50-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="3bf50-106">このチュートリアルでは、SMS や電子メールを使用した 2 要素認証 (2 fa) を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="3bf50-107">この記事は、Rick Anderson によって書き込まれました ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、最後に、ハオ トリムと Suhas Joshi です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="3bf50-108">NuGet サンプルは、最後にハオ トリムによって、主に書き込まれました。</span><span class="sxs-lookup"><span data-stu-id="3bf50-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="3bf50-109">このトピックは、次について説明します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-109">This topic covers the following:</span></span>

- [<span data-ttu-id="3bf50-110">Id サンプルのビルド</span><span class="sxs-lookup"><span data-stu-id="3bf50-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="3bf50-111">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="3bf50-112">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="3bf50-113">2 要素認証プロバイダーを登録する方法</span><span class="sxs-lookup"><span data-stu-id="3bf50-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="3bf50-114">ソーシャルとローカルのログイン アカウントを結合します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="3bf50-115">ブルート フォース攻撃からのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="3bf50-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="3bf50-116">Id サンプルのビルド</span><span class="sxs-lookup"><span data-stu-id="3bf50-116">Building the Identity sample</span></span>

<span data-ttu-id="3bf50-117">このセクションでは、させるサンプルをダウンロードする NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="3bf50-118">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="3bf50-119">Visual Studio のインストール[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="3bf50-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="3bf50-120">警告: Visual Studio をインストールする必要があります[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="3bf50-121">新しい***空***ASP.NET Web プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="3bf50-122">パッケージ マネージャー コンソールで、次を入力して、次のコマンド。</span><span class="sxs-lookup"><span data-stu-id="3bf50-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="3bf50-123">このチュートリアルで使用されます[SendGrid](http://sendgrid.com/)電子メールを送信して[Twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms テキストのです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="3bf50-124">`Identity.Samples`パッケージは操作するコードをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="3bf50-125">設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="3bf50-126">*省略可能な*: の指示に従って、my[電子メール確認チュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md)に SendGrid をフックするため、アプリを実行して、電子メール アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="3bf50-127">* 省略可能: * サンプルからデモ電子メール リンク確認コードを削除 (、`ViewBag.Link`アカウント コント ローラー内のコード。</span><span class="sxs-lookup"><span data-stu-id="3bf50-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="3bf50-128">参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。</span><span class="sxs-lookup"><span data-stu-id="3bf50-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="3bf50-129"><em>省略可能: * 削除する、`ViewBag.Status`コードと、*Views\Account\VerifyCode.cshtml 管理およびアカウント コント ローラーから</em>と<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor ビュー。</span><span class="sxs-lookup"><span data-stu-id="3bf50-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="3bf50-130">または、保持することができます、`ViewBag.Status`をフックして電子メールと SMS メッセージを送信しなくてもローカルでのこのアプリの動作をテストを表示します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="3bf50-131">警告: このサンプルでは、セキュリティ設定のいずれかを変更すると、運用アプリが行われた変更を明示的に呼び出すのセキュリティ監査を受ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="3bf50-132">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="3bf50-133">このチュートリアルは、Twilio または ASPSMS のいずれかを使用するための手順を説明しますが、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="3bf50-134">**SMS プロバイダーとユーザー アカウントの作成**</span><span class="sxs-lookup"><span data-stu-id="3bf50-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="3bf50-135">作成、 [Twilio](https://www.twilio.com/try-twilio)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)アカウント。</span><span class="sxs-lookup"><span data-stu-id="3bf50-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="3bf50-136">**その他のパッケージをインストールするか、サービス参照の追加**</span><span class="sxs-lookup"><span data-stu-id="3bf50-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="3bf50-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="3bf50-137">Twilio:</span></span>  
   <span data-ttu-id="3bf50-138">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="3bf50-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3bf50-139">ASPSMS:</span></span>  
   <span data-ttu-id="3bf50-140">次のサービス参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="3bf50-141">アドレス:</span><span class="sxs-lookup"><span data-stu-id="3bf50-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="3bf50-142">名前空間:</span><span class="sxs-lookup"><span data-stu-id="3bf50-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="3bf50-143">**SMS プロバイダーのユーザーの資格情報を見つけ出し**</span><span class="sxs-lookup"><span data-stu-id="3bf50-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="3bf50-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="3bf50-144">Twilio:</span></span>  
   <span data-ttu-id="3bf50-145">**ダッシュ ボード**コピー、Twilio アカウントのタブ、**アカウント SID**と**認証トークン**です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="3bf50-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3bf50-146">ASPSMS:</span></span>  
   <span data-ttu-id="3bf50-147">アカウントの設定からに移動**ユーザー キー**自己定義と共にコピー**パスワード**です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="3bf50-148">変数に後でこれらの値を格納は`SMSAccountIdentification`と`SMSAccountPassword`です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="3bf50-149">**SenderID を指定する/発信元**</span><span class="sxs-lookup"><span data-stu-id="3bf50-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="3bf50-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="3bf50-150">Twilio:</span></span>  
   <span data-ttu-id="3bf50-151">**番号** タブで、Twilio 電話番号をコピーします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="3bf50-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3bf50-152">ASPSMS:</span></span>  
   <span data-ttu-id="3bf50-153">内で、**発信者のロックを解除** メニューの 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="3bf50-154">変数に、この値を格納おは後で`SMSAccountFrom`です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="3bf50-155">**アプリに SMS プロバイダーの資格情報を転送します。**</span><span class="sxs-lookup"><span data-stu-id="3bf50-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="3bf50-156">資格情報と差出人の電話番号を使用できるように、アプリ。</span><span class="sxs-lookup"><span data-stu-id="3bf50-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="3bf50-157">セキュリティ - ソース コード内の機密データはストアことはありません。</span><span class="sxs-lookup"><span data-stu-id="3bf50-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="3bf50-158">アカウントと資格情報は、サンプルをシンプルにする上記のコードに追加されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="3bf50-159">Jon Atten を参照してください[ASP.NET MVC: ソース管理のプライベート設定の出力を保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="3bf50-160">**SMS プロバイダーへのデータ転送の実装**</span><span class="sxs-lookup"><span data-stu-id="3bf50-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="3bf50-161">構成、`SmsService`クラス内で、*アプリ\_Start\IdentityConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3bf50-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="3bf50-162">いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。</span><span class="sxs-lookup"><span data-stu-id="3bf50-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="3bf50-163">アプリを実行し、以前に登録したアカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="3bf50-164">ユーザー ID とアクティブ化するをクリックして、`Index`でアクション メソッド`Manage`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="3bf50-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="3bf50-165">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="3bf50-166">数秒で、確認コードと共にテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="3bf50-167">これを入力し、キーを押して**送信**です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="3bf50-168">電話番号が追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="3bf50-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="3bf50-169">コードを調べます</span><span class="sxs-lookup"><span data-stu-id="3bf50-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="3bf50-170">`Index`でアクション メソッド`Manage`コント ローラーは、前のアクションに基づいて、ステータス メッセージを設定し、ローカル パスワードを変更またはローカル アカウントを追加するリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="3bf50-171">`Index`メソッドも、状態を表示したり、2 fa 電話番号、外部ログイン、有効な場合、2 fa (後述) このブラウザーの 2 fa メソッドに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3bf50-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="3bf50-172">タイトル バーに、ユーザー ID (電子メール) をクリックすると、メッセージに合格しなかったです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="3bf50-173">クリックすると、**電話番号: 削除**パスをリンク`Message=RemovePhoneSuccess`クエリ文字列として。</span><span class="sxs-lookup"><span data-stu-id="3bf50-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="3bf50-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="3bf50-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="3bf50-175">`AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力 ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="3bf50-176">クリックすると、**確認コードを送信**ボタンは、HTTP POST に電話番号をポスト`AddPhoneNumber`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="3bf50-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="3bf50-177">`GenerateChangePhoneNumberTokenAsync`メソッドは、これは、SMS メッセージで設定がセキュリティ トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="3bf50-178">トークンが文字列として送信される場合は、SMS サービスが構成されている、&quot;セキュリティ コードが&lt;トークン&gt;&quot;です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="3bf50-179">`SmsService.SendAsync`するメソッドは、非同期的にし、アプリにリダイレクト、 `VerifyPhoneNumber` (次のダイアログ ボックスが表示されます) をアクション メソッド、確認コードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="3bf50-180">コードが HTTP POST にポストされたコードを入力して送信 をクリックすると、`VerifyPhoneNumber`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="3bf50-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="3bf50-181">`ChangePhoneNumberAsync`メソッドは、ポストされたセキュリティ コードをチェックします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="3bf50-182">コードが正しい場合は、電話番号が追加、`PhoneNumber`のフィールド、`AspNetUsers`テーブル。</span><span class="sxs-lookup"><span data-stu-id="3bf50-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="3bf50-183">その呼び出しが成功した場合、`SignInAsync`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="3bf50-184">`isPersistent`パラメーターは、複数の要求で認証セッションが保存されるかどうかを設定します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="3bf50-185">新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更するときに、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。</span><span class="sxs-lookup"><span data-stu-id="3bf50-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="3bf50-186">注意してください、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。</span><span class="sxs-lookup"><span data-stu-id="3bf50-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="3bf50-187">セキュリティ クッキーに保存されていない、`AspNetUsers`テーブル (または Identity db では、他の場所から)。</span><span class="sxs-lookup"><span data-stu-id="3bf50-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="3bf50-188">Cookie のセキュリティ トークンは、自己署名を使用して[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。</span><span class="sxs-lookup"><span data-stu-id="3bf50-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="3bf50-189">Cookie ミドルウェアでは、各要求の cookie を確認します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="3bf50-190">`SecurityStampValidator`メソッドで、`Startup`クラスの DB のヒット数およびセキュリティ スタンプを定期的にチェックで指定されたとおり、`validateInterval`です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="3bf50-191">これはセキュリティ プロファイルを変更する場合を除きます (この例では) で 30 分おきのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="3bf50-192">データベースへのトリップを最小限に抑える、30 分間隔が選択されました。</span><span class="sxs-lookup"><span data-stu-id="3bf50-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="3bf50-193">`SignInAsync`メソッドは、セキュリティ プロファイルに変更されるときに呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="3bf50-194">データベースの更新プログラムは、セキュリティ プロファイルが変更されたときに、`SecurityStamp`フィールド、およびを呼び出さず、`SignInAsync`メソッドでログオンしたまま、*のみ*OWIN パイプラインを次回までデータベースにアクセスする (、 `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="3bf50-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="3bf50-195">これをテストするには変更することによって、`SignInAsync`メソッドをすぐに返すし、cookie を設定`validateInterval`を 5 秒に 30 分からのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="3bf50-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="3bf50-196">上記のコード変更では、セキュリティ プロファイルを変更することができます (の状態を変更することによって、 **2 つの要素の有効な**) 5 秒以内にログアウトするは、ときに、`SecurityStampValidator.OnValidateIdentity`メソッドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="3bf50-197">戻り値の行を削除して、`SignInAsync`メソッド、別のセキュリティ プロファイルが変更を行い、する記録されません。`SignInAsync`メソッドは、新しいセキュリティ クッキーを生成します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="3bf50-198">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-198">Enable two-factor authentication</span></span>

<span data-ttu-id="3bf50-199">サンプル アプリでは、UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="3bf50-200">2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="3bf50-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="3bf50-201">有効にする 2 fa をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="3bf50-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="3bf50-202">ログアウトし、再びログインします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-202">Log out, then log back in.</span></span> <span data-ttu-id="3bf50-203">電子メールを有効にした場合 (を参照してください [前のチュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa 用の電子メールを選択できます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="3bf50-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="3bf50-204">コードの確認 ページが表示されます (SMS または電子メール) からコードを入力できます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="3bf50-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="3bf50-205">クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、そのコンピューターとブラウザーを使用してログオンする必要があるからです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="3bf50-206">2 fa を有効にしてをクリックすると、**このブラウザーに覚えて**が提供されます 2 fa の強力な保護でコンピューターへのアクセスがあるない限り、アカウントにアクセスしようとしている悪意のあるユーザーからです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="3bf50-207">これは、定期的に使用するすべてのプライベート コンピューターで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="3bf50-208">設定して**このブラウザーに覚えて**を定期的に使用しないコンピューターから 2 fa の追加のセキュリティを取得する、および自分のコンピューターで 2 fa を通過する必要がないで利便性を取得します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="3bf50-209">2 要素認証プロバイダーを登録する方法</span><span class="sxs-lookup"><span data-stu-id="3bf50-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="3bf50-210">新しい MVC プロジェクトを作成するときに、 *IdentityConfig.cs*ファイルには、2 要素認証プロバイダーを登録する次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3bf50-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="3bf50-211">2 fa の電話番号を追加します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="3bf50-212">`AddPhoneNumber`でアクション メソッド、`Manage`コント ローラーは、セキュリティ トークンを生成する番号、電話に送信が指定したとします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="3bf50-213">トークンを送信した後にリダイレクトする、`VerifyPhoneNumber`アクション メソッド、2 fa に SMS を登録するコードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="3bf50-214">電話番号を確認するまで、SMS 2 fa は使用されません。</span><span class="sxs-lookup"><span data-stu-id="3bf50-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="3bf50-215">2 fa を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-215">Enabling 2FA</span></span>

<span data-ttu-id="3bf50-216">`EnableTFA`アクション メソッドが 2 fa を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="3bf50-217">注、`SignInAsync`有効にする 2 fa が、セキュリティ プロファイルに変更のために呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="3bf50-218">2 fa を有効にすると、ユーザーはログインに、2 fa を使用する必要は (SMS やサンプル電子メール) を登録した 2 fa アプローチを使用しています。</span><span class="sxs-lookup"><span data-stu-id="3bf50-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="3bf50-219">QR コード ジェネレーターなどの追加の 2 fa プロバイダーを追加したり、自分が所有するを記述することができます (を参照してください[ASP.NET Id の Google Authenticator を使用して](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。</span><span class="sxs-lookup"><span data-stu-id="3bf50-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="3bf50-220">使用して 2 fa コードが生成された[ワンタイム パスワードのアルゴリズムの時間に基づく](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)とコードが 6 分以内に有効です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="3bf50-221">コードを入力する 6 分以上を実行する場合は、無効なコード エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3bf50-222">ソーシャルとローカルのログイン アカウントを結合します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-222">Combine social and local login accounts</span></span>

<span data-ttu-id="3bf50-223">電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3bf50-224">次の順序で&quot; RickAndMSFT@gmail.com &quot;が最初に、ローカル ログインとして作成しますが、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="3bf50-225">をクリックして、**管理**リンクします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-225">Click on the **Manage** link.</span></span> <span data-ttu-id="3bf50-226">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="3bf50-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="3bf50-227">サービス内の別のログへのリンクをクリックし、アプリケーション要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="3bf50-228">2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="3bf50-229">ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="3bf50-230">次の図では、Tom は、ソーシャル ログイン、(からわかるようにする、**外部ログイン: 1**ページに表示されます)。</span><span class="sxs-lookup"><span data-stu-id="3bf50-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="3bf50-231">クリックすると**パスワードを選択して**同じアカウントに関連付けられているを使用すると、ローカルのログを追加します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="3bf50-232">ブルート フォース攻撃からのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="3bf50-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="3bf50-233">辞書攻撃からアプリケーションにアカウントを保護するには、ユーザーのロックアウトを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="3bf50-234">次のコードで、`ApplicationUserManager Create`メソッドはロックアウトされ、構成します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="3bf50-235">上記のコードでは、2 要素認証のみのロックアウトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="3bf50-236">変更することでのログインのロックアウトを有効にすることができます、`shouldLockout`を true に、`Login`アカウント コント ローラーのメソッド、お勧めを有効にしないでのログインのロックを可能になったため、アカウントの影響を受け[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ログイン攻撃があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="3bf50-237">サンプル コードでロックアウトは無効で、管理者アカウントの作成、`ApplicationDbInitializer Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="3bf50-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="3bf50-238">検証された電子メール アカウントのユーザーが必要</span><span class="sxs-lookup"><span data-stu-id="3bf50-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="3bf50-239">次のコードには、ユーザー ログインすることが検証済みの電子メール アカウントを持つことが必要です。</span><span class="sxs-lookup"><span data-stu-id="3bf50-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="3bf50-240">2 fa 要件の SignInManager をチェックする方法</span><span class="sxs-lookup"><span data-stu-id="3bf50-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="3bf50-241">ローカル ログインとソーシャル ログイン 2 fa が有効かどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="3bf50-242">2 fa が有効になっている場合、`SignInManager`ログオン メソッドを返します`SignInStatus.RequiresVerification`、ユーザーにリダイレクトして、`SendCode`アクション メソッド、シーケンス内のログを完了するコードを入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3bf50-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="3bf50-243">ユーザーは RememberMe 場合、が設定されている場合、ユーザーのローカル cookie で、`SignInManager`戻ります`SignInStatus.Success`2 fa を通過する必要がないとします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="3bf50-244">次のコードは、`SendCode`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="3bf50-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="3bf50-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)ユーザーに対して有効になっているすべての 2 fa メソッドで作成されます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="3bf50-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)に渡される、 [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)ヘルパーに渡し、ユーザーが 2 fa アプローチ (通常の電子メールと SMS) を選択します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="3bf50-247">ユーザーがポスト 2 fa アプローチと、`HTTP POST SendCode`アクション メソッドが呼び出されると、 `SignInManager` 2 fa コードと、ユーザーがリダイレクトされている場合に送信、`VerifyCode`アクション メソッドでログを完了するコードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="3bf50-248">2 fa ロックアウト</span><span class="sxs-lookup"><span data-stu-id="3bf50-248">2FA Lockout</span></span>

<span data-ttu-id="3bf50-249">ログイン パスワード試行の失敗では、アカウントのロックアウトを設定できますが、その手法では、現在のログインの影響を受け[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトされます。</span><span class="sxs-lookup"><span data-stu-id="3bf50-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="3bf50-250">アカウントのロックアウトを 2 fa でのみ使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="3bf50-251">ときに、`ApplicationUserManager`が作成されると、サンプル コードは 2 fa ロックアウトを設定および`MaxFailedAccessAttemptsBeforeLockout`を 5 つです。</span><span class="sxs-lookup"><span data-stu-id="3bf50-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="3bf50-252">(ローカル アカウントまたはアカウントのソーシャル) 経由ユーザーをログに記録し 2 fa に失敗した場合はそれぞれが格納されているし、最大試行回数に達すると、ユーザーはロックアウトを 5 分間 (で時間のロックアウトを設定することができます`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="3bf50-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="3bf50-253">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3bf50-253">Additional Resources</span></span>

- <span data-ttu-id="3bf50-254">[ASP.NET Id のリソースをお勧め](../getting-started/aspnet-identity-recommended-resources.md)Identity ブログ、ビデオ、チュートリアルおよび優れたの完全な一覧は、リンクします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="3bf50-255">[Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="3bf50-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="3bf50-256">[ASP.NET MVC と Id 2.0: 基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="3bf50-257">アカウントの確認と ASP.NET の Id とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="3bf50-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="3bf50-258">ASP.NET Identity 入門</span><span class="sxs-lookup"><span data-stu-id="3bf50-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="3bf50-259">[Announcing ASP.NET Identity 2.0.0 の RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi でします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="3bf50-260">[アカウントの検証と 2 要素認証を設定する ASP.NET Identity 2.0:](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten でします。</span><span class="sxs-lookup"><span data-stu-id="3bf50-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
