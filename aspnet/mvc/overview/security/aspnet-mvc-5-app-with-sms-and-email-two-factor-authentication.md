---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、2 要素認証を使用した ASP.NET MVC 5 web アプリを構築する方法を示します。 Web アプリの作成、セキュリティで保護された ASP.NET MVC 5 を完了する必要があります.
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: f31015f5ebd660d0f95a2e978b305c1d272293d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834023"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="db869-104">SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ</span><span class="sxs-lookup"><span data-stu-id="db869-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="db869-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="db869-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="db869-106">このチュートリアルでは、2 要素認証を使用した ASP.NET MVC 5 web アプリを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db869-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="db869-107">完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成する](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="db869-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="db869-108">完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。</span><span class="sxs-lookup"><span data-stu-id="db869-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="db869-109">ダウンロードには、確認の電子メールと SMS を電子メールまたは SMS プロバイダーをセットアップすることがなくテストできるデバッグ ヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="db869-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="db869-110">このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="db869-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="db869-111">ASP.NET MVC アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="db869-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="db869-112">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="db869-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="db869-113">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="db869-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="db869-114">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="db869-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="db869-115">ASP.NET MVC アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="db869-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="db869-116">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。</span><span class="sxs-lookup"><span data-stu-id="db869-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="db869-117">インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="db869-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="db869-118">: 警告完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成する](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="db869-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="db869-119">インストールする必要があります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="db869-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="db869-120">新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="db869-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="db869-121">Web フォームには ASP.NET Identity もサポートしていますので、web フォーム アプリで同じ手順を実行できます。</span><span class="sxs-lookup"><span data-stu-id="db869-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="db869-122">既定の認証としてのままに**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="db869-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="db869-123">Azure でアプリをホストする場合は、チェック ボックスをオンのままにします。</span><span class="sxs-lookup"><span data-stu-id="db869-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="db869-124">チュートリアルの後半では、Azure を展開します。</span><span class="sxs-lookup"><span data-stu-id="db869-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="db869-125">できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。</span><span class="sxs-lookup"><span data-stu-id="db869-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="db869-126">設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。</span><span class="sxs-lookup"><span data-stu-id="db869-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="db869-127">2 要素認証のための SMS をセットアップします。</span><span class="sxs-lookup"><span data-stu-id="db869-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="db869-128">このチュートリアルでは、Twilio または ASPSMS のいずれかの使用方法について説明しますが、他の SMS プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="db869-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="db869-129">**SMS プロバイダーとユーザー アカウントを作成します。**</span><span class="sxs-lookup"><span data-stu-id="db869-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="db869-130">作成、 [Twilio](https://www.twilio.com/try-twilio)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)アカウント。</span><span class="sxs-lookup"><span data-stu-id="db869-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="db869-131">**その他のパッケージをインストールまたはサービス参照の追加**</span><span class="sxs-lookup"><span data-stu-id="db869-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="db869-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="db869-132">Twilio:</span></span>  
   <span data-ttu-id="db869-133">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="db869-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="db869-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="db869-134">ASPSMS:</span></span>  
   <span data-ttu-id="db869-135">次のサービス参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="db869-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="db869-136">アドレス:</span><span class="sxs-lookup"><span data-stu-id="db869-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="db869-137">名前空間:</span><span class="sxs-lookup"><span data-stu-id="db869-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="db869-138">**SMS プロバイダーのユーザーの資格情報を見極める**</span><span class="sxs-lookup"><span data-stu-id="db869-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="db869-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="db869-139">Twilio:</span></span>  
   <span data-ttu-id="db869-140">**ダッシュ ボード**、Twilio アカウントの コピーのタブ、**アカウント SID**と**Auth トークン**します。</span><span class="sxs-lookup"><span data-stu-id="db869-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="db869-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="db869-141">ASPSMS:</span></span>  
   <span data-ttu-id="db869-142">移動しますから、アカウントの設定、**別子-Userkey**し、自己定義型と共にコピー**パスワード**します。</span><span class="sxs-lookup"><span data-stu-id="db869-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="db869-143">これらの値を後で保存、 *web.config*キー内のファイル`"SMSAccountIdentification"`と`"SMSAccountPassword"`します。</span><span class="sxs-lookup"><span data-stu-id="db869-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="db869-144">**SenderID を指定する/発信元**</span><span class="sxs-lookup"><span data-stu-id="db869-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="db869-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="db869-145">Twilio:</span></span>  
   <span data-ttu-id="db869-146">**番号** タブで、Twilio 電話番号をコピーします。</span><span class="sxs-lookup"><span data-stu-id="db869-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="db869-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="db869-147">ASPSMS:</span></span>  
   <span data-ttu-id="db869-148">内で、**発信者のロックを解除**] メニューの [発信者の 1 つまたは複数のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。</span><span class="sxs-lookup"><span data-stu-id="db869-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="db869-149">後でこの値を保存、 *web.config*キー内のファイル`"SMSAccountFrom"`します。</span><span class="sxs-lookup"><span data-stu-id="db869-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="db869-150">**アプリへの SMS プロバイダーの資格情報の転送**</span><span class="sxs-lookup"><span data-stu-id="db869-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="db869-151">資格情報と差出人の電話番号を使用できるように、アプリ。</span><span class="sxs-lookup"><span data-stu-id="db869-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="db869-152">これらの値を保存する単純なために、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db869-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="db869-153">Azure にデプロイする、値で安全に保存できる、**アプリ設定**セクションで、web サイト タブを構成します。</span><span class="sxs-lookup"><span data-stu-id="db869-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="db869-154">セキュリティ - ソース コード内の機密データは store ことはありません。</span><span class="sxs-lookup"><span data-stu-id="db869-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="db869-155">アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。</span><span class="sxs-lookup"><span data-stu-id="db869-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="db869-156">参照してください[ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。</span><span class="sxs-lookup"><span data-stu-id="db869-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="db869-157">**SMS プロバイダーへのデータ転送の実装**</span><span class="sxs-lookup"><span data-stu-id="db869-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="db869-158">構成、`SmsService`クラス、*アプリ\_Start\IdentityConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db869-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="db869-159">いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。</span><span class="sxs-lookup"><span data-stu-id="db869-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="db869-160">更新プログラム、 *Views\Manage\Index.cshtml* Razor ビュー: (注: だけで既存のコードにコメントを解除して、次のコードを使用しない)。</span><span class="sxs-lookup"><span data-stu-id="db869-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="db869-161">確認、`EnableTwoFactorAuthentication`と`DisableTwoFactorAuthentication`内のアクション メソッド、`ManageController`が、[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="db869-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="db869-162">アプリを実行し、以前に登録したアカウントでログインします。</span><span class="sxs-lookup"><span data-stu-id="db869-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="db869-163">アクティブ化ユーザー ID を取得するには、をクリックして、`Index`内のアクション メソッド`Manage`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="db869-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="db869-164">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="db869-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="db869-165">`AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力するダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db869-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="db869-166">数秒で確認コードを含むテキスト メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db869-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="db869-167">入力してキーを押して**送信**します。</span><span class="sxs-lookup"><span data-stu-id="db869-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="db869-168">電話番号が追加された、管理ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="db869-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="db869-169">2 要素認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="db869-169">Enable two-factor authentication</span></span>

<span data-ttu-id="db869-170">生成されたテンプレートのアプリケーションで UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="db869-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="db869-171">2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="db869-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="db869-172">有効にする 2 fa をクリックします。</span><span class="sxs-lookup"><span data-stu-id="db869-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="db869-173">ログアウトし、再びログインします。</span><span class="sxs-lookup"><span data-stu-id="db869-173">Logout, then log back in.</span></span> <span data-ttu-id="db869-174">電子メールを有効にした場合 (を参照してください、[前のチュートリアル](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa の電子メールを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="db869-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="db869-175">(SMS または電子メール) から、コードを入力できますが、コードの確認ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db869-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="db869-176">クリックすると、**このブラウザーを記憶する** チェック ボックスの除外 2 fa を使用して、チェック ボックスに、ブラウザーとデバイスを使用する場合にログインする必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="db869-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="db869-177">悪意のあるユーザーは、デバイスへのアクセスを得ることはできません、限り、2 fa を有効にすると、クリックすると、**このブラウザーを記憶する**は便利な 1 つのステップ パスワード アクセスを提供、すべてのアクセスの保護を強力な 2 fa を維持しながら信頼されていないデバイス。</span><span class="sxs-lookup"><span data-stu-id="db869-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="db869-178">これは、定期的に使用するプライベートあらゆるデバイスで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="db869-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="db869-179">このチュートリアルでは、新しい ASP.NET MVC アプリで 2 fa を有効にする簡単な説明を提供します。</span><span class="sxs-lookup"><span data-stu-id="db869-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="db869-180">拙著のチュートリアル[ASP.NET Identity で SMS と電子メールを使用する 2 要素認証](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)サンプルの背後にあるコードの詳細を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="db869-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="db869-181">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="db869-181">Additional Resources</span></span>

- <span data-ttu-id="db869-182">[ASP.NET Identity で SMS と電子メールを使用する 2 要素認証](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)に 2 要素認証の詳細</span><span class="sxs-lookup"><span data-stu-id="db869-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="db869-183">リンクを ASP.NET Identity 推奨リソース</span><span class="sxs-lookup"><span data-stu-id="db869-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="db869-184">[アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、回復、アカウントのパスワードの確認にします。</span><span class="sxs-lookup"><span data-stu-id="db869-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="db869-185">[Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルでは、Facebook や Google の OAuth 2 承認を持つ ASP.NET MVC 5 アプリを記述する方法。</span><span class="sxs-lookup"><span data-stu-id="db869-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="db869-186">Id のデータベースにデータを追加する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="db869-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="db869-187">[Azure の Web へのメンバーシップ、OAuth、SQL Database と安全な ASP.NET MVC アプリのデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。</span><span class="sxs-lookup"><span data-stu-id="db869-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="db869-188">このチュートリアルでは、Azure のデプロイを追加します。 ロールを使用してアプリをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="db869-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="db869-189">OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="db869-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="db869-190">Facebook でアプリを作成し、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="db869-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="db869-191">プロジェクトの SSL の設定</span><span class="sxs-lookup"><span data-stu-id="db869-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
