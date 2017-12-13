---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "電子メールの確認とパスワードのリセット (c#) で、ログでセキュリティで保護された ASP.NET MVC 5 web アプリを作成 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、確認の電子メールとパスワード リセット、Asp.net メンバーシップ システムを使用した ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。 . Ca"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e3d8ad6e00b7fcb95f1c9bbe556021269c1a0624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="933ab-104">電子メールの確認とパスワードのリセット (c#) で、ログでセキュリティで保護された ASP.NET MVC 5 web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="933ab-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="933ab-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="933ab-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="933ab-106">このチュートリアルでは、確認の電子メールとパスワード リセット、Asp.net メンバーシップ システムを使用した ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="933ab-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="933ab-107">完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="933ab-108">ダウンロードには、電子メールや SMS プロバイダーを設定せずに確認の電子メールと SMS をテストすることのできるデバッグのヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="933ab-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="933ab-109">このチュートリアルは、によって書き込まれました[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="933ab-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="933ab-110">ASP.NET MVC アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="933ab-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="933ab-111">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="933ab-112">インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="933ab-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="933ab-113">警告: インストールする必要あります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="933ab-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="933ab-114">新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="933ab-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="933ab-115">Web フォームは、web フォーム アプリケーションで同じ手順を実行できなかったために、ASP.NET Identity もサポートします。</span><span class="sxs-lookup"><span data-stu-id="933ab-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="933ab-116">既定の認証としてのままにして**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="933ab-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="933ab-117">Azure でアプリケーションをホストする場合は、チェック ボックスをオンのままにします。</span><span class="sxs-lookup"><span data-stu-id="933ab-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="933ab-118">このチュートリアルで後ほどは、Azure に配置します。</span><span class="sxs-lookup"><span data-stu-id="933ab-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="933ab-119">実行できます[無料の Azure アカウントを開設](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-119">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="933ab-120">設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="933ab-121">アプリを実行する をクリックして、**登録**リンクし、ユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="933ab-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="933ab-122">この時点では、電子メールにのみ、検証、 [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="933ab-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="933ab-123">サーバー エクスプ ローラーに移動**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開き**です。</span><span class="sxs-lookup"><span data-stu-id="933ab-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="933ab-124">次の図は、`AspNetUsers`スキーマ。</span><span class="sxs-lookup"><span data-stu-id="933ab-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="933ab-125">右クリックして、 **AspNetUsers**テーブルを選択して**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="933ab-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="933ab-126">この時点で、電子メールが確認されていません。</span><span class="sxs-lookup"><span data-stu-id="933ab-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="933ab-127">行をクリックし、[削除] を選択します。</span><span class="sxs-lookup"><span data-stu-id="933ab-127">Click on the row and select delete.</span></span> <span data-ttu-id="933ab-128">次の手順でこの電子メールをもう一度追加して、確認の電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="933ab-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="933ab-129">確認の電子メール</span><span class="sxs-lookup"><span data-stu-id="933ab-129">Email confirmation</span></span>

<span data-ttu-id="933ab-130">他のユーザーを偽装するしていないことを確認する新規ユーザーの登録の電子メール アドレスを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。</span><span class="sxs-lookup"><span data-stu-id="933ab-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="933ab-131">しないようにする、ディスカッション フォーラムするいたと仮定`"bob@example.com"`としての登録から`"joe@contoso.com"`です。</span><span class="sxs-lookup"><span data-stu-id="933ab-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="933ab-132">電子メールの確認なし`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="933ab-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="933ab-133">として誤って Bob が登録されていると仮定します`"bib@example.com"`いなかったが検出されました。 これは、と彼は、アプリは、正しいメール アドレスが見つからないため、パスワードの回復を使用できなくします。</span><span class="sxs-lookup"><span data-stu-id="933ab-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="933ab-134">確認の電子メールが bot から制限の保護のみを提供し、決定スパム攻撃者から保護を提供しません、登録に使用できる多くの作業電子メール エイリアスが。</span><span class="sxs-lookup"><span data-stu-id="933ab-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="933ab-135">一般にする新しいユーザーが電子メール、SMS テキスト メッセージ、または別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="933ab-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="933ab-136">以下のセクションでは確認の電子メールを有効にして新たに登録されたユーザーが自分の電子メールが確認されるまでログ記録するを防ぐためにコードを変更します。、</span><span class="sxs-lookup"><span data-stu-id="933ab-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="933ab-137">SendGrid をフックします。</span><span class="sxs-lookup"><span data-stu-id="933ab-137">Hook up SendGrid</span></span>

<span data-ttu-id="933ab-138">このチュートリアルはのみを使用して電子メール通知を追加する方法を示しますが[SendGrid](http://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="933ab-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="933ab-139">パッケージ マネージャー コンソールで、次を入力して、次のコマンド。</span><span class="sxs-lookup"><span data-stu-id="933ab-139">In the Package Manager Console, enter the following the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="933ab-140">移動して、 [Azure SendGrid のサインアップ ページ](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)SendGrid アカウントの登録は無料とします。</span><span class="sxs-lookup"><span data-stu-id="933ab-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for free SendGrid account.</span></span> <span data-ttu-id="933ab-141">SendGrid を構成するのには、次のようなコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="933ab-141">Add code similar to the following to configure SendGrid:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="933ab-142">以下を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="933ab-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="933ab-143">このサンプルをシンプルにするを保存することがアプリの設定、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="933ab-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="933ab-144">セキュリティ - ソース コード内の機密データはストアことはありません。</span><span class="sxs-lookup"><span data-stu-id="933ab-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="933ab-145">アカウントと資格情報は、appSetting に格納されます。</span><span class="sxs-lookup"><span data-stu-id="933ab-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="933ab-146">Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。</span><span class="sxs-lookup"><span data-stu-id="933ab-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="933ab-147">参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="933ab-148">アカウント コント ローラーの確認の電子メールを有効にします。</span><span class="sxs-lookup"><span data-stu-id="933ab-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="933ab-149">確認、 *Views\Account\ConfirmEmail.cshtml*ファイルが正しい razor 構文を持ちます。</span><span class="sxs-lookup"><span data-stu-id="933ab-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="933ab-150">(、@、最初の文字、行が、不足している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="933ab-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="933ab-151">)</span><span class="sxs-lookup"><span data-stu-id="933ab-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="933ab-152">アプリを実行して、登録のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="933ab-152">Run the app and click the Register link.</span></span> <span data-ttu-id="933ab-153">登録フォームを送信すると後ログインしています。</span><span class="sxs-lookup"><span data-stu-id="933ab-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="933ab-154">電子メール アカウントを確認し、電子メールを確認するリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="933ab-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="933ab-155">ログインする前に確認の電子メールが必要</span><span class="sxs-lookup"><span data-stu-id="933ab-155">Require email confirmation before log in</span></span>

<span data-ttu-id="933ab-156">現在、ユーザーが完了したら、登録フォームに記録されます。</span><span class="sxs-lookup"><span data-stu-id="933ab-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="933ab-157">ログを記録する前に自分の電子メールを確認する一般にできます。</span><span class="sxs-lookup"><span data-stu-id="933ab-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="933ab-158">以下のセクションでおを新しいユーザーを認証するには記録前に確認された電子メールがある () を必要とするコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="933ab-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="933ab-159">更新プログラム、`HttpPost Register`メソッドを次の強調表示された変更。</span><span class="sxs-lookup"><span data-stu-id="933ab-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="933ab-160">コメント アウトすることによって、`SignInAsync`メソッド、ユーザーは署名されません登録しています。</span><span class="sxs-lookup"><span data-stu-id="933ab-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="933ab-161">`TempData["ViewBagLink"] = callbackUrl;`行に使用できる[、アプリのデバッグ](#dbg)および電子メールを送信せずに登録をテストします。</span><span class="sxs-lookup"><span data-stu-id="933ab-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="933ab-162">`ViewBag.Message`確認の手順の表示に使用します。</span><span class="sxs-lookup"><span data-stu-id="933ab-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="933ab-163">[サンプルをダウンロード](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)電子メールの設定せずに確認の電子メールをテストするコードが含まれており、アプリケーションをデバッグするも使用できます。</span><span class="sxs-lookup"><span data-stu-id="933ab-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="933ab-164">作成、`Views\Shared\Info.cshtml`ファイルし、次の razor マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="933ab-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="933ab-165">追加、 [Authorize attribute](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)を`Contact`Home コント ローラーのアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="933ab-165">Add the [Authorize attribute](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="933ab-166">クリックを使用することができます、**連絡先**リンク匿名ユーザーがアクセスを持っていないし、認証されたユーザーがアクセスを確認します。</span><span class="sxs-lookup"><span data-stu-id="933ab-166">You can use click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="933ab-167">更新する必要があります、`HttpPost Login`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="933ab-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="933ab-168">更新プログラム、 *Views\Shared\Error.cshtml*エラー メッセージを表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="933ab-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="933ab-169">内の任意のアカウントを削除、 **AspNetUsers**を使用してテストする電子メールのエイリアスを含むテーブル。</span><span class="sxs-lookup"><span data-stu-id="933ab-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="933ab-170">アプリを実行して、電子メール アドレスを確認するまでにログインできないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="933ab-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="933ab-171">クリックして、電子メール アドレスのことを確認したら、**連絡先**リンクします。</span><span class="sxs-lookup"><span data-stu-id="933ab-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="933ab-172">パスワードの回復またはリセット</span><span class="sxs-lookup"><span data-stu-id="933ab-172">Password recovery/reset</span></span>

<span data-ttu-id="933ab-173">コメント文字を削除、`HttpPost ForgotPassword`でアカウント コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="933ab-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="933ab-174">コメント文字を削除、 `ForgotPassword` [ActionLink](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)で、 *Views\Account\Login.cshtml* razor ファイルの表示。</span><span class="sxs-lookup"><span data-stu-id="933ab-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="933ab-175">ページのログでは、パスワード リセットへのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="933ab-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="933ab-176">再送信する電子メールの確認のリンク</span><span class="sxs-lookup"><span data-stu-id="933ab-176">Resend email confirmation link</span></span>

<span data-ttu-id="933ab-177">ユーザーは、新しいローカル アカウントを作成した後が確認のリンクを使用してログオンする前にする必要が電子メールで送信します。</span><span class="sxs-lookup"><span data-stu-id="933ab-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="933ab-178">誤ってユーザーが、確認メールを削除または電子メールが到着することはありません、確認のリンクを再送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="933ab-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="933ab-179">次のコード変更では、これを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="933ab-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="933ab-180">一番下に次のヘルパー メソッドを追加、 *Controllers\AccountController.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="933ab-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="933ab-181">新しいヘルパーを使用して Register メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="933ab-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="933ab-182">ログイン メソッドを更新して、パスワードを再送信するときに、ユーザー アカウントが確認されていない場合。</span><span class="sxs-lookup"><span data-stu-id="933ab-182">Update the Login method to resend the password when if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="933ab-183">ソーシャルとローカルのログイン アカウントを結合します。</span><span class="sxs-lookup"><span data-stu-id="933ab-183">Combine social and local login accounts</span></span>

<span data-ttu-id="933ab-184">電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="933ab-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="933ab-185">次の順序で **RickAndMSFT@gmail.com** が最初に、ローカル ログインとして作成しますが、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="933ab-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="933ab-186">をクリックして、**管理**リンクします。</span><span class="sxs-lookup"><span data-stu-id="933ab-186">Click on the **Manage** link.</span></span> <span data-ttu-id="933ab-187">このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="933ab-187">Note the 0 external (social logins) associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="933ab-188">サービス内の別のログへのリンクをクリックし、アプリケーション要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="933ab-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="933ab-189">2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。</span><span class="sxs-lookup"><span data-stu-id="933ab-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="933ab-190">ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="933ab-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="933ab-191">次の図では、Tom は、ソーシャル ログイン、(からわかるようにする、**外部ログイン: 1**ページに表示されます)。</span><span class="sxs-lookup"><span data-stu-id="933ab-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="933ab-192">クリックすると**パスワードを選択して**同じアカウントに関連付けられているを使用すると、ローカルのログを追加します。</span><span class="sxs-lookup"><span data-stu-id="933ab-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="933ab-193">さらに詳しく確認の電子メール</span><span class="sxs-lookup"><span data-stu-id="933ab-193">Email confirmation in more depth</span></span>

<span data-ttu-id="933ab-194">チュートリアル[アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)より詳細には、このトピックに移動します。</span><span class="sxs-lookup"><span data-stu-id="933ab-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="933ab-195">アプリのデバッグ</span><span class="sxs-lookup"><span data-stu-id="933ab-195">Debugging the app</span></span>

<span data-ttu-id="933ab-196">場合は、リンクを含む電子メールを取得しません。</span><span class="sxs-lookup"><span data-stu-id="933ab-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="933ab-197">迷惑メールまたは迷惑メール フォルダーをチェックします。</span><span class="sxs-lookup"><span data-stu-id="933ab-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="933ab-198">SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="933ab-199">電子メールなしの確認リンクをテストするには、ダウンロード、[完成したサンプル](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="933ab-200">ページで、確認のリンクと確認コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="933ab-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="933ab-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="933ab-201">Additional Resources</span></span>

- [<span data-ttu-id="933ab-202">ASP.NET Id へのリンクは、リソースを推奨</span><span class="sxs-lookup"><span data-stu-id="933ab-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="933ab-203">[アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)回復とアカウントのパスワードの確認の詳細に入ります。</span><span class="sxs-lookup"><span data-stu-id="933ab-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="933ab-204">[Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルは、Facebook、Google OAuth 2 の承認で ASP.NET MVC 5 アプリを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="933ab-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="933ab-205">ユーザー データベースにデータを追加する方法も示しています。</span><span class="sxs-lookup"><span data-stu-id="933ab-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="933ab-206">[メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。</span><span class="sxs-lookup"><span data-stu-id="933ab-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="933ab-207">このチュートリアルは、Azure のデプロイを追加の役割を使用してアプリケーションをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="933ab-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="933ab-208">OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続します。</span><span class="sxs-lookup"><span data-stu-id="933ab-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="933ab-209">Facebook でアプリを作成し、プロジェクトに、アプリを接続します。</span><span class="sxs-lookup"><span data-stu-id="933ab-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="933ab-210">プロジェクトで SSL を設定します。</span><span class="sxs-lookup"><span data-stu-id="933ab-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
