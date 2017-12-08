---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Katana で Windows 認証を有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "この記事では、Katana で Windows 認証を有効にする方法を示します。 2 つのシナリオに対応します IIS を使用してホスト Katana、を使用して HttpListener を Kat を自己ホストしています.。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="7dd51-104">Katana で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="7dd51-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7dd51-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="7dd51-106">この記事では、Katana で Windows 認証を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="7dd51-107">2 つのシナリオに対応します。 IIS を使用してホスト Katana、を使用して HttpListener をカスタム プロセスでセルフホスト Katana。</span><span class="sxs-lookup"><span data-stu-id="7dd51-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="7dd51-108">この記事のレビュー Barry Dorrans、David Matson および Chris Ross に感謝します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="7dd51-109">Katana は Microsoft の実装[OWIN](http://owin.org/).NET の Web インターフェイスを開きます。</span><span class="sxs-lookup"><span data-stu-id="7dd51-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="7dd51-110">OWIN と Katana の概要を読み取ることができる[ここ](an-overview-of-project-katana.md)です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="7dd51-111">OWIN アーキテクチャでは、複数のレイヤーがあります。</span><span class="sxs-lookup"><span data-stu-id="7dd51-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="7dd51-112">ホストは、OWIN パイプラインを実行するプロセスを管理します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="7dd51-113">サーバー: は、ネットワーク ソケットを開き、要求をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="7dd51-114">ミドルウェア: は、HTTP 要求と応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="7dd51-115">現在、Katana には、Windows 統合認証をサポートする 2 つのサーバーが提供しています。</span><span class="sxs-lookup"><span data-stu-id="7dd51-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="7dd51-116">**Microsoft.Owin.Host.SystemWeb**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="7dd51-117">ASP.NET パイプラインを持つには、IIS を使用します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="7dd51-118">**Microsoft.Owin.Host.HttpListener**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="7dd51-119">使用して[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="7dd51-120">このサーバーは現在、既定のオプションがセルフホストされているときに Katana です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="7dd51-121">Katana は現在提供されませんの OWIN ミドルウェア Windows 認証のため、この機能は、サーバーに表示されています。</span><span class="sxs-lookup"><span data-stu-id="7dd51-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="7dd51-122">IIS で Windows 認証</span><span class="sxs-lookup"><span data-stu-id="7dd51-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="7dd51-123">Microsoft.Owin.Host.SystemWeb を使用すると、単に、IIS で Windows 認証を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="7dd51-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="7dd51-124">「ASP.NET 空の Web アプリケーション」のプロジェクト テンプレートを使用して、新しい ASP.NET アプリケーションを作成して開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="7dd51-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="7dd51-125">次に、NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-125">Next, add NuGet packages.</span></span> <span data-ttu-id="7dd51-126">**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7dd51-127">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="7dd51-128">という名前のクラスを今すぐ追加`Startup`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7dd51-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="7dd51-129">IIS で実行されている、OWIN の"Hello world"アプリケーションを作成する必要がありますすべてです。</span><span class="sxs-lookup"><span data-stu-id="7dd51-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="7dd51-130">F5 キーを押してアプリケーションをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-130">Press F5 to debug the application.</span></span> <span data-ttu-id="7dd51-131">"Hello World!"が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7dd51-131">You should see "Hello World!"</span></span> <span data-ttu-id="7dd51-132">ブラウザー ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="7dd51-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="7dd51-133">次に、私たちは IIS Express での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="7dd51-134">**ビュー**メニューの **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="7dd51-135">プロジェクトのプロパティを表示するソリューション エクスプ ローラーでプロジェクト名をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="7dd51-136">**プロパティ**ウィンドウで、設定**匿名認証**に**無効になっている**設定と**Windows 認証**に**有効になっている**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="7dd51-137">Visual Studio からアプリケーションを実行するときに IIS Express とユーザーの Windows 資格情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="7dd51-138">使用してこれが表示[Fiddler](http://fiddler2.com/home)または別の HTTP デバッグ ツールです。</span><span class="sxs-lookup"><span data-stu-id="7dd51-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="7dd51-139">HTTP 応答の使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="7dd51-140">この応答で Www-authenticate ヘッダーは、サーバーをサポートしていることを示すため、 [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) Kerberos または NTLM を使用するプロトコル。</span><span class="sxs-lookup"><span data-stu-id="7dd51-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="7dd51-141">その後、サーバーにアプリケーションを展開するときに次の[手順](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)をそのサーバーに IIS で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="7dd51-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="7dd51-142">HttpListener で Windows 認証</span><span class="sxs-lookup"><span data-stu-id="7dd51-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="7dd51-143">Microsoft.Owin.Host.HttpListener Katana を自己ホストを使用している場合は上で直接に Windows 認証が有効にすることができます、 **HttpListener**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="7dd51-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="7dd51-144">最初に、新しいコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-144">First, create a new console application.</span></span> <span data-ttu-id="7dd51-145">次に、NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-145">Next, add NuGet packages.</span></span> <span data-ttu-id="7dd51-146">**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="7dd51-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7dd51-147">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="7dd51-148">という名前のクラスを今すぐ追加`Startup`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7dd51-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="7dd51-149">このクラスを実装する前に、同じ"Hello world"の例は、認証方法として Windows 認証を設定します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="7dd51-150">内部、`Main`関数を OWIN パイプラインを開始します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="7dd51-151">Fiddler は、アプリケーションが Windows 認証を使用していることを確認するには、要求を送信することができます。</span><span class="sxs-lookup"><span data-stu-id="7dd51-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="7dd51-152">関連トピック</span><span class="sxs-lookup"><span data-stu-id="7dd51-152">Related Topics</span></span>

[<span data-ttu-id="7dd51-153">プロジェクトの Katana の概要</span><span class="sxs-lookup"><span data-stu-id="7dd51-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="7dd51-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="7dd51-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[<span data-ttu-id="7dd51-155">MVC 5 の OWIN フォーム認証を理解します。</span><span class="sxs-lookup"><span data-stu-id="7dd51-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
