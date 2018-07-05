---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: ASP.NET Identity で SMS と電子メールを使用する 2 要素認証 |Microsoft Docs
author: HaoK
description: このチュートリアルでは、SMS と電子メールを使用して 2 要素認証 (2 fa) を設定する方法を説明します。 この記事の執筆者は、Rick Anderson ( @RickAndMSFT ) あたり、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0519ae69b3d2ef129d206a936b199f781fef5bf6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399680"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="40f72-104">ASP.NET Identity で SMS と電子メールを使用する 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="40f72-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="40f72-105">によって[Hao 力](https://github.com/HaoK)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="40f72-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="40f72-106">このチュートリアルでは、SMS と電子メールを使用して 2 要素認証 (2 fa) を設定する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="40f72-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="40f72-107">この記事の執筆者は、Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、Hao 力、および Suhas Joshi します。</span><span class="sxs-lookup"><span data-stu-id="40f72-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="40f72-108">NuGet のサンプルは、主に Hao 力によって記述されています。</span><span class="sxs-lookup"><span data-stu-id="40f72-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="40f72-109">このトピックでは、次の項目について説明します。</span><span class="sxs-lookup"><span data-stu-id="40f72-109">This topic covers the following:</span></span>

- [<span data-ttu-id="40f72-110">ビルド Id サンプル</span><span class="sxs-lookup"><span data-stu-id="40f72-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="40f72-111">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="40f72-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="40f72-112">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="40f72-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="40f72-113">2 要素認証プロバイダーを登録する方法</span><span class="sxs-lookup"><span data-stu-id="40f72-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="40f72-114">ソーシャル、ローカルのログイン アカウントを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="40f72-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="40f72-115">ブルート フォース攻撃からのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="40f72-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="40f72-116">ビルド Id サンプル</span><span class="sxs-lookup"><span data-stu-id="40f72-116">Building the Identity sample</span></span>

<span data-ttu-id="40f72-117">このセクションでは、処理を中心にサンプルをダウンロードする NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="40f72-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="40f72-118">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。</span><span class="sxs-lookup"><span data-stu-id="40f72-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="40f72-119">Visual Studio のインストール[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="40f72-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="40f72-120">警告: Visual Studio をインストールする必要があります[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="40f72-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="40f72-121">新規作成***空***ASP.NET Web プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="40f72-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="40f72-122">パッケージ マネージャー コンソールで、次を入力します。 次のコマンド。</span><span class="sxs-lookup"><span data-stu-id="40f72-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="40f72-123">このチュートリアルで使用します[SendGrid](http://sendgrid.com/)電子メールを送信して[Twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms のようにします。</span><span class="sxs-lookup"><span data-stu-id="40f72-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="40f72-124">`Identity.Samples`パッケージで使用されるコードをインストールします。</span><span class="sxs-lookup"><span data-stu-id="40f72-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="40f72-125">設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。</span><span class="sxs-lookup"><span data-stu-id="40f72-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="40f72-126">*省略可能な*: の指示に従って、[電子メール確認チュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md)SendGrid をフックし、アプリを実行し、電子メール アカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="40f72-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="40f72-127">* 省略可能: * デモ電子メール リンク確認コード サンプルから削除 (、 `ViewBag.Link` account コント ローラー コード。</span><span class="sxs-lookup"><span data-stu-id="40f72-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="40f72-128">参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。</span><span class="sxs-lookup"><span data-stu-id="40f72-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="40f72-129"><em>省略可能: * 削除、`ViewBag.Status`コードの管理およびアカウント コント ローラーと、*Views\Account\VerifyCode.cshtml から</em>と<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor ビュー。</span><span class="sxs-lookup"><span data-stu-id="40f72-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="40f72-130">または、保持することができます、`ViewBag.Status`テスト フックして、電子メールや SMS メッセージを送信することがなくローカルでこのアプリの動作を表示します。</span><span class="sxs-lookup"><span data-stu-id="40f72-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="40f72-131">警告: このサンプルでは、セキュリティ設定のいずれかを変更すると、運用アプリが行われた変更を明示的に呼び出すセキュリティ監査を受ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f72-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="40f72-132">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="40f72-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="40f72-133">このチュートリアルでは、Twilio または ASPSMS のいずれかの使用方法について説明しますが、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="40f72-134">**SMS プロバイダーとユーザー アカウントを作成します。**</span><span class="sxs-lookup"><span data-stu-id="40f72-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="40f72-135">作成、 [Twilio](https://www.twilio.com/try-twilio)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)アカウント。</span><span class="sxs-lookup"><span data-stu-id="40f72-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="40f72-136">**その他のパッケージをインストールまたはサービス参照の追加**</span><span class="sxs-lookup"><span data-stu-id="40f72-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="40f72-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="40f72-137">Twilio:</span></span>  
   <span data-ttu-id="40f72-138">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40f72-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="40f72-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="40f72-139">ASPSMS:</span></span>  
   <span data-ttu-id="40f72-140">次のサービス参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f72-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="40f72-141">アドレス:</span><span class="sxs-lookup"><span data-stu-id="40f72-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="40f72-142">名前空間:</span><span class="sxs-lookup"><span data-stu-id="40f72-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="40f72-143">**SMS プロバイダーのユーザーの資格情報を見極める**</span><span class="sxs-lookup"><span data-stu-id="40f72-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="40f72-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="40f72-144">Twilio:</span></span>  
   <span data-ttu-id="40f72-145">**ダッシュ ボード**、Twilio アカウントの コピーのタブ、**アカウント SID**と**Auth トークン**します。</span><span class="sxs-lookup"><span data-stu-id="40f72-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="40f72-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="40f72-146">ASPSMS:</span></span>  
   <span data-ttu-id="40f72-147">移動しますから、アカウントの設定、**別子-Userkey**し、自己定義型と共にコピー**パスワード**します。</span><span class="sxs-lookup"><span data-stu-id="40f72-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="40f72-148">これらの値を変数に保存後で`SMSAccountIdentification`と`SMSAccountPassword`します。</span><span class="sxs-lookup"><span data-stu-id="40f72-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="40f72-149">**SenderID を指定する/発信元**</span><span class="sxs-lookup"><span data-stu-id="40f72-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="40f72-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="40f72-150">Twilio:</span></span>  
   <span data-ttu-id="40f72-151">**番号** タブで、Twilio 電話番号をコピーします。</span><span class="sxs-lookup"><span data-stu-id="40f72-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="40f72-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="40f72-152">ASPSMS:</span></span>  
   <span data-ttu-id="40f72-153">内で、**発信者のロックを解除**] メニューの [発信者の 1 つまたは複数のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f72-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="40f72-154">この値を変数に後で保存`SMSAccountFrom`します。</span><span class="sxs-lookup"><span data-stu-id="40f72-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="40f72-155">**アプリへの SMS プロバイダーの資格情報の転送**</span><span class="sxs-lookup"><span data-stu-id="40f72-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="40f72-156">資格情報と差出人の電話番号を使用できるように、アプリ。</span><span class="sxs-lookup"><span data-stu-id="40f72-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="40f72-157">セキュリティ - ソース コード内の機密データは store ことはありません。</span><span class="sxs-lookup"><span data-stu-id="40f72-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="40f72-158">アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="40f72-159">Jon Atten を参照してください。 [ASP.NET MVC: ソース管理のプライベート設定の出力を保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="40f72-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="40f72-160">**SMS プロバイダーへのデータ転送の実装**</span><span class="sxs-lookup"><span data-stu-id="40f72-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="40f72-161">構成、`SmsService`クラス、*アプリ\_Start\IdentityConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="40f72-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="40f72-162">いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。</span><span class="sxs-lookup"><span data-stu-id="40f72-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="40f72-163">アプリを実行し、以前に登録したアカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="40f72-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="40f72-164">アクティブ化ユーザー ID を取得するには、をクリックして、`Index`内のアクション メソッド`Manage`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="40f72-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="40f72-165">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f72-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="40f72-166">数秒で確認コードを含むテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="40f72-167">入力してキーを押して**送信**します。</span><span class="sxs-lookup"><span data-stu-id="40f72-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="40f72-168">電話番号が追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="40f72-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="40f72-169">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="40f72-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="40f72-170">`Index`内のアクション メソッド`Manage`コント ローラーは、前のアクションに基づいて、ステータス メッセージを設定し、ローカル パスワードを変更またはローカル アカウントを追加するリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="40f72-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="40f72-171">`Index`メソッドは、状態もが表示されます。 または、2 fa 電話番号、外部ログイン、2 fa を有効にし、このブラウザー (後述) の 2 fa メソッドに注意してください。</span><span class="sxs-lookup"><span data-stu-id="40f72-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="40f72-172">タイトル バーで、ユーザー ID (電子メール) をクリックすると、メッセージが合格しなかった。</span><span class="sxs-lookup"><span data-stu-id="40f72-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="40f72-173">クリックすると、**電話番号: 削除**パスをリンク`Message=RemovePhoneSuccess`クエリ文字列として。</span><span class="sxs-lookup"><span data-stu-id="40f72-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="40f72-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="40f72-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="40f72-175">`AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力するダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="40f72-176">クリックすると、**確認コードを送信**ボタンは、HTTP POST に電話番号をポスト`AddPhoneNumber`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="40f72-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="40f72-177">`GenerateChangePhoneNumberTokenAsync`メソッドは、SMS メッセージで設定がセキュリティ トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="40f72-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="40f72-178">SMS サービスが構成されている場合、トークンは、文字列として送信&quot;セキュリティ コードが&lt;トークン&gt;&quot;します。</span><span class="sxs-lookup"><span data-stu-id="40f72-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="40f72-179">`SmsService.SendAsync`にメソッドが非同期的に呼び出されるに、アプリがリダイレクトされる、 `VerifyPhoneNumber` (これは、次のダイアログが表示されます) アクション メソッドでは、確認コードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="40f72-180">コードが HTTP POST にポストされたコードを入力して送信 をクリックすると、一度`VerifyPhoneNumber`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="40f72-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="40f72-181">`ChangePhoneNumberAsync`メソッドは、ポストされたセキュリティ コードを確認します。</span><span class="sxs-lookup"><span data-stu-id="40f72-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="40f72-182">電話番号を追加するコードが正しい場合、`PhoneNumber`のフィールド、`AspNetUsers`テーブル。</span><span class="sxs-lookup"><span data-stu-id="40f72-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="40f72-183">その呼び出しが成功した場合、`SignInAsync`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="40f72-184">`isPersistent`パラメーターは、認証セッションは複数の要求にわたって保持するかどうかを設定します。</span><span class="sxs-lookup"><span data-stu-id="40f72-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="40f72-185">新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更すると、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。</span><span class="sxs-lookup"><span data-stu-id="40f72-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="40f72-186">メモ、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。</span><span class="sxs-lookup"><span data-stu-id="40f72-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="40f72-187">セキュリティ クッキーに格納されていない、`AspNetUsers`テーブル (または任意の場所で Identity DB)。</span><span class="sxs-lookup"><span data-stu-id="40f72-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="40f72-188">使用して、セキュリティ cookie トークンが自己署名[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。</span><span class="sxs-lookup"><span data-stu-id="40f72-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="40f72-189">Cookie ミドルウェアは、各要求の cookie を確認します。</span><span class="sxs-lookup"><span data-stu-id="40f72-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="40f72-190">`SecurityStampValidator`メソッドで、`Startup`クラスが、DB をヒットして、定期的にセキュリティ スタンプをチェックに指定された、`validateInterval`します。</span><span class="sxs-lookup"><span data-stu-id="40f72-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="40f72-191">これは、セキュリティ プロファイルを変更しない限り (サンプル) では 30 分のみ発生します。</span><span class="sxs-lookup"><span data-stu-id="40f72-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="40f72-192">データベースへのトリップを最小限に抑える、30 分間隔が選択されました。</span><span class="sxs-lookup"><span data-stu-id="40f72-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="40f72-193">`SignInAsync`メソッドは、セキュリティ プロファイルに変更されるときに呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f72-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="40f72-194">データベースの更新プログラムは、セキュリティ プロファイルが変更されたときに、`SecurityStamp`フィールド、および呼び出さず、`SignInAsync`メソッドでログオンしたまま、*のみ*OWIN パイプラインを次回までデータベースにアクセスする (、 `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="40f72-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="40f72-195">これをテストするには変更することで、`SignInAsync`直ちにを返すメソッドをおよび cookie を設定`validateInterval`を 5 秒に 30 分間のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="40f72-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="40f72-196">上記のコード変更では、セキュリティ プロファイルを変更することができます (の状態を変更することで、 **2 つの要素の有効な**) と 5 秒以内にログアウトするはときに、`SecurityStampValidator.OnValidateIdentity`メソッドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="40f72-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="40f72-197">戻り値の行を削除して、`SignInAsync`メソッド、もう 1 つのセキュリティ プロファイルが変更を行い、する記録されません。`SignInAsync`メソッドは新しいセキュリティ クッキーを生成します。</span><span class="sxs-lookup"><span data-stu-id="40f72-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="40f72-198">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="40f72-198">Enable two-factor authentication</span></span>

<span data-ttu-id="40f72-199">サンプル アプリでは、UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f72-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="40f72-200">2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="40f72-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="40f72-201">有効にする 2 fa をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="40f72-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="40f72-202">ログアウトし、再度ログインします。</span><span class="sxs-lookup"><span data-stu-id="40f72-202">Log out, then log back in.</span></span> <span data-ttu-id="40f72-203">電子メールを有効にした場合 (を参照してください、[前のチュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa の電子メールを選択することができます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="40f72-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="40f72-204">(SMS または電子メール) から、コードを入力できますが、コードの確認ページが表示されます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="40f72-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="40f72-205">クリックすると、**このブラウザーを記憶する** チェック ボックスの除外 2 fa を使用して、そのコンピューターとブラウザーを使用してログオンする必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="40f72-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="40f72-206">2 fa を有効にして、クリックすると、**このブラウザーを記憶する**くれます 2 fa の強力な保護、コンピューターへのアクセスがあるない限り、アカウントにアクセスしようとした悪意のあるユーザーから。</span><span class="sxs-lookup"><span data-stu-id="40f72-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="40f72-207">これは、定期的に使用してプライベートのマシンで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="40f72-208">設定して**このブラウザーを記憶する**を定期的に使用しないコンピューターから 2 fa の追加のセキュリティを取得する、独自のコンピューターに 2 fa を経由する必要があるの利便性を取得します。</span><span class="sxs-lookup"><span data-stu-id="40f72-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="40f72-209">2 要素認証プロバイダーを登録する方法</span><span class="sxs-lookup"><span data-stu-id="40f72-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="40f72-210">新しい MVC プロジェクトを作成するときに、 *IdentityConfig.cs*ファイルには、2 要素認証プロバイダーを登録するには、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="40f72-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="40f72-211">2 fa の電話番号を追加します。</span><span class="sxs-lookup"><span data-stu-id="40f72-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="40f72-212">`AddPhoneNumber`内のアクション メソッド、`Manage`コント ローラーは、セキュリティ トークンを生成および指定した番号に電話に送信します。</span><span class="sxs-lookup"><span data-stu-id="40f72-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="40f72-213">リダイレクト、トークンを送信した後、`VerifyPhoneNumber`アクション メソッド、2 fa の SMS を登録するコードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="40f72-214">SMS 2 fa は、電話番号を確認するまでは使用されません。</span><span class="sxs-lookup"><span data-stu-id="40f72-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="40f72-215">2 fa を有効にします。</span><span class="sxs-lookup"><span data-stu-id="40f72-215">Enabling 2FA</span></span>

<span data-ttu-id="40f72-216">`EnableTFA`アクション メソッドは、2 fa を使用できます。</span><span class="sxs-lookup"><span data-stu-id="40f72-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="40f72-217">注、`SignInAsync`有効にする 2 fa は、セキュリティ プロファイルに変更するために呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="40f72-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="40f72-218">2 fa を有効にすると、ユーザーは、ログインには、2 fa を使用する必要が (SMS と電子メールのサンプル) を登録した 2 fa のアプローチを使用してあります。</span><span class="sxs-lookup"><span data-stu-id="40f72-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="40f72-219">QR コード ジェネレーターなど、追加の 2 fa プロバイダーを追加するか、自分が所有するを記述できます (を参照してください[ASP.NET Identity での Google Authenticator を使用して](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。</span><span class="sxs-lookup"><span data-stu-id="40f72-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="40f72-220">使用して、2 fa コードが生成された[ワンタイム パスワード アルゴリズムの時間ベース](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)と 6 分間のコードは無効です。</span><span class="sxs-lookup"><span data-stu-id="40f72-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="40f72-221">6 分以上のコードを入力する場合、無効なコード エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="40f72-222">ソーシャル、ローカルのログイン アカウントを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="40f72-222">Combine social and local login accounts</span></span>

<span data-ttu-id="40f72-223">電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="40f72-224">次の順序で&quot; RickAndMSFT@gmail.com &quot;が最初に、ローカル ログインとして作成は、まずソーシャル ログとしてアカウントを作成し、ローカル ログインを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="40f72-225">をクリックして、**管理**リンク。</span><span class="sxs-lookup"><span data-stu-id="40f72-225">Click on the **Manage** link.</span></span> <span data-ttu-id="40f72-226">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="40f72-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="40f72-227">サービス内の別のログへのリンクをクリックし、アプリの要求をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="40f72-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="40f72-228">2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="40f72-229">ユーザー、ソーシャル ログイン認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合は、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="40f72-230">次の図では、Tom は、ソーシャル ログイン (から確認できますが、**外部ログイン: 1**ページに表示)。</span><span class="sxs-lookup"><span data-stu-id="40f72-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="40f72-231">クリックすると**パスワードの入力**同じアカウントに関連付けられたでローカルのログを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="40f72-232">ブルート フォース攻撃からのアカウントのロックアウト</span><span class="sxs-lookup"><span data-stu-id="40f72-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="40f72-233">辞書攻撃のアプリにアカウントを保護するには、ユーザーのロックアウトを有効にします。</span><span class="sxs-lookup"><span data-stu-id="40f72-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="40f72-234">次のコードで、`ApplicationUserManager Create`メソッドのロックアウトを構成します。</span><span class="sxs-lookup"><span data-stu-id="40f72-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="40f72-235">上記のコードでは、2 要素認証のみのロックアウトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="40f72-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="40f72-236">変更することでログインをロックアウトを有効にすることができます、`shouldLockout`を true に、 `Login` account コント ローラーのメソッド、お勧めしますを有効にしないでログインのロックを可能になったため、アカウントに影響を受けやすい[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ログイン攻撃です。</span><span class="sxs-lookup"><span data-stu-id="40f72-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="40f72-237">作成した管理者アカウントのサンプル コードでロックアウトが無効に、`ApplicationDbInitializer Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="40f72-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="40f72-238">検証済みの電子メール アカウントを持っているユーザーを必要とします。</span><span class="sxs-lookup"><span data-stu-id="40f72-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="40f72-239">次のコードでは、ユーザーにログインできるようにする前に、検証済みの電子メール アカウントを持つことが必要です。</span><span class="sxs-lookup"><span data-stu-id="40f72-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="40f72-240">SignInManager が 2 fa の要件を確認する方法</span><span class="sxs-lookup"><span data-stu-id="40f72-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="40f72-241">ローカル ログインとソーシャル ログイン 2 fa が有効になっているかどうかにチェックします。</span><span class="sxs-lookup"><span data-stu-id="40f72-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="40f72-242">2 fa が有効になっている場合、 `SignInManager` logon メソッドを返します`SignInStatus.RequiresVerification`とユーザーにリダイレクトされます、`SendCode`シーケンスのログを実行するコードを入力する必要が、アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="40f72-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="40f72-243">RememberMe 場合は、ユーザーが設定されている場合、ユーザーのローカル cookie で、`SignInManager`戻ります`SignInStatus.Success`2 fa を経由する必要がないとします。</span><span class="sxs-lookup"><span data-stu-id="40f72-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="40f72-244">次のコードは、`SendCode`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="40f72-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="40f72-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)は、ユーザーに有効なすべての 2 fa 方式で作成されます。</span><span class="sxs-lookup"><span data-stu-id="40f72-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="40f72-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)に渡される、 [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)ヘルパーで、2 fa のアプローチ (通常は電子メールと SMS) を選択できます。</span><span class="sxs-lookup"><span data-stu-id="40f72-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="40f72-247">ユーザーは、2 fa のアプローチを投稿すると、`HTTP POST SendCode`アクション メソッドが呼び出されると、`SignInManager`送信に 2 fa のコードと、ユーザーがリダイレクト、`VerifyCode`アクション メソッドが、ログインを実行するコードを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="40f72-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="40f72-248">2 fa のロックアウト</span><span class="sxs-lookup"><span data-stu-id="40f72-248">2FA Lockout</span></span>

<span data-ttu-id="40f72-249">ログイン パスワードの試行の失敗では、アカウントのロックアウトを設定できますが、そのアプローチにより、ログインを受けやすい[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトします。</span><span class="sxs-lookup"><span data-stu-id="40f72-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="40f72-250">2 fa でのみアカウントのロックアウトを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="40f72-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="40f72-251">ときに、`ApplicationUserManager`はサンプル コードは、作成されると、2 fa のロックアウトを設定および`MaxFailedAccessAttemptsBeforeLockout`を 5 つです。</span><span class="sxs-lookup"><span data-stu-id="40f72-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="40f72-252">(ローカル アカウントまたはソーシャル アカウント) を使って、ユーザーがログインし、2 fa に失敗した場合はそれぞれが格納されているし、ユーザーが 5 分間ロックアウトしている最大試行回数に達すると、(ロックアウトの時間を設定する`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="40f72-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="40f72-253">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="40f72-253">Additional Resources</span></span>

- <span data-ttu-id="40f72-254">[ASP.NET Identity 推奨リソース](../getting-started/aspnet-identity-recommended-resources.md)Identity ブログ、ビデオ、チュートリアル、および優れたの完全な一覧は、リンクします。</span><span class="sxs-lookup"><span data-stu-id="40f72-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="40f72-255">[Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="40f72-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="40f72-256">[ASP.NET MVC と Id 2.0: 基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。</span><span class="sxs-lookup"><span data-stu-id="40f72-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="40f72-257">アカウントの確認と ASP.NET Identity によるパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="40f72-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="40f72-258">ASP.NET Identity 入門</span><span class="sxs-lookup"><span data-stu-id="40f72-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="40f72-259">[ASP.NET Identity 2.0.0 の RTM の発表](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)Pranav Rastogi が。</span><span class="sxs-lookup"><span data-stu-id="40f72-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="40f72-260">[アカウントの検証と 2 要素認証を設定する ASP.NET Identity 2.0:](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten でします。</span><span class="sxs-lookup"><span data-stu-id="40f72-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
