---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: 確認とパスワードのリセット (c#) を電子メールでは、ログをセキュリティで保護された ASP.NET MVC 5 web アプリを作成 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、確認の電子メールと、ASP.NET Identity メンバーシップ システムを使用してパスワード リセットによる ASP.NET MVC 5 web アプリをビルドする方法を示します。 . Ca
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387044"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="9bf90-104">確認とパスワードのリセット (c#) を電子メールでは、ログをセキュリティで保護された ASP.NET MVC 5 web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="9bf90-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9bf90-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="9bf90-106">このチュートリアルでは、確認の電子メールと、ASP.NET Identity メンバーシップ システムを使用してパスワード リセットによる ASP.NET MVC 5 web アプリをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="9bf90-107">完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="9bf90-108">ダウンロードには、確認の電子メールと SMS を電子メールまたは SMS プロバイダーをセットアップすることがなくテストできるデバッグ ヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9bf90-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="9bf90-109">このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="9bf90-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="9bf90-110">ASP.NET MVC アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="9bf90-111">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="9bf90-112">インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="9bf90-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf90-113">警告: をインストールする必要がある[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="9bf90-114">新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="9bf90-115">Web フォームには ASP.NET Identity もサポートしていますので、web フォーム アプリで同じ手順を実行できます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="9bf90-116">既定の認証としてのままに**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="9bf90-117">Azure でアプリをホストする場合は、チェック ボックスをオンのままにします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="9bf90-118">チュートリアルの後半では、Azure を展開します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="9bf90-119">できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="9bf90-120">設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="9bf90-121">アプリを実行し、をクリックして、**登録**リンクし、ユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="9bf90-122">この時点では、電子メールの検証のみ、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="9bf90-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="9bf90-123">サーバー エクスプ ローラーに移動します。**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開く**します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="9bf90-124">次の図は、`AspNetUsers`スキーマ。</span><span class="sxs-lookup"><span data-stu-id="9bf90-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="9bf90-125">右クリックして、 **AspNetUsers**テーブルを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="9bf90-126">この時点で、電子メールが確認されていません。</span><span class="sxs-lookup"><span data-stu-id="9bf90-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="9bf90-127">行をクリックし、[削除] を選択します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-127">Click on the row and select delete.</span></span> <span data-ttu-id="9bf90-128">次の手順でこの電子メールをもう一度追加し、確認の電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="9bf90-129">確認の電子メール</span><span class="sxs-lookup"><span data-stu-id="9bf90-129">Email confirmation</span></span>

<span data-ttu-id="9bf90-130">他のユーザーを偽装していないことを確認する新規ユーザー登録の電子メール アドレスを確認することをお勧め (つまりに登録していない他のユーザーの電子メールで)。</span><span class="sxs-lookup"><span data-stu-id="9bf90-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="9bf90-131">ようにしたい、ディスカッション フォーラムが`"bob@example.com"`としての登録から`"joe@contoso.com"`します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="9bf90-132">電子メールの確認を求めず`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9bf90-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="9bf90-133">として誤って Bob が登録されていると仮定`"bib@example.com"`気付いていなかったとパスワードの回復を使用して、アプリは、正しいメール アドレスがある見つからないため、彼はできません。</span><span class="sxs-lookup"><span data-stu-id="9bf90-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="9bf90-134">確認の電子メールがボットから制限の保護のみを提供し、決定スパムから保護を提供しません、登録に使用できる多くの作業用電子メールの別名が。</span><span class="sxs-lookup"><span data-stu-id="9bf90-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="9bf90-135">新しいユーザーが電子メール、SMS テキスト メッセージまたは別のメカニズムによって確認されて前に、web サイトにデータを投稿するを防ぐために一般的にします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="9bf90-136">以下のセクションでは確認の電子メールを有効にし、新しく登録されたユーザーがログインするまで、自分の電子メールが確認されていることを防ぐためにコードを変更します。、</span><span class="sxs-lookup"><span data-stu-id="9bf90-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="9bf90-137">SendGrid をフックします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-137">Hook up SendGrid</span></span>

<span data-ttu-id="9bf90-138">このチュートリアルを使用して電子メール通知を追加する方法だけでは[SendGrid](http://sendgrid.com/)、SMTP とその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="9bf90-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="9bf90-139">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="9bf90-140">移動して、 [Azure SendGrid へのサインアップ ページ](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)し、無料の SendGrid アカウントに登録します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="9bf90-141">SendGrid を構成するのには、次のようなコードを追加することで*App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="9bf90-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="9bf90-142">以下を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9bf90-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="9bf90-143">アプリの設定を格納すると、このサンプルをシンプルにするには、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9bf90-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="9bf90-144">セキュリティ - ソース コード内の機密データは store ことはありません。</span><span class="sxs-lookup"><span data-stu-id="9bf90-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="9bf90-145">アカウントと資格情報は、appSetting で格納されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="9bf90-146">Azure では安全に保管するこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portal でのタブ。</span><span class="sxs-lookup"><span data-stu-id="9bf90-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="9bf90-147">参照してください[ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="9bf90-148">アカウント コント ローラーで確認の電子メールを有効にします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="9bf90-149">確認、 *Views\Account\ConfirmEmail.cshtml*ファイルが適切な razor 構文。</span><span class="sxs-lookup"><span data-stu-id="9bf90-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="9bf90-150">(、@ 文字が最初の行が見つからないことができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="9bf90-151">)</span><span class="sxs-lookup"><span data-stu-id="9bf90-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="9bf90-152">アプリを実行し、登録リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-152">Run the app and click the Register link.</span></span> <span data-ttu-id="9bf90-153">登録フォームを送信するに記録されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="9bf90-154">電子メール アカウントを確認し、電子メールを確認するには、リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="9bf90-155">ログインする前に確認の電子メールが必要です。</span><span class="sxs-lookup"><span data-stu-id="9bf90-155">Require email confirmation before log in</span></span>

<span data-ttu-id="9bf90-156">現在、ユーザーには、登録フォームが完了すると後ログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="9bf90-157">ログを記録する前に自分の電子メールを確認する一般にできます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="9bf90-158">以下のセクションには、ログインには、確認された電子メール (認証された) 新しいユーザーを要求するコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="9bf90-159">更新プログラム、`HttpPost Register`メソッドを次の強調表示された変更。</span><span class="sxs-lookup"><span data-stu-id="9bf90-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="9bf90-160">コメント アウトすることによって、`SignInAsync`メソッド、ユーザーは署名されません登録をします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="9bf90-161">`TempData["ViewBagLink"] = callbackUrl;`行に使用できる[、アプリのデバッグ](#dbg)し、電子メールを送信することがなく登録をテストします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="9bf90-162">`ViewBag.Message` 確認手順の表示に使用します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="9bf90-163">[サンプルをダウンロード](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)メールをセットアップすることがなく確認の電子メールをテストするコードが含まれ、アプリケーションのデバッグにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="9bf90-164">作成、`Views\Shared\Info.cshtml`ファイルを開き、次の razor マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="9bf90-165">追加、 [Authorize 属性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)を`Contact`Home コント ローラーのアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="9bf90-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="9bf90-166">クリックして、**連絡先**匿名ユーザーにアクセスできないし、認証されたユーザーがアクセスを確認するリンク。</span><span class="sxs-lookup"><span data-stu-id="9bf90-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="9bf90-167">更新することも必要があります、`HttpPost Login`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="9bf90-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="9bf90-168">更新プログラム、 *Views\Shared\Error.cshtml*エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="9bf90-169">内の任意のアカウントを削除、 **AspNetUsers**を使用してテストする電子メールのエイリアスが含まれているテーブル。</span><span class="sxs-lookup"><span data-stu-id="9bf90-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="9bf90-170">アプリを実行し、電子メール アドレスを確認するまでにログインできないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="9bf90-171">電子メール アドレスのことを確認したら、クリックして、**連絡先**リンク。</span><span class="sxs-lookup"><span data-stu-id="9bf90-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="9bf90-172">パスワード回復/リセット</span><span class="sxs-lookup"><span data-stu-id="9bf90-172">Password recovery/reset</span></span>

<span data-ttu-id="9bf90-173">コメント文字を削除、`HttpPost ForgotPassword`アクション メソッド、account コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="9bf90-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="9bf90-174">コメント文字を削除、 `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)で、 *Views\Account\Login.cshtml* razor ビュー ファイル。</span><span class="sxs-lookup"><span data-stu-id="9bf90-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="9bf90-175">ログイン ページでは、パスワードのリセットへのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="9bf90-176">電子メールの確認リンクを再送信します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-176">Resend email confirmation link</span></span>

<span data-ttu-id="9bf90-177">ユーザーは、新しいローカル アカウントを作成した後は、確認リンクがログオンする前に、使用する必要が、メールで送信されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="9bf90-178">誤ってユーザーは、確認の電子メールを削除します。 または、電子メールが到着することはありません、確認リンクをもう一度送信される必要があります。</span><span class="sxs-lookup"><span data-stu-id="9bf90-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="9bf90-179">次のコード変更では、これを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="9bf90-180">下に次のヘルパー メソッドを追加、 *controllers \accountcontroller.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="9bf90-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="9bf90-181">新しいヘルパーを使用して Register メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="9bf90-182">ユーザー アカウントが確認されていない場合は、パスワードを再送信する、Login メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="9bf90-183">ソーシャル、ローカルのログイン アカウントを組み合わせる</span><span class="sxs-lookup"><span data-stu-id="9bf90-183">Combine social and local login accounts</span></span>

<span data-ttu-id="9bf90-184">電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="9bf90-185">次の順序で**RickAndMSFT@gmail.com**が最初に、ローカル ログインとして作成は、まずソーシャル ログとしてアカウントを作成し、ローカル ログインを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="9bf90-186">をクリックして、**管理**リンク。</span><span class="sxs-lookup"><span data-stu-id="9bf90-186">Click on the **Manage** link.</span></span> <span data-ttu-id="9bf90-187">注、**外部ログイン: 0**このアカウントに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="9bf90-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="9bf90-188">サービス内の別のログへのリンクをクリックし、アプリの要求をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="9bf90-189">2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="9bf90-190">ユーザー、ソーシャル ログイン認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合は、ローカル アカウントを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="9bf90-191">次の図では、Tom は、ソーシャル ログイン (から確認できますが、**外部ログイン: 1**ページに表示)。</span><span class="sxs-lookup"><span data-stu-id="9bf90-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="9bf90-192">クリックすると**パスワードの入力**同じアカウントに関連付けられたでローカルのログを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="9bf90-193">より詳細に確認の電子メール</span><span class="sxs-lookup"><span data-stu-id="9bf90-193">Email confirmation in more depth</span></span>

<span data-ttu-id="9bf90-194">拙著のチュートリアル[アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、このトピックに移動します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="9bf90-195">アプリのデバッグ</span><span class="sxs-lookup"><span data-stu-id="9bf90-195">Debugging the app</span></span>

<span data-ttu-id="9bf90-196">リンクを含むメールが届かない: 場合</span><span class="sxs-lookup"><span data-stu-id="9bf90-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="9bf90-197">迷惑メールまたはスパム メール フォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="9bf90-198">SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="9bf90-199">電子メールなしの確認リンクをテストするには、ダウンロード、[完全なサンプル](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="9bf90-200">ページ確認のリンクと確認コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9bf90-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="9bf90-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="9bf90-201">Additional Resources</span></span>

- [<span data-ttu-id="9bf90-202">リンクを ASP.NET Identity 推奨リソース</span><span class="sxs-lookup"><span data-stu-id="9bf90-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="9bf90-203">[アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、回復、アカウントのパスワードの確認にします。</span><span class="sxs-lookup"><span data-stu-id="9bf90-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="9bf90-204">[Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルでは、Facebook や Google の OAuth 2 承認を持つ ASP.NET MVC 5 アプリを記述する方法。</span><span class="sxs-lookup"><span data-stu-id="9bf90-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="9bf90-205">Id のデータベースにデータを追加する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="9bf90-206">[メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="9bf90-207">このチュートリアルでは、Azure のデプロイを追加します。 ロールを使用してアプリをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="9bf90-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="9bf90-208">OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="9bf90-209">Facebook でアプリを作成し、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="9bf90-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="9bf90-210">プロジェクトの SSL の設定</span><span class="sxs-lookup"><span data-stu-id="9bf90-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
