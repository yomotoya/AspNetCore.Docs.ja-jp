---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API を使用した外部認証サービス (c#) |Microsoft Docs
author: rmcmurray
description: ASP.NET Web API で外部の認証サービスの使用について説明します。
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: de9b64e6c582059ec66ab352f60773f50af7b1ff
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667857"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="e1b81-103">ASP.NET Web API を使用した外部認証サービス (c#)</span><span class="sxs-lookup"><span data-stu-id="e1b81-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="e1b81-104">展開のセキュリティ オプションの visual Studio 2017 と ASP.NET 4.7.2[シングル ページ アプリケーション](../../../single-page-application/index.md)(SPA) と[Web API](../../index.md)サービスがいくつかありますが、外部認証サービスと統合するにはOauth/openid とソーシャル メディア サービスの認証:Microsoft アカウント、Twitter、Facebook、および Google。</span><span class="sxs-lookup"><span data-stu-id="e1b81-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="e1b81-105">このチュートリアルでは</span><span class="sxs-lookup"><span data-stu-id="e1b81-105">In this Walkthrough</span></span>

- [<span data-ttu-id="e1b81-106">外部認証サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="e1b81-107">サンプル Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="e1b81-108">Facebook 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="e1b81-109">Google 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="e1b81-110">Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="e1b81-111">Twitter 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="e1b81-112">追加情報</span><span class="sxs-lookup"><span data-stu-id="e1b81-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="e1b81-113">外部認証サービスを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="e1b81-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="e1b81-114">IIS Express を使用して、完全修飾ドメイン名を構成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="e1b81-115">Microsoft 認証用のアプリケーションの設定を取得する方法</span><span class="sxs-lookup"><span data-stu-id="e1b81-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="e1b81-116">省略可能:ローカルの登録を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="e1b81-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e1b81-117">Prerequisites</span></span>

<span data-ttu-id="e1b81-118">このチュートリアルのサンプルを理解するには、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="e1b81-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="e1b81-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e1b81-119">Visual Studio 2017</span></span>
- <span data-ttu-id="e1b81-120">アプリケーション識別子とソーシャル メディアの次の認証サービスの 1 つの秘密鍵の開発者アカウント:</span><span class="sxs-lookup"><span data-stu-id="e1b81-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="e1b81-121">Microsoft アカウント ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="e1b81-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="e1b81-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="e1b81-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="e1b81-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="e1b81-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="e1b81-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="e1b81-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="e1b81-125">外部認証サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-125">Using External Authentication Services</span></span>

<span data-ttu-id="e1b81-126">開発の削減は、web 開発者のヘルプを現在利用できる外部認証サービスの豊富な新しい web アプリケーションを作成するときの時間します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="e1b81-127">Web ユーザーを通常いくつか既存のアカウントがある一般的な web サービスとソーシャル メディアの web サイトのため保存するとき、外部 web サービスやソーシャル メディアの web サイトから、認証サービスの web アプリケーションの実装、開発時間費やされた認証の実装を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="e1b81-128">外部認証サービスを使用して、web アプリケーションの別のアカウントを作成する必要がありませんし、別のユーザー名とパスワードを覚えておく必要がなくなりますエンドユーザーを保存します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="e1b81-129">以前は、開発者には 2 つの選択肢も必要があるが: 独自の認証の実装を作成または外部認証サービスをアプリケーションに統合する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="e1b81-130">最も基本的なレベルは、次の図は、外部認証サービスを使用するように構成されている web アプリケーションから情報を要求して、ユーザー エージェント (web ブラウザー) 単純な要求のフローを示しています。</span><span class="sxs-lookup"><span data-stu-id="e1b81-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

<span data-ttu-id="e1b81-131">[![](external-authentication-services/_static/image2.png "クリックして、イメージの展開")](external-authentication-services/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-131">[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)</span></span>

<span data-ttu-id="e1b81-132">前の図では、ユーザー エージェント (またはこの例では、web ブラウザー) は、外部認証サービスに web ブラウザーをリダイレクトする web アプリケーションに、要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e1b81-132">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="e1b81-133">ユーザー エージェントは、外部認証サービスにその資格情報を送信し、外部認証サービスが何らかの形で元の web アプリケーションにユーザー エージェントをリダイレクトして、ユーザー エージェントが正常に認証されると場合、トークンが、ユーザー エージェントは、web アプリケーションに送信されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-133">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="e1b81-134">Web アプリケーションは、外部認証サービスによってユーザー エージェントが正常に認証されたされ、web アプリケーションがトークンを使用してユーザー エージェントに関する情報を収集するのにことを確認トークンを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-134">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="e1b81-135">アプリケーションが完了するとユーザー エージェントの情報を処理するには、web アプリケーションはその承認設定に基づき、ユーザー エージェントを適切な応答が返すされます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-135">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="e1b81-136">この 2 つ目の例では、web アプリケーションと外部承認サーバーとネゴシエートし、ユーザー エージェントと web アプリケーションは、ユーザーに関する追加情報を取得する外部承認サーバーに追加の通信を実行します。エージェント:</span><span class="sxs-lookup"><span data-stu-id="e1b81-136">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

<span data-ttu-id="e1b81-137">[![](external-authentication-services/_static/image4.png "クリックして、イメージの展開")](external-authentication-services/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-137">[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)</span></span>

<span data-ttu-id="e1b81-138">Visual Studio 2017 と ASP.NET 4.7.2 やすく外部認証サービスとの統合開発者のため、次の認証サービスの組み込みの統合を提供することで。</span><span class="sxs-lookup"><span data-stu-id="e1b81-138">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="e1b81-139">Facebook</span><span class="sxs-lookup"><span data-stu-id="e1b81-139">Facebook</span></span>
- <span data-ttu-id="e1b81-140">Google</span><span class="sxs-lookup"><span data-stu-id="e1b81-140">Google</span></span>
- <span data-ttu-id="e1b81-141">Microsoft アカウント (Windows Live ID アカウント)</span><span class="sxs-lookup"><span data-stu-id="e1b81-141">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="e1b81-142">Twitter</span><span class="sxs-lookup"><span data-stu-id="e1b81-142">Twitter</span></span>

<span data-ttu-id="e1b81-143">このチュートリアルの例では、サポートされている外部認証サービスの各 Visual Studio 2017 に付属している新しい ASP.NET Web アプリケーション テンプレートを使用して構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-143">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="e1b81-144">必要に応じては、外部認証サービスの設定に、FQDN を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b81-144">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="e1b81-145">この要件は、クライアントによって使用される FQDN と一致する、アプリケーションの設定で FQDN を必要とするいくつかの外部認証サービスに対するセキュリティ制約に基づいています。</span><span class="sxs-lookup"><span data-stu-id="e1b81-145">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="e1b81-146">(この手順は各外部認証サービスの大幅に異なります。 これは必要なかどうかに表示するには、各外部認証サービスとこれらの設定を構成する方法のドキュメントを参照する必要があります)。IIS Express を参照してください、この環境をテストするために FQDN を使用して構成する必要がある場合、[完全修飾ドメイン名を使用する IIS Express を構成する](#FQDN)このチュートリアルの後半の「します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-146">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>


<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="e1b81-147">サンプル Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-147">Create a Sample Web Application</span></span>

<span data-ttu-id="e1b81-148">次の手順に従って、ASP.NET Web アプリケーション テンプレートを使用して、サンプル アプリケーションを作成して、このチュートリアルの後半で外部認証サービスごとにこのサンプル アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-148">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="e1b81-149">Visual Studio 2017 を起動し、選択**新しいプロジェクト**スタート ページから。</span><span class="sxs-lookup"><span data-stu-id="e1b81-149">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="e1b81-150">またはから、**ファイル**メニューの **新規**し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-150">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="e1b81-151">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、選択**インストール済み**展開**Visual C#** します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-151">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="e1b81-152">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-152">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e1b81-153">プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション (.Net Framework)** します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-153">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="e1b81-154">プロジェクトの名前を入力し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-154">Enter a name for your project and click **OK**.</span></span>

<span data-ttu-id="e1b81-155">[![](external-authentication-services/_static/image71.png "クリックして、イメージの展開")](external-authentication-services/_static/image71.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-155">[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)</span></span>

<span data-ttu-id="e1b81-156">ときに、**新しい ASP.NET プロジェクト**表示されている、select、 **Single Page Application**テンプレートをクリックします**プロジェクトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="e1b81-156">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

<span data-ttu-id="e1b81-157">[![](external-authentication-services/_static/image72.png "クリックして、イメージの展開")](external-authentication-services/_static/image72.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-157">[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)</span></span>

<span data-ttu-id="e1b81-158">Visual Studio 2017 と待機は、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-158">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="e1b81-159">Visual Studio 2017 のプロジェクトの作成が完了したら、開く、 *Startup.Auth.cs*に配置されているファイル、**アプリ\_開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e1b81-159">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="e1b81-160">外部認証サービスのどれも有効で最初にプロジェクトを作成するときに*Startup.Auth.cs*ファイルはどのようなコードのようになります次の図は、先の強調表示されているセクションを使用すると有効にします。外部認証サービスと ASP.NET アプリケーションで Microsoft アカウント、Twitter、Facebook、または Google の認証を使用するには関連の設定:</span><span class="sxs-lookup"><span data-stu-id="e1b81-160">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="e1b81-161">F5 キーをビルドして、web アプリケーションのデバッグを押すと、外部認証サービスが定義されていないことを「がログイン画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-161">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

<span data-ttu-id="e1b81-162">[![](external-authentication-services/_static/image73.png "クリックして、イメージの展開")](external-authentication-services/_static/image73.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-162">[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)</span></span>

<span data-ttu-id="e1b81-163">次のセクションでは、各 Visual Studio 2017 での ASP.NET で用意されている外部認証サービスを有効にする方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-163">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="e1b81-164">Facebook 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-164">Enabling Facebook authentication</span></span>

<span data-ttu-id="e1b81-165">Facebook を使用して認証では、Facebook 開発者アカウントを作成する必要があります、プロジェクトが必要し、なりますアプリケーション ID と秘密キーの Facebook から機能するためにします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-165">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="e1b81-166">Facebook 開発者アカウントを作成して、アプリケーション ID と秘密キーの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-166">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="e1b81-167">1 回、アプリケーション ID とシークレット キーを取得した、web アプリケーションの Facebook 認証を有効にする、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-167">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="e1b81-168">プロジェクトが Visual Studio 2017 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e1b81-168">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="e1b81-169">コードの Facebook 認証セクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-169">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="e1b81-170">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、アプリケーション ID とシークレット キーを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="e1b81-170">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="e1b81-171">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-171">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="e1b81-172">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Facebook を外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-172">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    <span data-ttu-id="e1b81-173">[![](external-authentication-services/_static/image74.png "クリックして、イメージの展開")](external-authentication-services/_static/image74.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-173">[![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)</span></span>
5. <span data-ttu-id="e1b81-174">クリックすると、 **Facebook**ボタン、お使いのブラウザーは、Facebook のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-174">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    <span data-ttu-id="e1b81-175">[![](external-authentication-services/_static/image22.png "クリックして、イメージの展開")](external-authentication-services/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-175">[![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)</span></span>
6. <span data-ttu-id="e1b81-176">Facebook の資格情報を入力し、をクリックした後**ログイン**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたい、Facebook のアカウント:</span><span class="sxs-lookup"><span data-stu-id="e1b81-176">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    <span data-ttu-id="e1b81-177">[![](external-authentication-services/_static/image24.png "クリックして、イメージの展開")](external-authentication-services/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-177">[![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)</span></span>
7. <span data-ttu-id="e1b81-178">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Facebook アカウント。</span><span class="sxs-lookup"><span data-stu-id="e1b81-178">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    <span data-ttu-id="e1b81-179">[![](external-authentication-services/_static/image26.png "クリックして、イメージの展開")](external-authentication-services/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-179">[![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)</span></span>

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="e1b81-180">Enabling Google Authentication</span><span class="sxs-lookup"><span data-stu-id="e1b81-180">Enabling Google Authentication</span></span>

<span data-ttu-id="e1b81-181">Google を使用して、認証では、Google developer アカウントを作成する必要があります、プロジェクトが機能するためにアプリケーション ID と Google からの秘密鍵を必要は。</span><span class="sxs-lookup"><span data-stu-id="e1b81-181">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="e1b81-182">Google developer アカウントを作成して、アプリケーション ID と秘密キーの取得については、次を参照してください。 [ https://developers.google.com](https://developers.google.com)します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-182">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>


<span data-ttu-id="e1b81-183">Web アプリケーション用に Google 認証を有効にするには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-183">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="e1b81-184">プロジェクトが Visual Studio 2017 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e1b81-184">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="e1b81-185">コードの Google 認証セクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-185">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="e1b81-186">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、アプリケーション ID とシークレット キーを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="e1b81-186">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="e1b81-187">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-187">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="e1b81-188">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Google が外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-188">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    <span data-ttu-id="e1b81-189">[![](external-authentication-services/_static/image75.png "クリックして、イメージの展開")](external-authentication-services/_static/image75.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-189">[![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)</span></span>
5. <span data-ttu-id="e1b81-190">クリックすると、 **Google**ボタン、お使いのブラウザーは Google のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-190">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    <span data-ttu-id="e1b81-191">[![](external-authentication-services/_static/image32.png "クリックして、イメージの展開")](external-authentication-services/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-191">[![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)</span></span>
6. <span data-ttu-id="e1b81-192">Google の資格情報を入力し をクリックした後**サインイン**Google には、web アプリケーションに Google アカウントにアクセスするアクセス許可があることを確認するように求められます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-192">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    <span data-ttu-id="e1b81-193">[![](external-authentication-services/_static/image34.png "クリックして、イメージの展開")](external-authentication-services/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-193">[![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)</span></span>
7. <span data-ttu-id="e1b81-194">クリックすると**Accept**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**Google アカウントに関連付けるにします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-194">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    <span data-ttu-id="e1b81-195">[![](external-authentication-services/_static/image36.png "クリックして、イメージの展開")](external-authentication-services/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-195">[![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)</span></span>
8. <span data-ttu-id="e1b81-196">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Google アカウント。</span><span class="sxs-lookup"><span data-stu-id="e1b81-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    <span data-ttu-id="e1b81-197">[![](external-authentication-services/_static/image38.png "クリックして、イメージの展開")](external-authentication-services/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-197">[![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)</span></span>

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="e1b81-198">Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-198">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="e1b81-199">Microsoft 認証では、開発者アカウントを作成する必要があり、機能するために、クライアント ID とクライアント シークレットが必要です。</span><span class="sxs-lookup"><span data-stu-id="e1b81-199">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="e1b81-200">Microsoft 開発者アカウントを作成して、クライアント ID とクライアント シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-200">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="e1b81-201">1 回、コンシューマー キーとコンシューマー シークレットを取得した、次の手順を使用して web アプリケーション用に Microsoft 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-201">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="e1b81-202">プロジェクトが Visual Studio 2017 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e1b81-202">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="e1b81-203">コードの Microsoft 認証セクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-203">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="e1b81-204">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、クライアント ID とクライアント シークレットを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="e1b81-204">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="e1b81-205">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-205">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="e1b81-206">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Microsoft が、外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-206">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    <span data-ttu-id="e1b81-207">[![](external-authentication-services/_static/image42.png "クリックして、イメージの展開")](external-authentication-services/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-207">[![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)</span></span>
5. <span data-ttu-id="e1b81-208">クリックすると、 **Microsoft**ボタン、お使いのブラウザーは Microsoft のログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-208">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    <span data-ttu-id="e1b81-209">[![](external-authentication-services/_static/image44.png "クリックして、イメージの展開")](external-authentication-services/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-209">[![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)</span></span>
6. <span data-ttu-id="e1b81-210">Microsoft の資格情報を入力し をクリックした後**サインイン**、web アプリケーションが Microsoft アカウントへのアクセス権限を持っていることを確認するメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-210">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    <span data-ttu-id="e1b81-211">[![](external-authentication-services/_static/image46.png "クリックして、イメージの展開")](external-authentication-services/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-211">[![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)</span></span>
7. <span data-ttu-id="e1b81-212">クリックすると**はい**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**を Microsoft アカウントに関連付けるにします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-212">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    <span data-ttu-id="e1b81-213">[![](external-authentication-services/_static/image48.png "クリックして、イメージの展開")](external-authentication-services/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-213">[![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)</span></span>
8. <span data-ttu-id="e1b81-214">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Microsoft アカウント。</span><span class="sxs-lookup"><span data-stu-id="e1b81-214">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    <span data-ttu-id="e1b81-215">[![](external-authentication-services/_static/image50.png "クリックして、イメージの展開")](external-authentication-services/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-215">[![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)</span></span>

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="e1b81-216">Twitter 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-216">Enabling Twitter Authentication</span></span>

<span data-ttu-id="e1b81-217">Twitter の認証では、開発者アカウントを作成する必要があり、機能するために、コンシューマー キーとコンシューマー シークレットが必要です。</span><span class="sxs-lookup"><span data-stu-id="e1b81-217">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="e1b81-218">Twitter 開発者アカウントを作成して、コンシューマー キーとコンシューマー シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-218">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="e1b81-219">1 回、コンシューマー キーとコンシューマー シークレットを取得した、web アプリケーション用に Twitter 認証を有効にする、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-219">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="e1b81-220">プロジェクトが Visual Studio 2017 で開いているときは、開く、 *Startup.Auth.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e1b81-220">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="e1b81-221">コードの Twitter 認証セクションを見つけます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-221">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="e1b81-222">削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、コンシューマー キーとコンシューマー シークレットを追加する文字。</span><span class="sxs-lookup"><span data-stu-id="e1b81-222">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="e1b81-223">これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-223">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="e1b81-224">Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Twitter が外部認証サービスとして定義されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-224">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    <span data-ttu-id="e1b81-225">[![](external-authentication-services/_static/image54.png "クリックして、イメージの展開")](external-authentication-services/_static/image53.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-225">[![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)</span></span>
5. <span data-ttu-id="e1b81-226">クリックすると、 **Twitter**ボタン、お使いのブラウザーは、Twitter ログイン ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-226">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    <span data-ttu-id="e1b81-227">[![](external-authentication-services/_static/image56.png "クリックして、イメージの展開")](external-authentication-services/_static/image55.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-227">[![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)</span></span>
6. <span data-ttu-id="e1b81-228">Twitter の資格情報を入力し、をクリックした後**Authorize app**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたいです。Twitter アカウント:</span><span class="sxs-lookup"><span data-stu-id="e1b81-228">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    <span data-ttu-id="e1b81-229">[![](external-authentication-services/_static/image58.png "クリックして、イメージの展開")](external-authentication-services/_static/image57.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-229">[![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)</span></span>
7. <span data-ttu-id="e1b81-230">ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Twitter アカウント。</span><span class="sxs-lookup"><span data-stu-id="e1b81-230">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    <span data-ttu-id="e1b81-231">[![](external-authentication-services/_static/image60.png "クリックして、イメージの展開")](external-authentication-services/_static/image59.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-231">[![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)</span></span>

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="e1b81-232">追加情報</span><span class="sxs-lookup"><span data-stu-id="e1b81-232">Additional Information</span></span>

<span data-ttu-id="e1b81-233">OAuth および OpenID を使用するアプリケーションの作成に関する追加情報は、次の Url を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1b81-233">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="e1b81-234">外部認証サービスを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="e1b81-234">Combining External Authentication Services</span></span>

<span data-ttu-id="e1b81-235">柔軟性を高め、同時に複数の外部認証サービスを定義する - これにより、web アプリケーションのユーザーからいずれかの外部認証を有効になっているサービス アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-235">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

<span data-ttu-id="e1b81-236">[![](external-authentication-services/_static/image62.png "クリックして、イメージの展開")](external-authentication-services/_static/image61.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-236">[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)</span></span>

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="e1b81-237">IIS Express で完全修飾ドメイン名を使用する構成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-237">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="e1b81-238">一部の外部認証プロバイダーは、アプリケーションのような HTTP アドレスを使用してテストをサポートしていません`http://localhost:port/`します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-238">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="e1b81-239">この問題を回避するには、は、静的な完全修飾ドメイン名 (FQDN) のマッピング、HOSTS ファイルに追加し、テストおよびデバッグの FQDN を使用する Visual Studio 2017 でプロジェクトのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-239">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="e1b81-240">これを行うには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-240">To do so, use the following steps:</span></span>

- <span data-ttu-id="e1b81-241">ホスト ファイル マッピング静的 FQDN を追加します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-241">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="e1b81-242">Windows では、管理者特権のコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-242">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="e1b81-243">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-243">Type the following command:</span></span>

      <span data-ttu-id="e1b81-244"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="e1b81-244"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="e1b81-245">次のようなエントリを HOSTS ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-245">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="e1b81-246"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="e1b81-246"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="e1b81-247">保存し、HOSTS ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-247">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="e1b81-248">FQDN を使用して、Visual Studio プロジェクトを構成するには。</span><span class="sxs-lookup"><span data-stu-id="e1b81-248">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="e1b81-249">プロジェクトが Visual Studio 2017 で開く、クリックして、**プロジェクト**] メニューの [し、プロジェクトのプロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-249">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="e1b81-250">たとえば、選択した可能性があります**WebApplication1 プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-250">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="e1b81-251">選択、 **Web**タブ。</span><span class="sxs-lookup"><span data-stu-id="e1b81-251">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="e1b81-252">FQDN を入力、<strong>プロジェクト Url</strong>します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-252">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="e1b81-253">たとえばは入力<kbd> <http://www.wingtiptoys.com> </kbd> HOSTS ファイルに追加した FQDN マッピングだったかどうか。</span><span class="sxs-lookup"><span data-stu-id="e1b81-253">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="e1b81-254">IIS Express で、アプリケーションの FQDN を使用するかを構成します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-254">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="e1b81-255">Windows では、管理者特権のコマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-255">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="e1b81-256">IIS Express フォルダーに変更するのには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-256">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="e1b81-257"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="e1b81-257"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="e1b81-258">アプリケーションに FQDN を追加するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-258">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="e1b81-259"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="e1b81-259"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="e1b81-260">場所**WebApplication1** 、プロジェクトの名前を指定し、 **bindingInformation**テストに使用するポート番号と FQDN が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1b81-260">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="e1b81-261">Microsoft 認証用のアプリケーションの設定を取得する方法</span><span class="sxs-lookup"><span data-stu-id="e1b81-261">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="e1b81-262">Microsoft 認証用の Windows Live にアプリケーションをリンクは、単純なプロセスです。</span><span class="sxs-lookup"><span data-stu-id="e1b81-262">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="e1b81-263">Windows Live にアプリケーションをリンクしていない場合は、次の手順を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-263">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="e1b81-264">参照する[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) 、Microsoft アカウント名とメッセージが表示されたら、パスワードを入力し、をクリックして**サインイン**:</span><span class="sxs-lookup"><span data-stu-id="e1b81-264">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="e1b81-265">選択**アプリの追加**とメッセージが表示されたら、アプリケーションの名前を入力し、クリックして**作成**:</span><span class="sxs-lookup"><span data-stu-id="e1b81-265">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    <span data-ttu-id="e1b81-266">[![](external-authentication-services/_static/image79.png "クリックして、イメージの展開")](external-authentication-services/_static/image79.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-266">[![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)</span></span>
3. <span data-ttu-id="e1b81-267">下でアプリを選択します。**名前**され、そのアプリケーションのプロパティ ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-267">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="e1b81-268">アプリケーション リダイレクト ドメインを入力します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-268">Enter the redirect domain for your application.</span></span> <span data-ttu-id="e1b81-269">コピー、**アプリケーション ID**し、**アプリケーション シークレット**、**パスワードを生成**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-269">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="e1b81-270">表示されるパスワードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-270">Copy the password that appears.</span></span> <span data-ttu-id="e1b81-271">アプリケーション ID とパスワードは、クライアント ID とクライアント シークレットが。</span><span class="sxs-lookup"><span data-stu-id="e1b81-271">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="e1b81-272">選択**Ok**し**保存**します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-272">Select **Ok** and then **Save**.</span></span>

    <span data-ttu-id="e1b81-273">[![](external-authentication-services/_static/image77.png "クリックして、イメージの展開")](external-authentication-services/_static/image77.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-273">[![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)</span></span>

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="e1b81-274">省略可能:ローカルの登録を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-274">Optional: Disable Local Registration</span></span>

<span data-ttu-id="e1b81-275">現在の ASP.NET ローカル登録機能も自動プログラム (ボット) からメンバーのアカウントを作成します。たとえば、ボット防止と検証などのテクノロジを使用して[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)します。</span><span class="sxs-lookup"><span data-stu-id="e1b81-275">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="e1b81-276">このため、ログイン ページのローカル ログイン フォームと登録リンクを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1b81-276">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="e1b81-277">これを行うには、開く、  *\_Login.cshtml*プロジェクトで、ページし、ローカル ログイン パネルと登録リンクの行をコメントにします。</span><span class="sxs-lookup"><span data-stu-id="e1b81-277">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="e1b81-278">結果のページは、次のコード サンプルのようになります。</span><span class="sxs-lookup"><span data-stu-id="e1b81-278">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="e1b81-279">ローカル ログイン パネルと登録リンクを無効になっていると、ログイン ページは有効にした外部認証プロバイダーをのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1b81-279">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

<span data-ttu-id="e1b81-280">[![](external-authentication-services/_static/image70.png "クリックして、イメージの展開")](external-authentication-services/_static/image69.png)</span><span class="sxs-lookup"><span data-stu-id="e1b81-280">[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)</span></span>
