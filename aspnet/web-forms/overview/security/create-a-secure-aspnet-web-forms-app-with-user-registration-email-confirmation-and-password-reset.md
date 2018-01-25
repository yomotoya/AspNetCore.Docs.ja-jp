---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: "電子メールの確認とパスワードのリセット (c#) でユーザー登録、セキュリティで保護された ASP.NET Web フォーム アプリを作成 |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルでは、ユーザー登録、確認の電子メール、ASP.NET Identity のメンバーを使用してパスワード リセットと ASP.NET Web フォーム アプリケーションをビルドする方法を示します."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: ed39295ed1bcaa924336a1faf52049e291abeadb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a><span data-ttu-id="d056c-103">電子メールの確認とパスワードのリセット (c#) でユーザー登録、セキュリティで保護された ASP.NET Web フォーム アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d056c-103">Create a secure ASP.NET Web Forms app with user registration, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="d056c-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="d056c-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="d056c-105">このチュートリアルでは、ユーザー登録、確認の電子メール、Asp.net メンバーシップ システムを使用してパスワード リセットと ASP.NET Web フォーム アプリケーションをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d056c-105">This tutorial shows you how to build an ASP.NET Web Forms app with user registration, email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="d056c-106">このチュートリアルは、Rick Anderson のに基づいてが[MVC チュートリアル](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-106">This tutorial was based on Rick Anderson's [MVC tutorial](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).</span></span>


## <a name="introduction"></a><span data-ttu-id="d056c-107">はじめに</span><span class="sxs-lookup"><span data-stu-id="d056c-107">Introduction</span></span>

<span data-ttu-id="d056c-108">このチュートリアルに従って、ユーザー登録してセキュリティで保護された Web フォーム アプリを作成、電子メールの確認とパスワードのリセットを Visual Studio と ASP.NET 4.5 を使用した ASP.NET Web フォーム アプリケーションを作成するために必要な手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="d056c-108">This tutorial guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio and ASP.NET 4.5 to create a secure Web Forms app with user registration, email confirmation and password reset.</span></span>

### <a name="tutorial-tasks-and-information"></a><span data-ttu-id="d056c-109">チュートリアルのタスクと情報。</span><span class="sxs-lookup"><span data-stu-id="d056c-109">Tutorial Tasks and Information:</span></span>

- [<span data-ttu-id="d056c-110">作成する ASP.NET Web フォーム アプリケーション</span><span class="sxs-lookup"><span data-stu-id="d056c-110">Create an ASP.NET Web Forms app</span></span>](#createWebForms)
- [<span data-ttu-id="d056c-111">SendGrid をフックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-111">Hook Up SendGrid</span></span>](#SG)
- [<span data-ttu-id="d056c-112">ログインする前に確認の電子メールが必要</span><span class="sxs-lookup"><span data-stu-id="d056c-112">Require Email Confirmation Before Log In</span></span>](#require)
- [<span data-ttu-id="d056c-113">パスワードの回復とリセット</span><span class="sxs-lookup"><span data-stu-id="d056c-113">Password Recovery and Reset</span></span>](#reset)
- [<span data-ttu-id="d056c-114">再送信する電子メールの確認のリンク</span><span class="sxs-lookup"><span data-stu-id="d056c-114">Resend Email Confirmation Link</span></span>](#rsend)
- [<span data-ttu-id="d056c-115">アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="d056c-115">Troubleshooting the App</span></span>](#dbg)
- [<span data-ttu-id="d056c-116">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="d056c-116">Additional Resources</span></span>](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a><span data-ttu-id="d056c-117">ASP.NET Web フォーム アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d056c-117">Create an ASP.NET Web Forms App</span></span>

<span data-ttu-id="d056c-118">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="d056c-119">インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降同様にします。</span><span class="sxs-lookup"><span data-stu-id="d056c-119">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher as well.</span></span>

> [!NOTE]
> <span data-ttu-id="d056c-120">警告: インストールする必要あります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="d056c-120">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="d056c-121">新しいプロジェクトを作成 (**ファイル** - &gt; **新しいプロジェクト**) を選択し、 **ASP.NET Web アプリケーション**テンプレートと、最新の .NET Frameworkバージョン、**新しいプロジェクト** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="d056c-121">Create a new project (**File** -&gt; **New Project**) and select the **ASP.NET Web Application** template and the latest .NET Framework version from the **New Project** dialog box.</span></span>
2. <span data-ttu-id="d056c-122">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="d056c-122">From the **New ASP.NET Project** dialog box, select the **Web Forms** template.</span></span> <span data-ttu-id="d056c-123">既定の認証としてのままにして**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="d056c-123">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="d056c-124">Azure でアプリケーションをホストする場合は、ままにして、**クラウド内のホスト**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="d056c-124">If you'd like to host the app in Azure, leave the **Host in the cloud** check box checked.</span></span>   
 <span data-ttu-id="d056c-125">[] をクリックして**OK**新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d056c-125">Then, click **OK** to create the new project.</span></span>  
    <span data-ttu-id="d056c-126">![新しい ASP.NET プロジェクト ダイアログ ボックス](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d056c-126">![New ASP.NET Project dialog box](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)</span></span>
3. <span data-ttu-id="d056c-127">プロジェクトの Secure Sockets Layer (SSL) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d056c-127">Enable Secure Sockets Layer (SSL) for the project.</span></span> <span data-ttu-id="d056c-128">使用可能な手順に従って、**プロジェクトの SSL を有効にする**のセクション、 [Web フォーム チュートリアル シリーズの概要](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-128">Follow the steps available in the **Enable SSL for the Project** section of the [Getting Started with Web Forms tutorial series](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).</span></span>
4. <span data-ttu-id="d056c-129">アプリを実行する をクリックして、**登録**リンクし、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="d056c-129">Run the app, click the **Register** link and register a new user.</span></span> <span data-ttu-id="d056c-130">この時点では、検証、電子メールにのみ基づきます、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)電子メール アドレスが整形式であることを確認する属性。</span><span class="sxs-lookup"><span data-stu-id="d056c-130">At this point, the only validation on the email is based on the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute to ensure the email address is well-formed.</span></span> <span data-ttu-id="d056c-131">確認の電子メールを追加するコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="d056c-131">You will modify the code to add email confirmation.</span></span> <span data-ttu-id="d056c-132">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="d056c-132">Close the browser window.</span></span>
5. <span data-ttu-id="d056c-133">**サーバー エクスプ ローラー** Visual Studio の (**ビュー**  - &gt; **サーバー エクスプ ローラー**) に移動**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開き**です。</span><span class="sxs-lookup"><span data-stu-id="d056c-133">In **Server Explorer** of Visual Studio (**View** -&gt; **Server Explorer**), navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span> 

    <span data-ttu-id="d056c-134">次の図は、`AspNetUsers`テーブルのスキーマ。</span><span class="sxs-lookup"><span data-stu-id="d056c-134">The following image shows the `AspNetUsers` table schema:</span></span>

    ![AspNetUsers テーブル スキーマ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="d056c-136">**サーバー エクスプ ローラー**を右クリックし、 **AspNetUsers**テーブルを選択して**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="d056c-136">In **Server Explorer**, right-click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
  
    ![AspNetUsers テーブルのデータ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="d056c-138">この時点で登録されたユーザーの電子メールが確認されていません。</span><span class="sxs-lookup"><span data-stu-id="d056c-138">At this point the email for the registered user has not been confirmed.</span></span>
7. <span data-ttu-id="d056c-139">ユーザーをクリック delete を削除し、行をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-139">Click on the row and select delete to delete the user.</span></span> <span data-ttu-id="d056c-140">次の手順でこの電子メールをもう一度追加して、電子メール アドレスに確認メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="d056c-140">You'll add this email again in the next step and send a confirmation message to the email address.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="d056c-141">確認の電子メール</span><span class="sxs-lookup"><span data-stu-id="d056c-141">Email Confirmation</span></span>

<span data-ttu-id="d056c-142">他のユーザーを偽装するしていないことを確認する新しいユーザーの登録中に、電子メールを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。</span><span class="sxs-lookup"><span data-stu-id="d056c-142">It's a best practice to confirm the email during the registration of a new user to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="d056c-143">しないようにする、ディスカッション フォーラムするいたと仮定`"bob@cpandl.com"`としての登録から`"joe@contoso.com"`です。</span><span class="sxs-lookup"><span data-stu-id="d056c-143">Suppose you had a discussion forum, you would want to prevent `"bob@cpandl.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="d056c-144">電子メールの確認なし`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d056c-144">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="d056c-145">として誤って Bob が登録されていると仮定します`"bib@cpandl.com"`いなかったが検出されました。 これは、と彼は、アプリは、正しいメール アドレスが見つからないため、パスワードの回復を使用できなくします。</span><span class="sxs-lookup"><span data-stu-id="d056c-145">Suppose Bob accidently registered as `"bib@cpandl.com"` and hadn't noticed it, he wouldn't be able to use password recovery because the app doesn't have his correct email.</span></span> <span data-ttu-id="d056c-146">確認の電子メールは、bot から制限の保護のみを提供し、決定スパム攻撃者から保護を提供しません。</span><span class="sxs-lookup"><span data-stu-id="d056c-146">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers.</span></span>

<span data-ttu-id="d056c-147">一般にする新しいユーザーがいずれかの電子メール、SMS テキスト メッセージ、または別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="d056c-147">You generally want to prevent new users from posting any data to your website before they have been confirmed by either email, an SMS text message or another mechanism.</span></span> <span data-ttu-id="d056c-148">以下のセクションでは確認の電子メールを有効にして新たに登録されたユーザーが自分の電子メールが確認されるまでログ記録するを防ぐためにコードを変更します。、</span><span class="sxs-lookup"><span data-stu-id="d056c-148">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span> <span data-ttu-id="d056c-149">このチュートリアルでは、電子メール サービス SendGrid を使用します。</span><span class="sxs-lookup"><span data-stu-id="d056c-149">You'll use the email service SendGrid in this tutorial.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="d056c-150">SendGrid をフックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-150">Hook up SendGrid</span></span>

<span data-ttu-id="d056c-151">このチュートリアルはのみを使用して電子メール通知を追加する方法を示しますが[SendGrid](http://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="d056c-151">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="d056c-152">Visual Studio で開く、 **Package Manager Console** (**ツール** - &gt; **NuGet パッケージ マネージャー**  - &gt;**Package Manager Console**)、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d056c-152">In Visual Studio, open the **Package Manager Console** (**Tools** -&gt; **NuGet Package Manger** -&gt; **Package Manager Console**), and enter the following command:</span></span>  
    `Install-Package SendGrid`
2. <span data-ttu-id="d056c-153">移動して、 [Azure SendGrid のサインアップ ページ](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)SendGrid アカウントの登録は無料とします。</span><span class="sxs-lookup"><span data-stu-id="d056c-153">Go to the [Azure SendGrid sign-up page](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) and register for free SendGrid account.</span></span> <span data-ttu-id="d056c-154">無料の SendGrid アカウント上で直接もサインアップできます[SendGrid のサイト](http://www.sendgrid.com)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-154">You can also sign-up for a free SendGrid account directly on [SendGrid's site](http://www.sendgrid.com).</span></span>
3. <span data-ttu-id="d056c-155">**ソリューション エクスプ ローラー**を開く、 *IdentityConfig.cs*ファイルで、*アプリ\_開始*フォルダー、に黄色で強調表示されている次のコードを追加`EmailService`を構成するクラス**SendGrid**:</span><span class="sxs-lookup"><span data-stu-id="d056c-155">From **Solution Explorer** open the *IdentityConfig.cs* file in the *App\_Start* folder and add the following code highlighted in yellow to the `EmailService` class to configure **SendGrid**:</span></span>

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. <span data-ttu-id="d056c-156">また、次の追加`using`ステートメントの先頭に、 *IdentityConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d056c-156">Also, add the following `using` statements to the beginning of the *IdentityConfig.cs* file:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. <span data-ttu-id="d056c-157">電子メール サービス アカウントの値をこのサンプルをシンプルにするには、保存する、`appSettings`のセクションで、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d056c-157">To keep this sample simple, you'll store the email service account values in the `appSettings` section of the *web.config* file.</span></span> <span data-ttu-id="d056c-158">次の XML に黄色で強調表示を追加、 *web.config*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="d056c-158">Add the following XML highlighted in yellow to the *web.config* file at the root of your project:</span></span>

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > <span data-ttu-id="d056c-159">セキュリティ - ソース コード内の機密データはストアことはありません。</span><span class="sxs-lookup"><span data-stu-id="d056c-159">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="d056c-160">この例に、アカウントと資格情報を格納します。、 **appSetting**のセクションで、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d056c-160">In this example, the account and credentials are stored in the **appSetting** section of the *Web.config* file.</span></span> <span data-ttu-id="d056c-161">Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。</span><span class="sxs-lookup"><span data-stu-id="d056c-161">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="d056c-162">関連情報については、Rick Anderson のトピックを参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](https://go.microsoft.com/fwlink/?LinkId=513141)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-162">For related information see Rick Anderson's topic titled [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](https://go.microsoft.com/fwlink/?LinkId=513141).</span></span>
6. <span data-ttu-id="d056c-163">(ユーザー名とパスワード)、SendGrid 認証値正常に実行できるように電子メール アプリから、送信を反映するように、電子メール サービスの値を追加します。</span><span class="sxs-lookup"><span data-stu-id="d056c-163">Add the email service values to reflect your SendGrid authentication values (User Name and Password) so that you can successful send email from your app.</span></span> <span data-ttu-id="d056c-164">SendGrid を入力した電子メール アドレスではなく、SendGrid アカウント名を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d056c-164">Be sure to use your SendGrid account name rather than the email address you provided SendGrid.</span></span>

### <a name="enable-email-confirmation"></a><span data-ttu-id="d056c-165">確認の電子メールを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d056c-165">Enable Email Confirmation</span></span>

 <span data-ttu-id="d056c-166">確認の電子メールを有効にするには、次の手順を使用して登録コードを変更します。</span><span class="sxs-lookup"><span data-stu-id="d056c-166">To enable email confirmation, you'll modify the registration code using the following steps.</span></span>  
 

1. <span data-ttu-id="d056c-167">*アカウント*フォルダーを開き、 *Register.aspx.cs*分離コードと更新、`CreateUser_Click`メソッドを次の強調表示された変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d056c-167">In the *Account* folder, open the *Register.aspx.cs* code-behind and update the `CreateUser_Click` method to enable the following highlighted changes:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. <span data-ttu-id="d056c-168">**ソリューション エクスプ ローラー**を右クリックして*Default.aspx*選択**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="d056c-168">In **Solution Explorer**, right-click *Default.aspx* and select **Set As Start Page**.</span></span>
3. <span data-ttu-id="d056c-169">キーを押して、アプリの実行**f5 キーを押します。**</span><span class="sxs-lookup"><span data-stu-id="d056c-169">Run the app by pressing **F5.**</span></span> <span data-ttu-id="d056c-170">ページが表示されたら、クリックして、**登録**登録ページを表示するリンクです。</span><span class="sxs-lookup"><span data-stu-id="d056c-170">After the page is displayed, click the **Register** link to display the Register page.</span></span>
4. <span data-ttu-id="d056c-171">電子メールとパスワードを入力し、クリックして、**登録**SendGrid を使用して電子メール メッセージを送信するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-171">Enter your email and password, then click the **Register** button to send an email message via SendGrid.</span></span>  
 <span data-ttu-id="d056c-172">プロジェクトとコードの現在の状態は、ユーザーが自分のアカウントを確認していない場合でも、登録フォームを完了するログインを許可します。</span><span class="sxs-lookup"><span data-stu-id="d056c-172">The current state of your project and code will allow the user to log in once they complete the registration form, even though they haven't confirmed their account.</span></span>
5. <span data-ttu-id="d056c-173">電子メール アカウントを確認し、電子メールを確認するリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-173">Check your email account and click on the link to confirm your email.</span></span>  
 <span data-ttu-id="d056c-174">登録フォームを送信すると後ログ記録します。</span><span class="sxs-lookup"><span data-stu-id="d056c-174">Once you submit the registration form, you will be logged in.</span></span>  
    ![サンプル web サイトのサインイン](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="d056c-176">ログインする前に確認の電子メールが必要</span><span class="sxs-lookup"><span data-stu-id="d056c-176">Require Email Confirmation Before Log In</span></span>

<span data-ttu-id="d056c-177">電子メール アカウントを確認した後、この時点で内にある完全署名の検証電子メールに含まれているリンクをクリックする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d056c-177">Although you have confirmed the email account, at this point you would not need to click on the link contained in the verification email to be fully signed-in.</span></span> <span data-ttu-id="d056c-178">次のセクションでは、新しいユーザーを認証するには記録前に確認された電子メールがある () を必要とするコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="d056c-178">In the following section, you will modify the code requiring new users to have a confirmed email before they are logged in (authenticated).</span></span>

1. <span data-ttu-id="d056c-179">**ソリューション エクスプ ローラー** 、Visual Studio の更新、`CreateUser_Click`内のイベント、 *Register.aspx.cs*に含まれている分離コード、*アカウント*フォルダーが、次には、変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="d056c-179">In **Solution Explorer** of Visual Studio, update the `CreateUser_Click` event in the *Register.aspx.cs* code-behind contained in the *Accounts* folder with the following highlighted changes:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. <span data-ttu-id="d056c-180">更新プログラム、`LogIn`メソッドで、 *Login.aspx.cs*次の強調表示された変更と分離コード。</span><span class="sxs-lookup"><span data-stu-id="d056c-180">Update the `LogIn` method in the *Login.aspx.cs* code-behind with the following highlighted changes:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a><span data-ttu-id="d056c-181">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="d056c-181">Run the Application</span></span>

 <span data-ttu-id="d056c-182">これで、ユーザーの電子メール アドレスが確認されているかどうかをチェックするコードを実装している両方の機能を確認できます、**登録**と**ログイン**ページ。</span><span class="sxs-lookup"><span data-stu-id="d056c-182">Now that you have implemented the code to check whether a user's email address has been confirmed, you can check the functionality on both the **Register** and **Login** pages.</span></span> 

1. <span data-ttu-id="d056c-183">内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。</span><span class="sxs-lookup"><span data-stu-id="d056c-183">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test.</span></span>
2. <span data-ttu-id="d056c-184">アプリを実行する (**f5 キーを押して**) し、電子メール アドレスを確認するまでユーザーとして登録できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d056c-184">Run the app (**F5**) and verify you cannot register as a user until you have confirmed your email address.</span></span>
3. <span data-ttu-id="d056c-185">だけに送信された電子メールを使用して、新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。</span><span class="sxs-lookup"><span data-stu-id="d056c-185">Before confirming your new account via the email that was just sent, attempt to log in with the new account.</span></span>  
 <span data-ttu-id="d056c-186">いないことにログインできなくなります、確認の電子メール アカウントに必要なが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d056c-186">You'll see that you are unable to log in and that you must have a confirmed email account.</span></span>
4. <span data-ttu-id="d056c-187">電子メール アドレスのことを確認したら、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="d056c-187">Once you confirm your email address, log in to the app.</span></span>

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a><span data-ttu-id="d056c-188">パスワードの回復とリセット</span><span class="sxs-lookup"><span data-stu-id="d056c-188">Password Recovery and Reset</span></span>

1. <span data-ttu-id="d056c-189">Visual Studio で、削除からコメント文字、`Forgot`メソッドで、 *Forgot.aspx.cs*に含まれている分離コード、*アカウント*フォルダーとして、メソッドが表示されるように依存しています。</span><span class="sxs-lookup"><span data-stu-id="d056c-189">In Visual Studio, remove the comment characters from the `Forgot` method in the *Forgot.aspx.cs* code-behind contained in the *Account* folder, so that the method appears as follows:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. <span data-ttu-id="d056c-190">開く、 *Login.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="d056c-190">Open the *Login.aspx* page.</span></span> <span data-ttu-id="d056c-191">末尾付近のマークアップを置き換える、 **loginForm**下強調表示されているセクションします。</span><span class="sxs-lookup"><span data-stu-id="d056c-191">Replace the markup near the end of the **loginForm** section as highlighted below:</span></span> 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. <span data-ttu-id="d056c-192">開く、 *Login.aspx.cs*分離コードと、次のコード行から黄色で強調表示のコメントを解除、`Page_Load`イベントのハンドラー。</span><span class="sxs-lookup"><span data-stu-id="d056c-192">Open the *Login.aspx.cs* code-behind and uncomment the following line of code highlighted in yellow from the `Page_Load` event handler:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. <span data-ttu-id="d056c-193">キーを押して、アプリの実行**f5 キーを押します。**</span><span class="sxs-lookup"><span data-stu-id="d056c-193">Run the app by pressing **F5.**</span></span> <span data-ttu-id="d056c-194">ページが表示されたら、クリックして、**ログイン**リンクします。</span><span class="sxs-lookup"><span data-stu-id="d056c-194">After the page is displayed, click the **Log in** link.</span></span>
5. <span data-ttu-id="d056c-195">クリックして、**パスワードを忘れた場合ですか?**表示へのリンク、**パスワードを忘れた場合**ページ。</span><span class="sxs-lookup"><span data-stu-id="d056c-195">Click the **Forgot your password?** link to display the **Forgot Password** page.</span></span>
6. <span data-ttu-id="d056c-196">電子メール アドレスを入力し、をクリックして、**送信**電子メール アドレスに送信する、パスワードをリセットできるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-196">Enter your email address and click the **Submit** button to send an email to your address which will allow you to reset your password.</span></span>   
 <span data-ttu-id="d056c-197">電子メール アカウントを確認し、表示するリンクをクリックして、**パスワードのリセット**ページ。</span><span class="sxs-lookup"><span data-stu-id="d056c-197">Check your email account and click on the link to display the **Reset Password** page.</span></span>
7. <span data-ttu-id="d056c-198">**パスワードのリセット** ページで、電子メール、パスワード、および確認入力したパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="d056c-198">On the **Reset Password** page, enter your email, password, and confirmed password.</span></span> <span data-ttu-id="d056c-199">次に、キーを押して、**リセット**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-199">Then, press the **Reset** button.</span></span>  
 <span data-ttu-id="d056c-200">パスワードを正常にリセットすると、**パスワード変更**ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d056c-200">When you successfully reset your password, the **Password Changed** page will be displayed.</span></span> <span data-ttu-id="d056c-201">これで、新しいパスワードでログインできます。</span><span class="sxs-lookup"><span data-stu-id="d056c-201">Now you can log in with your new password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="d056c-202">再送信する電子メールの確認のリンク</span><span class="sxs-lookup"><span data-stu-id="d056c-202">Resend Email Confirmation Link</span></span>

<span data-ttu-id="d056c-203">ユーザーは、新しいローカル アカウントを作成した後が確認のリンクを使用してログオンする前にする必要が電子メールで送信します。</span><span class="sxs-lookup"><span data-stu-id="d056c-203">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="d056c-204">誤ってユーザーが、確認メールを削除または電子メールが到着することはありません、確認のリンクを再送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d056c-204">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="d056c-205">次のコード変更では、これを有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d056c-205">The following code changes show how to enable this.</span></span>

1. <span data-ttu-id="d056c-206">Visual Studio で開く、 **Login.aspx.cs**分離コードと後に次のイベント ハンドラーを追加、`LogIn`イベントのハンドラー。</span><span class="sxs-lookup"><span data-stu-id="d056c-206">In Visual Studio, open the **Login.aspx.cs** code-behind and add the following event handler after the `LogIn` event handler:</span></span>   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. <span data-ttu-id="d056c-207">変更、`LogIn`内のイベント ハンドラー、 *Login.aspx.cs*次のように、黄色で強調表示コードを変更することによって分離コード。</span><span class="sxs-lookup"><span data-stu-id="d056c-207">Modify the `LogIn` event handler in the *Login.aspx.cs* code-behind by changing the code highlighted in yellow as follows:</span></span> 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. <span data-ttu-id="d056c-208">更新プログラム、 *Login.aspx*ページで、次のように、黄色で強調表示のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d056c-208">Update the *Login.aspx* page by adding the code highlighted in yellow as follows:</span></span> 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. <span data-ttu-id="d056c-209">内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。</span><span class="sxs-lookup"><span data-stu-id="d056c-209">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test.</span></span>
5. <span data-ttu-id="d056c-210">アプリを実行する (**f5 キーを押して**) と、電子メール アドレスを登録します。</span><span class="sxs-lookup"><span data-stu-id="d056c-210">Run the app (**F5**) and register your email address.</span></span>
6. <span data-ttu-id="d056c-211">だけに送信された電子メールを使用して、新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。</span><span class="sxs-lookup"><span data-stu-id="d056c-211">Before confirming your new account via the email that was just sent, attempt to log in with the new account.</span></span>  
 <span data-ttu-id="d056c-212">いないことにログインできなくなります、確認の電子メール アカウントに必要なが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d056c-212">You'll see that you are unable to log in and that you must have a confirmed email account.</span></span> <span data-ttu-id="d056c-213">さらに、電子メール アカウントに今すぐ確認メッセージを再送信することができます。</span><span class="sxs-lookup"><span data-stu-id="d056c-213">In addition, you can now resend a confirmation message to your email account.</span></span>
7. <span data-ttu-id="d056c-214">電子メール アドレスとパスワードを入力し、キーを押します、**確認を再送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-214">Enter your email address and password, then press the **Resend confirmation** button.</span></span>
8. <span data-ttu-id="d056c-215">新しく送信した電子メール メッセージに基づく電子メール アドレスのことを確認したら、アプリにログインします。</span><span class="sxs-lookup"><span data-stu-id="d056c-215">Once you confirm your email address based on the newly sent email message, log in to the app.</span></span>

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a><span data-ttu-id="d056c-216">アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="d056c-216">Troubleshooting the App</span></span>

<span data-ttu-id="d056c-217">場合は、資格情報を確認へのリンクを含む電子メールを受信しません。</span><span class="sxs-lookup"><span data-stu-id="d056c-217">If you don't receive an email containing the link to verify your credentials:</span></span>

- <span data-ttu-id="d056c-218">迷惑メールまたは迷惑メール フォルダーをチェックします。</span><span class="sxs-lookup"><span data-stu-id="d056c-218">Check your junk or spam folder.</span></span>
- <span data-ttu-id="d056c-219">SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)です。</span><span class="sxs-lookup"><span data-stu-id="d056c-219">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>
- <span data-ttu-id="d056c-220">として、SendGrid のユーザーのアカウント名を使用することを確認して、 *Web.config* SendGrid アカウントの電子メール アドレスではなく値します。</span><span class="sxs-lookup"><span data-stu-id="d056c-220">Be certain you used your SendGrid user account name as a *Web.config* value rather than your SendGrid account email address.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="d056c-221">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="d056c-221">Additional Resources</span></span>

- [<span data-ttu-id="d056c-222">ASP.NET Id へのリンクは、リソースを推奨</span><span class="sxs-lookup"><span data-stu-id="d056c-222">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [<span data-ttu-id="d056c-223">アカウントの確認と ASP.NET の Id とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="d056c-223">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="d056c-224">ASP.NET Web フォームのチュートリアル シリーズの OAuth 2.0 プロバイダーの追加します。</span><span class="sxs-lookup"><span data-stu-id="d056c-224">ASP.NET Web Forms tutorial series - Add an OAuth 2.0 Provider</span></span>](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [<span data-ttu-id="d056c-225">Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="d056c-225">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [<span data-ttu-id="d056c-226">ASP.NET Web フォーム チュートリアル シリーズのプロジェクトの SSL を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d056c-226">ASP.NET Web Forms tutorial series - Enable SSL for the Project</span></span>](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
