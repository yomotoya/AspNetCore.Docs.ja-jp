---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API を使用した外部認証サービス (c#) |Microsoft Docs
author: rmcmurray
description: ASP.NET Web API で外部の認証サービスの使用について説明します。
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 0b23baac7eca0297e063c682a8ae199f9543d75e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830318"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="67ff1-103">ASP.NET Web API を使用した外部認証サービス (c#)</span><span class="sxs-lookup"><span data-stu-id="67ff1-103">External Authentication Services with ASP.NET Web API (C#)</span></span>
====================
<span data-ttu-id="67ff1-104">によって[Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="67ff1-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="67ff1-105">Visual Studio 2013 と ASP.NET 4.5.1 の展開のセキュリティ オプション[シングル ページ アプリケーション](../../../single-page-application/index.md)(SPA) と[Web API](../../index.md)サービスがいくつかありますが、外部認証サービスと統合するにはOauth/openid とソーシャル メディア サービスの認証: Microsoft アカウント、Twitter、Facebook、Google、します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-105">Visual Studio 2013 and ASP.NET 4.5.1 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>

### <a name="in-this-walkthrough"></a><span data-ttu-id="67ff1-106">このチュートリアルでは</span><span class="sxs-lookup"><span data-stu-id="67ff1-106">In This Walkthrough</span></span>

- [<span data-ttu-id="67ff1-107">外部認証サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-107">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="67ff1-108">サンプル Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-108">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="67ff1-109">Facebook 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-109">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="67ff1-110">Google 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-110">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="67ff1-111">Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-111">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="67ff1-112">Twitter 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-112">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="67ff1-113">追加情報</span><span class="sxs-lookup"><span data-stu-id="67ff1-113">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="67ff1-114">外部認証サービスを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="67ff1-114">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="67ff1-115">IIS Express を使用して、完全修飾ドメイン名を構成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-115">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="67ff1-116">Microsoft 認証用のアプリケーションの設定を取得する方法</span><span class="sxs-lookup"><span data-stu-id="67ff1-116">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="67ff1-117">省略可能: ローカルの登録を無効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-117">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="67ff1-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="67ff1-118">Prerequisites</span></span>

<span data-ttu-id="67ff1-119">このチュートリアルのサンプルを理解するには、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="67ff1-119">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="67ff1-120">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="67ff1-120">Visual Studio 2013</span></span>
- <span data-ttu-id="67ff1-121">次の外部認証サービスの少なくとも 1 つのアカウント:</span><span class="sxs-lookup"><span data-stu-id="67ff1-121">An account for at least one of the following external authentication services:</span></span>

    - <span data-ttu-id="67ff1-122">Google のユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="67ff1-122">A Google user account</span></span>
    - <span data-ttu-id="67ff1-123">アプリケーション識別子とソーシャル メディアの次の認証サービスの 1 つの秘密鍵の開発者アカウント:</span><span class="sxs-lookup"><span data-stu-id="67ff1-123">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

        - <span data-ttu-id="67ff1-124">Microsoft アカウント ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="67ff1-124">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
        - <span data-ttu-id="67ff1-125">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="67ff1-125">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
        - <span data-ttu-id="67ff1-126">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="67ff1-126">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="67ff1-127">外部認証サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-127">Using External Authentication Services</span></span>

<span data-ttu-id="67ff1-128">開発の削減は、web 開発者のヘルプを現在利用できる外部認証サービスの豊富な新しい web アプリケーションを作成するときの時間します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-128">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="67ff1-129">Web ユーザーを通常いくつか既存のアカウントがある一般的な web サービスとソーシャル メディアの web サイトのため保存するとき、外部 web サービスやソーシャル メディアの web サイトから、認証サービスの web アプリケーションの実装、開発時間費やされた認証の実装を作成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-129">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="67ff1-130">外部認証サービスを使用して、web アプリケーションの別のアカウントを作成する必要がありませんし、別のユーザー名とパスワードを覚えておく必要がなくなりますエンドユーザーを保存します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-130">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="67ff1-131">以前は、開発者には 2 つの選択肢も必要があるが: 独自の認証の実装を作成または外部認証サービスをアプリケーションに統合する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-131">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="67ff1-132">最も基本的なレベルは、次の図は、外部認証サービスを使用するように構成されている web アプリケーションから情報を要求して、ユーザー エージェント (web ブラウザー) 単純な要求のフローを示しています。</span><span class="sxs-lookup"><span data-stu-id="67ff1-132">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

<span data-ttu-id="67ff1-133">[![](external-authentication-services/_static/image2.png "クリックして、イメージの展開")](external-authentication-services/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-133">[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)</span></span>

<span data-ttu-id="67ff1-134">前の図では、ユーザー エージェント (またはこの例では、web ブラウザー) は、外部認証サービスに web ブラウザーをリダイレクトする web アプリケーションに、要求を行います。</span><span class="sxs-lookup"><span data-stu-id="67ff1-134">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="67ff1-135">ユーザー エージェントは、外部認証サービスにその資格情報を送信し、外部認証サービスが何らかの形で元の web アプリケーションにユーザー エージェントをリダイレクトして、ユーザー エージェントが正常に認証されると場合、トークンが、ユーザー エージェントは、web アプリケーションに送信されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-135">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="67ff1-136">Web アプリケーションは、外部認証サービスによってユーザー エージェントが正常に認証されたされ、web アプリケーションがトークンを使用してユーザー エージェントに関する情報を収集するのにことを確認トークンを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-136">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="67ff1-137">アプリケーションが完了するとユーザー エージェントの情報を処理するには、web アプリケーションはその承認設定に基づき、ユーザー エージェントを適切な応答が返すされます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-137">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="67ff1-138">この 2 つ目の例では、web アプリケーションと外部承認サーバーとネゴシエートし、ユーザー エージェントと web アプリケーションは、ユーザーに関する追加情報を取得する外部承認サーバーに追加の通信を実行します。エージェント:</span><span class="sxs-lookup"><span data-stu-id="67ff1-138">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

<span data-ttu-id="67ff1-139">[![](external-authentication-services/_static/image4.png "クリックして、イメージの展開")](external-authentication-services/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-139">[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)</span></span>

<span data-ttu-id="67ff1-140">Visual Studio 2013 と ASP.NET 4.5.1 やすく外部認証サービスとの統合開発者のため、次の認証サービスの組み込みの統合を提供することで。</span><span class="sxs-lookup"><span data-stu-id="67ff1-140">Visual Studio 2013 and ASP.NET 4.5.1 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="67ff1-141">Facebook</span><span class="sxs-lookup"><span data-stu-id="67ff1-141">Facebook</span></span>
- <span data-ttu-id="67ff1-142">Google</span><span class="sxs-lookup"><span data-stu-id="67ff1-142">Google</span></span>
- <span data-ttu-id="67ff1-143">Microsoft アカウント (Windows Live ID アカウント)</span><span class="sxs-lookup"><span data-stu-id="67ff1-143">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="67ff1-144">Twitter</span><span class="sxs-lookup"><span data-stu-id="67ff1-144">Twitter</span></span>

<span data-ttu-id="67ff1-145">このチュートリアルの例では、サポートされている外部認証サービスの各 Visual Studio 2013 に付属している新しい ASP.NET Web アプリケーション テンプレートを使用して構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-145">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2013.</span></span>

> [!NOTE]
> <span data-ttu-id="67ff1-146">必要に応じては、外部認証サービスの設定に、FQDN を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67ff1-146">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="67ff1-147">この要件は、クライアントによって使用される FQDN と一致する、アプリケーションの設定で FQDN を必要とするいくつかの外部認証サービスに対するセキュリティ制約に基づいています。</span><span class="sxs-lookup"><span data-stu-id="67ff1-147">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="67ff1-148">(この手順は各外部認証サービスの大幅に異なります。 これは必要なかどうかに表示するには、各外部認証サービスとこれらの設定を構成する方法のドキュメントを参照する必要があります)。IIS Express を参照してください、この環境をテストするために FQDN を使用して構成する必要がある場合、[完全修飾ドメイン名を使用する IIS Express を構成する](#FQDN)このチュートリアルの後半の「します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-148">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a><span data-ttu-id="67ff1-149">サンプル Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-149">Creating a Sample Web Application</span></span>

<span data-ttu-id="67ff1-150">次の手順に従って、ASP.NET Web アプリケーション テンプレートを使用して、サンプル アプリケーションを作成して、このチュートリアルの後半で外部認証サービスごとにこのサンプル アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-150">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="67ff1-151">Visual Studio 2013 の選択の開始**新しいプロジェクト**スタート ページから。</span><span class="sxs-lookup"><span data-stu-id="67ff1-151">Start Visual Studio 2013 select **New Project** from the Start page.</span></span> <span data-ttu-id="67ff1-152">またはから、**ファイル**メニューの **新規**し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-152">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="67ff1-153">[![](external-authentication-services/_static/image6.png "クリックして、イメージの展開")](external-authentication-services/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-153">[![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png)</span></span>

<span data-ttu-id="67ff1-154">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、選択**インストール済み****テンプレート**展開**Visual c#** します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-154">When the **New Project** dialog box is displayed, select **Installed** **Templates** and expand **Visual C#**.</span></span> <span data-ttu-id="67ff1-155">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-155">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="67ff1-156">プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-156">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="67ff1-157">プロジェクトの名前を入力し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-157">Enter a name for your project and click **OK**.</span></span>

<span data-ttu-id="67ff1-158">[![](external-authentication-services/_static/image8.png "クリックして、イメージの展開")](external-authentication-services/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-158">[![](external-authentication-services/_static/image8.png "Click to Expand the Image")](external-authentication-services/_static/image7.png)</span></span>

<span data-ttu-id="67ff1-159">ときに、**新しい ASP.NET プロジェクト**表示されている、select、 **SPA**テンプレートをクリックします**プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="67ff1-159">When the **New ASP.NET Project** is displayed, select the **SPA** template and click **Create Project**.</span></span>

<span data-ttu-id="67ff1-160">[![](external-authentication-services/_static/image10.png "クリックして、イメージの展開")](external-authentication-services/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-160">[![](external-authentication-services/_static/image10.png "Click to Expand the Image")](external-authentication-services/_static/image9.png)</span></span>

<span data-ttu-id="67ff1-161">Visual Studio 2013 と待機は、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-161">Wait as Visual Studio 2013 creates your project.</span></span>

<span data-ttu-id="67ff1-162">[![](external-authentication-services/_static/image12.png "クリックして、イメージの展開")](external-authentication-services/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-162">[![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png)</span></span>

<span data-ttu-id="67ff1-163">Visual Studio 2013 は、プロジェクトの作成が完了したら、開く、 *Startup.Auth.cs*に配置されているファイル、**アプリ\_開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="67ff1-163">When Visual Studio 2013 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="67ff1-164">[![](external-authentication-services/_static/image14.png "クリックして、イメージの展開")](external-authentication-services/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-164">[![](external-authentication-services/_static/image14.png "Click to Expand the Image")](external-authentication-services/_static/image13.png)</span></span>

<span data-ttu-id="67ff1-165">外部認証サービスのどれも有効で最初にプロジェクトを作成するときに*Startup.Auth.cs*ファイルはどのようなコードのようになります次の図は、先の強調表示されているセクションを使用すると有効にします。外部認証サービスと ASP.NET アプリケーションで Microsoft アカウント、Twitter、Facebook、または Google の認証を使用するには関連の設定:</span><span class="sxs-lookup"><span data-stu-id="67ff1-165">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="67ff1-166">F5 キーをビルドして、web アプリケーションのデバッグを押すと、外部認証サービスが定義されていないことを「がログイン画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-166">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

<span data-ttu-id="67ff1-167">[![](external-authentication-services/_static/image16.png "クリックして、イメージの展開")](external-authentication-services/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-167">[![](external-authentication-services/_static/image16.png "Click to Expand the Image")](external-authentication-services/_static/image15.png)</span></span>

<span data-ttu-id="67ff1-168">次のセクションでは、各 Visual Studio 2013 での ASP.NET で用意されている外部認証サービスを有効にする方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-168">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2013.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="67ff1-169">Facebook 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-169">Enabling Facebook Authentication</span></span>

<span data-ttu-id="67ff1-170">Facebook を使用して認証では、Facebook 開発者アカウントを作成する必要があります、プロジェクトが必要し、なりますアプリケーション ID と秘密キーの Facebook から機能するためにします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-170">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="67ff1-171">Facebook 開発者アカウントを作成して、アプリケーション ID と秘密キーの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-171">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="67ff1-172">1 回、アプリケーション ID とシークレット キーを取得した、web アプリケーションの Facebook 認証を有効にする、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-172">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="67ff1-173">プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67ff1-173">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="67ff1-174">[![](external-authentication-services/_static/image18.png "クリックして、イメージの展開")](external-authentication-services/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-174">[![](external-authentication-services/_static/image18.png "Click to Expand the Image")](external-authentication-services/_static/image17.png)</span></span>
2. <span data-ttu-id="67ff1-175">コードの強調表示されたセクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-175">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="67ff1-176">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、アプリケーション ID とシークレット キーを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="67ff1-176">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="67ff1-177">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-177">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="67ff1-178">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Facebook を外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-178">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    <span data-ttu-id="67ff1-179">[![](external-authentication-services/_static/image20.png "クリックして、イメージの展開")](external-authentication-services/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-179">[![](external-authentication-services/_static/image20.png "Click to Expand the Image")](external-authentication-services/_static/image19.png)</span></span>
5. <span data-ttu-id="67ff1-180">クリックすると、 **Facebook**ボタン、お使いのブラウザーは、Facebook のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-180">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    <span data-ttu-id="67ff1-181">[![](external-authentication-services/_static/image22.png "クリックして、イメージの展開")](external-authentication-services/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-181">[![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)</span></span>
6. <span data-ttu-id="67ff1-182">Facebook の資格情報を入力し、をクリックした後**ログイン**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたい、Facebook のアカウント:</span><span class="sxs-lookup"><span data-stu-id="67ff1-182">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    <span data-ttu-id="67ff1-183">[![](external-authentication-services/_static/image24.png "クリックして、イメージの展開")](external-authentication-services/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-183">[![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)</span></span>
7. <span data-ttu-id="67ff1-184">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Facebook アカウント。</span><span class="sxs-lookup"><span data-stu-id="67ff1-184">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    <span data-ttu-id="67ff1-185">[![](external-authentication-services/_static/image26.png "クリックして、イメージの展開")](external-authentication-services/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-185">[![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)</span></span>

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="67ff1-186">Google 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-186">Enabling Google Authentication</span></span>

<span data-ttu-id="67ff1-187">Google ははるかに簡単に、開発者アカウントを必要としないことも、アプリケーションの ID やシークレット キーなど、他の外部認証サービスとしての追加情報は必要であるために、有効に外部認証サービスの必要になります。</span><span class="sxs-lookup"><span data-stu-id="67ff1-187">Google is by far the easiest of the external authentication services to enable because it doesn't require a developer account, nor does it require additional information like your application ID or secret key as the other external authentication services necessitate.</span></span>

<span data-ttu-id="67ff1-188">Web アプリケーション用に Google 認証を有効にするには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-188">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="67ff1-189">プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67ff1-189">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="67ff1-190">[![](external-authentication-services/_static/image28.png "クリックして、イメージの展開")](external-authentication-services/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-190">[![](external-authentication-services/_static/image28.png "Click to Expand the Image")](external-authentication-services/_static/image27.png)</span></span>
2. <span data-ttu-id="67ff1-191">コードの強調表示されたセクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-191">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="67ff1-192">削除、 &quot; // &quot;コードの強調表示された行をコメント解除し、プロジェクトを再コンパイルする文字。</span><span class="sxs-lookup"><span data-stu-id="67ff1-192">Remove the &quot;//&quot; characters to uncomment the highlighted line of code, and then recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="67ff1-193">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Google が外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-193">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    <span data-ttu-id="67ff1-194">[![](external-authentication-services/_static/image30.png "クリックして、イメージの展開")](external-authentication-services/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-194">[![](external-authentication-services/_static/image30.png "Click to Expand the Image")](external-authentication-services/_static/image29.png)</span></span>
5. <span data-ttu-id="67ff1-195">クリックすると、 **Google**ボタン、お使いのブラウザーは Google のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-195">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    <span data-ttu-id="67ff1-196">[![](external-authentication-services/_static/image32.png "クリックして、イメージの展開")](external-authentication-services/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-196">[![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)</span></span>
6. <span data-ttu-id="67ff1-197">Google の資格情報を入力し をクリックした後**サインイン**Google には、web アプリケーションに Google アカウントにアクセスするアクセス許可があることを確認するように求められます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-197">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    <span data-ttu-id="67ff1-198">[![](external-authentication-services/_static/image34.png "クリックして、イメージの展開")](external-authentication-services/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-198">[![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)</span></span>
7. <span data-ttu-id="67ff1-199">クリックすると**Accept**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**Google アカウントに関連付けるにします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-199">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    <span data-ttu-id="67ff1-200">[![](external-authentication-services/_static/image36.png "クリックして、イメージの展開")](external-authentication-services/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-200">[![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)</span></span>
8. <span data-ttu-id="67ff1-201">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Google アカウント。</span><span class="sxs-lookup"><span data-stu-id="67ff1-201">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    <span data-ttu-id="67ff1-202">[![](external-authentication-services/_static/image38.png "クリックして、イメージの展開")](external-authentication-services/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-202">[![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)</span></span>

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="67ff1-203">Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-203">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="67ff1-204">Microsoft 認証では、開発者アカウントを作成する必要があり、機能するために、クライアント ID とクライアント シークレットが必要です。</span><span class="sxs-lookup"><span data-stu-id="67ff1-204">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="67ff1-205">Microsoft 開発者アカウントを作成して、クライアント ID とクライアント シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-205">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="67ff1-206">1 回、コンシューマー キーとコンシューマー シークレットを取得した、次の手順を使用して web アプリケーション用に Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-206">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="67ff1-207">プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67ff1-207">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="67ff1-208">[![](external-authentication-services/_static/image40.png "クリックして、イメージの展開")](external-authentication-services/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-208">[![](external-authentication-services/_static/image40.png "Click to Expand the Image")](external-authentication-services/_static/image39.png)</span></span>
2. <span data-ttu-id="67ff1-209">コードの強調表示されたセクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-209">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="67ff1-210">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、クライアント ID とクライアント シークレットを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="67ff1-210">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="67ff1-211">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-211">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="67ff1-212">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Microsoft が、外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-212">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    <span data-ttu-id="67ff1-213">[![](external-authentication-services/_static/image42.png "クリックして、イメージの展開")](external-authentication-services/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-213">[![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)</span></span>
5. <span data-ttu-id="67ff1-214">クリックすると、 **Microsoft**ボタン、お使いのブラウザーは Microsoft のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-214">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    <span data-ttu-id="67ff1-215">[![](external-authentication-services/_static/image44.png "クリックして、イメージの展開")](external-authentication-services/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-215">[![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)</span></span>
6. <span data-ttu-id="67ff1-216">Microsoft の資格情報を入力し をクリックした後**サインイン**、web アプリケーションが Microsoft アカウントへのアクセス権限を持っていることを確認するメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-216">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    <span data-ttu-id="67ff1-217">[![](external-authentication-services/_static/image46.png "クリックして、イメージの展開")](external-authentication-services/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-217">[![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)</span></span>
7. <span data-ttu-id="67ff1-218">クリックすると**はい**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**を Microsoft アカウントに関連付けるにします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-218">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    <span data-ttu-id="67ff1-219">[![](external-authentication-services/_static/image48.png "クリックして、イメージの展開")](external-authentication-services/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-219">[![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)</span></span>
8. <span data-ttu-id="67ff1-220">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Microsoft アカウント。</span><span class="sxs-lookup"><span data-stu-id="67ff1-220">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    <span data-ttu-id="67ff1-221">[![](external-authentication-services/_static/image50.png "クリックして、イメージの展開")](external-authentication-services/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-221">[![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)</span></span>

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="67ff1-222">Twitter 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-222">Enabling Twitter Authentication</span></span>

<span data-ttu-id="67ff1-223">Twitter の認証では、開発者アカウントを作成する必要があり、機能するために、コンシューマー キーとコンシューマー シークレットが必要です。</span><span class="sxs-lookup"><span data-stu-id="67ff1-223">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="67ff1-224">Twitter 開発者アカウントを作成して、コンシューマー キーとコンシューマー シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-224">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="67ff1-225">1 回、コンシューマー キーとコンシューマー シークレットを取得した、web アプリケーション用に Twitter 認証を有効にする、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-225">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="67ff1-226">プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67ff1-226">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="67ff1-227">[![](external-authentication-services/_static/image52.png "クリックして、イメージの展開")](external-authentication-services/_static/image51.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-227">[![](external-authentication-services/_static/image52.png "Click to Expand the Image")](external-authentication-services/_static/image51.png)</span></span>
2. <span data-ttu-id="67ff1-228">コードの強調表示されたセクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-228">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="67ff1-229">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、コンシューマー キーとコンシューマー シークレットを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="67ff1-229">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="67ff1-230">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-230">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="67ff1-231">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Twitter が外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-231">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    <span data-ttu-id="67ff1-232">[![](external-authentication-services/_static/image54.png "クリックして、イメージの展開")](external-authentication-services/_static/image53.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-232">[![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)</span></span>
5. <span data-ttu-id="67ff1-233">クリックすると、 **Twitter**ボタン、お使いのブラウザーは、Twitter ログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-233">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    <span data-ttu-id="67ff1-234">[![](external-authentication-services/_static/image56.png "クリックして、イメージの展開")](external-authentication-services/_static/image55.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-234">[![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)</span></span>
6. <span data-ttu-id="67ff1-235">Twitter の資格情報を入力し、をクリックした後**Authorize app**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたいです。Twitter アカウント:</span><span class="sxs-lookup"><span data-stu-id="67ff1-235">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    <span data-ttu-id="67ff1-236">[![](external-authentication-services/_static/image58.png "クリックして、イメージの展開")](external-authentication-services/_static/image57.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-236">[![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)</span></span>
7. <span data-ttu-id="67ff1-237">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Twitter アカウント。</span><span class="sxs-lookup"><span data-stu-id="67ff1-237">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    <span data-ttu-id="67ff1-238">[![](external-authentication-services/_static/image60.png "クリックして、イメージの展開")](external-authentication-services/_static/image59.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-238">[![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)</span></span>

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="67ff1-239">追加情報</span><span class="sxs-lookup"><span data-stu-id="67ff1-239">Additional Information</span></span>

<span data-ttu-id="67ff1-240">OAuth および OpenID を使用するアプリケーションの作成に関する追加情報は、次の Url を参照してください。</span><span class="sxs-lookup"><span data-stu-id="67ff1-240">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="67ff1-241">外部認証サービスを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="67ff1-241">Combining External Authentication Services</span></span>

<span data-ttu-id="67ff1-242">柔軟性を高め、同時に複数の外部認証サービスを定義する - これにより、web アプリケーションのユーザーからいずれかの外部認証を有効になっているサービス アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-242">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

<span data-ttu-id="67ff1-243">[![](external-authentication-services/_static/image62.png "クリックして、イメージの展開")](external-authentication-services/_static/image61.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-243">[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)</span></span>

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="67ff1-244">IIS Express を使用して、完全修飾ドメイン名を構成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-244">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>

<span data-ttu-id="67ff1-245">一部の外部認証プロバイダーは、アプリケーションのような HTTP アドレスを使用してテストをサポートしていません`http://localhost:port/`します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-245">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="67ff1-246">この問題を回避するには、は、静的な完全修飾ドメイン名 (FQDN) のマッピング、HOSTS ファイルに追加し、テストおよびデバッグの FQDN を使用する Visual Studio 2013 で、プロジェクトのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-246">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2013 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="67ff1-247">これを行うには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-247">To do so, use the following steps:</span></span>

- <span data-ttu-id="67ff1-248">ホスト ファイル マッピング静的 FQDN を追加します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-248">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="67ff1-249">Windows では、管理者特権のコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-249">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="67ff1-250">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-250">Type the following command:</span></span>

      <span data-ttu-id="67ff1-251"><kbd>メモ帳 %WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="67ff1-251"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="67ff1-252">次のようなエントリを HOSTS ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-252">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="67ff1-253"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="67ff1-253"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="67ff1-254">保存し、HOSTS ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-254">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="67ff1-255">FQDN を使用して、Visual Studio プロジェクトを構成するには。</span><span class="sxs-lookup"><span data-stu-id="67ff1-255">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="67ff1-256">プロジェクトが Visual Studio 2013 で開いているときは、クリックして、**プロジェクト**] メニューの [し、プロジェクトのプロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-256">When your project is open in Visual Studio 2013, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="67ff1-257">たとえば、選択した可能性があります**WebApplication1 プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-257">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="67ff1-258">選択、 **Web**タブ。</span><span class="sxs-lookup"><span data-stu-id="67ff1-258">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="67ff1-259">FQDN を入力、<strong>プロジェクト Url</strong>します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-259">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="67ff1-260">たとえばは入力<kbd> <http://www.wingtiptoys.com> </kbd> HOSTS ファイルに追加した FQDN マッピングだったかどうか。</span><span class="sxs-lookup"><span data-stu-id="67ff1-260">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="67ff1-261">IIS Express で、アプリケーションの FQDN を使用するかを構成します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-261">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="67ff1-262">Windows では、管理者特権のコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-262">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="67ff1-263">IIS Express フォルダーに変更するのには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-263">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="67ff1-264"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="67ff1-264"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="67ff1-265">アプリケーションに FQDN を追加するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-265">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="67ff1-266"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="67ff1-266"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="67ff1-267">場所**WebApplication1** 、プロジェクトの名前を指定し、 **bindingInformation**テストに使用するポート番号と FQDN が含まれています。</span><span class="sxs-lookup"><span data-stu-id="67ff1-267">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="67ff1-268">Microsoft 認証用のアプリケーションの設定を取得する方法</span><span class="sxs-lookup"><span data-stu-id="67ff1-268">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="67ff1-269">Microsoft 認証用の Windows Live にアプリケーションをリンクは、単純なプロセスです。</span><span class="sxs-lookup"><span data-stu-id="67ff1-269">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="67ff1-270">Windows Live にアプリケーションをリンクしていない場合は、次の手順を使用できます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-270">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="67ff1-271">参照する[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) 、Microsoft アカウント名とメッセージが表示されたら、パスワードを入力し、をクリックして**サインイン**:</span><span class="sxs-lookup"><span data-stu-id="67ff1-271">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

    <span data-ttu-id="67ff1-272">[![](external-authentication-services/_static/image64.png "クリックして、イメージの展開")](external-authentication-services/_static/image63.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-272">[![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png)</span></span>
2. <span data-ttu-id="67ff1-273">メッセージが表示されたら、アプリケーションの言語と名前を入力し、クリックして**同意**:</span><span class="sxs-lookup"><span data-stu-id="67ff1-273">Enter the name and language of your application when prompted, and then click **I accept**:</span></span>

    <span data-ttu-id="67ff1-274">[![](external-authentication-services/_static/image66.png "クリックして、イメージの展開")](external-authentication-services/_static/image65.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-274">[![](external-authentication-services/_static/image66.png "Click to Expand the Image")](external-authentication-services/_static/image65.png)</span></span>
3. <span data-ttu-id="67ff1-275">**API 設定**、アプリケーションのページで、コピー、アプリケーション リダイレクト ドメインを入力、**クライアント ID**と**クライアント シークレット**プロジェクトのしクリックして**保存**:</span><span class="sxs-lookup"><span data-stu-id="67ff1-275">On the **API Settings** page for your application, enter the redirect domain for your application and copy the **Client ID** and **Client secret** for your project, and then click **Save**:</span></span>

    <span data-ttu-id="67ff1-276">[![](external-authentication-services/_static/image68.png "クリックして、イメージの展開")](external-authentication-services/_static/image67.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-276">[![](external-authentication-services/_static/image68.png "Click to Expand the Image")](external-authentication-services/_static/image67.png)</span></span>

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="67ff1-277">省略可能: ローカルの登録を無効にします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-277">Optional: Disable Local Registration</span></span>

<span data-ttu-id="67ff1-278">現在の ASP.NET ローカル登録機能も自動プログラム (ボット) からメンバーのアカウントを作成します。たとえば、ボット防止と検証などのテクノロジを使用して[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)します。</span><span class="sxs-lookup"><span data-stu-id="67ff1-278">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="67ff1-279">このため、ログイン ページのローカル ログイン フォームと登録リンクを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67ff1-279">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="67ff1-280">これを行うには、開く、  *\_Login.cshtml*プロジェクトで、ページし、ローカル ログイン パネルと登録リンクの行をコメントにします。</span><span class="sxs-lookup"><span data-stu-id="67ff1-280">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="67ff1-281">結果のページは、次のコード サンプルのようになります。</span><span class="sxs-lookup"><span data-stu-id="67ff1-281">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="67ff1-282">ローカル ログイン パネルと登録リンクを無効になっていると、ログイン ページは有効にした外部認証プロバイダーをのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="67ff1-282">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

<span data-ttu-id="67ff1-283">[![](external-authentication-services/_static/image70.png "クリックして、イメージの展開")](external-authentication-services/_static/image69.png)</span><span class="sxs-lookup"><span data-stu-id="67ff1-283">[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)</span></span>
