---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 作成する MVC 5 アプリを Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン (c#) |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、ユーザー外部の認証から資格情報で OAuth 2.0 を使用してログインできるようにする ASP.NET MVC 5 web アプリケーションを構築する方法を紹介しています.
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824707"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="c57c4-103">Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン (c#) で ASP.NET MVC 5 アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="c57c4-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c57c4-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="c57c4-105">このチュートリアルで使用してユーザーにログインできるようにする ASP.NET MVC 5 web アプリケーションをビルドする方法は[OAuth 2.0](http://oauth.net/2/) Facebook、Twitter、LinkedIn、Microsoft、Google などの外部認証プロバイダーからの資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="c57c4-106">わかりやすくするため、このチュートリアルは Facebook や Google の資格情報の操作に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="c57c4-107">数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるため、大きな利点を提供して web サイトでこれらの資格情報を有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="c57c4-108">これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="c57c4-109">参照してください[SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="c57c4-110">メンバーシップ API を使用してロールを追加する方法と、ユーザーのプロファイル データを追加する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="c57c4-111">このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter 私に従ってください: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="c57c4-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="c57c4-112">作業の開始</span><span class="sxs-lookup"><span data-stu-id="c57c4-112">Getting Started</span></span>

<span data-ttu-id="c57c4-113">インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="c57c4-114">Visual Studio のインストール[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="c57c4-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="c57c4-115">Dropbox、GitHub、Linkedin、Instagram、バッファー、Salesforce、ストリーム、Stack Exchange、Tripit、Twitch、Twitter、yahoo!、および詳細については、この参照してください。[サンプル プロジェクト](https://github.com/matthewdunsdon/oauthforaspnet)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="c57c4-116">Visual Studio をインストールする必要があります[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降は、Google OAuth 2 を使用して、SSL の警告せずにローカルでデバッグします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="c57c4-117">クリックして**新しいプロジェクト**から、**開始** ページで、かすることができます、メニューを使用してを選択**ファイル**、し**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="c57c4-118">最初のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-118">Creating Your First Application</span></span>

<span data-ttu-id="c57c4-119">をクリックして**新しいプロジェクト**を選択し、 **Visual c#** し、左側の**Web**選び**ASP.NET Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="c57c4-120">"MvcAuth"をプロジェクトに名前をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="c57c4-121">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="c57c4-122">認証がない場合**個々 のユーザー アカウント**、クリックして、**認証の変更**ボタンをクリックし、選択**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="c57c4-123">チェックして**クラウドでホスト**アプリを Azure でホストする非常に簡単になります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="c57c4-124">選択した場合**クラウドでホスト**、[構成] ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="c57c4-125">NuGet を使用して、OWIN ミドルウェアを最新に更新するには</span><span class="sxs-lookup"><span data-stu-id="c57c4-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="c57c4-126">NuGet パッケージ マネージャーを使用して、更新、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="c57c4-127">選択**更新**左側のメニュー。</span><span class="sxs-lookup"><span data-stu-id="c57c4-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="c57c4-128">クリックして、**すべて更新**OWIN パッケージのみが (の次の図に示す) のボタンをクリックを検索できます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="c57c4-129">次の図では、OWIN パッケージのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="c57c4-130">入力したパッケージ マネージャー コンソール (PMC) から、`Update-Package`コマンドで、すべてのパッケージを更新します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="c57c4-131">キーを押して**f5 キーを押して**または**Ctrl + F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="c57c4-132">次の図では、ポート番号は、1234 です。</span><span class="sxs-lookup"><span data-stu-id="c57c4-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="c57c4-133">アプリケーションを実行するときに、別のポート番号を確認します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="c57c4-134">ナビゲーション アイコンをクリックする必要があります、ブラウザー ウィンドウのサイズに応じて、**ホーム**、**について**、**にお問い合わせください**、 **登録**と**ログイン**リンク。</span><span class="sxs-lookup"><span data-stu-id="c57c4-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="c57c4-135">プロジェクトの SSL の設定</span><span class="sxs-lookup"><span data-stu-id="c57c4-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="c57c4-136">Google や Facebook などの認証プロバイダーに接続するには SSL を使用する IIS Express を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="c57c4-137">ユーザー名とパスワードと、ネットワーク経由でクリア テキストで送信している SSL を使用せずに、シークレットと同様、ログイン cookie が、HTTP にバックアップを削除しないログイン後に SSL を使用して保持することは重要です。</span><span class="sxs-lookup"><span data-stu-id="c57c4-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="c57c4-138">ハンドシェイクを実行し、(つまり、HTTPS HTTP よりも低速の一括) チャネルをセキュリティで保護する時間を既に撮影する以外にも、MVC パイプラインを実行すると、前にログインしているために HTTP リダイレクトすることはありません、現在の要求または将来要求がはるかに高速です。</span><span class="sxs-lookup"><span data-stu-id="c57c4-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="c57c4-139">**ソリューション エクスプ ローラー**、クリックして、 **MvcAuth**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="c57c4-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="c57c4-140">プロジェクトのプロパティを表示する F4 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="c57c4-141">またはから、**ビュー**メニューを選択できます**プロパティ ウィンドウ**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="c57c4-142">変更**SSL が有効になっている**を True にします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="c57c4-143">SSL URL をコピー (になる`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。</span><span class="sxs-lookup"><span data-stu-id="c57c4-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="c57c4-144">**ソリューション エクスプ ローラー**を右クリックして、 **MvcAuth**順に選択して**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="c57c4-145">選択、 **Web**タブをクリックしに SSL URL を貼り付けて、**プロジェクト Url**ボックス。</span><span class="sxs-lookup"><span data-stu-id="c57c4-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="c57c4-146">(Ctl + S) ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="c57c4-147">この URL を Facebook、Google の認証アプリを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="c57c4-148">追加、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)属性を`Home`すべての要求を必要とするコント ローラーは、HTTPS を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="c57c4-149">安全な方法は、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)フィルターがアプリケーションにします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="c57c4-150">セクションを参照して&quot;SSL と承認属性を使用してアプリケーションを保護する&quot;マイ tutoral で[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="c57c4-151">Home コント ローラーの一部は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="c57c4-152">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="c57c4-153">過去の証明書をインストールしている場合は、このセクションの残りの部分を省略し、ジャンプ[OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続する](#goog)、それ以外の場合、次の手順については、自己署名を信頼するにはIIS Express が生成した証明書。</span><span class="sxs-lookup"><span data-stu-id="c57c4-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="c57c4-154">読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。</span><span class="sxs-lookup"><span data-stu-id="c57c4-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="c57c4-155">Ie、*ホーム*ページし、SSL の警告はありません。</span><span class="sxs-lookup"><span data-stu-id="c57c4-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="c57c4-156">Google Chrome も証明書を承認し、警告を表示せず、HTTPS コンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="c57c4-157">警告に表示されるように、Firefox は独自の証明書ストアを使用します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="c57c4-158">アプリケーションを安全にクリックして、**リスクを理解しました**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="c57c4-159">OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="c57c4-160">現在 Google OAuth 手順については、次を参照してください。 [ASP.NET Core での構成の Google 認証](/aspnet/core/security/authentication/social/google-logins)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="c57c4-161">移動し、 [Google Developers Console](https://console.developers.google.com/)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="c57c4-162">前にプロジェクトを作成していない場合は、選択**資格情報**クリックして、左側のタブで**作成**です。</span><span class="sxs-lookup"><span data-stu-id="c57c4-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="c57c4-163">左側のタブで次のようにクリックします。**資格情報**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="c57c4-164">クリックして**資格情報を作成**し**OAuth クライアント ID**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="c57c4-165">**Create Client ID**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。</span><span class="sxs-lookup"><span data-stu-id="c57c4-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="c57c4-166">設定、**承認済みの JavaScript**オリジン上記で使用する SSL URL を (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)</span><span class="sxs-lookup"><span data-stu-id="c57c4-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="c57c4-167">設定、 **Authorized リダイレクト URI**に。</span><span class="sxs-lookup"><span data-stu-id="c57c4-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="c57c4-168">OAuth 同意画面のメニュー項目をクリックし、電子メール アドレスと製品名を設定します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="c57c4-169">フォームのクリックが完了したら**保存**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="c57c4-170">ライブラリ メニュー項目をクリックし、検索、 **Google + API**、それをクリックし、有効にするキーを押します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="c57c4-171">次の図は、有効になっている Api を示します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="c57c4-172">Google Api API マネージャーを参照してください、**資格情報**を取得するには、タブ、**クライアント ID**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="c57c4-173">ダウンロードすると、アプリケーション シークレットの JSON ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="c57c4-174">コピーして貼り付け、 **ClientId**と**ClientSecret**に、`UseGoogleAuthentication`メソッド、 *Startup.Auth.cs*ファイル、 *App_Start*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="c57c4-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="c57c4-175">**ClientId**と**ClientSecret**の下に表示される値のサンプルし、動作しません。</span><span class="sxs-lookup"><span data-stu-id="c57c4-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="c57c4-176">セキュリティ - ソース コード内の機密データは store ことはありません。</span><span class="sxs-lookup"><span data-stu-id="c57c4-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="c57c4-177">アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="c57c4-178">参照してください[ASP.NET と Azure App Service にパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="c57c4-179">キーを押して**CTRL + F5**をビルドして、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="c57c4-180">をクリックして、**ログイン**リンク。</span><span class="sxs-lookup"><span data-stu-id="c57c4-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="c57c4-181">[**別のサービスを使用してログイン**、] をクリックして**Google**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="c57c4-182">上記の手順のいずれかがない場合は、HTTP 401 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="c57c4-183">上記の手順を再確認します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-183">Recheck your steps above.</span></span> <span data-ttu-id="c57c4-184">必要な設定がない場合 (たとえば**製品名**) は、不足している項目を追加し、保存、認証が機能するのに数分かかることができます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="c57c4-185">資格情報を入力は Google のサイトにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="c57c4-186">資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を付与が求められます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="c57c4-187">クリックして**受け入れる**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-187">Click **Accept**.</span></span> <span data-ttu-id="c57c4-188">今すぐにリダイレクトされます、**登録**Google アカウントを登録する MvcAuth アプリケーションのページ。</span><span class="sxs-lookup"><span data-stu-id="c57c4-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="c57c4-189">Gmail アカウントに使用されるローカルの電子メール登録名を変更するオプションがありますが、通常、既定の電子メール エイリアス (認証に使用するもの) を保持したいです。</span><span class="sxs-lookup"><span data-stu-id="c57c4-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="c57c4-190">**[登録]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="c57c4-191">Facebook でアプリを作成し、アプリ プロジェクトを接続します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="c57c4-192">現在の Facebook OAuth2 認証の手順では、次を参照してください[構成の Facebook 認証。](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="c57c4-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="c57c4-193">Facebook の OAuth2 認証の場合は、Facebook で作成したアプリケーションから一部の設定をプロジェクトにコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="c57c4-194">ブラウザーに移動します。 [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) Facebook の資格情報を入力してログインします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="c57c4-195">まだ、Facebook 開発者として登録されていない場合はクリックして**開発者として登録**を登録する指示に従います。</span><span class="sxs-lookup"><span data-stu-id="c57c4-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="c57c4-196">**アプリ**] タブで [ **Create New App**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![新しいアプリを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="c57c4-198">入力、**アプリ名**と**カテゴリ**、 をクリックし、**アプリの作成**。</span><span class="sxs-lookup"><span data-stu-id="c57c4-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="c57c4-199"><strong>アプリ Namespace</strong>は、アプリが認証に Facebook アプリケーションへのアクセスに使用する URL の一部です (たとえば、https\://apps.facebook.com/{App Namespace})。</span><span class="sxs-lookup"><span data-stu-id="c57c4-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="c57c4-200">指定しない場合、<strong>アプリ Namespace</strong>、<strong>アプリ ID</strong> URL に使用します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="c57c4-201"><strong>アプリ ID</strong>はシステムによって生成された時間の数が次の手順で表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![新しいアプリのダイアログを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="c57c4-203">標準的なセキュリティ チェックを送信します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-203">Submit the standard security check.</span></span>

    ![セキュリティ チェック](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="c57c4-205">選択**設定**左側のメニュー バーの![Facebook 開発者のメニュー バー](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="c57c4-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="c57c4-206">**基本的な**ページの設定 セクションを選択**プラットフォームの追加**web サイト アプリケーションを追加することを指定します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="c57c4-207">![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="c57c4-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="c57c4-208">選択**web サイト**プラットフォームの選択肢から。</span><span class="sxs-lookup"><span data-stu-id="c57c4-208">Select **Website** from the platform choices.</span></span>  
  
    ![プラットフォームの選択肢](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="c57c4-210">メモしておきます、**アプリ ID**と**アプリ シークレット**両方を MVC アプリケーションに、このチュートリアルの後半で追加できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="c57c4-211">また、サイトの URL を追加 (`https://localhost:44300/`)、MVC アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="c57c4-212">また、追加、 **Contact Email**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="c57c4-213">次に、選択**変更の保存**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-213">Then, select **Save Changes**.</span></span>   

    ![基本的なアプリケーションの詳細ページ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="c57c4-215">登録した電子メール エイリアスを使用して認証のみすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c57c4-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="c57c4-216">他のユーザーとテスト用のアカウントを登録することができなきます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="c57c4-217">その他の Facebook アカウント、Facebook 上のアプリケーションへのアクセスを付与する**開発者ロール**タブ。</span><span class="sxs-lookup"><span data-stu-id="c57c4-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="c57c4-218">Visual Studio で開く*アプリ\_Start\Startup.Auth.cs*します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="c57c4-219">コピーして貼り付け、 **AppId**と**アプリ シークレット**に、`UseFacebookAuthentication`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c57c4-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="c57c4-220">**AppId**と**アプリ シークレット**の下に表示される値のサンプルし、は機能しません。</span><span class="sxs-lookup"><span data-stu-id="c57c4-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="c57c4-221">クリックして**変更を保存**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="c57c4-222">キーを押して**CTRL + F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="c57c4-223">選択**ログイン**ログイン ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="c57c4-224">クリックして**Facebook** **別のサービスを使用してログインします。**</span><span class="sxs-lookup"><span data-stu-id="c57c4-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="c57c4-225">Facebook の資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="c57c4-226">パブリック プロファイルや友達リストにアクセスするアプリケーションのアクセス許可を付与するように促されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook アプリの詳細](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="c57c4-228">ログインしているようになりました。</span><span class="sxs-lookup"><span data-stu-id="c57c4-228">You are now logged in.</span></span> <span data-ttu-id="c57c4-229">アプリケーションでこのアカウントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="c57c4-230">エントリを追加登録するときに、*ユーザー*メンバーシップ データベースのテーブル。</span><span class="sxs-lookup"><span data-stu-id="c57c4-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="c57c4-231">メンバーシップ データを調べる</span><span class="sxs-lookup"><span data-stu-id="c57c4-231">Examine the Membership Data</span></span>

<span data-ttu-id="c57c4-232">**ビュー**  メニューのをクリックして**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="c57c4-233">展開**DefaultConnection (MvcAuth)**、展開**テーブル**を右クリックして**AspNetUsers**クリック**テーブル データの表示**。</span><span class="sxs-lookup"><span data-stu-id="c57c4-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers テーブル データ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="c57c4-235">プロファイル データをユーザー クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="c57c4-236">このセクションで追加します生年月日とホーム町のユーザー データを登録時に、次の図のようにします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![出身地と Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="c57c4-238">開く、 *Models\IdentityModels.cs*ファイルを開き生年月日の日付とホーム町のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="c57c4-239">開く、 *Models\AccountViewModels.cs*ファイルと、セットで日付とホームの町プロパティを birth`ExternalLoginConfirmationViewModel`します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="c57c4-240">開く、 *controllers \accountcontroller.cs*生年月日の日付とホーム町でのコードを追加するファイルを開き、`ExternalLoginConfirmation`に示すように、アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="c57c4-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="c57c4-241">追加の生年月日とホームに町に来ました、 *Views\Account\ExternalLoginConfirmation.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c57c4-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="c57c4-242">もう一度アプリケーションを自分の Facebook アカウントを登録し、新しい生年月日とホーム町プロファイル情報を追加することを確認できるように、メンバーシップ データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="c57c4-243">**ソリューション エクスプ ローラー**、クリックして、 **すべてのファイル**アイコン、し、右クリック*追加\_Data\aspnet-MvcAuth -&lt;日付スタンプ&gt;.mdf*クリック**削除**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="c57c4-244">**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**(PMC)。</span><span class="sxs-lookup"><span data-stu-id="c57c4-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="c57c4-245">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="c57c4-246">Enable-migrations</span><span class="sxs-lookup"><span data-stu-id="c57c4-246">Enable-Migrations</span></span>
2. <span data-ttu-id="c57c4-247">Add-migration Init</span><span class="sxs-lookup"><span data-stu-id="c57c4-247">Add-Migration Init</span></span>
3. <span data-ttu-id="c57c4-248">データベースの更新</span><span class="sxs-lookup"><span data-stu-id="c57c4-248">Update-Database</span></span>

<span data-ttu-id="c57c4-249">アプリケーションを実行し、FaceBook、Google を使用してにログインし、一部のユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="c57c4-250">メンバーシップ データを調べる</span><span class="sxs-lookup"><span data-stu-id="c57c4-250">Examine the Membership Data</span></span>

<span data-ttu-id="c57c4-251">**ビュー**  メニューのをクリックして**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="c57c4-252">右クリックして**AspNetUsers**クリック**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="c57c4-253">`HomeTown`と`BirthDate`フィールドを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="c57c4-254">アプリからログオフして、別のアカウントでのログ記録</span><span class="sxs-lookup"><span data-stu-id="c57c4-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="c57c4-255">Facebook、およびを使用してアプリにログオンして、ログアウトするとログインしようとしています。 もう一度 (同じブラウザーを使用)、別の Facebook アカウントですぐにログインされます以前に使用した Facebook アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="c57c4-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="c57c4-256">別のアカウントを使用するには、Facebook に移動し、Facebook にログアウトする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="c57c4-257">その他のサード パーティの認証プロバイダーに同じ規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="c57c4-258">または、別のブラウザーを使用して別のアカウントを使用してログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c57c4-259">次の手順</span><span class="sxs-lookup"><span data-stu-id="c57c4-259">Next Steps</span></span>

<span data-ttu-id="c57c4-260">参照してください[owin Yahoo と LinkedIn OAuth のセキュリティ プロバイダーの概要](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)Jerrie Pelser Yahoo と LinkedIn 手順については、します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="c57c4-261">Jerrie を参照してください。 有効にするソーシャル ログイン ボタンを取得する ASP.NET MVC 5 用ソーシャル ログイン ボタン。</span><span class="sxs-lookup"><span data-stu-id="c57c4-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="c57c4-262">拙著のチュートリアルに従ってください[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)、このチュートリアルを続行して、次に示します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="c57c4-263">アプリを Azure にデプロイする方法。</span><span class="sxs-lookup"><span data-stu-id="c57c4-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="c57c4-264">ロールを使用したアプリをセキュリティで保護する方法。</span><span class="sxs-lookup"><span data-stu-id="c57c4-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="c57c4-265">使用してアプリをセキュリティで保護する方法、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)と[Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)フィルター。</span><span class="sxs-lookup"><span data-stu-id="c57c4-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="c57c4-266">メンバーシップ API を使用して、ユーザーとロールを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="c57c4-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="c57c4-267">このチュートリアルの立った方法で改善できましたフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="c57c4-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="c57c4-268">また、新しいトピックを要求することもできます。[表示 Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="c57c4-269">要求し、ASP.NET に追加する新しい機能に投票できます。</span><span class="sxs-lookup"><span data-stu-id="c57c4-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="c57c4-270">ツールを投票するなど、[の作成し、ユーザーとロールを管理します。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="c57c4-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="c57c4-271">ASP.NET の外部認証サービスのしくみの良いについては、Robert McMurray を参照してください。[外部認証サービス](https://asp.net/web-api/overview/security/external-authentication-services)します。</span><span class="sxs-lookup"><span data-stu-id="c57c4-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="c57c4-272">Robert の記事では、Microsoft、Twitter の認証の有効化の詳細にもなります。</span><span class="sxs-lookup"><span data-stu-id="c57c4-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="c57c4-273">Tom Dykstra の優れた[EF と MVC チュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Entity Framework を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c57c4-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
