---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure Authentication |Microsoft ドキュメント"
author: Rick-Anderson
description: "Windows Azure Active Directory 用の Microsoft ASP.NET ツールを簡単に Windows Azure Web サイトでホストされている web アプリケーションの認証を有効にしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4deb3536699f1ef3025f8858ee71a76a1c2def18
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="windows-azure-authentication"></a><span data-ttu-id="6d1c6-103">Windows Azure の認証</span><span class="sxs-lookup"><span data-stu-id="6d1c6-103">Windows Azure Authentication</span></span>
====================
<span data-ttu-id="6d1c6-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6d1c6-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6d1c6-105">Microsoft ASP.NET ツールを Windows Azure Active Directory では、ホストされている web アプリケーションの認証を有効にする単純な[Windows Azure Web サイト](https://www.windowsazure.com/home/features/web-sites/)です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-105">Microsoft ASP.NET tools for Windows Azure Active Directory makes it simple to enable authentication for web applications hosted on [Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/).</span></span> <span data-ttu-id="6d1c6-106">Windows Azure 認証を使用して、組織、内部設置型 Active Directory から同期された会社のアカウントまたはカスタムの Windows Azure Active Directory ドメインで作成されたユーザーから Office 365 のユーザーを認証することができます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-106">You can use Windows Azure Authentication to authenticate Office 365 users from your organization, corporate accounts synced from your on-premise Active Directory or users created in your own custom Windows Azure Active Directory domain.</span></span> <span data-ttu-id="6d1c6-107">Windows Azure 認証を有効にすると、1 つを使用してユーザーを認証するアプリケーションを構成[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)テナントです。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-107">Enabling Windows Azure Authentication configures your application to authenticate users using a single [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.</span></span>
> 
> <span data-ttu-id="6d1c6-108">ASP.NET Windows Azure 認証ツールは web ロール、クラウド サービスでサポートされていませんが、そのためには、将来のリリースで予定します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-108">The ASP.NET Windows Azure Authentication tool is not supported for web roles in a cloud service but we plan to do so in a future release.</span></span> <span data-ttu-id="6d1c6-109">[Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) は Windows Azure web ロールでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-109">[Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) is supported in Windows Azure web roles.</span></span>
> 
> <span data-ttu-id="6d1c6-110">内部設置型 Active Directory と Windows Azure Active Directory テナント間の同期をセットアップする方法の詳細については、「[実装および管理を使用して AD FS 2.0 シングル サインオン](https://technet.microsoft.com/library/jj205462.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-110">For details on how to setup synchronization between your on-premise Active Directory and your Windows Azure Active Directory tenant please see [Use AD FS 2.0 to implement and manage single sign-on](https://technet.microsoft.com/library/jj205462.aspx).</span></span>
> 
> <span data-ttu-id="6d1c6-111">Windows Azure Active Directory は現在として使用できますが、[プレビュー サービスを無料](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-111">Windows Azure Active Directory is currently available as a [free preview service](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>


## <a name="requirements"></a><span data-ttu-id="6d1c6-112">要件:</span><span class="sxs-lookup"><span data-stu-id="6d1c6-112">Requirements:</span></span>

- <span data-ttu-id="6d1c6-113">Visual Studio 2012 または[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)</span><span class="sxs-lookup"><span data-stu-id="6d1c6-113">Visual Studio 2012 or [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)</span></span>
- <span data-ttu-id="6d1c6-114">[Web ツールの Visual Studio 2012 用の拡張機能](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)または[Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="6d1c6-114">[Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) or [Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span></span>
- <span data-ttu-id="6d1c6-115">[Microsoft ASP.NET Tools Windows for Visual Studio 2012 – Azure Active Directory](https://go.microsoft.com/fwlink/?LinkID=282306)または[Microsoft ASP.NET Tools Windows 用 Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span><span class="sxs-lookup"><span data-stu-id="6d1c6-115">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) or [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span></span>

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a><span data-ttu-id="6d1c6-116">Visual Studio 2012 での ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-116">Create an ASP.NET Web Application with Visual Studio 2012</span></span>

<span data-ttu-id="6d1c6-117">Visual Studio 2012 で任意の Web アプリケーションを作成することができます、このチュートリアルは、ASP.NET MVC イントラネット テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-117">You can create any Web Application with Visual Studio 2012, this tutorial uses the ASP.NET MVC intranet template.</span></span>

1. <span data-ttu-id="6d1c6-118">新しい ASP.NET MVC 4 イントラネット アプリケーションを作成し、すべての既定値をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-118">Create a new ASP.NET MVC 4 Intranet Application and accept all the defaults.</span></span> <span data-ttu-id="6d1c6-119">(In する必要があります**検査**net ではなく**を入力してください**net プロジェクト)。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-119">(It must be an In **tra** net and not In **ter** net project).</span></span>  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a><span data-ttu-id="6d1c6-120">(この場合、テナントのグローバル管理者である場合)、Window Azure 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-120">Enable Window Azure Authentication (When you are a Global Administrator of the Tenet)</span></span>

<span data-ttu-id="6d1c6-121">サインアップ、新しいテナントを作成するには (既存の Office 365 アカウント) を使用して、既存の Windows Azure Active Directory テナントがあるない場合、[新しい Windows Azure Active Directory アカウント](http://g.microsoftonline.com/0AX00en/5)です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-121">If you do not have an existing Windows Azure Active Directory tenant (For example, through an existing Office 365 account) you can create a new tenant by signing up for a [new Windows Azure Active Directory account](http://g.microsoftonline.com/0AX00en/5).</span></span>

1. <span data-ttu-id="6d1c6-122">[プロジェクト] メニューから選択**を有効にする Windows Azure Authentication**:</span><span class="sxs-lookup"><span data-stu-id="6d1c6-122">From the Project menu select **Enable Windows Azure Authentication**:</span></span>  
  
 ![](windows-azure-authentication/_static/image2.png)

2. <span data-ttu-id="6d1c6-123">Windows Azure Active Directory テナント (たとえば、contoso.onmicrosoft.com) のドメインを入力し、クリックして**を有効にする**:</span><span class="sxs-lookup"><span data-stu-id="6d1c6-123">Enter the domain for your Windows Azure Active Directory tenant (for example, contoso.onmicrosoft.com) and click **Enable**:</span></span>

![](windows-azure-authentication/_static/image3.png)

3. <span data-ttu-id="6d1c6-124">Windows Azure Active Directory テナントの管理者としてで Web 認証ダイアログ サインイン。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-124">In the Web Authentication dialog sign in as an administrator for your Windows Azure Active Directory tenant:</span></span>  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a><span data-ttu-id="6d1c6-125">Window Azure をテナントの管理者以外で有効にします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-125">Enable Window Azure by a non-administrator of the Tenet</span></span>

<span data-ttu-id="6d1c6-126">Windows Azure Active Directory テナントのグローバル管理者特権がない、アプリケーションのプロビジョニングのチェック ボックスをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-126">If you do not have Global Administrator privilege for your Windows Azure Active Directory tenant, you can uncheck the checkbox for provisioning the application.</span></span>

![](windows-azure-authentication/_static/image6.png)

<span data-ttu-id="6d1c6-127">ダイアログ ボックスが表示されます、**ドメイン**、**アプリケーションのプリンシパル Id**と**応答 URL** Azure Active Directory とアプリケーションのプロビジョニングに必要な理念です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-127">The dialog will display the **Domain**, **Application Principal Id** and **Reply URL** which are required for provisioning the application with an Azure Active Directory tenet.</span></span> <span data-ttu-id="6d1c6-128">アプリケーションをプロビジョニングするための十分な特権を持つユーザーにこの情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-128">You need to give this information to someone who has sufficient privilege to provision the application.</span></span> <span data-ttu-id="6d1c6-129">参照してください[Windows Azure Active Directory の ASP.NET アプリケーションにシングル サインオンを実装する方法](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)コマンドレットを使用して手動でサービス プリンシパルを作成する方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-129">See[How to implement single sign-on with Windows Azure Active Directory - ASP.NET Application](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) for details on how to use cmdlet to create the service principal manually.</span></span>  
<span data-ttu-id="6d1c6-130">アプリケーションが正常にプロビジョニングされるをクリックすると**、選択した設定を web.config の更新を続行**です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-130">Once the application has been successfully provisioned, you can click on **Continue to update web.config with the selected settings**.</span></span> <span data-ttu-id="6d1c6-131">クリックして、発生する可能性にプロビジョニングするための待機中にアプリケーションの開発を続行する場合**プロジェクト ファイルで設定を保存するに近い**です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-131">If you want to continue developing the application while waiting for provisioning to happen, you can click **Close to remember the settings in project file**.</span></span> <span data-ttu-id="6d1c6-132">次回を有効にする Windows Azure 認証を呼び出すし、プロビジョニングのチェック ボックスをオフに同じ設定が表示されますをクリックして**続行**、クリックして、 **web.configでこれらの設定を適用**.</span><span class="sxs-lookup"><span data-stu-id="6d1c6-132">The next time you invoke Enable Windows Azure Authentication and uncheck the provisioning checkbox, you will see the same settings and you can click **Continue**, then click, **Apply these settings in web.config**.</span></span>

1. <span data-ttu-id="6d1c6-133">アプリケーションが Windows Azure の認証用に構成され、Windows Azure Active Directory でプロビジョニングされています。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-133">Wait while your application is configured for Windows Azure Authentication and provisioned with Windows Azure Active Directory.</span></span>
2. <span data-ttu-id="6d1c6-134">アプリケーションの Windows Azure 認証を有効にすると、クリックして**閉じる。**</span><span class="sxs-lookup"><span data-stu-id="6d1c6-134">Once Windows Azure Authentication has been enabled for your application, click **Close:**</span></span> 

    ![](windows-azure-authentication/_static/image7.png)
3. <span data-ttu-id="6d1c6-135">F5 キーを押して、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-135">Hit F5 to run your application.</span></span> <span data-ttu-id="6d1c6-136">ログイン ページに自動的にリダイレクトする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-136">You should automatically get redirected to login page.</span></span> <span data-ttu-id="6d1c6-137">ディレクトリ テナントのユーザーの資格情報を使用して、アプリケーションにログインする.</span><span class="sxs-lookup"><span data-stu-id="6d1c6-137">Use the directory tenet user credentials to login to the application..</span></span>  

    ![](windows-azure-authentication/_static/image1.jpg)
4. <span data-ttu-id="6d1c6-138">アプリケーションが現在テスト用の自己署名証明書を使用しているため、証明書が信頼された証明機関から発行されていません、ブラウザーから、警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-138">Because your application is currently using a self-signed test certificate you will receive a warning from the browser that the certificate was not issued by a trusted certificate authority.</span></span>

    <span data-ttu-id="6d1c6-139">この警告は無視してかまいませんローカルの開発中をクリックして**このサイトの閲覧を続行します。**</span><span class="sxs-lookup"><span data-stu-id="6d1c6-139">This warning can be safely ignored during local development by clicking **Continue to this website:**</span></span> 

    ![](windows-azure-authentication/_static/image8.png)
5. <span data-ttu-id="6d1c6-140">今すぐログインに成功した Windows Azure 認証を使用して、アプリケーションに!</span><span class="sxs-lookup"><span data-stu-id="6d1c6-140">You have now successfully logged in to your application using Windows Azure Authentication!</span></span>

    ![](windows-azure-authentication/_static/image2.jpg)

<span data-ttu-id="6d1c6-141">Windows Azure を有効にする認証は、アプリケーションに、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-141">Enabling Windows Azure authentication makes the following changes to your application:</span></span>

- <span data-ttu-id="6d1c6-142">対策、クロスサイト リクエスト フォージェリ ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) クラス (*アプリ\_Start\AntiXsrfConfig.cs* ) プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-142">An Anti-Cross-Site Request Forgery ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) class ( *App\_Start\AntiXsrfConfig.cs* ) is added to your project.</span></span>
- <span data-ttu-id="6d1c6-143">NuGet パッケージの`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-143">The NuGet packages `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` is added to your project.</span></span>
- <span data-ttu-id="6d1c6-144">Windows Identity Foundation 設定、アプリケーションでは、Windows Azure Active Directory テナントからセキュリティ トークンを受け入れるように構成されます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-144">Windows Identity Foundation settings in your application will be configured to accept security tokens from your Windows Azure Active Directory tenant.</span></span> <span data-ttu-id="6d1c6-145">変更の展開ビューを表示する次の画像をクリックして、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-145">Click on the image below to see an expanded view of the changes made to the *Web.config* file.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.png)
- <span data-ttu-id="6d1c6-146">Windows Azure Active Directory テナントでアプリケーションのサービス プリンシパルがプロビジョニングされます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-146">A service principal for your application in your Windows Azure Active Directory tenant will be provisioned.</span></span>
- <span data-ttu-id="6d1c6-147">HTTPS が有効になっているとします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-147">HTTPS is enabled.</span></span>

## <a name="deploy-the-application-to-windows-azure"></a><span data-ttu-id="6d1c6-148">Windows Azure にアプリケーションを配置します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-148">Deploy the application to Windows Azure</span></span>

<span data-ttu-id="6d1c6-149">完全な手順については、次を参照してください。 [ASP.NET Web アプリケーションを、Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)です。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-149">For complete instructions, see [Deploying an ASP.NET Web Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="6d1c6-150">Azure Web サイトを Windows Azure 認証を使用してアプリケーションを発行するには。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-150">To publish an application using Windows Azure Authentication to an Azure Web Site:</span></span>

1. <span data-ttu-id="6d1c6-151">アプリケーションを右クリックし、選択**発行。**</span><span class="sxs-lookup"><span data-stu-id="6d1c6-151">Right click on your application and select **Publish:**</span></span> 

    ![](windows-azure-authentication/_static/image3.jpg)
2. <span data-ttu-id="6d1c6-152">[Web の発行] ダイアログ ボックスからダウンロードして、Azure Web サイトの発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-152">From the Publish Web dialog download and import a publishing profile for your Azure Web Site.</span></span>

    ![](windows-azure-authentication/_static/image4.jpg)
3. <span data-ttu-id="6d1c6-153">**接続** タブの表示、**送信先 URL** (、パブリックに公開されたアプリケーションの URL)。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-153">The **Connection** tab shows the **Destination URL** (the public facing URL for your application).</span></span> <span data-ttu-id="6d1c6-154">をクリックして**接続の検証**接続をテストします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-154">Click **Validate Connection** to test your connection:</span></span>

    ![](windows-azure-authentication/_static/image5.jpg)
4. <span data-ttu-id="6d1c6-155">この Azure Web サイトを発行している場合は、確認してください以前、**先で追加ファイルを削除する**アプリケーションを確認するには設定が正常に発行します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-155">If you have published to this Azure Web Site previously consider checking the **Remove additional files at destination** setting to ensure your application publishes cleanly.</span></span> <span data-ttu-id="6d1c6-156">通知、**を有効にする Windows Azure Authentication**  チェック ボックスは slected します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-156">Notice the **Enable Windows Azure Authentication** check box is slected.</span></span>  

    ![](windows-azure-authentication/_static/image10.png)
5. <span data-ttu-id="6d1c6-157">: 省略可能**プレビュー**タブをクリックして**開始プレビュー**に展開されたファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-157">Optional: On the **Preview** tab click **Start Preview** to see the files deployed.</span></span>

    ![](windows-azure-authentication/_static/image6.jpg)
6. <span data-ttu-id="6d1c6-158">をクリックして**を発行します。**</span><span class="sxs-lookup"><span data-stu-id="6d1c6-158">Click **Publish.**</span></span>

    <span data-ttu-id="6d1c6-159">ターゲット ホストを Windows Azure 認証を有効にするように促されます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-159">You will be prompted to enable Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="6d1c6-160">をクリックして**を有効にする**を続行します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-160">Click **Enable** to continue:</span></span>

    ![](windows-azure-authentication/_static/image11.png)
7. <span data-ttu-id="6d1c6-161">Windows Azure Active Directory テナントの管理者の資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-161">Enter your administrator credentials for your Windows Azure Active Directory tenant:</span></span>

    ![](windows-azure-authentication/_static/image7.jpg)
8. <span data-ttu-id="6d1c6-162">アプリケーションが正常に発行されると、公開された web サイト、ブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-162">Once your application has been successfully published, a browser will open to the published web site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6d1c6-163">5 分 (通常は少ない) アプリケーションのターゲット ホストの Windows Azure 認証を有効にした後は Windows Azure Active Directory でプロビジョニングされている完全に時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-163">It may take up to five minutes (typically must less) for your application to be fully provisioned with Windows Azure Active Directory after enabling Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="6d1c6-164">初めて実行する場合、アプリケーション エラー ACS50001 発生した場合: 名前 '[領域]' の証明書利用者が見つかりませんでした。 数分待ってから、し、もう一度アプリケーションを実行してください。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-164">When you first run your application if you receive error ACS50001: Relying party with name ‘[realm]' was not found, then wait a few minutes and try running the application again.</span></span>
9. <span data-ttu-id="6d1c6-165">メッセージが表示されたら、ディレクトリ内のユーザーとしてログインします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-165">When prompted, log in as a user in your directory:</span></span>

    ![](windows-azure-authentication/_static/image8.jpg)
10. <span data-ttu-id="6d1c6-166">正常に、Azure にログインして今すぐ Windows Azure 認証を使用してアプリケーションをホストします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-166">You have now successfully logged into your Azure hosted application using Windows Azure Authentication.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a><span data-ttu-id="6d1c6-167">既知の問題</span><span class="sxs-lookup"><span data-stu-id="6d1c6-167">Known Issues</span></span>

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a><span data-ttu-id="6d1c6-168">Windows Azure Authentication </o:p >< を使用して失敗したロールベースの承認//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-168">Role-based authorization fails when using Windows Azure Authentication<o:p></o:p></span></span>

<span data-ttu-id="6d1c6-169">Windows Azure Authentication が提供されていません、必要なロール要求ロールベースの承認を実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-169">Windows Azure Authentication does not currently provide the necessary role claim so that role-based authorization can be performed.</span></span> <span data-ttu-id="6d1c6-170">認証されたユーザーのロールを手動で取得から Windows Azure Active Directory </o:p ><//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-170">The role of the authenticated user must be manually retrieved from Windows Azure Active Directory.<o:p></o:p></span></span>

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="6d1c6-171">エラーの Windows Azure の認証結果を含むアプリケーションへの参照「ACS20016 にログオンしたユーザー (live.com) のドメインと一致しません、許可されてこのドメイン STS」< o:p ><//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-171">Browsing to an application with Windows Azure Authentication results in the error "ACS20016 The domain of the logged in user (live.com) does not match any allowed domain of this STS"<o:p></o:p></span></span>

<span data-ttu-id="6d1c6-172">400 エラー応答が返る可能性があります (たとえば、hotmail.com、live.com、outlook.com など) は、Microsoft アカウントにログインして既におり、Windows Azure 認証が有効になっているアプリケーションにアクセスしようとする場合、Microsoft アカウントのドメインWindows Azure Active Directory では認識されません。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-172">If you are already logged in to a Microsoft Account (for example hotmail.com, live.com, outlook.com) and you attempt to access an application that has enabled Windows Azure Authentication you may get a 400 error response because the domain of your Microsoft Account is not recognized by Windows Azure Active Directory.</span></span> <span data-ttu-id="6d1c6-173">アプリケーションへのログイン、ログアウトして Microsoft アカウントから最初にします </o:p ><//o:p >。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-173">To log into the application, log out from your Microsoft Account first.<o:p></o:p></span></span>

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a><span data-ttu-id="6d1c6-174"></O:p >< accounts.accesscontrol.windows.net 証明書の証明書の検証エラーの結果なし 以外の場合、アプリケーションと Windows Azure 認証が有効になっていると、X509CertificateValidationMode にログイン//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-174">Logging into an application with Windows Azure Authentication enabled and a X509CertificateValidationMode other than None results in certificate validation errors for the accounts.accesscontrol.windows.net certificate<o:p></o:p></span></span>

<span data-ttu-id="6d1c6-175">証明書の検証は不要であり無効のままにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-175">Certificate validation is not required and should be left disabled.</span></span> <span data-ttu-id="6d1c6-176">WSFederationAuthenticationModule の </o:p >< が発行者証明書の拇印を検証//o:p >。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-176">The thumbprint of the issuer certificate is validated by the WSFederationAuthenticationModule.<o:p></o:p></span></span>

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="6d1c6-177">Web 認証ダイアログがエラーを表示する Windows Azure 認証を有効にしようとすると"ACS20016: ログインのユーザー (contoso.onmicrosoft.com) のドメインが、この STS の許可されたどのドメインと一致しません"。</o:p ><//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-177">When attempting to enable Windows Azure Authentication the Web Authentication dialog shows the error "ACS20016: The domain of the logged in user (contoso.onmicrosoft.com) does not match any allowed domain of this STS."<o:p></o:p></span></span>

<span data-ttu-id="6d1c6-178">正常に同一の Visual Studio プロセス内から別の Windows Azure Active Directory アカウントを使用してログインしていたときに、このエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-178">You may see this error when you have previously successfully logged in using a different Windows Azure Active Directory account from within the same Visual Studio process.</span></span> <span data-ttu-id="6d1c6-179">指定されたアカウントからログアウトするか、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-179">Log out from the specified account or restart Visual Studio.</span></span> <span data-ttu-id="6d1c6-180">以前に記録「にサインインしたまま」するオプションを選択しと、ブラウザーの cookie </o:p >< をオフにする必要があります//o:p >。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-180">If you previously logged in and selected the option to "Keep me signed in" then you may need to clear your browser cookies.<o:p></o:p></span></span>

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a><span data-ttu-id="6d1c6-181">ACS20012: 要求が有効な Ws-federation プロトコル メッセージ </o:p ><//o:p ></span><span class="sxs-lookup"><span data-stu-id="6d1c6-181">ACS20012: The request is not a valid WS-Federation protocol message <o:p></o:p></span></span>

<span data-ttu-id="6d1c6-182">これは、既に Azure サービスのいずれかにその他のいくつかの Microsoft ID でログインしている場合に発生することができます。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-182">This can happen if you are already logged in with some other Microsoft ID to one of the Azure services.</span></span> <span data-ttu-id="6d1c6-183">使用するプライベート ブラウザー ウィンドウでは、IE で InPrivate または Incognito Chrome 内と同様にまたは、すべての cookie を消去します。</span><span class="sxs-lookup"><span data-stu-id="6d1c6-183">Use Private browser window like InPrivate in IE or Incognito in Chrome or clear all the cookies.</span></span> <span data-ttu-id="6d1c6-184"><o:p></o:p></span><span class="sxs-lookup"><span data-stu-id="6d1c6-184"><o:p></o:p></span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d1c6-185">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="6d1c6-185">Additional Resources</span></span>

- <span data-ttu-id="6d1c6-186">[Microsoft ASP.NET Tools Windows for Visual Studio 2012 – Azure Active Directory](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="6d1c6-186">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="6d1c6-187">Windows Azure の機能: Identity</span><span class="sxs-lookup"><span data-stu-id="6d1c6-187">Windows Azure Features: Identity</span></span>](https://docs.microsoft.com/azure/active-directory/)
- [<span data-ttu-id="6d1c6-188">TechNet: Windows Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d1c6-188">TechNet: Windows Azure Active Directory</span></span>](https://technet.microsoft.com/library/hh967619.aspx)
- [<span data-ttu-id="6d1c6-189">Windows Azure Active Directory: 組織向けアプリの開発</span><span class="sxs-lookup"><span data-stu-id="6d1c6-189">Windows Azure Active Directory: Develop Apps for your organization</span></span>](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [<span data-ttu-id="6d1c6-190">Windows Azure Active Directory: 複数の組織向けアプリの開発</span><span class="sxs-lookup"><span data-stu-id="6d1c6-190">Windows Azure Active Directory: Develop Apps for multiple organizations</span></span>](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [<span data-ttu-id="6d1c6-191">Windows Azure Active Directory にシングル サインオンを実装する方法</span><span class="sxs-lookup"><span data-stu-id="6d1c6-191">How to implement single sign-on with Windows Azure Active Directory</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- <span data-ttu-id="6d1c6-192">[シングル サインオンが windows Azure Active Directory: Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="6d1c6-192">[Single Sign-On with Windows Azure Active Directory: a Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="6d1c6-193">実装および管理を使用して AD FS 2.0 シングル サインオン</span><span class="sxs-lookup"><span data-stu-id="6d1c6-193">Use AD FS 2.0 to implement and manage single sign-on</span></span>](https://technet.microsoft.com/library/jj205462.aspx)
